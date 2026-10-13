---
blocks:
- block_id: 146e0937be44ba4e22a94c0f10165221cefd76f129b0e98ec7a5b39b981b430e
  indexed: true
  sha256: 777444c5080fe382ec0cbd580c8d2460fed4554ce0ccac249f7a385cfe4119bf
  source_range:
    end: 16777310
    part: 0
    start: 0
  summary: '围绕 PR #13277 的 KV cache 四平面布局，实验先后发现旧 R21 的 device-wide sync 恢复属于跨 case carry-over，隔离污染后 current-stream 和全卡同步均无法修复命中精度；R22 全程同步仍在真实 hit（prefix_hits=384、query=896）中输出 1024 个 0/“!”。后续正式方案保留 upstream 共享 BlockPool，不强制不同 cache group 共用 block ID，改查物理 plane view、状态生命周期和地址所有权；R23 的 producer-event 等待同样失败。'
  title: 四平面布局与全卡同步因果复核
- block_id: 1f18384ab6848d7efef33c1a4e15ca2125d93df5fa42396d141aaa13367df08f
  indexed: true
  sha256: fe9357abb4f21c7524e007ea5167901485a0e05c8c6e5c9623f9840866dd54cc
  source_range:
    end: 16777310
    part: 1
    start: 0
  summary: R24 在 forward 前后加入 current-stream 或 device-wide sync、绕过 draft 和 fused Mamba postprocess，均保持 miss 逐 token 命中 golden、真实 hit 首 token 为 0，尾 control 也稳定失败，因而同步、异步完成顺序和 postprocess 内容写入被显著降级。一次 warm 的 R25 也已复现同样错误，剩余重点是 zeroer 的地址/语义、cached recurrent source 的选择，以及 preprocess 的 state-block 映射；建议用最小 warm+hit 链路记录 block owner、prev_state_idx/curr_state_idx、物理列、zero 前后、copy 前后和 forward 后内容。
  title: 命中状态链路与 zeroer/preprocess 排查
- block_id: 63514e34ff5715f017eb64dc1998168e93bfdd765a0275b842941802a2eab484
  indexed: true
  sha256: adbb29094e0e7d6c8bed596b9f07d113e660fe788cd616722667a5b4c83b5327
  source_range:
    end: 16777310
    part: 2
    start: 0
  summary: 对 Fable5 评论及 R16/R18 dump 的核对确认 raw cache 由 torch.zeros(int8) 初始化，未写入数据应为有限零；但 recurrent source 整块 98304/98304 非有限，且多个记录共享相同 393216-byte hash，不能简单解释为正常 MLA NoPE bf16 被按 fp32 重解释。当前最高嫌疑是 P1 被外来 writer 覆盖、命中了错误历史列，或 KDA prefill_state_indices 写错列；preprocess copy 更像搬运已损坏数据，zeroer、postprocess、同步和 async scheduler 暂降级，但仍需同一运行内的 owner、写入前后 CRC/finite count 与读取账本闭环验证。
  title: P1 状态块污染与地址取证
- block_id: 7c4e8eff5f42f736121d643ed63021400c73a298f34eb73fee9407c9c5749484
  indexed: true
  sha256: 84ef9114caed14ddfbb9cf074ce6f61f92d8ee82a85e7c89c5925c9a1cdbf948
  source_range:
    end: 16777310
    part: 3
    start: 0
  summary: R26 的低扰动 host-only ledger 复现 hit 错误 token `[0]`，并基本排除了 zeroer 清错 source、worker block 选择错误和 preprocess 裸指针解析错误；R27 进一步保留 warm、copy、KDA 更新及 hit 前的设备快照。静态核查曾发现 NULL_BLOCK_ID=0 与 PAD_SLOT_ID=-1 的语义差异，但随后确认 AscendC 以 `seqLen==0` 跳过 padding，且 prefill 会压缩空 segment，因此该 sentinel 路线降为防御性核查，不能直接视为本案根因。
  title: R26/R27：地址账本排除后追踪 recurrent 内容腐化
- block_id: a30237791810236310fe0e00ddec5077c5dad1bbbc664bb44ca862e8cd8ff6ba
  indexed: true
  sha256: 49c18b0f8bb234d45acaae2b9e04d7fa68d259c1068e08c0e938d6543effa715
  source_range:
    end: 16777310
    part: 4
    start: 0
  summary: R27 在 16/16 TP rank 上发现 block143 在 warm q1、q2 preprocess/copy 和 KDA 更新后仍有限且内容正确，但到下一次 hit 的 preprocess 前变为与 block 0 逐字节相同的全非有限 payload，每个 FP32 word 为 `0x7fffffff`。这排除了 warm checkpoint 生产、preprocess copy 和 zeroer，并将腐化窗口压缩到 warm q2 KDA 完成后至 hit preprocess 前；R28 被设计为按层及 postprocess 边界定位该窗口。块表间接寻址缺少 null/range 护栏被保留为真实潜在缺陷，但其对本次腐化的因果性尚未确认。
  title: R27：确认 block 0 poison 被复制到有效状态块
- block_id: 9b723ce096cb42cd606a5c643a28d1ff206513072b8554e035ac1f91fd2f5f57
  indexed: true
  sha256: e630aad5fbb006339806cb2d9d8912599f8b972ecd1719e5d3c4c079ae18b0f2
  source_range:
    end: 16777310
    part: 5
    start: 0
  summary: R28 显示 block143 从 layer0 至 full forward、Mamba postprocess 前后都保持有限且 SHA 不变，故排除了 warm 生产、后续层物理写碰撞、draft 和 postprocess 本体；腐化只发生在下一次 hit preprocess 前的跨 step 间隙。R29 正在用 execute 入口、`_update_states`、`_prepare_inputs` 等边界取证，当前头号假设是 DP dummy batch/波次收尾在 finished request 的旧 row 尚未清理时复用残留 state index；但静态核对修正了“dummy 走 execute_model”的说法，实际路径是 `EngineCore.execute_dummy_batch → worker._dummy_run`，且 remove_request 通常会清空 block-table row，仍需实验证明竞态和具体写入方向。
  title: R28/R29：腐化窗口收敛到跨 step 空转阶段
- block_id: 54c6088987a0c34d91ab62c22fffe48c7f2519844aa7fe5fe41ec6da6251f45d
  indexed: true
  sha256: c790826f3656dd9d33defe2c18ce11fae28c8a22f838326b08de241499ea7f7f
  source_range:
    end: 17271421
    part: 0
    start: 16777310
  summary: R29 在 16/16 rank 复现了同一边界：postprocess 后至第一次真实 `_update_states` 前后 block143 仍为原 SHA，第二次真实 `execute_model` 首条指令时已变为 block0 的 poison，因而暂未支持 `_update_states`、zeroer 或 `_prepare_inputs` 是写手；scheduler 账本显示中间发生了未经过 `execute_model` 的 dummy 空转。静态证据指向 `_dummy_run` 可能继承旧 block-table/state-index 及 `num_decode_draft_tokens`、`num_accepted_tokens`，但 dummy 假设尚未完成动态验证；R30 将分别抓取 target dummy 与 draft dummy 前后状态、控制量、GDN `spec_state_indices` 和 row0。另有分析脚本将 16/16 误计为 9 条的计数错误，不影响 dump 结论；图捕获期间的 shared-memory 60 秒等待与约 3 分钟启动轨迹被判断为正常长编译提示，而非请求期 c64 hang。
  title: R29：腐化边界锁定，待用 R30 分离 dummy 写入路径
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 05691d983bfe13a0a58a12abafa238b323f3b7b783e8f136958082dc03105f66
  kind: experience
  material_digest: 84374e01fc4ecf023135ce096db1a80389561060baef3c7f26ee43c01109b1a2
  revision: 325e0428633b2bcc3fc98e0e3e2e128e7b11c3295f3e956eb36d4a00eec44233
  schema: mindie-entry/3
  summary: 已排除或暂不支持全卡同步、一般异步时序、zeroer 清错源、warm checkpoint 生产、preprocess copy、主要 forward 层写碰撞及 postprocess 本体；R27 仍证明 block143 被 block0 的 `0x7fffffff` poison 整块替换，R29 将窗口收敛到 warm 后一次真实 0-token execute 与下一次 hit execute 之间的未经过 `execute_model` 的空转。当前最强但未证实的假设是 `_dummy_run`/捕获图消费持久 stale state-index 或 control buffer，且 speculative dummy 未重置控制量；R30 必须区分 target dummy 与 draft dummy 的前后写入，并保留失败结果。若假设成立，修复方向是专用 null/scratch state row 加 explicit non-spec/no-op 控制，同时保留 finished-row 清理；此前 postprocess 间接拷贝缺陷仅作为防御性问题，不升级为已证实根因。
  title: 当前任务导航：验证 dummy 空转造成的跨 step Mamba 状态腐化
navigation: 已排除或暂不支持全卡同步、一般异步时序、zeroer 清错源、warm checkpoint 生产、preprocess copy、主要 forward 层写碰撞及 postprocess 本体；R27 仍证明 block143 被 block0 的 `0x7fffffff` poison 整块替换，R29 将窗口收敛到 warm 后一次真实 0-token execute 与下一次 hit execute 之间的未经过 `execute_model` 的空转。当前最强但未证实的假设是 `_dummy_run`/捕获图消费持久 stale state-index 或 control buffer，且 speculative dummy 未重置控制量；R30 必须区分 target dummy 与 draft dummy 的前后写入，并保留失败结果。若假设成立，修复方向是专用 null/scratch state row 加 explicit non-spec/no-op 控制，同时保留 finished-row 清理；此前 postprocess 间接拷贝缺陷仅作为防御性问题，不升级为已证实根因。
schema: mindie-material-task/1
status: complete
task_id: 05691d983bfe13a0a58a12abafa238b323f3b7b783e8f136958082dc03105f66
---

# 当前任务导航：验证 dummy 空转造成的跨 step Mamba 状态腐化

已排除或暂不支持全卡同步、一般异步时序、zeroer 清错源、warm checkpoint 生产、preprocess copy、主要 forward 层写碰撞及 postprocess 本体；R27 仍证明 block143 被 block0 的 `0x7fffffff` poison 整块替换，R29 将窗口收敛到 warm 后一次真实 0-token execute 与下一次 hit execute 之间的未经过 `execute_model` 的空转。当前最强但未证实的假设是 `_dummy_run`/捕获图消费持久 stale state-index 或 control buffer，且 speculative dummy 未重置控制量；R30 必须区分 target dummy 与 draft dummy 的前后写入，并保留失败结果。若假设成立，修复方向是专用 null/scratch state row 加 explicit non-spec/no-op 控制，同时保留 finished-row 清理；此前 postprocess 间接拷贝缺陷仅作为防御性问题，不升级为已证实根因。

## Materials

- [四平面布局与全卡同步因果复核](blocks/146e0937be44ba4e22a94c0f10165221cefd76f129b0e98ec7a5b39b981b430e.md): 围绕 PR #13277 的 KV cache 四平面布局，实验先后发现旧 R21 的 device-wide sync 恢复属于跨 case carry-over，隔离污染后 current-stream 和全卡同步均无法修复命中精度；R22 全程同步仍在真实 hit（prefix_hits=384、query=896）中输出 1024 个 0/“!”。后续正式方案保留 upstream 共享 BlockPool，不强制不同 cache group 共用 block ID，改查物理 plane view、状态生命周期和地址所有权；R23 的 producer-event 等待同样失败。
- [命中状态链路与 zeroer/preprocess 排查](blocks/1f18384ab6848d7efef33c1a4e15ca2125d93df5fa42396d141aaa13367df08f.md): R24 在 forward 前后加入 current-stream 或 device-wide sync、绕过 draft 和 fused Mamba postprocess，均保持 miss 逐 token 命中 golden、真实 hit 首 token 为 0，尾 control 也稳定失败，因而同步、异步完成顺序和 postprocess 内容写入被显著降级。一次 warm 的 R25 也已复现同样错误，剩余重点是 zeroer 的地址/语义、cached recurrent source 的选择，以及 preprocess 的 state-block 映射；建议用最小 warm+hit 链路记录 block owner、prev_state_idx/curr_state_idx、物理列、zero 前后、copy 前后和 forward 后内容。
- [P1 状态块污染与地址取证](blocks/63514e34ff5715f017eb64dc1998168e93bfdd765a0275b842941802a2eab484.md): 对 Fable5 评论及 R16/R18 dump 的核对确认 raw cache 由 torch.zeros(int8) 初始化，未写入数据应为有限零；但 recurrent source 整块 98304/98304 非有限，且多个记录共享相同 393216-byte hash，不能简单解释为正常 MLA NoPE bf16 被按 fp32 重解释。当前最高嫌疑是 P1 被外来 writer 覆盖、命中了错误历史列，或 KDA prefill_state_indices 写错列；preprocess copy 更像搬运已损坏数据，zeroer、postprocess、同步和 async scheduler 暂降级，但仍需同一运行内的 owner、写入前后 CRC/finite count 与读取账本闭环验证。
- [R26/R27：地址账本排除后追踪 recurrent 内容腐化](blocks/7c4e8eff5f42f736121d643ed63021400c73a298f34eb73fee9407c9c5749484.md): R26 的低扰动 host-only ledger 复现 hit 错误 token `[0]`，并基本排除了 zeroer 清错 source、worker block 选择错误和 preprocess 裸指针解析错误；R27 进一步保留 warm、copy、KDA 更新及 hit 前的设备快照。静态核查曾发现 NULL_BLOCK_ID=0 与 PAD_SLOT_ID=-1 的语义差异，但随后确认 AscendC 以 `seqLen==0` 跳过 padding，且 prefill 会压缩空 segment，因此该 sentinel 路线降为防御性核查，不能直接视为本案根因。
- [R27：确认 block 0 poison 被复制到有效状态块](blocks/a30237791810236310fe0e00ddec5077c5dad1bbbc664bb44ca862e8cd8ff6ba.md): R27 在 16/16 TP rank 上发现 block143 在 warm q1、q2 preprocess/copy 和 KDA 更新后仍有限且内容正确，但到下一次 hit 的 preprocess 前变为与 block 0 逐字节相同的全非有限 payload，每个 FP32 word 为 `0x7fffffff`。这排除了 warm checkpoint 生产、preprocess copy 和 zeroer，并将腐化窗口压缩到 warm q2 KDA 完成后至 hit preprocess 前；R28 被设计为按层及 postprocess 边界定位该窗口。块表间接寻址缺少 null/range 护栏被保留为真实潜在缺陷，但其对本次腐化的因果性尚未确认。
- [R28/R29：腐化窗口收敛到跨 step 空转阶段](blocks/9b723ce096cb42cd606a5c643a28d1ff206513072b8554e035ac1f91fd2f5f57.md): R28 显示 block143 从 layer0 至 full forward、Mamba postprocess 前后都保持有限且 SHA 不变，故排除了 warm 生产、后续层物理写碰撞、draft 和 postprocess 本体；腐化只发生在下一次 hit preprocess 前的跨 step 间隙。R29 正在用 execute 入口、`_update_states`、`_prepare_inputs` 等边界取证，当前头号假设是 DP dummy batch/波次收尾在 finished request 的旧 row 尚未清理时复用残留 state index；但静态核对修正了“dummy 走 execute_model”的说法，实际路径是 `EngineCore.execute_dummy_batch → worker._dummy_run`，且 remove_request 通常会清空 block-table row，仍需实验证明竞态和具体写入方向。
- [R29：腐化边界锁定，待用 R30 分离 dummy 写入路径](blocks/54c6088987a0c34d91ab62c22fffe48c7f2519844aa7fe5fe41ec6da6251f45d.md): R29 在 16/16 rank 复现了同一边界：postprocess 后至第一次真实 `_update_states` 前后 block143 仍为原 SHA，第二次真实 `execute_model` 首条指令时已变为 block0 的 poison，因而暂未支持 `_update_states`、zeroer 或 `_prepare_inputs` 是写手；scheduler 账本显示中间发生了未经过 `execute_model` 的 dummy 空转。静态证据指向 `_dummy_run` 可能继承旧 block-table/state-index 及 `num_decode_draft_tokens`、`num_accepted_tokens`，但 dummy 假设尚未完成动态验证；R30 将分别抓取 target dummy 与 draft dummy 前后状态、控制量、GDN `spec_state_indices` 和 row0。另有分析脚本将 16/16 误计为 9 条的计数错误，不影响 dump 结论；图捕获期间的 shared-memory 60 秒等待与约 3 分钟启动轨迹被判断为正常长编译提示，而非请求期 c64 hang。

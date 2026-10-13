---
blocks:
- block_id: 2790607cb5fbe77a2e82059ffd5ff6f19f40f99c72f199e17ab582e225f5e380
  indexed: true
  sha256: ebcd3581469774cc9cc557b4b3c8be895e2097401cbc3cbe35d699b99d2f9126
  source_range:
    end: 10466393
    part: 0
    start: 0
  summary: R30 运行时证据修正了此前关于 dummy spec 路径的假设：dummy 自身构造的是 non-spec prefill，`metadata.num_accepted_tokens=None`，但 FULL graph 仍残留上一轮真实 spec 分支的固定缓冲区。该残留 GDN/KDA task 使 16/16 rank 的 block143 在 target forward 后变为 block0 的 poison 内容；因此 writer 是 target FULL graph 内的 stale spec task，而不是当前 dummy metadata 或 drafter。
  title: R30定位：FULL图残留 spec 分支改写状态
- block_id: 2d2cb7886f68d1d19166f68db935c290a1407902dd19cd5645673ef0817f1660
  indexed: true
  sha256: eca3f0b5d7ea417224c8caae48d24f09ded84dcca568d4d242d664e14c27e5e9
  source_range:
    end: 10466393
    part: 1
    start: 0
  summary: 修复将 FULL replay 中 `use_spec_decode=True` 且 `num_spec_decodes==0` 的 captured spec 输入刷新为 no-op，同时保留真实 non-spec GDN/KDA 状态索引；不开 speculative decode 时不触碰 spec buffer。目标容器 UT 为 4/4 通过，R31 canonical B2 恢复 golden；async-off 下 B2、P1、C64 通过，但 64K c1/c2 的采信率趋势未稳定复现，且 c2 query 统计按 chunk lookup 累加，不能直接作为结论。64K 输入由固定 GSM8K 样本、A/B marker 和尾部指令构成精确 65,536 token，曾出现 batch shape 改变输出轨迹的现象。
  title: R31修复验证及 async-off 压测
- block_id: 20087312a63668e1323f5894f5e304187d6ad78f79701472a0c19987b7754e10
  indexed: true
  sha256: acda2d96522258dae4b8df5e4f2b9f144c1a8f0e7bdf7da21d861b81bf247715
  source_range:
    end: 10466393
    part: 2
    start: 0
  summary: R32 已开启异步调度，FULL graph capture 和核心状态判据通过：16/16 rank 的 captured spec 输入为 `state=-1/query=0/accepted=0/actual=0`，block143 哈希不变，B2 与 C64 均通过。生产态不清 Prefix Cache 的混合压测中 HTTP 和 preemption 正常，GPQA 8/8 判对；但高并发 GSM8K 曾出现可见正文过短或 `<|open|>`，后续 raw token 说明主要是 reasoning 预算在 512 token 截断边界耗尽，仍属于生产输出协议风险。8K/1K 约 89.06% 命中时 c1/c8 采信率为 69.11%/53.54%，64K/1K 约 89.65% 命中时 c1/c2 为 83.81%/78.12%，均无 preemption 和基础乱码；profiling、MLA DSpark、PR 和 push 尚未完成。
  title: R32 async-on 及生产态压测结果
- block_id: 983d33ae8088f8bc76a620252a781c4d60119db778dde5c0fb9138a54c081ded
  indexed: true
  sha256: cd008beb3c1016a98088f37e6145aca4c33d62953ab982bc1f23911bf3bf6014
  source_range:
    end: 10466393
    part: 3
    start: 0
  summary: 'R33 完成 8K/5 profiling：rank0 trace_view.json 通过完整性与 SHA256 校验；随后定位并修复 K3 配置序列化崩溃，并删除 K3/GQA 中错误的 `block_size == num_speculative_tokens` 校验，相关配置测试通过。MLA R35 已完成 canonical B2 和 8K/1K 验证，但 c8 初始 `all_passed=false` 是 `Discuss rounding: none` 被 `\\bNone\\b` 规则误报，人工复核后确认请求均成功；代码以提交 `1661e2f67` 推送至 PR #13277。'
  title: R33/R35 profiling、配置修复与 MLA 验证
- block_id: 8589752110ef2087246fcc80c1cd77b87958a9acae373308c6f1b308e065e8f6
  indexed: true
  sha256: 1df596a37cc316ad5d9c1af74a13e14556c5a385fd5fceb203f90bb4b7d1649f
  source_range:
    end: 10466393
    part: 4
    start: 0
  summary: MLA c1 采信率为 54.96%、c8 为 42.28%，c8 的质量失败经拆解确认是验证器误报；用户随后指出剩余采信率问题可能与异步调度被同步打断有关。带 stack 的 R36 单机裁剪专家服务复现出约 2.0009 秒的 `aten::to`，调用链落在 `llm_base_proposer → build_draft_attn_metadata → attention_v1.py:291 → Tensor.tolist`，确认 draft attention metadata 将设备 `seq_lens` 拉回 host。对 DSv4/Qwen3.5 旧实现的对比显示，应复用独立 stream/event 异步复制 pinned CPU 上的 `valid_sampled_token_count`，再用 accepted count 修正乐观长度；当前公共路径尚待接回该契约并复测。
  title: MLA 采信率异常与长 aten::to 根因定位
- block_id: d8a4ef991f92a309ee5c5e44ba68cc8453e1de2032f40693413938effae9902c
  indexed: true
  sha256: ad489b4bf881af22e40effb8592059ee61e3e34a04dc0326db906dd2df3ba892
  source_range:
    end: 27244448
    part: 0
    start: 10466393
  summary: '通过复用 pinned accepted-count buffer/event，修复 draft attention 构造 `actual_seq_lengths_kv` 时的设备同步；R37 UT 为 10 passed，`aten::to` 从 2.001s 降至 7.97ms，但 MLA 无 exact 标记分支仍曾引入约 8ms D2H。性能修复以提交 `e77d79a99` 推至 PR #13277，随后历史回溯发现 `34f2ed0e03` 曾处理 async condense 后的 `prev_positions` 重排，而当前代码可能遗漏全量换批时将新 row 的 accepted count 重置为 1；该假设尚未验证为采信率差距的唯一根因。'
  title: accepted-count 长度同步修复与历史回溯
- block_id: 69ef3ffef4438ce570d1ab4488e9f69f7571e92d48a16da5226761b412905e0a
  indexed: true
  sha256: ce520633087889d60bc79b41d6e0d3d787ac539b6302802b083f07331258bfb3
  source_range:
    end: 27244448
    part: 1
    start: 10466393
  summary: 四机 R39 初次启动发现旧 launcher 硬编码 R32 `PYTHONPATH`，修正后确认四 rank 使用 R39、async 默认开启且服务 health=200；四平面容量约为 `311,682 / 135,000 = 2.31x`，不是旧 three-lane 的 1.73x。8K/1K、90% prefix-hit A/B 显示 c1 AL 不变为 5.8378，c8 AL 从 4.7477 降至 4.5852，差距扩大到 1.2526 token，因此 accepted-count 全量换批修复未解决问题并已停止服务、回滚；64K 测试中断，结果不采信。随后 PR 清理为 8 个独立提交，仅删除全局 fence 的添加/移除噪声，并确认独立 draft KV pool 曾改善首轮结果但后续输出退化，尚未形成与 SGLang 同权重同 token 的严格闭环。
  title: 四机 accepted-count A/B 失败并回滚
- block_id: a8929224015ead01c6675423f2e48dcbb89c5145aeb28cd701a20b8d1dbae2d8
  indexed: true
  sha256: 88d68edfaa0efcec8b8479ec98d9c85bd5d98c736bec19244bfbb918d2b31a1f
  source_range:
    end: 27244448
    part: 2
    start: 10466393
  summary: 新增证据显示 `--no-async-scheduling` 的 R29 仍出现多并发采信率下降，因此异步调度不是必要条件，accepted-count row remap 更可能是放大因素；高命中场景中 position 0 即下降，GQA/MLA 均受影响，优先级转向 target 多行 batch、KDA initial-state、共享 prefix 后的 block/slot 元数据及 draft KV 生产消费链。已排除 MLA/GQA 物理 block-ID 直接冲突、request-major/head-major 简单转置、KDA 多序列独立 initial state 算子不一致，以及共享 prefix state 扇出复制错误；相关算子测试此前缺少“多序列+不同 initial state”覆盖，但单卡逐字节 A/B 通过。当前正在进行 DSpark 多行 context/query 映射内核 probe，尚无根因或修复结论。
  title: 从异步回归转向 prefix-hit 多行公共路径
- block_id: 7338ac84324e7b3df3d0654e6e050aedee735179ca597589160b54d7f3a99e8a
  indexed: true
  sha256: 366699ffd52df666972e30c3ecf710fc0ec8fd1148600800d814a2a54aadc6b3
  source_range:
    end: 27244448
    part: 3
    start: 10466393
  summary: GQA/MLA context-KV、draft attention、KDA 预拷贝/causal-conv/recurrent，以及 target MLA causal prefill 的 B=8 对串行探针均通过；重复物理块和 batch 支持问题暂未成立。async-off 仍可复现同 prompt、多并发且 prefix-hit 时的下降，因此异步最多是放大因素；但 async-off 的输出 token hash 也发生变化，采信率差异可能混入目标模型轨迹分叉。当前重点转向真实请求的 block-table/slot-mapping、MLA/GQA group 归属、Mamba align 跨 step 状态搬运及共享边界读写，同时保留“尚未定位根因或验证修复”的结论。
  title: 算子级排除后聚焦共享前缀状态与轨迹分叉
- block_id: 9f3ca01930ea1d77389d3372904b54a4761ca082db305257f10060aa13b13552
  indexed: true
  sha256: d0a1bf3b5f74ff06901c8cfaeee2dbee45ad731730e9c3985903870d085feb0b
  source_range:
    end: 44045521
    part: 0
    start: 27244448
  summary: 在16专家裁剪权重上，TP16 与 DP2×TP8 的128K采信率约98%，未复现 TP8 为0；P/D 的100%采信是假象，命中后输出全为 `!`。8K no-hit 与128K no-hit正常，故障集中在 P/D 长前缀二次命中；随后发现即使清理逻辑缓存，物理 recurrent state 仍可能污染，问题尚未归因闭合。
  title: TP16/DP2×TP8/P-D 128K 采信率与长前缀异常
- block_id: 34df82f7df83ad42581a7d51ea3f83f569102781810d7b75acdb53073da9c61b
  indexed: true
  sha256: b36d09458600820d06142ca4fbb26f515af9788f14e891c781d87748105e9526
  source_range:
    end: 44045521
    part: 1
    start: 27244448
  summary: P/D 读取前增加1秒延迟后仍稳定复现二次命中全 `!`，排除了简单的 DMA 抢读竞态；P 自身在传输前已生成错误 token，D 只是继承坏状态。普通 TP16/DP2×TP8 通过补充所有 FullAttention group 的 hybrid hit trim 恢复，但 P/D producer 路径仍失败，进一步指向 producer scheduler 的错误 partial-group 命中选择。
  title: 长前缀 recurrent state 命中损坏及 hybrid trim 修复验证
- block_id: 1b4bc94d0f28754945dcce946cb9361005f70c9a30f6de8194413c94f86c9cf7
  indexed: true
  sha256: d33c33e23ff1590249b02ae89336423ee6dbf5f552cb8701602e2cb051d4f1a5
  source_range:
    end: 44045521
    part: 2
    start: 27244448
  summary: producer runtime dummy 改为 non-uniform 及整体 no-op 均未解决有效 P/D 命中，相关假设被否定；最终证据显示默认 BalanceScheduler 在 P 节点错误使用仅适用于 D 的 per-group lookup，导致命中129408 token却缺少对应 Mamba checkpoint。窄化 producer/Mamba 条件后的修复通过 CPU UT（15 passed, 1 skipped），P 直连及真实 P/D 冷/命中和重复轮次均恢复正常；混部 trim 修复与 P/D 修复已分别提交并推送，但后续全量 GSM8K、随机命中/不命中及单/多并发压测尚在启动阶段，不能视为已完成验证。
  title: P/D producer scheduler 根因修复与回归
- block_id: 12230bed50f760ce8f5445ce240667197734ad9cd8640cf3dca271995efc906c
  indexed: true
  sha256: 8bd56e65c28ae155e1d4a0ce660b48b7e2072a6475226ac5d8e43fc379cfe11a
  source_range:
    end: 44045521
    part: 3
    start: 27244448
  summary: 统一 workload 使用同一 token-ID 文件和 SHA，P/D 在旧实现中于 boundary/random 阶段出现全 `!`、100% 采信及状态池污染；直连与 reset 结果曾将问题定位到 P-DP0 物理状态，但后续又发现公共 AscendC `block0` 合约和 Prefix Cache 生命周期均可能相关，均未完全验证。按用户纠正后，主修复改为 idle dummy 的一致 zero-seqlen GDN metadata：保留图形状，令 `query_start_loc`、`num_actual_tokens`、`max_query_len` 为零，使 conv/KDA 跳过状态读写，普通 decode 不走该分支；host 契约测试通过，P/D 重启和首个真实请求未出现断言或 worker failure。exact-8K 前置波次已通过，但这只代表局部验证，后续随机混压仍需继续。
  title: P/D zero-seqlen 修复与随机压测前置结果
- block_id: 3835c577e8f914f84db8ab9a4d740286645d6e283e739bb416ccee1900f33ecc
  indexed: true
  sha256: 4b04ac666f5fd268fb5909e5534cf3b8e1940ed3ade810b226984e4567a0e714
  source_range:
    end: 44045521
    part: 4
    start: 27244448
  summary: zero-seqlen 版本的 exact-8K phase 通过：总采信 15208/15708（96.8169%），17/17 transport、17/17 quality，零乱码、零 preemption 和采信异常；128K、383/384/385 边界及 19/19 边界请求也通过，说明该修复覆盖了部分旧的 dummy 生命周期问题。随机混压仍未通过：64/64 传输成功但仅 33/64 无乱码，31 条出现全 `!`，与旧版结果近似；坏请求在相同 32K/65K 前缀第二次命中时出现，暂指向 Prefix Cache 命中 checkpoint 的 copy-on-write/物理槽别名，而非已验证根因。另发现 DSpark draft 状态/通信写入与 Mamba 异步 postprocess 之间可能缺少完整 event 依赖；混部 prefix-on、P/D prefix-on 和 prefix-off 的最终门禁尚未完成。
  title: P/D 边界通过但随机混压仍失败，转向 COW/异步依赖
- block_id: a17c6943f828198717bfc936d8d8b49242efd0c9ca82a1b27e494db2edc95507
  indexed: true
  sha256: 3a05728b0af536850d14887a7a8934679e73f0b7cbfe444e43f704d8658e74d3
  source_range:
    end: 60824897
    part: 0
    start: 44045521
  summary: 最初确认整批结束后新请求可能继承旧 `num_accepted_tokens`，该修复使首轮重复命中恢复，但后续 wave4 仍出现 9/64 乱码，且单并发 32K 回放呈坏/好交替；混部同批 7/7 正常，而 P/D 仅 3/7 正常，问题收敛到 P/D 专属状态传输或复用。Mooncake checkpoint 索引、DP 路由、GQA V 注册及 producer stream 同步等候选先后被实验削弱或否定；accepted-state 发布顺序修复在清缓存后仍复现，且只同步 P 端、D 端元数据或全设备流均未根治。当前更强的未决方向是 D 侧远端接收完成与所有 TP rank 状态可见性、waiting-loop/dummy 覆盖，以及首轮 target forward 前的 recurrent state 或 attention metadata 语义。
  title: P/D 状态生命周期与异步复用排查
- block_id: 00dd2bd0d2554274d20802bbafaf28996a1d5770a899e4f2595239d10a90e50e
  indexed: true
  sha256: 94d25d0f082c357168b8ca2fbd16c3cc78b22ecbd5852f69b2f4e2ab3db58e93
  source_range:
    end: 60824897
    part: 1
    start: 44045521
  summary: 清空 P/D 缓存后，原始坏例仍稳定复现：warm 双端均为 0 命中，wave1/wave3 采信率约 55.49%/53.97%；P 直出与混部逐条一致，偏差只在 P/D 传输后出现。图捕获 `_warmup_and_capture()`、运行期 dummy、DSpark draft context-KV 写入、`spec_state_indices`/`num_accepted_tokens` 固定图输入、`update_stream` 等均被分别检查或 A/B，多个候选要么破坏 capture、要么与坏基线相同；`.cpu()` 造成的改善被视为偶然同步，不能作为修复。探针曾因 `.cpu()` 隐式同步、部署参数不一致和 D 端端口回收问题产生干扰；跨 TP 聚合框架存在，当前仍需确认 D 的 waiting 状态是否在所有接收线程/TP rank 完成前放行，以及是否有 dummy 再次覆盖刚接收的状态。
  title: D 端图输入与跨流/跨 rank 就绪实验
- block_id: 91677f93f319e58f0efbf8ce558204917bac702d449937f21ee521d26a05473c
  indexed: true
  sha256: ea7267986202580555d62a2ea03b5afaf74ec24cae8d7b5f05cebd0d9ee1471d
  source_range:
    end: 60824897
    part: 2
    start: 44045521
  summary: 确认 D 的 K3 DSpark dummy 会复用 `_context_slot_mapping_buffers` 并执行 context KV 预写，因此把 context 长度置零或跳过共享 DFlash/DSpark first-pass 被验证为真实越界风险；但该候选在修正 RoPE 空形状崩溃后仍与坏基线完全相同，不能视为当前 P/D 腐化根因。字节级摘要显示 14/14 个 Mamba group 的 P 发布块与 D 收包块一致，且收包后到首个 forward 前未被改写，排除了 Mooncake 源块选择、传输字节和简单覆盖；新请求行 accepted-count 重置也再次被否定。限制为单 token 后，D 首 token 已偏离 P 直出，说明问题早于 draft acceptance，正在用最后 prompt token、`h(N-1)`/`h(N)` 与 attention KV/metadata 对照定位；一次单步探针因把 `inputs_embeds` 路径误读为 `input_ids=None` 尚未得到有效计算。
  title: Dummy 状态污染与字节级 P/D 对账
- block_id: 038c3859330267f0c198c9efbba6e9897f232ddd013f89ce80ea38d815ab9691
  indexed: true
  sha256: 028fc6c38e4b0c6137c73ecc4c3f28a8410bcc2a8507909e5059172d939b6ddd
  source_range:
    end: 60824897
    part: 3
    start: 44045521
  summary: 'Mooncake 方向/长度/内容、dummy/pre-copy 覆盖、producer/current-stream/device 同步及 draft KV 等候选仍有不确定性；将输出限制为 1 token 后，D 首 token 已从 P 的 `Carolyn` 变为 `The`，且尚无 draft/acceptance 统计，因此“draft KV 导致首个坏点”目前可排除，问题早于 draft KV proposal。待用 token id `25`、position `32767` 重算并比较 D 的 `h(N)` 与 P 的 `h(N)`，以区分首轮 GDN/KDA 输入分类/索引/状态推进和 attention KV/metadata；该单步结论尚未完成。idle dummy 的 `seqlen=0` 修复已生成并推送 commit `7aae3659e` 到 PR #13277，P/D 调试改动仍未提交；本机仅通过 Ruff、`diff --check` 和 Python 编译，因缺少 torch/vLLM 未运行 pytest。后续计划增加 opt-in 的 hidden→logits/top-k→sampler/accepted→emitted token dump，配对同一减层权重、同一 prompt 的混部与 P/D，且需注意 SHA 探针 `.cpu()` 可能引入同步并掩盖异步竞态。'
  title: P/D 首轮 target forward 偏差与 dump 定位
- block_id: 716886971ad1d7e7e11341eb855e00a6ccfe57520f1139194d863c4c8ab622db
  indexed: true
  sha256: 2f4a2333eed8c0d7cc6a725f6f5edb9d7ec3e76465f257f8aba9a1c0c11cc26e
  source_range:
    end: 77116066
    part: 0
    start: 60824897
  summary: 通过 target logprobs、recurrent state、Full-Attention KV 及 block-table 的多轮对账，已报告 P→D 状态传输和实际寻址逐字节一致；本地 D prefill 与外部 continuation 的差异则从 layer 0 开始累积。直连 consumer 的 prefix-hit 复读被限定为缺少远端 Mamba 状态的无效对照；随后逐层 DFX 先出现编译常量折叠导致的空 dump，R34 才取得有效 DP0/DP1 数据。该块保留了服务重启、容器只读、端口冲突、探针读取错误等失败和未提交诊断代码。
  title: P/D 传输与 forward 边界定位
- block_id: 5db0b9034d416ea68212e3b0afce3d7d7ed6ba82894ff1dab496db1fd3b4d512
  indexed: true
  sha256: 4ba8637aaee2c96fd8580fe8354e5e2dc44c0d38f90f8e00e3d908805e05cbce
  source_range:
    end: 77116066
    part: 1
    start: 60824897
  summary: '在 exact-HEAD 的混部与 P/D 测试中，Prefix Cache 关闭矩阵曾全部通过，但开启后随机混压复现 token0/连续 `!`，P/D 也出现同类失败；因此不能把 PR #13277 判为已安全。`num_accepted_tokens` 全新 batch 重置候选只改善部分 wave，async-off、full-device fence 和 early-wait 候选仍分别出现失败，显示问题不只由异步调度或单一等待顺序造成。测试还记录了旧实验补丁污染、客户端 SSH/Docker 引号失败，以及 48/64、49/64 等未通过结果。'
  title: PR HEAD 压测复现异步状态污染
- block_id: b653310fb7e3451e6fdd5bd2de62584d3292dac0ae5bbc66a3d0b64935473926
  indexed: true
  sha256: 34ead65662e4bcaf0c27e69b462b73a9c028315255573937e699c60d4dd1257a
  source_range:
    end: 77116066
    part: 2
    start: 60824897
  summary: 失败交集集中在 DP1 的 `random-hit-28`、131K 冷请求及前序 finalized 行/空转 dummy 后的状态污染；单独冷 prefill、单次 dummy 或并发本身均未稳定复现。累计 warmup 后曾出现 token0，也出现 HTTP 正常但 `random-hit-26` 的 “The answer is that…” 周期性复读，说明仅检查乱码不足。将 GDN/KDA 空转 dummy 改为 eager/no-op 后，同一 warmup→C8 链从旧实现 4/8 提升至 8/8，关键 token hash 恢复；但这只是候选实验结果，当前基于 PR HEAD、同时含 accepted-token 重置和 dummy 修复的全矩阵仍在运行，尚未完成验证。
  title: dummy eager/no-op 候选的初步正向结果
- block_id: 3d04b0964fae0709f079b628fcc94d3b6745626e40f9242374d8155a4995793d
  indexed: true
  sha256: d319f5589281187deeb43de03c39b5fb733e4ee2c826b731e6ef7db86bb9249a
  source_range:
    end: 77116066
    part: 3
    start: 60824897
  summary: 在 16 专家减层权重上，GQA 的 Prefix Cache 命中压力复现了 token0/连续 `!`，并定位到 undersized FULL graph padding 行可能因 `fill_(0)` 写入 block0；但部分 reader、non-spec hit 等 A/B 未证明任意 reader 都会破坏 checkpoint。MLA DSpark 单机及后续 MLA P/D fresh 测试未复现同类硬乱码，却出现约 8%–16% 采信率和明显复读，因此只能记为逻辑质量告警，受减层权重不匹配等因素干扰。8K→5 单 rank profiling 成功采集并离线解析：`aten::to` 最大 0.338 ms，主要 host 阻塞是 `Event::synchronize` 506.143 ms，定位到 `llm_base_proposer.py:_prepare_parallel_draft_seq_lens_cpu`；另有 MoE `_preprocess` 同步链，相关路径和文件名含有 redacted 占位符。
  title: MLA DSpark 与 GQA 的 Prefix Cache 污染对照及单 rank profiling
- block_id: 614220ccc94a880000ed1a51537c5d8fdaa505de133ba466ce86f14d6fa02a31
  indexed: true
  sha256: 1fe21732c43e98747377147555a209b2d408c7bd7cb89c8dbba6c1a49a6fa793
  source_range:
    end: 77116066
    part: 4
    start: 60824897
  summary: 评估认为将 GQA draft 从 target `kv_cache_config` 移出、共享逻辑 block/slot 但使用独立物理 KV 池，理论上可把 MLA DSpark 的 metadata group 从 20 降至 4（1 个 MLA、3 个 Mamba/GDN），并解除 target/draft backing 的物理别名；但 dummy `seqlen=0` 生命周期修复及 P/D companion KV 传输仍必须保留或补齐。profiling 显示 `_build_attention_metadata` 平均约 7.45 ms/forward，三个 GDN builder 约 5.97 ms/forward，独立池不能单独消除公共 metadata 计算或 D2H 风险。随后实现了 draft tensor 独立分配、容量/生命周期和传输元数据的初步切分，远端 CPU 单测报告 `78 passed`；但 COW 的 Ascend block-major 与 conv/recurrent/K/V 平面布局冲突仍在收口，尚无实机 Prefix Cache/P/D 验收或根治结论。
  title: SGLang 式独立 draft KV 池方案与初步实现验证
- block_id: 4c98eec294ae03f1a9c4a0384694989e231efcb3192fe9918da6a803abc0b6e7
  indexed: true
  sha256: befad787723f5ff5bb63d74e92b3a7c7c350473bb2521ddffcc032a92430db18
  source_range:
    end: 90721073
    part: 0
    start: 77116066
  summary: 'GQA DSpark 方向被冻结，MLA DSpark 曾实测发现 CANN 9.1/torch-npu 2.10 的 FIA 可接受 NPU seqlen，但 schema 仅声明 `SymInt[]?`，因此 device tensor 方案被否定。随后按 Qwen3.5 MTP 改用 K3 MLA async 的 host optimistic seqlen，并修复 QuaRot `AscendVocabParallelEmbedding` 深拷贝递归到不可 pickle 的 `ProcessGroup`；远端测试曾报告 63 passed、后续 PR #13277 提交为 `387125a53`。153/154 A/B 启动期间遇到端口占用和旧服务，QuaRot 启动错误根因为 `deepcopy` 通信组，修复后服务曾健康运行，但后续 profiling/A-B 证据仍需谨慎解释。'
  title: MLA DSpark 假 seqlen、QuaRot 修复与 PR 提交
- block_id: b44f9bd0f89999fb3ab4572b6079dfe70b13ac660056781f83c947dcb2b497d4
  indexed: true
  sha256: cd8d82fae41a8e2fcc6f349cad9619bda2522e507f78848963ace51cab990087
  source_range:
    end: 90721073
    part: 1
    start: 77116066
  summary: 153 baseline 与 154 fake-seqlen MLA 服务均曾成功拉起并健康，但 8K1K c1/c2 中 154 decode ITL 约 488–490 ms，明显高于 baseline，采信率在输出一致时相同。代码检查发现 `parallel_draft_seq_lens_cpu` 被直接作为 FIA `actual_seq_kvlen`，且移除 `valid_sampled_token_count_event.synchronize()` 同时移除了 target 到 DSpark draft 的完成栅栏；后续 profiling 又发现 154 混入 dummy-eager/旧字节码，因而不能把早期退化直接归因于 fake-seqlen。远端分支 `548d0a4cf` 提出把 D2H/event 等待延迟到第一次 FIA 前，并应限制在 DSpark eager 路径；153/154 的 profiling 还显示 153 有 25 次 ACLGraph replay，而污染的 154 为 0 次。
  title: 153/154 对照暴露 fake-seqlen 退化与污染环境
- block_id: 2d5cdc57e8ffcf3d86f25e7ce4fcccf8a6d74425bb0cfdea6629184092b3862a
  indexed: true
  sha256: 74a7de5cc4e41333ddfe7244b9bfdebce47b0856c41616dac4f7d2746b0ea93c
  source_range:
    end: 90721073
    part: 2
    start: 77116066
  summary: 验证分支将 `548d0a4cf` 干净叠加到 PR 基线，曾报告 71 passed，并在 154 以 profiling 配置但未先启动采集的方式重新拉起；随后将 `387125a53` 替换为父提交 `7aae3659e` 上的 replacement，最终记录的 PR HEAD 为 `42f80061b`，保留 QuaRot `ProcessGroup` 修复并改为首次 FIA 前修正真实 reject。新的 8K1K 对照中 c1/c2 曾显示采信率、平均采信长度与 baseline 接近，ITL 约快 3.1%，但这是阶段性运行结果；后续按无 stack 的 start→8K20→stop 采集并解析了 154 rank0 `trace_view.json`，同时保留了带客户端 import 空闲前奏的失败尝试。最新未完成问题是旋转量化权重采信率接近 0，待用用户提供的单机权重复现，重点核查主模型 hidden states 是否仍为旋转域、draft 输入是否缺少反旋转适配，并参考 Eagle3 等中间层 hidden-state 投机路径；目前尚无已验证根因。
  title: defer reject 替换 fake-seqlen 与旋转量化采信率问题
- block_id: b432e30e4c9825c6529b46f41a4b07b0ce7ee17f8598be7f543e5e8958e5f5a2
  indexed: true
  sha256: c1ea00d2b6eba9213903e1ab031836f62894f6d01d0cc6821e7ec35c6bb13d15
  source_range:
    end: 90721073
    part: 3
    start: 77116066
  summary: '0726 QuaRot 的 K3 MLA DSpark draft 缺少对 `model.context_proj` 五路 hidden-state 输入块的 `global_rotation` 对齐，导致相同 8K/256 请求采信长度从 7.939 降至 1.041；加入 16/16 rank 启动期对齐后恢复至 6.525，target 输出 token hash 未变，相关 UT 报告为 7 passed。修复以 commit `2c16a64d5` 推送至 PR #13277；四机 TP16×DP4、8K/1K、DP0 c1 的三轮 TPOT 为 20.50/20.59/20.24 ms，均值 20.44 ms，未复现 30+ ms 回退。一次启动失败由缺少 `binary_info_config.json` 导致 `AddRmsNormBias` 注册错误，补齐完整 vendor 包后服务健康；短 profiling 样本的 41.98 ms 不应作为吞吐结论，时间线显示主要成本为设备侧 all-gather、reduce-scatter 与 `GroupedMatmul`，未见重复长 `aten::to` 或明显 host 空泡。'
  title: QuaRot context_proj 修复与四机 TPOT 复测
- block_id: 732e5eebf700127cd5bb6ecc151366554eb8dd396e608cf8ee362b13b13c5f9a
  indexed: true
  sha256: 0563616f5cf82872adbd9412b60bfde11af61a96d11a2e9236d40664961738ea
  source_range:
    end: 90721073
    part: 4
    start: 77116066
  summary: 按原生 `vllm bench serve` 测得精确 8192→1024 token：单 DP0、全局 c1 的 TPOT 为 20.16 ms、ITL 122.06 ms、采信长度 6.059、采信率 72.274%，与此前自定义 runner 的 20.44 ms 基本一致。每个 DP 各 1 并发、全局 c4 的平均 TPOT 为 18.62 ms、ITL 112.24 ms、采信长度约 6.054、采信率 72.207%，4/4 成功且四个 DP 未出现明显采信率下降。短 warmup 曾出现采信长度 3.65，但长输出未复现，不能作为正式性能结论；历史同口径 TPOT 19.39 ms、采信长度 6.55 与当前结果仍存在口径或性能差异，结果文件路径已被脱敏。
  title: vLLM benchmark 单 DP 与每 DP 并发结果
- block_id: 4cbfdc309801d9d13f9e209c158bc9364771fc12fa8afd6496924399ebd8dc3d
  indexed: true
  sha256: 1e8a37226eccb610dfe53b79f6883dcc00d0cfb4eb69455e35818e9f80c38d16
  source_range:
    end: 97668302
    part: 0
    start: 90721073
  summary: 'AISBench 3.1.0 在独立环境安装并完成了精确 token-ID 的 8K→1K 测试；全局并发 1 时 TPOT 19.6 ms、采信长度 6.06，改用字符串重分词后曾得到 TPOT 42.6 ms、采信长度 2.80，显示输入边界会显著影响结果。随后用全局并发 4 验证了 DP4 每 DP 一路，4/4 成功、TPOT 17.7 ms、采信率 72.274%，但这些结果依赖具体配置和路由核验。GPQA-Diamond 测试在 24/198 部分结果后被中断；PR #13277 随后被重写为 MLA-only，并开始用单机 TP16 裁剪权重验证混部与 P/D，初次请求因缺少 `get_draft_attn_causal()` 报 `AttributeError`，补充返回 `False` 的兼容接口并通过 33 项测试后才继续。'
  title: AISBench复现与PR命令整理
- block_id: 97f63994d17e99f39f3115a06c58c68b2b7d1fe39cb4bdfba583876b43750bf1
  indexed: true
  sha256: a3701f4a4baa224410088524cc5ef685ea7eec043d1934733393c9d02cd8c483
  source_range:
    end: 97668302
    part: 1
    start: 90721073
  summary: 四机 Prefix Cache 开启后，8K→1K 无命中测得 Mean ITL 113.98 ms、TPOT 18.69 ms、采信率 73.09%；128K→1K 的设计命中约 99.02%，实际命中 129408/131072（98.73%），但采信率仅 6.55%，TPOT 80.63 ms 不宜直接与 8K 结果横比。AISBench Prefix-on 对照的 Mean ITL 为 109.8 ms、TPOT 18.0 ms，Prefix-off ITL 108.3 ms，主要可见 TTFT 从约 3.49 s 降至 0.85 s。GPQA-Diamond 以并发 32 执行后因用户要求停止，最终仅保留约 24/198 的部分结果；随后旧四机服务被终止，相关日志和部分数据保留。
  title: Prefix Cache与GPQA中断
- block_id: 318c964d352e214bbcae97c7ae0f2af3e42e231bb43e5ca6b8f3705a624f67ad
  indexed: true
  sha256: c9f8870924b1f8314a8bf5dc2f5eb10ee983e619745e70f4df45207210a5a3d2
  source_range:
    end: 97668302
    part: 2
    start: 90721073
  summary: MLA-only HEAD 恢复四机服务后，固定全局并发 4 的 8K→1K 基线为 ITL 109.15 ms、TPOT 17.82 ms、采信长度 6.126；相对旧口径的改善方向已记录，但仍受 workload、版本和采信差异影响。原计划的 8K→1K profiling 产生约 672 GB 数据并被中止、删除，性能结果保留；替换采集的 8K→20 请求在 DP0 完成，Profiler start/stop 均返回 200，单 rank trace 解析成功，`trace_view.json` 约 1.11 GB，SHA-256 为 `dab09b1c…403f8a`。服务当时仍返回 `health=200`，但日志中出现固定数量的 `RuntimeError` 文本，已开始区分可捕获异常说明与真正 traceback，不能据此直接宣称无异常。
  title: 四机性能与8K→20 profiling
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 63f2105b1d6ed40da4c9f226e427591befb5ce03ed724dbd1da0685c835eb1d6
  kind: experience
  material_digest: c4ab6e49c06be111b6ed584748bb345bafc67062e53c97648dfe3a4f1abd13e8
  revision: 37620bcf7a137c3e40d30043815306ac6253039d6d2606df6534b879aacde2f2
  schema: mindie-entry/3
  summary: '当前可检索上下文包括：AISBench 精确 token-ID 与全局 c4 复现、Prefix Cache 的 8K/1K 和 128K/1K 数据、GPQA 在 24/198 左右中断、以及 PR #13277 重写为 MLA-only 后的单机 TP16 混部和 P/D 快速功能验证。四机 MLA-only 服务的最新固定 8K→1K 基线记录为 ITL 109.15 ms、TPOT 17.82 ms，但旧结果之间仍存在输入、采信、路由和版本口径差异，不能把改善直接提升为普遍回归结论。8K→1K profiling 原始数据已删除，替换的 8K→20 profiling 已完成单 rank 解析并生成约 1.11 GB 的 `trace_view.json`；仍需保留并核实日志中的 `RuntimeError` 文本含义，以及后续 profiling 解释、Prefix Cache 污染、draft KV 池和 Ascend block/COW 合约等未闭环事项。'
  title: 当前任务导航：MLA-only四机性能与短请求profiling核验
navigation: '当前可检索上下文包括：AISBench 精确 token-ID 与全局 c4 复现、Prefix Cache 的 8K/1K 和 128K/1K 数据、GPQA 在 24/198 左右中断、以及 PR #13277 重写为 MLA-only 后的单机 TP16 混部和 P/D 快速功能验证。四机 MLA-only 服务的最新固定 8K→1K 基线记录为 ITL 109.15 ms、TPOT 17.82 ms，但旧结果之间仍存在输入、采信、路由和版本口径差异，不能把改善直接提升为普遍回归结论。8K→1K profiling 原始数据已删除，替换的 8K→20 profiling 已完成单 rank 解析并生成约 1.11 GB 的 `trace_view.json`；仍需保留并核实日志中的 `RuntimeError` 文本含义，以及后续 profiling 解释、Prefix Cache 污染、draft KV 池和 Ascend block/COW 合约等未闭环事项。'
schema: mindie-material-task/1
status: complete
task_id: 63f2105b1d6ed40da4c9f226e427591befb5ce03ed724dbd1da0685c835eb1d6
---

# 当前任务导航：MLA-only四机性能与短请求profiling核验

当前可检索上下文包括：AISBench 精确 token-ID 与全局 c4 复现、Prefix Cache 的 8K/1K 和 128K/1K 数据、GPQA 在 24/198 左右中断、以及 PR #13277 重写为 MLA-only 后的单机 TP16 混部和 P/D 快速功能验证。四机 MLA-only 服务的最新固定 8K→1K 基线记录为 ITL 109.15 ms、TPOT 17.82 ms，但旧结果之间仍存在输入、采信、路由和版本口径差异，不能把改善直接提升为普遍回归结论。8K→1K profiling 原始数据已删除，替换的 8K→20 profiling 已完成单 rank 解析并生成约 1.11 GB 的 `trace_view.json`；仍需保留并核实日志中的 `RuntimeError` 文本含义，以及后续 profiling 解释、Prefix Cache 污染、draft KV 池和 Ascend block/COW 合约等未闭环事项。

## Materials

- [R30定位：FULL图残留 spec 分支改写状态](blocks/2790607cb5fbe77a2e82059ffd5ff6f19f40f99c72f199e17ab582e225f5e380.md): R30 运行时证据修正了此前关于 dummy spec 路径的假设：dummy 自身构造的是 non-spec prefill，`metadata.num_accepted_tokens=None`，但 FULL graph 仍残留上一轮真实 spec 分支的固定缓冲区。该残留 GDN/KDA task 使 16/16 rank 的 block143 在 target forward 后变为 block0 的 poison 内容；因此 writer 是 target FULL graph 内的 stale spec task，而不是当前 dummy metadata 或 drafter。
- [R31修复验证及 async-off 压测](blocks/2d2cb7886f68d1d19166f68db935c290a1407902dd19cd5645673ef0817f1660.md): 修复将 FULL replay 中 `use_spec_decode=True` 且 `num_spec_decodes==0` 的 captured spec 输入刷新为 no-op，同时保留真实 non-spec GDN/KDA 状态索引；不开 speculative decode 时不触碰 spec buffer。目标容器 UT 为 4/4 通过，R31 canonical B2 恢复 golden；async-off 下 B2、P1、C64 通过，但 64K c1/c2 的采信率趋势未稳定复现，且 c2 query 统计按 chunk lookup 累加，不能直接作为结论。64K 输入由固定 GSM8K 样本、A/B marker 和尾部指令构成精确 65,536 token，曾出现 batch shape 改变输出轨迹的现象。
- [R32 async-on 及生产态压测结果](blocks/20087312a63668e1323f5894f5e304187d6ad78f79701472a0c19987b7754e10.md): R32 已开启异步调度，FULL graph capture 和核心状态判据通过：16/16 rank 的 captured spec 输入为 `state=-1/query=0/accepted=0/actual=0`，block143 哈希不变，B2 与 C64 均通过。生产态不清 Prefix Cache 的混合压测中 HTTP 和 preemption 正常，GPQA 8/8 判对；但高并发 GSM8K 曾出现可见正文过短或 `<|open|>`，后续 raw token 说明主要是 reasoning 预算在 512 token 截断边界耗尽，仍属于生产输出协议风险。8K/1K 约 89.06% 命中时 c1/c8 采信率为 69.11%/53.54%，64K/1K 约 89.65% 命中时 c1/c2 为 83.81%/78.12%，均无 preemption 和基础乱码；profiling、MLA DSpark、PR 和 push 尚未完成。
- [R33/R35 profiling、配置修复与 MLA 验证](blocks/983d33ae8088f8bc76a620252a781c4d60119db778dde5c0fb9138a54c081ded.md): R33 完成 8K/5 profiling：rank0 trace_view.json 通过完整性与 SHA256 校验；随后定位并修复 K3 配置序列化崩溃，并删除 K3/GQA 中错误的 `block_size == num_speculative_tokens` 校验，相关配置测试通过。MLA R35 已完成 canonical B2 和 8K/1K 验证，但 c8 初始 `all_passed=false` 是 `Discuss rounding: none` 被 `\\bNone\\b` 规则误报，人工复核后确认请求均成功；代码以提交 `1661e2f67` 推送至 PR #13277。
- [MLA 采信率异常与长 aten::to 根因定位](blocks/8589752110ef2087246fcc80c1cd77b87958a9acae373308c6f1b308e065e8f6.md): MLA c1 采信率为 54.96%、c8 为 42.28%，c8 的质量失败经拆解确认是验证器误报；用户随后指出剩余采信率问题可能与异步调度被同步打断有关。带 stack 的 R36 单机裁剪专家服务复现出约 2.0009 秒的 `aten::to`，调用链落在 `llm_base_proposer → build_draft_attn_metadata → attention_v1.py:291 → Tensor.tolist`，确认 draft attention metadata 将设备 `seq_lens` 拉回 host。对 DSv4/Qwen3.5 旧实现的对比显示，应复用独立 stream/event 异步复制 pinned CPU 上的 `valid_sampled_token_count`，再用 accepted count 修正乐观长度；当前公共路径尚待接回该契约并复测。
- [accepted-count 长度同步修复与历史回溯](blocks/d8a4ef991f92a309ee5c5e44ba68cc8453e1de2032f40693413938effae9902c.md): 通过复用 pinned accepted-count buffer/event，修复 draft attention 构造 `actual_seq_lengths_kv` 时的设备同步；R37 UT 为 10 passed，`aten::to` 从 2.001s 降至 7.97ms，但 MLA 无 exact 标记分支仍曾引入约 8ms D2H。性能修复以提交 `e77d79a99` 推至 PR #13277，随后历史回溯发现 `34f2ed0e03` 曾处理 async condense 后的 `prev_positions` 重排，而当前代码可能遗漏全量换批时将新 row 的 accepted count 重置为 1；该假设尚未验证为采信率差距的唯一根因。
- [四机 accepted-count A/B 失败并回滚](blocks/69ef3ffef4438ce570d1ab4488e9f69f7571e92d48a16da5226761b412905e0a.md): 四机 R39 初次启动发现旧 launcher 硬编码 R32 `PYTHONPATH`，修正后确认四 rank 使用 R39、async 默认开启且服务 health=200；四平面容量约为 `311,682 / 135,000 = 2.31x`，不是旧 three-lane 的 1.73x。8K/1K、90% prefix-hit A/B 显示 c1 AL 不变为 5.8378，c8 AL 从 4.7477 降至 4.5852，差距扩大到 1.2526 token，因此 accepted-count 全量换批修复未解决问题并已停止服务、回滚；64K 测试中断，结果不采信。随后 PR 清理为 8 个独立提交，仅删除全局 fence 的添加/移除噪声，并确认独立 draft KV pool 曾改善首轮结果但后续输出退化，尚未形成与 SGLang 同权重同 token 的严格闭环。
- [从异步回归转向 prefix-hit 多行公共路径](blocks/a8929224015ead01c6675423f2e48dcbb89c5145aeb28cd701a20b8d1dbae2d8.md): 新增证据显示 `--no-async-scheduling` 的 R29 仍出现多并发采信率下降，因此异步调度不是必要条件，accepted-count row remap 更可能是放大因素；高命中场景中 position 0 即下降，GQA/MLA 均受影响，优先级转向 target 多行 batch、KDA initial-state、共享 prefix 后的 block/slot 元数据及 draft KV 生产消费链。已排除 MLA/GQA 物理 block-ID 直接冲突、request-major/head-major 简单转置、KDA 多序列独立 initial state 算子不一致，以及共享 prefix state 扇出复制错误；相关算子测试此前缺少“多序列+不同 initial state”覆盖，但单卡逐字节 A/B 通过。当前正在进行 DSpark 多行 context/query 映射内核 probe，尚无根因或修复结论。
- [算子级排除后聚焦共享前缀状态与轨迹分叉](blocks/7338ac84324e7b3df3d0654e6e050aedee735179ca597589160b54d7f3a99e8a.md): GQA/MLA context-KV、draft attention、KDA 预拷贝/causal-conv/recurrent，以及 target MLA causal prefill 的 B=8 对串行探针均通过；重复物理块和 batch 支持问题暂未成立。async-off 仍可复现同 prompt、多并发且 prefix-hit 时的下降，因此异步最多是放大因素；但 async-off 的输出 token hash 也发生变化，采信率差异可能混入目标模型轨迹分叉。当前重点转向真实请求的 block-table/slot-mapping、MLA/GQA group 归属、Mamba align 跨 step 状态搬运及共享边界读写，同时保留“尚未定位根因或验证修复”的结论。
- [TP16/DP2×TP8/P-D 128K 采信率与长前缀异常](blocks/9f3ca01930ea1d77389d3372904b54a4761ca082db305257f10060aa13b13552.md): 在16专家裁剪权重上，TP16 与 DP2×TP8 的128K采信率约98%，未复现 TP8 为0；P/D 的100%采信是假象，命中后输出全为 `!`。8K no-hit 与128K no-hit正常，故障集中在 P/D 长前缀二次命中；随后发现即使清理逻辑缓存，物理 recurrent state 仍可能污染，问题尚未归因闭合。
- [长前缀 recurrent state 命中损坏及 hybrid trim 修复验证](blocks/34df82f7df83ad42581a7d51ea3f83f569102781810d7b75acdb53073da9c61b.md): P/D 读取前增加1秒延迟后仍稳定复现二次命中全 `!`，排除了简单的 DMA 抢读竞态；P 自身在传输前已生成错误 token，D 只是继承坏状态。普通 TP16/DP2×TP8 通过补充所有 FullAttention group 的 hybrid hit trim 恢复，但 P/D producer 路径仍失败，进一步指向 producer scheduler 的错误 partial-group 命中选择。
- [P/D producer scheduler 根因修复与回归](blocks/1b4bc94d0f28754945dcce946cb9361005f70c9a30f6de8194413c94f86c9cf7.md): producer runtime dummy 改为 non-uniform 及整体 no-op 均未解决有效 P/D 命中，相关假设被否定；最终证据显示默认 BalanceScheduler 在 P 节点错误使用仅适用于 D 的 per-group lookup，导致命中129408 token却缺少对应 Mamba checkpoint。窄化 producer/Mamba 条件后的修复通过 CPU UT（15 passed, 1 skipped），P 直连及真实 P/D 冷/命中和重复轮次均恢复正常；混部 trim 修复与 P/D 修复已分别提交并推送，但后续全量 GSM8K、随机命中/不命中及单/多并发压测尚在启动阶段，不能视为已完成验证。
- [P/D zero-seqlen 修复与随机压测前置结果](blocks/12230bed50f760ce8f5445ce240667197734ad9cd8640cf3dca271995efc906c.md): 统一 workload 使用同一 token-ID 文件和 SHA，P/D 在旧实现中于 boundary/random 阶段出现全 `!`、100% 采信及状态池污染；直连与 reset 结果曾将问题定位到 P-DP0 物理状态，但后续又发现公共 AscendC `block0` 合约和 Prefix Cache 生命周期均可能相关，均未完全验证。按用户纠正后，主修复改为 idle dummy 的一致 zero-seqlen GDN metadata：保留图形状，令 `query_start_loc`、`num_actual_tokens`、`max_query_len` 为零，使 conv/KDA 跳过状态读写，普通 decode 不走该分支；host 契约测试通过，P/D 重启和首个真实请求未出现断言或 worker failure。exact-8K 前置波次已通过，但这只代表局部验证，后续随机混压仍需继续。
- [P/D 边界通过但随机混压仍失败，转向 COW/异步依赖](blocks/3835c577e8f914f84db8ab9a4d740286645d6e283e739bb416ccee1900f33ecc.md): zero-seqlen 版本的 exact-8K phase 通过：总采信 15208/15708（96.8169%），17/17 transport、17/17 quality，零乱码、零 preemption 和采信异常；128K、383/384/385 边界及 19/19 边界请求也通过，说明该修复覆盖了部分旧的 dummy 生命周期问题。随机混压仍未通过：64/64 传输成功但仅 33/64 无乱码，31 条出现全 `!`，与旧版结果近似；坏请求在相同 32K/65K 前缀第二次命中时出现，暂指向 Prefix Cache 命中 checkpoint 的 copy-on-write/物理槽别名，而非已验证根因。另发现 DSpark draft 状态/通信写入与 Mamba 异步 postprocess 之间可能缺少完整 event 依赖；混部 prefix-on、P/D prefix-on 和 prefix-off 的最终门禁尚未完成。
- [P/D 状态生命周期与异步复用排查](blocks/a17c6943f828198717bfc936d8d8b49242efd0c9ca82a1b27e494db2edc95507.md): 最初确认整批结束后新请求可能继承旧 `num_accepted_tokens`，该修复使首轮重复命中恢复，但后续 wave4 仍出现 9/64 乱码，且单并发 32K 回放呈坏/好交替；混部同批 7/7 正常，而 P/D 仅 3/7 正常，问题收敛到 P/D 专属状态传输或复用。Mooncake checkpoint 索引、DP 路由、GQA V 注册及 producer stream 同步等候选先后被实验削弱或否定；accepted-state 发布顺序修复在清缓存后仍复现，且只同步 P 端、D 端元数据或全设备流均未根治。当前更强的未决方向是 D 侧远端接收完成与所有 TP rank 状态可见性、waiting-loop/dummy 覆盖，以及首轮 target forward 前的 recurrent state 或 attention metadata 语义。
- [D 端图输入与跨流/跨 rank 就绪实验](blocks/00dd2bd0d2554274d20802bbafaf28996a1d5770a899e4f2595239d10a90e50e.md): 清空 P/D 缓存后，原始坏例仍稳定复现：warm 双端均为 0 命中，wave1/wave3 采信率约 55.49%/53.97%；P 直出与混部逐条一致，偏差只在 P/D 传输后出现。图捕获 `_warmup_and_capture()`、运行期 dummy、DSpark draft context-KV 写入、`spec_state_indices`/`num_accepted_tokens` 固定图输入、`update_stream` 等均被分别检查或 A/B，多个候选要么破坏 capture、要么与坏基线相同；`.cpu()` 造成的改善被视为偶然同步，不能作为修复。探针曾因 `.cpu()` 隐式同步、部署参数不一致和 D 端端口回收问题产生干扰；跨 TP 聚合框架存在，当前仍需确认 D 的 waiting 状态是否在所有接收线程/TP rank 完成前放行，以及是否有 dummy 再次覆盖刚接收的状态。
- [Dummy 状态污染与字节级 P/D 对账](blocks/91677f93f319e58f0efbf8ce558204917bac702d449937f21ee521d26a05473c.md): 确认 D 的 K3 DSpark dummy 会复用 `_context_slot_mapping_buffers` 并执行 context KV 预写，因此把 context 长度置零或跳过共享 DFlash/DSpark first-pass 被验证为真实越界风险；但该候选在修正 RoPE 空形状崩溃后仍与坏基线完全相同，不能视为当前 P/D 腐化根因。字节级摘要显示 14/14 个 Mamba group 的 P 发布块与 D 收包块一致，且收包后到首个 forward 前未被改写，排除了 Mooncake 源块选择、传输字节和简单覆盖；新请求行 accepted-count 重置也再次被否定。限制为单 token 后，D 首 token 已偏离 P 直出，说明问题早于 draft acceptance，正在用最后 prompt token、`h(N-1)`/`h(N)` 与 attention KV/metadata 对照定位；一次单步探针因把 `inputs_embeds` 路径误读为 `input_ids=None` 尚未得到有效计算。
- [P/D 首轮 target forward 偏差与 dump 定位](blocks/038c3859330267f0c198c9efbba6e9897f232ddd013f89ce80ea38d815ab9691.md): Mooncake 方向/长度/内容、dummy/pre-copy 覆盖、producer/current-stream/device 同步及 draft KV 等候选仍有不确定性；将输出限制为 1 token 后，D 首 token 已从 P 的 `Carolyn` 变为 `The`，且尚无 draft/acceptance 统计，因此“draft KV 导致首个坏点”目前可排除，问题早于 draft KV proposal。待用 token id `25`、position `32767` 重算并比较 D 的 `h(N)` 与 P 的 `h(N)`，以区分首轮 GDN/KDA 输入分类/索引/状态推进和 attention KV/metadata；该单步结论尚未完成。idle dummy 的 `seqlen=0` 修复已生成并推送 commit `7aae3659e` 到 PR #13277，P/D 调试改动仍未提交；本机仅通过 Ruff、`diff --check` 和 Python 编译，因缺少 torch/vLLM 未运行 pytest。后续计划增加 opt-in 的 hidden→logits/top-k→sampler/accepted→emitted token dump，配对同一减层权重、同一 prompt 的混部与 P/D，且需注意 SHA 探针 `.cpu()` 可能引入同步并掩盖异步竞态。
- [P/D 传输与 forward 边界定位](blocks/716886971ad1d7e7e11341eb855e00a6ccfe57520f1139194d863c4c8ab622db.md): 通过 target logprobs、recurrent state、Full-Attention KV 及 block-table 的多轮对账，已报告 P→D 状态传输和实际寻址逐字节一致；本地 D prefill 与外部 continuation 的差异则从 layer 0 开始累积。直连 consumer 的 prefix-hit 复读被限定为缺少远端 Mamba 状态的无效对照；随后逐层 DFX 先出现编译常量折叠导致的空 dump，R34 才取得有效 DP0/DP1 数据。该块保留了服务重启、容器只读、端口冲突、探针读取错误等失败和未提交诊断代码。
- [PR HEAD 压测复现异步状态污染](blocks/5db0b9034d416ea68212e3b0afce3d7d7ed6ba82894ff1dab496db1fd3b4d512.md): 在 exact-HEAD 的混部与 P/D 测试中，Prefix Cache 关闭矩阵曾全部通过，但开启后随机混压复现 token0/连续 `!`，P/D 也出现同类失败；因此不能把 PR #13277 判为已安全。`num_accepted_tokens` 全新 batch 重置候选只改善部分 wave，async-off、full-device fence 和 early-wait 候选仍分别出现失败，显示问题不只由异步调度或单一等待顺序造成。测试还记录了旧实验补丁污染、客户端 SSH/Docker 引号失败，以及 48/64、49/64 等未通过结果。
- [dummy eager/no-op 候选的初步正向结果](blocks/b653310fb7e3451e6fdd5bd2de62584d3292dac0ae5bbc66a3d0b64935473926.md): 失败交集集中在 DP1 的 `random-hit-28`、131K 冷请求及前序 finalized 行/空转 dummy 后的状态污染；单独冷 prefill、单次 dummy 或并发本身均未稳定复现。累计 warmup 后曾出现 token0，也出现 HTTP 正常但 `random-hit-26` 的 “The answer is that…” 周期性复读，说明仅检查乱码不足。将 GDN/KDA 空转 dummy 改为 eager/no-op 后，同一 warmup→C8 链从旧实现 4/8 提升至 8/8，关键 token hash 恢复；但这只是候选实验结果，当前基于 PR HEAD、同时含 accepted-token 重置和 dummy 修复的全矩阵仍在运行，尚未完成验证。
- [MLA DSpark 与 GQA 的 Prefix Cache 污染对照及单 rank profiling](blocks/3d04b0964fae0709f079b628fcc94d3b6745626e40f9242374d8155a4995793d.md): 在 16 专家减层权重上，GQA 的 Prefix Cache 命中压力复现了 token0/连续 `!`，并定位到 undersized FULL graph padding 行可能因 `fill_(0)` 写入 block0；但部分 reader、non-spec hit 等 A/B 未证明任意 reader 都会破坏 checkpoint。MLA DSpark 单机及后续 MLA P/D fresh 测试未复现同类硬乱码，却出现约 8%–16% 采信率和明显复读，因此只能记为逻辑质量告警，受减层权重不匹配等因素干扰。8K→5 单 rank profiling 成功采集并离线解析：`aten::to` 最大 0.338 ms，主要 host 阻塞是 `Event::synchronize` 506.143 ms，定位到 `llm_base_proposer.py:_prepare_parallel_draft_seq_lens_cpu`；另有 MoE `_preprocess` 同步链，相关路径和文件名含有 redacted 占位符。
- [SGLang 式独立 draft KV 池方案与初步实现验证](blocks/614220ccc94a880000ed1a51537c5d8fdaa505de133ba466ce86f14d6fa02a31.md): 评估认为将 GQA draft 从 target `kv_cache_config` 移出、共享逻辑 block/slot 但使用独立物理 KV 池，理论上可把 MLA DSpark 的 metadata group 从 20 降至 4（1 个 MLA、3 个 Mamba/GDN），并解除 target/draft backing 的物理别名；但 dummy `seqlen=0` 生命周期修复及 P/D companion KV 传输仍必须保留或补齐。profiling 显示 `_build_attention_metadata` 平均约 7.45 ms/forward，三个 GDN builder 约 5.97 ms/forward，独立池不能单独消除公共 metadata 计算或 D2H 风险。随后实现了 draft tensor 独立分配、容量/生命周期和传输元数据的初步切分，远端 CPU 单测报告 `78 passed`；但 COW 的 Ascend block-major 与 conv/recurrent/K/V 平面布局冲突仍在收口，尚无实机 Prefix Cache/P/D 验收或根治结论。
- [MLA DSpark 假 seqlen、QuaRot 修复与 PR 提交](blocks/4c98eec294ae03f1a9c4a0384694989e231efcb3192fe9918da6a803abc0b6e7.md): GQA DSpark 方向被冻结，MLA DSpark 曾实测发现 CANN 9.1/torch-npu 2.10 的 FIA 可接受 NPU seqlen，但 schema 仅声明 `SymInt[]?`，因此 device tensor 方案被否定。随后按 Qwen3.5 MTP 改用 K3 MLA async 的 host optimistic seqlen，并修复 QuaRot `AscendVocabParallelEmbedding` 深拷贝递归到不可 pickle 的 `ProcessGroup`；远端测试曾报告 63 passed、后续 PR #13277 提交为 `387125a53`。153/154 A/B 启动期间遇到端口占用和旧服务，QuaRot 启动错误根因为 `deepcopy` 通信组，修复后服务曾健康运行，但后续 profiling/A-B 证据仍需谨慎解释。
- [153/154 对照暴露 fake-seqlen 退化与污染环境](blocks/b44f9bd0f89999fb3ab4572b6079dfe70b13ac660056781f83c947dcb2b497d4.md): 153 baseline 与 154 fake-seqlen MLA 服务均曾成功拉起并健康，但 8K1K c1/c2 中 154 decode ITL 约 488–490 ms，明显高于 baseline，采信率在输出一致时相同。代码检查发现 `parallel_draft_seq_lens_cpu` 被直接作为 FIA `actual_seq_kvlen`，且移除 `valid_sampled_token_count_event.synchronize()` 同时移除了 target 到 DSpark draft 的完成栅栏；后续 profiling 又发现 154 混入 dummy-eager/旧字节码，因而不能把早期退化直接归因于 fake-seqlen。远端分支 `548d0a4cf` 提出把 D2H/event 等待延迟到第一次 FIA 前，并应限制在 DSpark eager 路径；153/154 的 profiling 还显示 153 有 25 次 ACLGraph replay，而污染的 154 为 0 次。
- [defer reject 替换 fake-seqlen 与旋转量化采信率问题](blocks/2d5cdc57e8ffcf3d86f25e7ce4fcccf8a6d74425bb0cfdea6629184092b3862a.md): 验证分支将 `548d0a4cf` 干净叠加到 PR 基线，曾报告 71 passed，并在 154 以 profiling 配置但未先启动采集的方式重新拉起；随后将 `387125a53` 替换为父提交 `7aae3659e` 上的 replacement，最终记录的 PR HEAD 为 `42f80061b`，保留 QuaRot `ProcessGroup` 修复并改为首次 FIA 前修正真实 reject。新的 8K1K 对照中 c1/c2 曾显示采信率、平均采信长度与 baseline 接近，ITL 约快 3.1%，但这是阶段性运行结果；后续按无 stack 的 start→8K20→stop 采集并解析了 154 rank0 `trace_view.json`，同时保留了带客户端 import 空闲前奏的失败尝试。最新未完成问题是旋转量化权重采信率接近 0，待用用户提供的单机权重复现，重点核查主模型 hidden states 是否仍为旋转域、draft 输入是否缺少反旋转适配，并参考 Eagle3 等中间层 hidden-state 投机路径；目前尚无已验证根因。
- [QuaRot context_proj 修复与四机 TPOT 复测](blocks/b432e30e4c9825c6529b46f41a4b07b0ce7ee17f8598be7f543e5e8958e5f5a2.md): 0726 QuaRot 的 K3 MLA DSpark draft 缺少对 `model.context_proj` 五路 hidden-state 输入块的 `global_rotation` 对齐，导致相同 8K/256 请求采信长度从 7.939 降至 1.041；加入 16/16 rank 启动期对齐后恢复至 6.525，target 输出 token hash 未变，相关 UT 报告为 7 passed。修复以 commit `2c16a64d5` 推送至 PR #13277；四机 TP16×DP4、8K/1K、DP0 c1 的三轮 TPOT 为 20.50/20.59/20.24 ms，均值 20.44 ms，未复现 30+ ms 回退。一次启动失败由缺少 `binary_info_config.json` 导致 `AddRmsNormBias` 注册错误，补齐完整 vendor 包后服务健康；短 profiling 样本的 41.98 ms 不应作为吞吐结论，时间线显示主要成本为设备侧 all-gather、reduce-scatter 与 `GroupedMatmul`，未见重复长 `aten::to` 或明显 host 空泡。
- [vLLM benchmark 单 DP 与每 DP 并发结果](blocks/732e5eebf700127cd5bb6ecc151366554eb8dd396e608cf8ee362b13b13c5f9a.md): 按原生 `vllm bench serve` 测得精确 8192→1024 token：单 DP0、全局 c1 的 TPOT 为 20.16 ms、ITL 122.06 ms、采信长度 6.059、采信率 72.274%，与此前自定义 runner 的 20.44 ms 基本一致。每个 DP 各 1 并发、全局 c4 的平均 TPOT 为 18.62 ms、ITL 112.24 ms、采信长度约 6.054、采信率 72.207%，4/4 成功且四个 DP 未出现明显采信率下降。短 warmup 曾出现采信长度 3.65，但长输出未复现，不能作为正式性能结论；历史同口径 TPOT 19.39 ms、采信长度 6.55 与当前结果仍存在口径或性能差异，结果文件路径已被脱敏。
- [AISBench复现与PR命令整理](blocks/4cbfdc309801d9d13f9e209c158bc9364771fc12fa8afd6496924399ebd8dc3d.md): AISBench 3.1.0 在独立环境安装并完成了精确 token-ID 的 8K→1K 测试；全局并发 1 时 TPOT 19.6 ms、采信长度 6.06，改用字符串重分词后曾得到 TPOT 42.6 ms、采信长度 2.80，显示输入边界会显著影响结果。随后用全局并发 4 验证了 DP4 每 DP 一路，4/4 成功、TPOT 17.7 ms、采信率 72.274%，但这些结果依赖具体配置和路由核验。GPQA-Diamond 测试在 24/198 部分结果后被中断；PR #13277 随后被重写为 MLA-only，并开始用单机 TP16 裁剪权重验证混部与 P/D，初次请求因缺少 `get_draft_attn_causal()` 报 `AttributeError`，补充返回 `False` 的兼容接口并通过 33 项测试后才继续。
- [Prefix Cache与GPQA中断](blocks/97f63994d17e99f39f3115a06c58c68b2b7d1fe39cb4bdfba583876b43750bf1.md): 四机 Prefix Cache 开启后，8K→1K 无命中测得 Mean ITL 113.98 ms、TPOT 18.69 ms、采信率 73.09%；128K→1K 的设计命中约 99.02%，实际命中 129408/131072（98.73%），但采信率仅 6.55%，TPOT 80.63 ms 不宜直接与 8K 结果横比。AISBench Prefix-on 对照的 Mean ITL 为 109.8 ms、TPOT 18.0 ms，Prefix-off ITL 108.3 ms，主要可见 TTFT 从约 3.49 s 降至 0.85 s。GPQA-Diamond 以并发 32 执行后因用户要求停止，最终仅保留约 24/198 的部分结果；随后旧四机服务被终止，相关日志和部分数据保留。
- [四机性能与8K→20 profiling](blocks/318c964d352e214bbcae97c7ae0f2af3e42e231bb43e5ca6b8f3705a624f67ad.md): MLA-only HEAD 恢复四机服务后，固定全局并发 4 的 8K→1K 基线为 ITL 109.15 ms、TPOT 17.82 ms、采信长度 6.126；相对旧口径的改善方向已记录，但仍受 workload、版本和采信差异影响。原计划的 8K→1K profiling 产生约 672 GB 数据并被中止、删除，性能结果保留；替换采集的 8K→20 请求在 DP0 完成，Profiler start/stop 均返回 200，单 rank trace 解析成功，`trace_view.json` 约 1.11 GB，SHA-256 为 `dab09b1c…403f8a`。服务当时仍返回 `health=200`，但日志中出现固定数量的 `RuntimeError` 文本，已开始区分可捕获异常说明与真正 traceback，不能据此直接宣称无异常。

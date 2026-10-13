---
blocks:
- block_id: b659e28a8300b465cf019c7f10abf1dcc08c4c8d37328d0b929a32f523401e34
  indexed: true
  sha256: b0282a1119e0c457b883d14eaf64c4dd397cd34993c5d3d25df81a9380e58c0c
  source_range:
    end: 16781401
    part: 0
    start: 0
  summary: '围绕 vLLM 0.27.1 原生 Kimi K3 能力，重新实现 Ascend 侧 KDA、no-RoPE MLA、SiTU/MoE、MTP、DSpark、FlashComm、Hybrid KV cache 和 A5 MLA Prolog 等适配，并保留了多处接口、算子装载和 CI 路由失败及修正记录。模型层 dummy 验证曾完成真实一 token eager 前向，代码检查、单测和静态门禁多次通过，但 MTP/DSpark 完整分布式运行、完整 checkpoint、长上下文和性能仍未验证。最终形成依赖算子 PR #14426 的 Draft PR #14454；硬件矩阵因缺少维护者 ready 标签而跳过，不能视为正式硬件验收。'
  title: Kimi K3 vLLM 0.27 Ascend 适配与 Draft PR
- block_id: 8a232c6a33a314e09e6fc56f55d4615ab4fe8660fff93f1be3cb121b6bb5dfb4
  indexed: true
  sha256: b0db0e29a1c886f564fb70459306b9679cf236710dd4759b9d0939f8de7ceb5e
  source_range:
    end: 16781401
    part: 1
    start: 0
  summary: '为单机真实权重验证，固定 vLLM 0.27.1 与 PR #14454 源码，完成 A3 算子构建并处理 editable 安装、构建依赖下载和容器旧 PYTHONPATH 遮蔽等环境问题。构建产物生成了数百个 kernel binary，但当时尚未完成服务启动或完整功能矩阵；后续验证计划包括真实权重、流式、Prefix Cache、ACLGraph、EP/FlashComm、DSpark 和输出完整性检查。'
  title: 单机验证准备与精确源码构建
- block_id: eeea6bb75264c6f2de0852146fed32f69ff46b4d12caad9ce90a0bd3bd34fa90
  indexed: true
  sha256: c03afcb6260cda046999c463ff9a4136d6980f9f04eb060c5ebf192cd5bea534
  source_range:
    end: 16781401
    part: 2
    start: 0
  summary: 验证先排除 155 机器，改用 153 单机 16-expert 真实权重，并随后计划使用 153/154 做 P/D 分离；网络网卡绑定、量化映射、Kimi K3 外层/内层别名、MoE 并行补齐、latent projection 和 shared-expert runner 契约等问题已分别暴露并修复，focused UT 达到 58 passed，且 340/340 shard 曾完成加载。最新 profile run 已越过 shared-expert 维度错误，但又触发 AMD Triton attention residual 的 UB overflow；已实现并测试 NPU 组合路径，r9 重启验证尚未完成，不能宣称单机全功能通过或进入四机 GPQA。
  title: 16 专家单机启动中的兼容性修复
- block_id: 28d629af37432921a74f9e50b5969de715049df602bbd191aaac1de326d523df
  indexed: true
  sha256: 5e44a35f59f72a730af32864f119e14964b0df2d9bca82b5d7deae5fc28d4e65
  source_range:
    end: 16781401
    part: 3
    start: 0
  summary: 在 153 单机 EP16、FlashComm、131K context、Prefix Cache 和 FULL decode ACLGraph 配置下，修复了 MLA gate 的 16/256 token 分片错配及 KDA beta 未 sigmoid 导致的 NaN；r13 完成 183 秒图捕获并验证无 NaN、无字面量 None，16 并发 replay 不崩。此前 r10 的连续 `!` 已定位为 NaN logits，第 10 层 KDA 是首个非 finite 点；beta 修复后真实 93 层 prefill 恢复正常文本，临时诊断移除且单测为 134 passed。当前 16 专家裁剪权重的文本质量、reasoning/tool/prefix 语义验收仍不达标，不能据此宣称全通过；下一步仅补 ignore_eos Prefix Cache 冷热一致性及接近 131K 的 prefill+decode，完整权重和四机阶段再评估语义能力。
  title: r13 图捕获通过但语义验收受限
- block_id: 34b70137c3f079d048990a073ce171c7560a1dc85d0a0def90aaadff15a03257
  indexed: true
  sha256: 2a3b3df0752257c6062b211a805c1d2cfb3ef2e819dda100d43b3f4b9347c710
  source_range:
    end: 33572401
    part: 0
    start: 16781401
  summary: 153 单机 16 专家服务完成 131K 精确边界、Prefix Cache、并发和多模态结构验证；8K prefill 配置避免了此前 MLA `g_proj` 额外申请 578 MiB 导致的 NPU OOM，但裁剪权重的 reasoning/tool 语义未被证明正确。DSpark 期间修复了 shared-expert 3584/7168 宽度、K3 多模态 token index、target/draft KV 分组、proposer metadata、embedding 替换和 FlashComm 分片等 vLLM 0.27 迁移问题，单机最终 7/7 场景与 16 并发通过。同步启动 153/154 P/D 后，基础接口虽通过，但 Prefix Cache 重复前缀输出不一致、真实图片请求首轮 500，尚不能视为 P/D 通过；155、156 未访问。
  title: 153 单机与 DSpark 兼容修复及 P/D 首轮失败
- block_id: 500bf61a1e4c0eba970e24014cdaa1d86f1125e824e1585b4d386ea15b907ba3
  indexed: true
  sha256: 76aa59ff8be4d2dfef606af1c61ee223bea1205a9b00b6019e0e8a3577d4afc0
  source_range:
    end: 33572401
    part: 1
    start: 16781401
  summary: 冷启动与热缓存对照确认，早期输出差异来自 P/D + Prefix Cache 状态一致性回归；按 vLLM 0.27 接口修复后，冷/热缓存四次输出一致，多模态 data URI 请求为 200。32 并发、基础功能、Prefix Cache 和图片路径通过，但 64 并发在第 49 个请求附近触发 TP rank 的 AIVector `invalid GM address / cross-device timeout`，最终 EngineCore 或 P 侧退出；多次尝试的 pointer metadata、Mamba event 顺序、decode-graph 分类和旧 zeroer 移植均未证明能修复，旧 zeroer 反而使 r6 提前出现空响应和 500。r7 启动还经历过 CANN `acl` 环境缺失等实验性启动错误；单机同配置 cold/warm 各 64 全通过，问题进一步限定为 P/D 传输生命周期或缓存交接路径，155 仍未使用。
  title: P/D 缓存一致性修复与 c64 EngineCore 崩溃
- block_id: daeff4d49ba553767b9bfaae6063f4d238e8046621b1d102f97a811cca474c75
  indexed: true
  sha256: 9187fd2955a96faa63aca42e87257d9acccc1dd81615064c955750e6d319cefc
  source_range:
    end: 33572401
    part: 2
    start: 16781401
  summary: r7c/r8/r9 在两机 P/D 上均重复出现冷 c64 仅 49/64 成功、随后 P 退出，CANN dump 将首个故障定位到 `KvRmsNormRopeCache`；关闭 target graph 后冷 64 全通过但热 64 仅 17 个成功，表明 graph replay 会提前暴露问题，根因仍涉及跨波次 KV/slot 或传输生命周期。0.27 物理 KV group 拓扑核查未证明普通单机 block 复用越界，单机 cold/warm 各 64 共 128/128 成功，因此排查转向 Mooncake/HMA 的状态所有权、对齐基址、远端读取与释放时序。补齐三项 0.27 兼容后，8×16 多轮请求达到 128/128，但直接 c64 仍前 33 个成功、后 31 个 500，说明累计传输或高排队交接问题仍未解决；随后尝试的生产端完成事件同步尚待最终压力验证。
  title: KV cache kernel 定位与 P/D 传输边界实验
- block_id: 1cd8acff7e3a0535fc76652dabb533e0b92346c00f88a16a1251791772e2b987
  indexed: true
  sha256: 6a4b645122f37b3a5221e9dc939d24032249cae085fad7b2557798ff7946dbe9
  source_range:
    end: 33572401
    part: 3
    start: 16781401
  summary: 生产端状态可见性修复后，两轮 c64、每请求 1 token 达到 128/128，P/D 全程健康且响应结构检查通过；基础 Chat、SSE、Completion 和 stop 也通过，但裁剪权重的文本语义退化仍不能作为精度结论。更长解码暴露独立问题：32 并发、16 token 通过，64 并发、16 token 时 D 侧退出；单机相同负载两轮 128/128 成功，故障继续指向 P/D 生命周期中的异步 Mamba 状态更新、混合缓存块回收或远端状态接收竞争。针对 D 块回收边界新增同步后，P/D 首轮仅 33/64、P 退出，未形成修复证据；全量功能矩阵、图模式复测、四机 GPQA 和 155 访问仍未完成。
  title: P/D 1-token 修复通过但 16-token 压力再次失败
- block_id: 092fbd7ee1a931cd8febd0b350d97885c2fc38b6721d403d820003999a5b449d
  indexed: true
  sha256: 94f533b8e685b63cf7013330b450365e944881d4b0549e133d66a14ca486dba0
  source_range:
    end: 55429504
    part: 0
    start: 33572401
  summary: '定位确认错误注册基址是已有回归保护的隐患，但不是当前崩溃的充分原因；纯目标模型 P/D 在相同条件下 128/128 成功，草稿缓存跳过、全设备栅栏和关闭 Prefix Cache 均未改变 DSpark 失败。混部 `max-num-seqs=64` 首批 0/64 退出，而 16 槽两轮 128/128 稳定；34 并发通过、35 并发失败，故障与 Mamba/KDA 高 block-id 跨过 4096 高度吻合，#13836 的 recurrent KDA 原地更新成为重点排查对象。已提交脱敏的 Codex issue #39119，并向 OpenAI Support 发送反馈；后续定位应保留服务器容器内 `127.0.0.1` 压测、网络安全误报和未验证的算子根因。'
  title: 混合缓存高位块号边界与反馈提交
- block_id: 2a956f4a20c577561d2b7c3af567990cc14c40641730fd89b0d4528970fd5379
  indexed: true
  sha256: b0f3b385cf59c49abe9e15a5889a7f5cb23b59d42b59658c65cf75c1eb0396d8
  source_range:
    end: 55429504
    part: 1
    start: 33572401
  summary: '`K3_STATE_DEBUG` 显示 recurrent state 为连续布局 `(14125, 6, 128, 128)`，35 请求批次最大索引 4026 可执行，随后请求使用 4137–4144 并触发设备异常；这支持 4096 附近的边界，但尚未证明首个报错来自 causal-conv 还是 #13836 的 recurrent KDA。OpenAI Support 工单 #13359600 已收到包含多次 `cyber_policy` 触发信息的回复，原始日志 ZIP 已压缩并校验，但 Gmail/Chrome 附件上传持续超时，尚未确认日志已成功附到原线程。此前邮件中提及的 issue、工单和会话线索均属来源记录，不应视为安全问题或算子修复已验证。'
  title: KDA 高索引证据与工单附件未完成
- block_id: a94ef48f79aee4abef9c04088dbd23431730e85db36c7375e3ac9b231daee835
  indexed: true
  sha256: 3805c14eda1167635bbf550371f8ed2cc4a3f95d4a378695572ab578ad655d16
  source_range:
    end: 72207360
    part: 0
    start: 55429504
  summary: 故障证据最终指向 DSpark 的物理页大小 384 与 attention 使用的逻辑块大小 128 不一致，导致高 block ID 下 KvRmsNormRopeCache 写入越界；修复已恢复按 cache group 传递逻辑块大小。相关定向单测 28/28、547 项集合 547 passed/1 skipped，且单机高块位、Prefix Cache、默认请求事件同步、canonical AttnRes 及多模态矩阵均通过，但这些结果不等于完整 P/D 或最终精度闭环。r35 曾在约 20 分钟后收到有序 SIGTERM，原因未确认；测试中还发现并修正了多个 fixture/API 适配问题。
  title: DSpark 逻辑块大小错配修复及单机验证
- block_id: 51eb48f45c8f7869d4f7652f78bb553ffb7318fd12ec0da5ed661c64bf036296
  indexed: true
  sha256: de64f1fb3a724254e6f8ef8d9c7eb4c2c39cd3cef5c82f5686bc286a7f81b7e5
  source_range:
    end: 72207360
    part: 1
    start: 55429504
  summary: r36 的无命中长请求和长短混部均通过，r37 的 Prefix Cache 命中矩阵 c4/c16/c64 也通过，未出现 None、空输出、全感叹号或 507035；默认不返回 token IDs 的事件竞态 UT 为 4/4，真实 c16 与两波 c35 也通过。canonical K3 AttnRes 定向测试 14/14，r39 的无命中、命中和混部单机回归全部通过，acceptance 与旧路径接近，但完整定向集合此前暴露的失败主要是测试夹具问题。后续仍需在 153/154 的隔离 P/D 环境完成验证，不能把单机通过推广为端到端成功。
  title: Prefix Cache 与 canonical K3 单机回归
- block_id: 172214bd9e3bcca25bf385c19b7456764c894046fe72e48e779ddb80b74127bd
  indexed: true
  sha256: 6d3b59067be69b38d0cc064d90f921d3dd57a7c5ce98694ee62fc3f1c4254d5c
  source_range:
    end: 72207360
    part: 2
    start: 55429504
  summary: 两机 P/D 的冷态对照显示 P 直出、D 直出和 P→D 修复前存在 token 0 分叉，而无 Prefix Cache 时三路 128/128 一致；根因被收窄为 align 模式 load-only step 将 mamba_state_idx 持久化为 -1，导致下一次 forward 漏拷跨块状态。修复后的冷态三路 token 完全一致，共享前缀 16 并发也通过；但低 KV 容量边界的 6 并发中仍有 2 条出现连续“!”。直连 P/D 和单条 P→D 均正常，当前未决假设是并发状态下 384-token 物理父页承载多个 128-token 子槽时引用计数或父页生命周期错误，尚未验证为根因。
  title: P/D Mamba 状态对齐修复与并发污染线索
- block_id: e7c734a713284d8b8d24a2ba1e161d2f018c97a4d94c3e48a569c25a41af3b87
  indexed: true
  sha256: be1148a2bd3eb13012c49800499b49d6c105e9fc6bf0740af874329936bd9100
  source_range:
    end: 72207360
    part: 3
    start: 55429504
  summary: '直连 P 的 6 路长上下文并发也复现连续 `!`，控制组显示问题集中在接近 10000 上限、经过 9984 边界且请求退出后 batch 收缩的尾部；`-1 → token 0` 探针未发现占位符泄漏。曾定位到 accepted-count 异步 D2H 与 batch `condense/swap` 共享行的竞态，并尝试稳定缓冲区、提前 event 同步等修复；部分实验达到 36/36 正确，但 acceptance 从约 0.55–0.59 降至 0.11–0.33，不能视为成功。与 #13277 对照后确认其最终 event 同步已在当前代码中，额外同步修复在第二轮仍复现，因此已撤销；非连续 stride `recurrent_kda` 96 步对照也未复现，当前重点转向 0.27 的动态 draft 宽度、持久 batch 行及跨 wave 状态复用。'
  title: 并发尾部 token 0 退化与同步修复排除
- block_id: 9e670e51da78cfe49d4faf4f22c7714d5a8eb1986f5698037aad055052ae0e4c
  indexed: true
  sha256: ae4fa91733df47db1a270e67d8e85d3296c481987bc2df367008218ab5fbde5a
  source_range:
    end: 72207360
    part: 4
    start: 55429504
  summary: '已确认 #13277 的 `num_accepted_tokens` 精确 event 同步已移植，额外提前同步不能解决问题；因此不能把当前故障归因于同步修复漏移植。新的已知正确基线是 `releases/v0.26.0rc`，三方对照发现 main 在其之后引入了 DSpark 输入展开的多程序/向量化 kernel，且 0.27 还可能删除了“拒绝 token 数 → MLA `actual_seq_kvlen`”修正链路，同时默认开启 async scheduling。旧 kernel 回退实验尚在进行，MLA 使用未修正 CPU `seq_lens` 的缺口只是待验证嫌疑，不是已证实根因。'
  title: 0.26 正确基线与 0.27 差异对照
- block_id: c4e60c268165bc375dbe2dc5915af32b12afa085fb150aaa61eec2e02aa22c68
  indexed: true
  sha256: 10abf243485ae63c6468bdfa09b0fde9969cb8682439df05771778d8e8bcc12f
  source_range:
    end: 89035402
    part: 0
    start: 72207360
  summary: 旧 kernel、reject→MLA KV 长度修正、postprocess kernel、额外同步及多种长度探针均未解释 token 0；严格 raw token ID 检查显示，问题只稳定出现在 DSpark + async、K>1、长 prompt 分块并与并发请求混部的场景。target-only、K=1、短 prompt、单请求及 `--no-async-scheduling` 多轮通过，但 P/D connector 冷启动会显著放大失败，表现为后完成请求出现连续 token 0 尾缀。后续又发现 `async_tokens_to_discard` 仅覆盖 reset/preemption，不能解释正常 `max_tokens` 结束，因此该线索被修正为非直接根因。
  title: 异步多 draft 长 prefill 的精度退化范围收敛
- block_id: bb4b52895701c37d2c39dd6b1b09069f6ae1f5a38f6e46c80f05003d7cdb2e87
  indexed: true
  sha256: 1a59394b90617fe9e3281f55edf75e42a927dd6d4cc439692ceb6958fc975dbb
  source_range:
    end: 89035402
    part: 1
    start: 72207360
  summary: 单机冷启动高并发稳定通过，而启用 P/D connector 后出现递增的 token 0 尾部；对齐 storage view、`register_buffer`、每步 tracker、Mooncake 后台线程及若干生命周期假设均未能消除故障。针对 target/draft 物理 KV 分域的 r48/r53 候选虽改变了 capacity（约 479,861 降至 244,548 tokens），但仍为约 2/6 通过，因此不是充分修复。r49 进一步确认原始 `AsyncGPUModelRunnerOutput` 已包含全 0，且 `invalid_req_indices=[]`，但首批尚未 shrink 就已出现异常，排除了 API 解析和简单请求丢弃。
  title: P/D connector 触发条件与 cache-layout 候选被逐步排除
- block_id: 0d9d4104414809116892753c4d0e24ef87076e79aa40a52963bdb641ecf992f6
  indexed: true
  sha256: 30f6896e03fdac56114d11ba62d909baa3d086a963cd53cc5b2363cb0a113641
  source_range:
    end: 89035402
    part: 2
    start: 72207360
  summary: r50 在首次 token 0 时观察到 target 与 bonus logits 的全部 163,840 个词表值均为 NaN，说明 rejection sampler、输出解析和 Mooncake 不是首个坏点。将 target/draft 合并逻辑 cache group 因 target=4、draft=6 heads 的 v0.27 约束不可直接移植；保持逻辑分组并隔离物理 storage 的 r53 仍失败。恢复 0.26 的完整 K3 embedding 在 v0.27 产生 shape mismatch，清零 `split_inputs_tp_to_sp` 的非整除 padding 尾部也未改善；c1/c2/c6 的新观察显示请求通常在同伴完成、batch 删除/压缩后开始连续输出 0，但这一映射假设仍待验证。
  title: NaN logits 指向动态 batch 状态或输入路径，padding 修复失败
- block_id: ee0e3a652905a53a29350ae664f1472038126dc45ee75c0de94f161aa0c2755f
  indexed: true
  sha256: 39a294b086ef3843f1eb783bf73679cd7ea203a153aa82da1d6bbdee5875dbba
  source_range:
    end: 89035402
    part: 3
    start: 72207360
  summary: 单请求正常；并发时首个请求完成并触发 batch shrink/condense 后，被从高槽位搬到低槽位的请求开始连续输出 0，且 0 出现前 target logits 已整行 NaN。当前必要条件收敛为 DSpark + async + 多请求删除/condense；target-only、关闭 async、K=1、关闭 connector 等对照未复现，先前大量同步、缓存、embedding、padding 等候选均未验证为根因。代码差异的最强候选是 v0.27 清空 block-table 源行后，Ascend FULL graph 仍消费全部 padded metadata，且 padding state ID 被改为物理块 0，导致伪造 KDA/Mamba 状态更新；这是待验证的根因候选，尚非已确认修复。
  title: 问题收敛至 batch condense 后的 K3 padded 状态污染
- block_id: b491d87763a96bd18496202919536a794663384de09ed50c29f485baf269feeb
  indexed: true
  sha256: 603f4d0e92b6ed4df53ae5ef698238fad8c41b6b2e0c46474cd2e971fd1ccdb3
  source_range:
    end: 105822671
    part: 0
    start: 89035402
  summary: 排查将主要根因收敛到 v0.27 中 DSpark 把 384-token 物理页按 128-token 逻辑块错误解释，导致 cache index 放大、越界及历史 KV 污染；同时发现 arch35 的 42+ task 空 Vnew 分支可能少归还 workspace 事件。隔离回归和单机矩阵曾报告 135 项、93/93 通过，但 P/D 和真实 Prefix Cache 命中仍待最终验证；期间确认部分旧怀疑（zeroer、KDA gate/g2 丢失）并不成立。另发现 dynamic K、FlashComm 多模态 positions、空序列 chunk_indices、非流式 JSON 分片等未完全闭环的边界问题。
  title: K3 384/128 映射与候选验证
- block_id: 61e2f4e5386ab34e2c0671829043b5d0e74153e4e550edf25ebef872bf17d3bb
  indexed: true
  sha256: 316439aa0cafe1612d52e75dd9c923ecc4fdcc33f57e24af06a756070dc22170
  source_range:
    end: 105822671
    part: 1
    start: 89035402
  summary: 正向对照确认 BlockTable.move_row() 在 batch condense 后必须清空源行，当前 Ascend 覆盖实现遗漏该动作，可能使 GDN/Mamba 图 padding 继续引用已释放并复用的 state block。另确认两个 P/D proxy 仍可能吞掉 decoder 或流式响应异常，造成空尾、截断 200、重复内容；拟改为首个 chunk 前允许有界重试，之后异常直接传播，但结果尚未验证。物理页到逻辑块的 metadata/reshape 链路已找到对应实现，zeroer 未获版本差异证据。
  title: 状态行生命周期与 proxy 异常出口
- block_id: 06d35f8e78f6e1242bfd6b6b669fa150356fbd3040a2f8b4516d7ef32aaaf0cf
  indexed: true
  sha256: f1a1413314281525bc703d1294e65126297094843999b666a5e211543cbb5b81
  source_range:
    end: 105822671
    part: 2
    start: 89035402
  summary: 当前新增差异仍集中在两类：混部或 condense 后源行未清可能污染 GDN 状态，以及 proxy 两层吞异常可能产生空尾或重复响应。后续将继续核对 P/D producer/consumer 的 Prefix Cache 判定、状态块归一化和双机路径，确认单机修复不会引入新的分叉。此阶段仍是计划中的核对，尚无新的成功验证结论。
  title: P/D Prefix Cache 与候选冻结前核对
- block_id: 618e59a1471d5e24d7d5b9793c93a8c5907d785cd2455591f827e96f36ba7188
  indexed: true
  sha256: 992f2ea36ee9e25d0e72bc63a775accb89abec1fc22821bcdf20c73b8b24316d
  source_range:
    end: 122618241
    part: 0
    start: 105822671
  summary: 正向审计将当前差异收敛为 K3 全宽 `g_proj` 的 SP 误判、K=7 padding 行 accepted-token 契约、42-task 空子块事件，以及全 0 draft 的 spec 降级等边界；其中部分属于对齐缺口而非已确认根因。删除了不必要的 `_clear_speculative_conv_tail` cache 写入，恢复 `accepted=1` 等基线语义，并统一 proxy 的 `None` 归一化。r57 单机 Prefix Cache、K7 mixed 和 c64 定向结果通过，42-task 直测返回 0，新增隔离回归共 13 passed；这些结果主要证明无挂、非空和边界行为，尚不构成完整精度结论。
  title: 0.27 K3 状态边界与单机定向验证
- block_id: 8edd09ab9c0714a7dbf30e93567f2ee118ab5bd4926ad8ae7d2fafc38842ee17
  indexed: true
  sha256: 64f86b2981f545f105eb54d426fe1730fe010c5eb6851c01973fbc6e1177af4e
  source_range:
    end: 122618241
    part: 1
    start: 105822671
  summary: 合成 filler 输入导致的重复短语被排除为精度证据，随后使用同一 16 专家 checkpoint、8192/1024 token 和历史脚本校验；c1 与 0.26 同权重基线一致，唯一一次 c16 为 16/16 完成且无 `None`、空输出、乱码或崩溃。混部审计发现 `kv_both`/connector 需要按请求的 `do_remote_prefill` 分流，并将滞后的 `BalanceScheduler` 副本改为直接调用 vLLM 0.27.1 原生调度器；这些修复已静态检查或 UT 验证，但两机 P/D 尚未完成。r58 仅开启 main `FULL_DECODE_ONLY` graph 后 6/16 命中 `repeated_exclamation`，因此问题暂时收敛到 main ACL graph replay，而非 async、网络或混部；此前把 0.26 balance 路径当作无问题基线的判断已被纠正。
  title: GSM8K 基线与 main graph 回归定位
- block_id: 23d5ceb3d32cf2b2c6504fedd67f9cc2fe1c63b46b216fcd88646f28ffb56759
  indexed: true
  sha256: b1590d1484e469709a3dd67e0a02e091d25873ee58eb926c852989bebb74cf24
  source_range:
    end: 122618241
    part: 2
    start: 105822671
  summary: '同权重、同 c16、同请求集下，r57 eager 通过而 r58 main graph 稳定出现后半批重复输出，说明质量回归与图回放路径相关；ACL graph 外壳与 #13277、v0.26 哈希一致，尚未证明是同步修复漏移植。源码差异集中到 v0.27 KDA 的原地 state/别名输出协议、dummy capture 的 spec 元数据，以及 causal-conv 的 `conv_state` schema：`output_final_state=false` 时仍声明必选同址 `final_state`，而实际写入的 `output`/`conv_state` 被标为只读。单算子硬件回归因 153/154 卡被占用尚未执行，因此这些是待验证的合同错误候选，不应升级为已确认根因。'
  title: ACL Graph 状态算子契约候选
- block_id: 7862db17544f71901fdc1266517cc8578dbc8df10367768e340c8f6ea5a0fee0
  indexed: true
  sha256: d742563a570a0b7b60d88631884a5a819a09c77943e9e4943c08fdb6a0cf608c
  source_range:
    end: 139395583
    part: 0
    start: 122618241
  summary: 审计将 K3 FULL graph 风险集中到 causal-conv 与 recurrent-KDA 的 mutation/alias 契约、动态 padding、norm-gate 和状态生命周期；发现并修正了若干测试自身问题，包括 conv state 长度、slot 写冲突及 CPU `index_copy_` 的 int32/int64 差异。隔离编译和 CPU 定向测试多次通过，但 NPU 验证尚未完成；prefix-cache tail、dummy no-op 输出、NaN padding 和 producer prefix-hit 仍有未完成或需继续确认的分支。最后提出 dummy 分支应在无 KDA 工作时直接清零并返回，避免 `0 * NaN` 污染。
  title: K3 FULL graph 状态契约与回归收敛
- block_id: 09b0e55b926bd4406849de0a8d34495953c0a3e3a273830dd5f30ed6dc9d4781
  indexed: true
  sha256: 7b589a7fa7a588f1cbdf1538789d2ef5fb630f8e2bb07dafe7004cfa846a270b
  source_range:
    end: 139395583
    part: 1
    start: 122618241
  summary: 正向对照继续以 0.26 高并发基线为参照，重点检查 0.27 将外层 opaque KDA 展开后暴露的 causal-conv、recurrent、norm 和 padding 契约。新增或确认的风险包括 recurrent 空长度行从 `empty` 泄漏未初始化值、c16→c11 replay 的动态 padding 经 gate/norm 后被重新污染，以及 KDA RMSNorm `eps=1e-6`、beta sigmoid 和 native dispatch 等移植差异；其中旧的 speculative tail 清零假设已被参考语义否定并撤回。CPU 验证曾报告 65/65，但随后确认使用了旧隔离副本，因此该结果一度被撤销；后续 23/23 强化回归通过，但对应硬件 ACLGraph 结果仍缺失。
  title: FULL graph padding 与移植差异审计
- block_id: 39f118e8ab73f298a47df207371cd94601412d32471c4471d2aa8456222c9ae6
  indexed: true
  sha256: ffe60e26efaca5e21d160b13b62ae0636a77c15bd482ad01fe942e8c67f2f1c3
  source_range:
    end: 139395583
    part: 2
    start: 122618241
  summary: 最新精确源码隔离回归报告 65/65 通过，强化 KDA adapter 回归为 23/23；但这些仍主要是 CPU/静态证据，不能替代 NPU ACLGraph 验证。153/154 无真正空闲卡，随后在 153 的隔离构建上运行 schema 用例：动态库路径问题已排除，schema 检查通过，却发现当前 CANN 的 `aclnnRecurrentKda` 会以 `EZ1001` 拒绝空 `finalState`，因此“省略未使用 final state”的修法不成立。当前改为让 CANN 使用不对外暴露的 scratch `final_state`，只保留顶层 `initial_state` mutation；新 recurrent 编译已进入 kernel/protobuf 阶段，但尚未得到重跑结果，r58 服务仍报告 200。
  title: NPU schema 反证：finalState 不能为空
- block_id: 4522f22d7ddc626bd4adf29ffaaf8d7a7e86786fa62f098b9fbc7a6ebda24d60
  indexed: true
  sha256: 437203a429033a542d385a81c2b140d3967079aff8fea78601c7c104b32a273f
  source_range:
    end: 148520470
    part: 0
    start: 139395583
  summary: recurrent KDA 与 causal-conv 的 NPU 回归、定向 UT 和显存诊断曾通过，最终采用非空、独立且仅占 1 个元素存储的 expanded scratch；但完整 TP16 服务尚未完成验证。随后停止既有服务，尝试构建并部署同时包含两类算子的统一 runtime，整包构建被无关 GMM 的 CANN 9.1 `limited` 字符串/float 配置错误阻断；新 binding 与旧统一 vendor 的 12/12 算子组合检查通过。跨 153/154 的多种 SCP/SFTP/SSH 传输路径曾失败或卡住，固定 tar 包约 61 MB，后改用临时 HTTP，最终 archive SHA 与 runtime manifest 一致。
  title: 算子级修复与完整服务回归前置准备
- block_id: b105b9332b5ffba608d27a1c2a273ee201fc7039919a2aa506194c55fb2a47a2
  indexed: true
  sha256: dad24bf6cfb39ebb2385aaadccfaf0e5712d0c4fc7dcabf75c403f2b86f9f631
  source_range:
    end: 148520470
    part: 1
    start: 139395583
  summary: 153 上将 FULL 图捕获从 8 张收敛为 `[16,128]` 两张后，服务健康检查为 200，但首请求及 chat、reasoning、raw completions 均稳定输出连续感叹号；因此停止大矩阵和相关服务，未将其视为完整回归成功。当前证据仍缺少同一代码、权重与运行时下的 eager/ACLGraph、target-only/DSpark 对照，且完整 clean vendor rebuild 被无关 GMM 配置阻断。材料随后将诊断边界收窄为 0.26 已验证算子作为 golden、直接比较 0.26/0.27 整机集成，而不是继续扩大算子修改范围。
  title: 双图服务首请求暴露退化并暂停扩展回归
- block_id: f237f083b126c802b5a5fdcb75eb1a16608bc3711edde2cd2ef8b0b9445a93f8
  indexed: true
  sha256: ef6438dad380fa5b3727e56f5f021dca163652681335c82019a5f755f3e84c23
  source_range:
    end: 148520470
    part: 2
    start: 139395583
  summary: 隔离快照固定了同一 40-op vendor、权重配置、async scheduling、两张图 `[16,128]` 和相同 prompt IDs；0.26 正常返回 16 个 token，而 0.27 首次请求因 `nan` 序列化返回 400，去掉 `logprobs` 后仍输出连续 `!`，表明不是单独的 API/logprobs 问题。随后两侧 eager 服务使用相同输入仅生成 1 个 token，均返回相同首 token 空格，初步把差异从算子普通 eager 数值收窄到 0.27 的图执行/图输入复用链路。为比较关键节点，双方隔离副本加入统一 dump；首次门控因 padding 长度未命中，后改为服务启动后标记并对真实 forward dump `NaN/Inf/max/mean/RMS`，但后续小文件同步遇到 SSH 连接阻塞，尚未完成新的部署或节点证据。
  title: 0.26/0.27 同构对照定位到图执行链路
- block_id: 976142f4d54fb5c7cb9d79e53679ebf5edfc77728940971d0eb16e3e561986c0
  indexed: true
  sha256: 551e9d1e07bf361f0796458d54fa51a2c0e897ea29eec151e26cf713c80a0e48
  source_range:
    end: 148520470
    part: 3
    start: 139395583
  summary: 曾提出 0.27 图模式丢弃 causal-conv 返回值可能导致未初始化张量，并完成单变量部署；但硬件结果显示该修复后 1 token 正常，2/16 token 仍因 NaN logprob 返回 400。compile-only 模式下 1/2/16 token 均正常，因此问题进一步指向 ACLGraph decode/replay，而非 torch.compile 或算子数学实现。尝试增加 opaque/mutation outer custom op 后仍失败，当前怀疑需要真正的 attention graph boundary；该结论仍是实验性定位，尚未完成最终修复验收。
  title: 0.27 图回放中 causal-conv 修复无效
- block_id: 0dab04044a8f2ac9ad925cfc3f9fc6a2f6c3366242df06df73212e3562a4b1e4
  indexed: true
  sha256: ad023e19b43868b07ce8c2af8124cfee7365a1ae1ecd6f4a54e6fd20ff654361
  source_range:
    end: 148520470
    part: 4
    start: 139395583
  summary: 将 KDA op 加入 `splitting_ops` 未改变 1 token 正常、2/16 token 从第二步 NaN 的现象，且 FULL_DECODE_ONLY 会重置该列表，说明模型初始化时追加配置可能过晚。回退 GDN builder 的 168 行未提交改动后结果仍失败，排除了这组改动作为当前精度根因；现有差异重点转向 graph replay 输入同步、`num_accepted_tokens`、DSpark metadata 以及 KDA norm 的别名/`copy_` 回写。最新实验已启动，尝试将 norm 移回 0.26 的 op 外位置，但尚未报告 capture 或回归结果。
  title: split 边界无效，转查 metadata 与 norm 回写
- block_id: a16103d91783737c588f739483987b9837c7d208b12976744d6b2861a81d5e85
  indexed: true
  sha256: c3f50cab6522152defa3f50fd4b39b0db006b79d69c76f1dbc8abd20acd28830
  source_range:
    end: 160825531
    part: 0
    start: 148520470
  summary: 对照验证显示 target-only 普通 KDA decode 的 1/2/16-token 请求均为 HTTP 200 且 finite，故障限定在 DSpark speculative decode 的 ACLGraph replay 路径；一次调试 dump 曾因不存在的 `self.gdn_query_start_loc` 触发 500，并造成“全 0 输入”的假象。随后确认 0.27 在 FULL 图中将 padding 请求误判为 non-spec prefill，且 `KimiLinearConfig.is_linear_attn` 计算属性未被识别，导致 GDN metadata 索引错位并从前两层产生 NaN；按 0.26 语义移植检测逻辑后，2-token 及固定 1/2/16 回归恢复，均无 NaN/Inf。高并发 Prefix Cache、长上下文和混部回归后来通过，但回归脚本曾因 `serve.pid` 中字符串 `baseline=...` 强制转整数而未实际发请求，修正后才继续。
  title: 定位并修复 0.27 FULL 图第二 token NaN
- block_id: 5e88d08693386e8d782f0d87ecd336cf98db7849bb496b85ad93c78c2e07751c
  indexed: true
  sha256: 4698a2e28e4dc144de575dce81c73a06073c2b3c9180b975705e406d260c2263
  source_range:
    end: 160825531
    part: 1
    start: 148520470
  summary: 修复版单机完成 c16、两轮 c35、8K no-hit/prefix-hit 及长短混部回归，分别报告 16/16、70/70、32/32 等结果，health=200 且 fatal=0；针对性单测初次受精简 overlay 缺少 `tests/` 骨架影响，补齐后 GDN 4/4、Kimi 19/19 通过。向 153 复制源码时流式 tar 管道近乎停滞、scp 又卡在 SFTP 握手，均被终止并改用容器间传输；P/D 首次启动还发现安装 metadata 显示 `v0.26.0`，于是停止精确 PGID、显式设置 `VLLM_VERSION=0.27.1` 后重启。P/D 冷热输出与单机参考一致，Mooncake 覆盖和 DSpark 接受率检查通过，固定 1/2/16 也返回 200 且 logprobs finite；随后开始清理未编译的算子、padding、dummy、causal-conv 等实验性改动，但部分 model-runner 全量 UT 因精简包结构和错误 mock 不具备完整验证条件。
  title: 单机与 P/D 回归通过，部署准备经历传输和版本问题
- block_id: 109e4454cda03964178864bc3a099022f08c83abed4380c75c956722a7ed92ba
  indexed: true
  sha256: 66c814087170f912fdaeaa1e6613f9bb342e35856d96c1f9f6196e51a2382cc9
  source_range:
    end: 160825531
    part: 2
    start: 148520470
  summary: 审计中纠正了 deferred-reject 判断：由于 K3 DSpark 覆盖基类 `set_inputs_first_pass`，该同步链是 0.26 已验证的正式迁移，恢复后定向测试通过；`balance_schedule` 则收缩为 producer Mamba prefix-hit 的薄适配，proxy 仅保留 0.26 已合入的多模态 content-part 处理，并保留 `<redacted:employee-id:fdcf2fc0fdc5>` 占位。最终源码在两端重启后完成固定 1/2/16、Prefix Cache、流式、completions、多模态、精确 8192→1024 和 32 路长短混部回归；一次 10,545-token 请求返回 HTTP 400，被确认是脚本超出 `max_model_len=10000`，不能算模型失败，改用精确 8192-token 构造后通过。CPU 定向回归为 526 passed、2 skipped，最新 main 审计树为 527 passed、2 skipped；但 `mla_prolog_v3` 与最新 main/独立 operator PR 的归属仍在审计，4 机 GPQA 因 155 未释放尚未完成。
  title: 纠正补丁归属并完成最终源码 P/D 验证
- block_id: c3fc265f0595ddb795a46e223fac21b6b15d6d6432ba50990739fd68df79068c
  indexed: true
  sha256: 82bddfecd5bf36641e9547abf1440101a7d142ea14a4838b1321dbfd508550b4
  source_range:
    end: 160825531
    part: 3
    start: 148520470
  summary: 模型树已清理掉 operator 源码差异，0.27 单机及 P/D 的功能面、c64、8192→1024 长上下文和 16 长+16 短混合请求均报告通过；但单机曾出现一次低概率空输出，不能视为已关闭。随后按同构配置完成 0.26/0.27 A/B：聊天请求两边均为 112/112，但 tokenizer/rendering 导致 prompt token 数不同；固定 `prompt_token_ids` 的 completions 对照中，0.27 为 112/112，0.26 为 111/112，唯一空文本仍带 16 个有限 logprobs，因此未证明 0.27 更易产生空输出，也未完成 155 上的四机 GPQA 或最终提交推送。
  title: 0.26/0.27 固定 token A/B 对照完成
- block_id: 92cbd78bdf44952d53a628b659a30d386190e133ca4ddf9fdbcb4ec03267851d
  indexed: true
  sha256: 0c21e92b7ed5a36041f59e934fa94b518cfcfcd99392cb52190cb709beb18c22
  source_range:
    end: 177622741
    part: 0
    start: 160825531
  summary: 已按特性维度处理六个非算子子 PR 的检视意见：Hybrid 补充 Qwen3/Qwen3.5 兼容测试，Attention 清理 DSpark 旧链路、收紧接口并修复 g_proj 重复 all-gather，SiTU/MoE 对齐 vLLM 0.27 原生契约，Model 清理不必要的 getattr，DSpark 删除额外策略限制，P/D 完成基线检查。过程中发现并修正了 CRLF、测试夹具缺少 VllmConfig、共享专家入口变化等问题，但部分早期修改后来被证明过度，需以之后的回退和补齐记录为准。
  title: 六个子 PR 的逐条检视修复进展
- block_id: 85e3ad58073edf930e801383e5ec02d83635524e337143c5ca73d1d1fa156dec
  indexed: true
  sha256: aa23afc4efb768a5cc6fcb3e79db5468946b013570c6560a15c680a56dae00d3
  source_range:
    end: 177622741
    part: 1
    start: 160825531
  summary: 六个子 PR 与四个算子提交已重建为十个 signed commit 的大 PR，整合静态检查通过；服务器定向 UT 曾出现 CustomOp 配置夹具问题、算子 tiny-shape 基线失败，以及部署目录缺少已编译算子包等非同类问题。修复配置上下文并设置正确算子路径后，服务完成 TP16/EP、两张 FULL decode 图捕获并 health=200；随后发现 review 中去掉兼容访问后暴露 `initial_state_mode` 缺失，定位为拆 PR 漏掉的 GDN 固定图状态和 DSpark host KV/reject 同步配套修复。Attention 侧定向测试 33/33 通过，DSpark 测试在修正 `additional_config` 夹具后 79/79 通过，但这些结果不等于最终服务验收已完成。
  title: 大 PR 整合、服务器启动与运行时错误定位
- block_id: e08183e764739067b8996333f71c21677af0e44ee0786c5bc4cbb5634effadad
  indexed: true
  sha256: ad7dc8e71d2ed1cb94e306a513bfb2e81c08c37c2e95261d62b9840fc2b55932
  source_range:
    end: 177622741
    part: 2
    start: 160825531
  summary: DSpark 后续按检视收窄为仅保留异步拒绝同步和主机侧序列长度链路，去掉初始化期配置拦截；Attention 126 项全过，整合 UT 最终为 596 passed、2 skipped，已知算子 tiny-shape 基线测试被排除。单机功能矩阵虽无 `None`、空响应或卡死，但裁剪权重导致的语义质量差被确认不应作为精度验收依据；更关键的是当前分支冷启动首个并发 C16 仍出现一个空输出，且同批请求轨迹分叉，单请求却稳定。此前一次冷启动缺少 `--async-scheduling`，因此该实验不能归因到代码；最新动作是补回 async 后重跑，并并行检查 Model/P/D 的 pre-commit 失败。
  title: 整合回归通过后的单机 C16 空输出复现
- block_id: 5f40463790da40ccdd2509062e109aa7583383c23d59af0d449d3e8de1f67154
  indexed: true
  sha256: 45732108be2f79cb171aafdbe42e4ce22b2914b3e3474a5accaa10b0ff834b8b
  source_range:
    end: 177622741
    part: 3
    start: 160825531
  summary: 核实后，#14597 中 Gemini 关于压缩 MLA cache `page_size_bytes` 的判断不适用于当前 vLLM 0.27 虚属性链，已有单测覆盖，相关评论已解释并 resolve。#14426 已 rebase 最新 main，删除零引用的 `kimi_k3_situ_quant_cases.json`，移除自定义 MLA Prolog V3 源码、绑定和测试；KDA 形状意见被判定为将内部 identity-copy barrier 误解为公开布局转换。#14598 恢复使用默认 `torch_npu.npu_mla_prolog_v3`，#14454 重建为 7 个特性 commit；#14600 中 REST 不可见的 pending review comment 已通过 GraphQL 删除，遍历相关 PR 后确认作者为 maoxx241 的残留为 0。
  title: 子 PR 重建、冲突处理与评论清理
- block_id: 9c9026407c6a7c5eecf9376388bec512fb6cbf4614127720258e3c032f5e42d6
  indexed: true
  sha256: 1cbcd418503806dc5bba55afdb3dcd4a6a489408035a589f64e8fa554614eb6b
  source_range:
    end: 177622741
    part: 4
    start: 160825531
  summary: '历史问题被重新界定为完整 cache block 后剩 1 个 token 时的 prefill/decode 错分，而不是 attention-residual 的 `N=129` 精度问题；当前 #14598 已包含 `is_prefilling && query_len == 1 && seq_len > 1` 的 GDN metadata 修复。新增回归明确覆盖 `block_size=16` 下 `seq_len=1` 保持 prefill、`seq_len=17` 使用 recurrent-state decode metadata，隔离 CPU metadata 测试为 2 passed；但运行中的 P/D 服务使用更早源码，不能作为该最新场景的实机证明。后续父 PR 需要加入 K3 nightly 看护用例和部署文档，并区分裁剪权重可验证的执行/部署能力、dummy 权重可验证的启动与图捕获，以及必须由完整真实权重承担的 GPQA 语义精度门禁；具体 nightly 实现尚在设计/落代码阶段。'
  title: block size+1 分类修复与 K3 nightly 方案
- block_id: 5a59017940f4d2b469da0132473619c7aea024327485c50d6eafa5feb1a4edef
  indexed: true
  sha256: df36dd9ced212777590a2f7f29b478f419f56f63c95de587ac55ab9283ec5eed
  source_range:
    end: 194400529
    part: 0
    start: 177622741
  summary: 在 153 上针对 vLLM `58d3918e`、大 PR `1ab5a53c3` 和 QuaRot 16 专家权重复现了 `ModelSlim` 混合精度报错，定位为 K3 七路投影合并后 q/k/v 为 W8A8、g/f_a/b 为 FLOAT，不能沿用统一 fused shard 校验。实现逐步收敛为官方 merged-linear shard loader：量化 q/k/v 一组 GEMM、BF16 g/f_a/b 一组 GEMM，并修复了模块级 loader、普通参数误路由、DSpark `context_kv_proj` 映射及 latent-MoE runner 分发；153 实例最终完成 333 个主模型分片、DSpark、两张 `[16,32]` 图和服务请求回归。真实 `K=7168,N=912` microbenchmark 中，ND 在 M=128 等较大形状更快，NZ 仅在 M=16/32 有约 1–2% 小优势，因此保留库默认 BF16 ND；Grok 代理未实际执行实验，不能计入证据。
  title: 153 旋转量化混精度加载与 BF16 ND/NZ 实验
- block_id: f36278e3d0188e8fab850331408d3c40083b1c39b60c19af614f8d0ba1e5281f
  indexed: true
  sha256: af737eb3d6af0c9d0e3539cc13ff7fcb3215b53be90722ac238120e75d24a3ce
  source_range:
    end: 194400529
    part: 1
    start: 177622741
  summary: 直接在 153 执行了原计划由 Grok 完成的单卡 ND/NZ 对照，确认首轮 `nz_format=2` 的结果无效，随后获得真实 ND/FRACTAL_NZ 数据并支持 BF16 保持 ND。r9–r11 期间先后修复普通参数误路由、DSpark 权重名兼容和 `moe_comm_method=None`；最终服务通过 profile、KV cache 分配、两张图编译、health 200、1-token/block-size+1、并发和有限性检查，prefix cache 在 769/1537 重复前缀分别命中 384/1152 token，但强制工具场景未生成 tool call，属于能力限制而非崩溃。父 PR 及子 PR 已按职责拆分并更新，部分 `ci-gate` 仍因缺少 `ready-precise`/`ready-all` 标签而受权限阻塞；随后按七个子 PR 合并净差异验证，父 PR 目标仅保留文档。
  title: 153 端到端服务验证与 PR 门禁问题
- block_id: 871a4310f27ca1bb501459a3720e21ef0c10f98ccaf27ed784f553c42fb15cf8
  indexed: true
  sha256: 4c5d72d69284217b0711d86f153c5680bab82c7f6f4436b271a7405bcdf78a09
  source_range:
    end: 194400529
    part: 2
    start: 177622741
  summary: '#14426 已精确删除 csrc 下 3 个算子 README 和 `recurrent_kda/tests/pta` 的 3 个文件，同时保留正式 API 文档及仓库级 nightly 用例；子 PR 与父 PR rebase 后本地格式门禁、远端 pre-commit/mypy 和文档检查通过，父 PR 仍只差 K3 文档及索引。核对后确认真实 K3 LoRA MoE 未验证，现有改动只有 mock/随机 tensor UT，且真实服务未启用 `--enable-lora`，因此应从 #14599、父 PR 及说明中移除，当前尚未执行远端修改。与之不同，`_KIMI_VENDOR_LATENT_RUNNERS` 被判断为需要保留但应缩窄：K3 显式传入上游 `ROCmLatentMoERunner` 会绕过 Ascend 的 `AscendMoERunner` 注册；该判断来自真实 K3 调用链和此前运行路径，最终实现范围仍需继续确认。'
  title: 子 PR 清理、LoRA 结论与 latent runner patch
- block_id: 1fb2ca624a1e9f5dae86b030e6e2ae1bc56b10015333bdc6f0d616c33f4161dd
  indexed: true
  sha256: 380b70f3aa8c59d8edbc267c6e17fd4b1dfff39dc892560daaeef6099b9ec1d4
  source_range:
    end: 194400529
    part: 3
    start: 177622741
  summary: '确认普通模型的 `runner_cls=None` 会进入 Ascend runner 分发，但 vLLM 0.27 的 K3 显式传入 `ROCmLatentMoERunner`，会绕过旧 patch；因此旧 patch 并非完全失效，`_KIMI_VENDOR_LATENT_RUNNERS` 的最终必要范围仍需继续验证。LoRA MoE 两处改动已从子 PR #14599 和父 PR #14454 回退并推送，focused UT 为 `155 passed, 1 deselected`，被排除的 N=4 对齐测试在独立 main 基线同样失败。153 上真实 93 层 QuaRot 16 专家服务在补设 `GLOO_SOCKET_IFNAME=lo` 后完成权重加载、ACLGraph `[16,32]` 编译及文本、PNG 多模态和 C16 请求；这些结构与完整性检查通过，但裁剪权重输出语义较差，CI 仍有未完成项，且主线程曾发现进程清理状态与 subagent 报告不一致，不能把所有运行环境结论视为完全核实。'
  title: LoRA 回退、K3 runner 绕过与 153 实机验证
- block_id: 05606e995cec01b620ab0f659f7d4bdc1bbcb6215c520b9ee964bd7480b51d7b
  indexed: true
  sha256: 11e6cece0bf345481cda41d78cec182f239eb073eb6db20ed1fdd8a4f9e0782b
  source_range:
    end: 194400529
    part: 4
    start: 177622741
  summary: 审计将“仅因未在 153 验证就拒绝”的 `NotImplementedError` 与真正防止静默错误的 shape、地址、状态和物理契约检查区分开；SiTU EPLB、W4A16/antiquant、KDA gate、混合 RoPE metadata 等定向回归已通过，153 历史触发用例也报告通过。后续发现 QuaRot 自定义 loader、K3 全局 latent-runner 特判、重复 MTP/DSpark forward、手工 MLA merge、私有防御检查等应改为复用仓库或 vLLM 上游接口；用户进一步要求所有特性遵循“先复用，只有硬件差异才新增实现”。P/D 验证受 154 使用 `v0.26.0` 混合环境及 SSH 密钥交换失败阻断，不能将旧服务结果包装成当前 0.27 head 的通过；最新 main 已覆盖部分 DSpark/MRV2/GDN 公共逻辑，后续 diff 必须以最新 `origin/main` 重新审计。
  title: K3 PR 全量审计：移除过度门禁并复用上游实现
- block_id: 0d6cf56d2d3a3c893745f1aff94ee5177ac21b850d6c446e844854175946e7c5
  indexed: true
  sha256: ccdc3bf3b0172cda352ad617d57e0c1a9dba31be641f8f906da55351bdf1b64c
  source_range:
    end: 211180780
    part: 0
    start: 194400529
  summary: K3 PR 栈持续按“优先复用当前仓库和配套 vLLM、仅保留 Ascend 差异”收敛，删除了重复的 MLA、SiTU、注意力残差、加载器及 P/D 逻辑，并将 MoE、DSpark 等实现边界重新审计。153 初次真实启动曾分别遇到 Gloo hostname 解析失败、覆盖 PYTHONPATH 导致 `acl` 丢失、缺少一级算子产物，以及 DSpark 重复注册 MLA；部分问题已通过环境固定或代码调整越过，但服务启动和权重加载尚不等于请求正确。后续出现的 W4A8 `moe_comm_method=None` 和 hybrid KV `scale_dim` 错误推动了 runner 分发与 cache merge 修复，仍需保留未完成验证边界。
  title: K3 PR 栈复用审计与 153 初次验证
- block_id: bbc3cc2b655a0f3c2bebcae523f280077f78f9ec3427c48cc3758a02368f1b7e
  indexed: true
  sha256: ccc74657aa8ec77957fd1818ebf372947bcbae6ffc4205252ddcd9a3e088ee07
  source_range:
    end: 211180780
    part: 1
    start: 194400529
  summary: 针对 AMD `ROCmLatentMoERunner` 继承导致反向 patch 的问题，MoE 改为独立 Ascend 组装层，仅复用通用组件和 `FusedMoEFactory`，由标准 `MoERunner` 插件分发到现有 `AscendMoERunner`；结构 UT 通过，真实加载也越过了原 runner、DSpark 和 hybrid KV 异常。153 服务虽完成权重、两张图和健康启动，但固定请求在 logprobs 路径出现 NaN：127/128/129-token 等用例失败，且后续 32-token 请求可能被污染；关闭 shared-expert overlap、prefix caching、DSpark，或清空 KDA cache 均未解决。155/156 均被 16 卡 DP/EP 服务占用，当前可用对照资源受限。
  title: 独立 Ascend MoE 与 153 NaN 阻断
- block_id: dc28f7c51fb32498c6c6981b60a3db2b024e26af4a059d69b56983ca3b56ee87
  indexed: true
  sha256: 9919529ac3484ec5f350f23b63e0cd7ca3681e037cb247f9e830ae76db79297d
  source_range:
    end: 211180780
    part: 2
    start: 194400529
  summary: 153 已重组为前 8 卡 vLLM 0.26、后 8 卡当前 0.27，双方 TP8、attention block size 768、eager 配置和权重对齐；0.26 在固定请求中始终有限，而 0.27 从 127→128/129 或更早的 31→32 临界点开始出现首 token logprob NaN。补齐 0.26 的 `_cann_ops_custom` OPP 包后两边均健康，排除了共同 AscendC 二进制、权重、机器和 block size 等因素；逐层 `.item()` 同步会暂时使问题消失，提示异步时序或持久状态污染，但 KDA 返回同步和 prefill 前清空 KDA cache 都未奏效。进一步结构 dump 显示 0.27 有 14 个 Mamba cache group/5529 blocks，而 0.26 为 3 个 group/943 blocks，NaN 在物理块复用后出现；这仍是当前假设和定位线索，不是已验证根因或完整 0.27 精度结论。
  title: 同机 TP8 对照与 KDA 状态线索
- block_id: d6771e6cd82c04e69438043d68f0354916cd8fc6616b8d2390aa2b7ead3748a7
  indexed: true
  sha256: 023f50ebb5631441f14f937f57e77fb26e4839cfbf0712733a52a5c579ec8aee
  source_range:
    end: 211180780
    part: 3
    start: 194400529
  summary: '定位并修复了 0.27 预分配 block_residual 的非连续前缀切片被 Triton kernel 按紧凑布局寻址的问题；单卡连续/非连续回归及原始 8 请求序列均通过，随后同机 TP8 P/D 也完成了 16/16 边界请求和 8192→256 长请求验证，未观察到 None、NaN/Inf 或 HTTP 错误。期间首轮 P/D 失败源于旧脚本设置 VLLM_VERSION=0.27.1、容器 vllm metadata 不一致及 154 库布局差异，后经隔离环境、版本文件和路径修正恢复；PR #14600、#14454 的格式与 mypy 问题已修正，硬件证据仍对应其直接父提交，剩余 CI 红灯是缺少 ready-precise/ready-all 标签导致的门禁失败。'
  title: attn-residual stride 修复及 TP8 P/D 回归
- block_id: ce2e5db094088d5f8e44d2d55fab3d42a2ba41c0e4787e108fbf5d6f600fa60f
  indexed: true
  sha256: a5fde59016556ba3fb7f4b6e321f57a9985e71cd929a670b11e0d0910d961013
  source_range:
    end: 211180780
    part: 4
    start: 194400529
  summary: 计划使用旋转量化 16 专家 QuaRot、GQA DSpark 草稿，在单机与 P/D TP8 下比较确定性请求的 token、logprob、Prefix Cache、C16 和长上下文结果，并打印实际 KVCacheConfig、group spec/block/page/layers 及 tensor 配置；历史分组结论仍仅作待核对参考。实机启动时发现 GQA 草稿仍路由到 deepseek_v4/dspark.py，该实现无条件读取 MLA 专用的 hc_mult，而 Qwen3Config 不含此字段，导致所有 rank 在 KV 初始化前失败；因此本轮尚未得到精度或 KV 分组实测结果。
  title: GQA DSpark 精度测试被加载阶段缺口阻塞
- block_id: 71be1ac4cdea00e0acbeb35b55b0851c02767b8fffab31450279fe01954f57f9
  indexed: true
  sha256: d7a4d9898fb0d3b5228f0ce0da9cf7808c155f9e2afcb89842d05f818d914621
  source_range:
    end: 218799068
    part: 0
    start: 211180780
  summary: 按授权清理了 155/156 的服务及 153/154 的旧实验进程，创建了带 QuaRot 标准描述符的隔离权重视图，并修复 154 staging tree 不一致；原始权重未修改。四机 MLA 以 TP16×DP4×EP64 启动，完整 345 分片和 MLA draft 正在加载，64 个 HCCL rank 尚处于初始化/图捕获阶段，尚未形成精度结论。
  title: 四机验证环境清理与 MLA 启动
- block_id: 89c2c5464aa9af2b16d3539e1233d3e4c9be3450b62c5a03de23cf2e1573104c
  indexed: true
  sha256: 0ffc3e9c9da0455893b31f0a570c29a3316dbe8ad5dbdd279c6a976bc0afccb8
  source_range:
    end: 218799068
    part: 1
    start: 211180780
  summary: 四机 MLA 服务最终达到 64/64 worker、两张 ACLGraph 完成、fatal=0，KV 配置为 20 个逻辑组、5 个物理 tensor、scheduler block=384。短题、384/385 边界、Prefix Cache 冷热清空、A/B1/B2/C、8K c1/c2/c4/c16、c35/c64 和 128K c1/c2 均未复现 None、NaN、连续 `!`、Invalid-GM 或高并发卡死；DSpark 产生并接受了 draft token。GSM8K 串行 no-hit 已开始，后续 16 路并发、GPQA 及 GQA 专项仍未完成；跨 DP 的重复 token 文本存在合法 hash 差异，不能当作精度失败。
  title: MLA 历史精度用例首轮回归
- block_id: 95d32b0e4302647ae8d790306bd64a6acce02695b2f725e4f0f5bbcaa7eab491
  indexed: true
  sha256: c80f35113b75d22d809e73a67baa96f81e69d115500b9fa83edf3d1909e04a6e
  source_range:
    end: 218799068
    part: 2
    start: 211180780
  summary: GSM8K no-hit 串行 8/8 精确，但 16 路中 case-12 稳定输出 12 而标准答案为 13；历史 0.26/PR13277 也同样输出 12，故保留为既有权重质量边界。MLA 的 128K c1/c2、384/385、4095/4096/4097、hot-rank 图回放和多模态 DSpark smoke 已通过，未见历史坏输出或 KV 冲突；GPQA c64 中集中出现的 21 个 `pred=None` 已确认是客户端三次 `ReadTimeout` 累计约 5403 秒，不是模型返回 None。GPQA 随后改以 c16、7200 秒超时重跑，进度到 16/198 且暂为全对，仍未完成。
  title: MLA 补充边界与 GPQA 超时归因
- block_id: b78a19217925720b431bcaca62ed1aed01869e9dacc54d29c17752f5e26f64c0
  indexed: true
  sha256: 51806b66ac5c291a1ef27b46a0c6370fec5204471c46b129072338697f442647
  source_range:
    end: 218799068
    part: 3
    start: 211180780
  summary: GPQA c16 继续运行至 47/198，46 题正确、1 题错误；错误主要是可解析的语义选择题差异，未出现 None、NaN、解析失败、请求错误或服务异常。长题最长约 2807 秒，四个 engine 持续生成、无 waiting，日志约 18 tokens/s、KV 占用约 10–13%，因此暂未支持卡死判断。历史精度问题专项大部分已通过，但 GPQA 尚未完成，GQA 以及若干未完成矩阵仍不能宣告通过。
  title: MLA GPQA 长推理持续回归
- block_id: d3fd1a70cfea77048c7bb13a7e80732ee21de216bba1d93acb6e402a9853173c
  indexed: true
  sha256: f4adbc2e3ce8eb2c9458f0634309ecf7a6499008026e586646186717c383a63e
  source_range:
    end: 224914120
    part: 0
    start: 218799068
  summary: GPQA 中途保留到 111/198（104 对、7 个语义错题、请求错误为 0）后暂停，转测 GQA DSpark。GQA 完成 345/345 target、draft、QuaRot 共享权重加载及两档图编译；KV dump 显示 20 组、block_size=384，四个 DP 的 1-token、384/385、边界、Prefix Cache 基础矩阵均报告 ALL_PASS。上述结果是运行记录中的通过结论，不能外推到尚未测试的高并发或完整任务。
  title: GQA 启动与基础矩阵
- block_id: 13cc2057274a026d83c4de503cf2275e3ab87117288d6a56d23723687a04e336
  indexed: true
  sha256: b9de5d7b38f13ecfd647c713e0577d7d3f963b92527a45dc7564ff9d6a23ed8f
  source_range:
    end: 224914120
    part: 1
    start: 218799068
  summary: GQA 的 4K、Prefix Cache 冷/暖/重置、128K 和 B2 对照均报告通过，但 8K 同一 DP 高并发出现静默连续 `!`：全 miss c8 约 5/8 正常，c16 约 9/16 正常，而失败 prompt 单独 c1 均正常。一次正文出现 `None` 后经重跑确认是自然语言词汇导致 harness 误判；真正问题被定位为并发状态更新或 prefix-enabled Mamba/KDA 路径，不能把 prefix-on c8/c16 原实现视为通过。
  title: GQA 高并发静默退化
- block_id: 995674d497708d1ab62b9399c3ced14ba9a636cbc650ba5d262310ea7d3aac7c
  indexed: true
  sha256: 612eb544c8155d3b011d4788ae2e31891a9453913b960b74e50c6186a8bf1393
  source_range:
    end: 224914120
    part: 2
    start: 218799068
  summary: 显式 prefix-off 的 c8 为 8/8 正常，c16 的唯一失败后来确认是正文包含 `None.` 的测试器误报，实际为 16/16 正常；late-H2D 补丁的 targeted UT 为 6 passed，并使 prefix-on all-miss c8 达到 8/8、all-miss c16 达到 16/16。可是 warm prefix c16 仍只有 5/16 正常，随后发现 GQA/MLA/Mamba 共享 backing 的 KV 页布局冲突；四平面+late-H2D 的 UT 报告 57 passed，但用户要求释放四机且不带四平面改动，最终仅保留 late-H2D 文件差异，未启动最终组合验证。
  title: A/B 修复与四平面撤回
- block_id: 9950bb334c6041e67df47de898daa27c6190561a1a32d412a4cb364c41034e41
  indexed: true
  sha256: 987d9095011f1fbc7294bbebc459187c68dc33b03e640c061a69b5cf2a7b8068
  source_range:
    end: 237633933
    part: 0
    start: 224914120
  summary: '`dc9e8375` 在 TP16×DP4×EP64、GQA DSpark、Prefix Cache 和异步调度下完成四机回归；cold/warm 各 16 条请求语义均通过，warm 命中率为 93.75%，未复现重复 `!`、空输出或乱码。旧 harness 的失败来自正文普通单词 `None` 的误报；随后 `/collective_rpc` 的 cache-spec 探查因 `torch.dtype` 安全序列化失败，仅是测试后诊断噪声，运行时 KV group 对象仍未独立 dump 闭环。'
  title: GQA+MLA 共分组方案首次四机验证
- block_id: 8a4294b2f8860118ba6421cb388ad26f26266fb7c6bf4a2f7f43ad33be96a936
  indexed: true
  sha256: 00f350723c31e0f0aa4dc381806901343f5740d1ce499b82812bc7b8babefd65
  source_range:
    end: 237633933
    part: 1
    start: 224914120
  summary: 单机 16 专家 overlay 在 rotation descriptor 修正后完成 cold/warm 16/16，DSpark 接受率约 70%；随后四机全量权重服务启动成功，并按用户要求扩展到 c16、c35、c64、1-token/42-task、block-size+1、128K 及后继请求等历史边界。旧 15/16 仍被确认是 `None` 文本误报；c35 高容量排队期间持续推进，阶段性结果均未见卡死或异常，但该块结束时四机全面矩阵尚未完全收尾，GPQA 仍跳过。
  title: 先单机后四机的精度验收与历史边界覆盖
- block_id: 0facc9b27017af7c752c7e28fc3ad9ceebaf5bbfd83a7ee22fb636a6342a5ed7
  indexed: true
  sha256: 54da8b577705476b30781e1b2929deec2c72a5f6f48064d1f24d64117cf3d7d6
  source_range:
    end: 237633933
    part: 2
    start: 224914120
  summary: '四机历史矩阵最终报告 327 条正式请求通过，覆盖 Prefix Cache、c35/c64 容量压力、128K c1/c2、自然 GSM8K、多模态和工具调用；日志无 ERROR/CRITICAL、traceback、EngineDead、invalid-GM、HCCL timeout 或 NaN。强制 `ignore_eos=True` 的 K3 控制标记被保留为压力输出，不与正常 EOS 的 GSM8K 语义结果混同；服务和产物仍保留在 153–156，GPQA 未执行。后续 PR 整理把 Attention Residual 的任意非连续 stride 兼容改为完整连续 buffer 加 `num_valid_blocks` 契约，并新建 GQA/MLA KV 分组子 PR #14765；针对 0.26/0.27 多约 4.45 GiB 显存的单机 A/B 已锁定 exact commit，但因 153/154 仍有未知占卡进程而暂停，尚未定位最终来源。'
  title: 四机全面精度回归通过，但显存 A/B 尚未完成
- block_id: a190d88b91c89d67a30157d183ec98e91303b056789a248ce3f6b1b92b940f77
  indexed: true
  sha256: a2bb4373258530ea187368441ea0f4ddf555b82ebf2fa0f75ce36f787a142bfc
  source_range:
    end: 237633933
    part: 3
    start: 224914120
  summary: 在统一 TP16/EP16、DSpark、8192 profile tokens、capture size 16 和同一份 16 专家 QuaRot 权重下，旧 0.27 的 KV 余量为 30.05 GiB，0.26 为 33.40 GiB；0.27 权重反而少约 0.17 GiB，但激活/非权重开销多约 3.52 GiB，graph 均约 0.51 GiB。实验经历了 PGID 命名空间错误、SSH/VPN/代理不可达、PYTHONPATH 与 spawn 的 `acl` 丢失、tokenizer cache 只读、`dequant_situ_quant`/`libcust_opapi.so` 不匹配及单机 EP16 加载全量 896 专家导致 OOM；这些均不能作为版本显存结论。0.27 最终完成 graph 和本机 HTTP 200 请求，但裁剪权重语义不可信；0.26 服务可 ready，MLA prefill 请求因 key tensor 为 `RESERVED` 返回 500，因此运行成功与请求通过均需分开记录。
  title: 0.26/0.27 单机显存 A/B 与环境排障
- block_id: a7c9baff11ab7a82a113d7847920162d3c848e50144abb33b854e75dcbb51d84
  indexed: true
  sha256: 669208bba8da88465b3d03edf7b523981c15843b83ca1ae73d9df5d5994bff2a
  source_range:
    end: 237633933
    part: 4
    start: 224914120
  summary: 旧 0.27 对照显示 peak activation 为 4.23 GiB、0.26 为 0.70 GiB；当前父 PR 在隔离副本清理三处 FlashComm v1 残留后，激活峰值降至 2.19 GiB、KV 余量回升至 32.09 GiB，但这不是未经修改 current head 的直接结果。current head 曾暴露 `is_vl_first_layer`、两参数 gather、`maybe_chunk_residual`、256/4096 shape mismatch 等组网同步问题，诊断性删除后才完成 graph 和本机 HTTP 200；这些改动尚未推送，且不能把隔离结果宣称为正式修复。随后已开始单变量实验，将 residual bank 从全容量预分配恢复为 0.26 的 compact 追加方式，以判断剩余约 1 GiB 峰值是否由 residual 管理造成，但实验因 SSH 认证失效中断，最终显存分项和请求结果尚未取得。
  title: 当前父 PR 的残留问题与 residual 显存复核
- block_id: f1271dc904acb95230f0e7072a1a937caf3a6e34871c52fdae8f9c80db9a5c86
  indexed: true
  sha256: e1eb0c3e7ccb8afe3bb9a564dc36679574a638eb0870104011e33387bbd0e82c
  source_range:
    end: 251698944
    part: 0
    start: 237633933
  summary: 围绕四机 GQA 全量验证，先清理了 155 上旧 0.26 服务，并以父 PR 全栈启动 DP4×TP16×EP64；r3 在完整权重加载后于 Attention residual Triton 的 8192-token profile 触发 vector core timeout，不能视为验证成功。将 residual kernel 恢复为 0.26 的 N/B/capacity 编译期特化后，单卡同形状算子测试约 2.49 秒完成且与 PyTorch 参考一致，独立提交为 `928a7012e`；但四机 r4 profile 仍在数分钟内持续高负载、未返回，随后按用户要求停止并改用双机 16 专家裁剪权重定位。
  title: 双机裁剪验证与 profile 卡死定位启动
- block_id: 1e78c0a61316bacda160dcf11ed8f790edd6b6a506680b118afe0fdd4ef9c7e2
  indexed: true
  sha256: 9ef4ae6e9f56f57838e448f3a3494250dd238c011d7a3832a1b768cdf130300a
  source_range:
    end: 251698944
    part: 1
    start: 237633933
  summary: KV grouping 已从固定 TP16、层数和 page size 改为按 ownership 语义动态分组：full-sequence target/draft attention 合并，Mamba 按比例均衡分组，local/sliding-window 等不同语义回退通用逻辑。TP8 实测使用 block size 768，得到 `[29,23,23,23]`，TP8/TP16、GQA/MLA、动态层数及 scheduler/worker 合约测试通过；双机/四机之外的 TP8 真实裁剪服务也启动并返回非空响应。此前基于错误 vLLM core 添加的 worker helper 已撤回，不能把旧 core 导致的 `None - 1` 问题当成当前 PR 缺陷。
  title: KV 逻辑分组已通过 TP8/TP16 与真实服务验证
- block_id: d812c76d128f5a7dc995674aef63ef5a6b88f2f1af59da89453c597c9e9eb0b9
  indexed: true
  sha256: 0e8ce2cce7a078862fe1abcd6035fb403940bbbb223f46c914454b8ea714d6cc
  source_range:
    end: 251698944
    part: 2
    start: 237633933
  summary: 四机 r3 完成 352 shard 加载及 post-load，但在 DP2/TP0 的 `_apply_attn_res_kernel` 发生 vector core timeout，随后出现无效 stream allocator；因此尚未进入精度用例或 GPQA。修复 kernel 编译期特化后启动 r4，仍在 8192-token profile 长时间保持 64 卡 AICore=100% 且未返回，故不能仅凭无 fatal 判定通过。按新的验证顺序，已放弃先测单机显存，改为双机 DP2×TP8×EP16 的 16 专家裁剪权重；双机 profile 约 80 秒仍无 worker 返回，已停止并准备使用临时同步 DFX 逐层定位。
  title: 四机 residual timeout 后转双机 DFX 定位
- block_id: 44f442ce83b1698c731e50e5b7828acaac048324f152b0ae0a4b6909b22ee8ec
  indexed: true
  sha256: ae9998e12ac6e2fa819d7f0e754207d60a1499d9b69e14678a8d04d4355e6977
  source_range:
    end: 251698944
    part: 3
    start: 237633933
  summary: 双机验证确认卡死特定于 `SP=true + shared_expert_dp=false + multistream_overlap_shared_expert=true`，而不是通用 SP、HCCL、MoE、权重或 task queue；恢复 `shared_expert_dp=true` 可通过但每卡 KV 降至 10.66 GiB，不能作为最终方案。首个“把输入 TP all-gather 放主流”的修复思路被纠正，目标改为让 TP AllGather/ReduceScatter 保持辅流，并与 routed dispatch/combine 错开；随后按“入口 AllGather→quant→共享专家首个 matmul→与 routed 路径并行→combine 后 ReduceScatter”的时间线实现。focused UT 在容器中由 8 passed 增至 9 passed，但首次 r9 硬件复测失败源于残留进程导致显存不足，尚未证明该版本硬件修复成功。
  title: SP 共享专家多流卡死的定位与通信时序修复尝试
- block_id: b5297848645243404c583d7c4d0cac2bc5ac258dd6da229fafc95ccc1a8cc55c
  indexed: true
  sha256: a54ae6a90d06400d6885cace533ff483022984a6252d13d857706cd6697d42c2
  source_range:
    end: 251698944
    part: 4
    start: 237633933
  summary: 新时间线的 focused UT 为 `9 passed`；TP16 单机 profile 约 27 秒返回，peak activation 为 0.79 GiB/card，双机 TP8×DP2 约 0.93 GiB/card，均未复现卡死且保持低显存。四机全量权重服务成功启动，profile 和图捕获通过，四机本地 `2+2` 多数返回 4，但出现特殊 token 泄漏；进一步 A/B 显示当前多流配置 7 次中有 2 次泄漏，而 `multistream=false` 在四个 rank、20/20 次稳定返回 4。由此应保留“通信卡死已修复”的结论，但不能宣称精度通过；剩余问题已收窄为共享专家多流计算重叠，尚未完成下一轮针对性时序修正。
  title: 通信卡死已解除，但共享专家多流引入输出不确定性
- block_id: a9a487f9602ce352867c2807e5771ea9ddaf306cf45bcc7e783cf0c2c8662594
  indexed: true
  sha256: 17f7f2fc4f2fae04128323567fe43583030863c09d158ef773a3ecf7a71c004d
  source_range:
    end: 260985970
    part: 0
    start: 251698944
  summary: 通过错开 shared down-proj 与 routed combine，四机完整权重的高信号用例由旧方案 7 次泄漏 2 次变为 20/20，聚焦回归 62/62 通过；随后将 Ascend Mamba 的 temporal grid 从 `32×138×16=70656` 调整为单 temporal tile，避免 `coreDim > 65535`。单机裁剪权重 C32 为 32/32、无 EngineCore 退出，四机完整权重 C128 为 128/128，且无 `None`、乱码或特殊 token 泄漏。GPQA 后台运行期间仅完成部分题目，曾记录 26/198、146/198、166/198、178/198 等中间状态，最终状态在该块内未给出。
  title: 共享专家时序与 Mamba 网格修复验证
- block_id: 390647ca13894afcf156084a11ef31c30bd339132ce983d16f7b6c7cfffed0a1
  indexed: true
  sha256: b8547230b69a14d418d6ba32d2cc5e369d8a033a168c3dadad7cb344cc8a25d5
  source_range:
    end: 260985970
    part: 1
    start: 251698944
  summary: 'GPQA 曾观察到 draft 采信率 `17.49%`、平均接收长度 `2.224 token/轮`，并在不同阶段出现 92% 左右的中间准确率；进程运行正常但速度较慢，不能将中间快照当作最终 198 题结果。父 PR #14454 及 Attention、SiTU MoE、Hybrid Runtime 子 PR 已更新，明确 GPQA 仍在进行中，并计划进一步优化 CANN/Ascend Triton kernel，而非把 `_TEMPORAL_TILES=1` 视为最终性能方案。'
  title: GPQA 中间结果与 PR 更新
- block_id: 5638a467d96ee4b9d8d6914451c8e47db90f6425adfcd33ad91f3f80f10d1158
  indexed: true
  sha256: 77d7c341ad38f2e609d1f1058887da1d466bd9a75c8887ec6b04cb4548ab0fc9
  source_range:
    end: 260985970
    part: 2
    start: 251698944
  summary: 按 `[16,32,48,64,80,96,112,128]` 捕获图完成 2K–256K、固定 1K 输出的四机矩阵；最终 r4 48/48 请求质量通过，但 30 ms TPOT 门限下只有部分长度保留 c1，16K 与 64K 的 c2 线性度分别为 0.487 和 0.435。随后用相同历史 8K prompt、清空 prefix cache 做对照，确认当前平均接收长度约 `3.226`、TPOT `36.92 ms`，显著差于 0.26 的 `6.529` 和 `23.20 ms`；初步根因是 0.27 删除了 GQA draft 所需的 materialized hidden-state hook，使 draft logits 偏离，而非 SP 或单轮算子吞吐变慢。
  title: 四机性能矩阵与 DSpark 接收率回退
- block_id: 098fad8b44b92612705796927390b82cbbb859c5a7be9ddb9d50fa918035911f
  indexed: true
  sha256: 04c9c75e658a55c5aabaa503b27accc1763e142a89d81adb628f44eb77cceacf
  source_range:
    end: 260985970
    part: 3
    start: 251698944
  summary: 报告称恢复了 Qwen3 GQA DSpark 的 materialized attention-residual stream，同时保持 K3 MLA 为 raw stream；固定 8K/1K 两轮结果中，平均接收长度报告为 6.403、TPOT 18.20 ms，但这些仍是来源中的测试记录而非独立核验。随后完成 2K–256K、1K 输出的自适应并发 sweep；32K 因 `literal_none` 误报补测，64K/c4 与更高档位因 TPOT 超过 30 ms 停止，256K/c1 报告为 30.72 ms。
  title: GQA materialized auxiliary stream 修复与全量性能矩阵
- block_id: 72cac8db3f00cde6b5c0902ac8c3cc479605fee6c68ffb6636ad37c9519dbcce
  indexed: true
  sha256: 8cc579cdb7c3e8d4071f340fb18b59c80d85ad496db369233c63fee190bf9132
  source_range:
    end: 260985970
    part: 4
    start: 251698944
  summary: 来源报告性能矩阵达到 600/600 结构性通过，精度 focused 集为 62/62；短数学集为 128/128 结构通过但语义正则仅 126/128，两个 miss 后续串行复测 9/9，分别涉及 `5/8` 表达形式和输出截断。`literal_none` 将题面合法单词 “none” 误判为输出 None，曾导致 8K/c16 与 32K/c4 提前停止，后者已用修正规则补测；首轮精度 runner 还因 `ModuleNotFoundError` 失败并保留记录。来源最后称已按精确 PGID 停止 153–156 服务、端口不可达且 NPU 无运行进程，但这些是报告结果，不能升级为独立验证。
  title: 精度回归、harness 误报与四机释放
- block_id: 07934b2fa3f82958ac7de147578d31c3a2a758f50e258c890831b42e0e8122b9
  indexed: true
  sha256: f0ac9313078d4189d897e7a09faccc950dd28172fffda67e29e9051f2caed094
  source_range:
    end: 277780598
    part: 0
    start: 260985970
  summary: 来源记录先确认普通 W4A8 非 QuaRot，随后找到并修正真实 QuaRot 权重自身的 `global_rotation` 配置，日志进入 `[spec_decode/quarot]` 路径。QuaRot 扫描报告 18 个稳定达标波次，并保留 8K/16K 临界档复测失败、DSpark 接收率和输出轨迹差异等不确定性；精度回归、PR 独立评论和四机释放均为来源陈述。记录还确认 `_finalize_pending_parallel_draft_rejects` 属于后来误带入 0.27 PR 的旧 reject D2H/event 链，计划删除而改用 canonical host sequence length。
  title: QuaRot 双机前的四机验证与 PR 评论
- block_id: 20ffea028fa97cc7c9de0c0f1448b99fd0d75474a4816219ef074c63e9a6e9c0
  indexed: true
  sha256: c1793870131176d9920888ea28503ca3ec4e211b675082c5376c67d9041e4ed5
  source_range:
    end: 277780598
    part: 1
    start: 260985970
  summary: 来源记录删除了跨 worker、attention 和 spec-decode 的旧 reject D2H、event 与 finalizer 链，并清理仅为 UT 引入的一次性 helper、不可达 vendor 分支和格式化噪音；算子侧则报告 106 个文件中 98 个与 0.26 实现一致。父 PR 与多个子 PR 的类型检查、pre-commit、mypy、review thread 状态和可合并状态均属于当时报告，不能视为独立验证；153/154 曾因 VPN 不可达而缺少当前 head 的 NPU 回归。随后只读估算认为双机可尝试 TP16×DP2×EP32、短中上下文，但 133K 配置可能容量不足；最新验证在 154 遇到 vLLM 0.26/0.27 镜像不一致，修正后又因漏同步 `_build_info.py` 导致 154 在建引擎前退出，当前尚未形成精度通过结论。
  title: 旧拒绝链清理与评审前整体审计
- block_id: 3cb787806406a363da2a11b01bc93e90f7fa1aa5d1876f46005f128cb8af6bec
  indexed: true
  sha256: 5ef8db0f96d88e672cf705f7dfa9a3f62344b9738e809d8aa479a7f9e481c5bd
  source_range:
    end: 277780598
    part: 2
    start: 260985970
  summary: 来源记录对父 PR 当前 head `cbeac40f2` 进行双机完整 QuaRot 验证准备，确认两机 16 卡、共享权重和自定义算子编译产物，并计划覆盖 `None`、乱码、1-token、Prefix Cache、GQA DSpark 等用例。153 的 PR 当前 head 算子编译报告为 RC=0，双机服务也曾启动并确认 TP16×DP2×EP32，但这只代表启动阶段证据，不代表精度或性能成功。154 因缺少编译生成的 `_build_info.py` 在真正建引擎前退出，153 仍等待 DP peer；后续需要补齐部署产物、重启并重新验证。
  title: 双机 TP16×DP2×EP32 精度验证启动受阻
- block_id: 25c73fe7a9603229082027d8692ed3399cf6297a1859908d5a479784468c50a3
  indexed: true
  sha256: c1649e54da1f0cef362dfa063c32893c039cb95da8733070244dcd0d1ba8f696
  source_range:
    end: 277780598
    part: 3
    start: 260985970
  summary: 记录了 TP16×DP2×EP32、EP32、GQA DSpark 双机部署从 headless rank、权重加载到图捕获和真实请求的过程：`gpu_memory_utilization=0.98` 在第二档图捕获时因仅余约 229.79 MiB 而 OOM，`0.97` 完成图捕获但运行态仍偏紧。`0.961` 实测 KV 为 66,859 tokens、图后最差约 1,166 MiB，但 8,192-token prefill 因 DSpark 一次申请约 526 MiB、仅余 389.56 MiB 而 OOM；随后提出固定 2.625 GiB KV 与 `max_num_batched_tokens=2048` 的新配置，但其最终安全性尚未验证。期间还记录了 `num_prefill_lookahead=None` 接口错误、Mamba block capacity 10→17 修复及相关 UT 通过，之后用户要求暂停实验并转向 PR 审计。
  title: 双机 QuaRot 显存与 DSpark 安全性排查
- block_id: 49299291e9850b8acba207a8437fa8d8798cbb97d5778fbf8c123eeee1854a05
  indexed: true
  sha256: 0552f9ac565ba0be4fd4ada0059e941b4f9ce5d7f26e70a3e370eba0accb769c
  source_range:
    end: 277780598
    part: 4
    start: 260985970
  summary: 该块报告了针对父 PR `vllm_ascend/` 的审计和清理：据助手陈述，删除了仅供测试的字段、重复校验、loader hack、无关格式化及 CRLF 噪声，并声称生成了覆盖 28 个生产文件的全景文档和两个提交；但 CI 仍排队，聚焦 UT 受本机缺少 `torch` 影响，不能把旧结果当作当前 head 的验证。后续审阅聚焦 `kv_cache_interface.py` 的 DeepSeek V4 风险，确认讨论点包括 `compress_ratio` 是否由父类 `MLAAttentionSpec.merge()` 保留、`real_page_size_bytes` 是否覆盖 DSV4/V3.2 特殊布局，以及 K3 自定义 `scale_dim`；这些结论在材料中主要是解释与待核对的审计判断，不应视为已完成的 DeepSeek 运行验证。
  title: vllm_ascend 大 PR 清理与 DeepSeek KV 契约争议
- block_id: 5d7cd9b7825f37baf84d4305d8accee3024c9f6c1d6f944d0a5ba8b7f0bafad3
  indexed: true
  sha256: cd95856a468d1de1f60e95ce015889895fdd6e7eab5f9f1896c929e7a14808dd
  source_range:
    end: 277780598
    part: 5
    start: 260985970
  summary: 材料说明 vLLM 0.27 原生 hybrid grouping 对 K3 的 `24 MLA + 5 draft attention + 69 Mamba` 使用最小 bucket 5，GQA 形成 20 个 KV group/约 20 次 build，而 MLA 合并为 29 个 attention layer 后形成 4 个 group。当前 PR 据称将两类 attention 共用 scheduler ownership、把 Mamba 均衡为 `[23,23,23]`，使 GQA 变为 4 个 group、5 个 builder，但代价是统一物理 stride 带来约 18/116 的空槽；同时材料指出 MLA 的“4 builders”测试按 Python 类型计数可能错误，按 exact spec/backend 应为 5。最后用户提出按投机、PD 分离、prefix cache、Mamba、MLA、modeling、算子、KV cache 重新复用子 PR 或拆父 PR commit，当前仅开始依赖和独立测试边界审计，尚未形成最终拆分方案。
  title: GQA/MLA DSpark 分组权衡与父 PR 拆分方向
- block_id: 2b409f97d54bb7fbc16a066c0a64f2abc2c059c5920ee696aaeef47b34e81d49
  indexed: true
  sha256: 8d7f64285ec1c91198257233e858659bed266c30dc118980ccdf2d24e9239f88
  source_range:
    end: 294576783
    part: 0
    start: 277780598
  summary: 材料记录了将父 PR 拆为 10 个职责清晰的子 PR，并把 Prefix Cache 定位为跨 Mamba、KV、投机和 P/D 的验证契约，而非独立代码 PR；随后各子 PR 曾基于最新 main 重建，DSpark 需要人工处理 breakable ACL graph wrapper 与 K3 drafter 逻辑的冲突。后续同步中报告了 vLLM PCP import、CRLF/Ruff、KV 测试格式等 CI 问题及修补，但部分独立 PR 的失败被归因于主线自身的旧 import 缓存问题，不能视为全部验证成功。最新状态显示父 PR/DSpark 仍需继续处理主线推进后的冲突，尤其是新增概率采样初始化测试、EAGLE3 `_maybe_remove_d2t` 与 QuaRot 共享层逻辑的并存。
  title: K3 大 PR 原子化重组与最新主线同步
- block_id: 82d337c7d2b4a1e3aeeaaf5980e64acfe55f71a22ff40e9a4ebf309a8338c51a
  indexed: true
  sha256: 79b48fb001baec1b3598ae7ffd8ae910e475bbf0d55655873927b2dc87be4495
  source_range:
    end: 294576783
    part: 1
    start: 277780598
  summary: '材料报告了 rebase 后的双版本 PCP import 兼容、MLA/父 PR 的静态检查结果，以及 #14768 在 QuaRot source of truth、固定 GQA 拓扑、全局 `UniformTypeKVCacheSpecs` monkey patch 和 TP8 测试方面与 0.27 逻辑并不完全等价。另记录了 VIT-FIA 中 `key/value.contiguous()` 的遗漏补丁归入 #14600，及随后加载阶段因 checkpoint 含 `mm_projector.rot_proj.weight` 而描述符未声明 QuaRot 导致的 `There is no module or parameter named ''mm_projector.rot_proj''`；该问题尚未通过代码修改解决。针对真人 review，#14599 保留 ModelSlim 量化适配并改写标题/描述，不再拆独立 PR；同时声称已处理 SiTU 重复 forward、KV 契约和 P/D proxy 抽象等意见，但硬件回归受现有双机服务占用而未执行。'
  title: Review 修复、ModelSlim 归属与 VIT-FIA 权重排障
- block_id: 4ca603a54d21cff96721d861ff829927f0da866b3e8c7d36145cc287e837c914
  indexed: true
  sha256: b178dbfda883c298f1bfbae08319c90b2e6657940ae4bf7abba61ca20bde686c
  source_range:
    end: 294576783
    part: 2
    start: 277780598
  summary: '材料将 25 条真人未解决意见按代码、PR 边界、解释和文档类别整理，重点指出 #14765 的 coordinator 参数、page-size 字段、Ascend MLA spec uniform 限制，以及 #14599 SiTU/ModelSlim、#14602 proxy 抽象等问题；随后报告已回复并解决这些线程，但这些状态仍属于过程记录。用户要求不拆 ModelSlim PR 后，#14599 改为承担 SiTU MoE、共享专家和 K3 ModelSlim 量化适配，并同步父 PR。最新推进中，DSpark #14601 已在新 main 上重放，但父 PR 集成仍遇 DSpark/KV 测试冲突；同时 #14839 暴露 `SimpleNamespace` 缺失导入和 PCP 兼容导入未使用等静态问题，需先修子 PR 再同步父 PR。'
  title: 真人 Review 汇总与父 PR 冲突同步
- block_id: 6ed019ec695a6d6c55cd22cbc615ee2eec110240bbb4262068a75f0a10c2f529
  indexed: true
  sha256: 7e836347c56c75c5f9cb11da6756ab038fd27ab00880e629bc70055e17c650d8
  source_range:
    end: 294576783
    part: 3
    start: 277780598
  summary: '父 PR #14454 及多个子 PR 曾完成 rebase、冲突解除和真人 Review 回复，但后续审查又发现过时说明、失效 proxy UT、源码字符串断言、重复 helper 及错误的 `k_pe.contiguous()` 等问题，并进行了删除或重构；Gemini 意见被明确排除。旋转量化最初因错误的架构判断未迁移到 modeling，后来确认应由 modeling/load_weights 管理，proposer 只保留通用 hook；47 个定向行为测试通过，但未完成新的整权重 NPU 验证。AscendC 算子侧仅声称复用重复的 `IntegralConstant/IsSame`，未改变 kernel 流水或整体逻辑，不过仍需 A5 clean cross-compile 才能完成更严格的算子验证。'
  title: PR 冲突、Review 收口与低质量测试清理
- block_id: 3074dee34e95e8a2e5da6d4e553065416bda9963e92de91141d53723b64fec4b
  indexed: true
  sha256: 4b869a28cbe6eed7644523a73c6f64d22255d41f69e6ee0ee6d8dc640f9257ab
  source_range:
    end: 294576783
    part: 4
    start: 277780598
  summary: '确认将 QuaRot 加载、rotation 保存、draft projection 转换及共享 embedding/lm_head 准备迁入 #14600 的 modeling，#14601 的通用 proposer 不再识别 K3/Qwen 或读取 QuaRot 文件；同时修复了 vLLM `ba07e4a` 下的 `WeightsMapper` 契约和一个无意义测试赋值。47 个聚焦测试通过，相关 pre-commit 被报告通过，但 coverage gate 可能因缺少 `ready-precise/ready-all` 标签而失败，不能等同于代码验证成功。另采纳了 `situ_mx_quant_common.h` 的模板去重；历史评论和空 pending review 也被清理，但整权重 NPU 验证因 153–156 有在用服务而未执行。'
  title: 旋转量化归属迁移与 Review 再评估
- block_id: 6a928642daddd4963146c0353f332d7d13ab3741dbfde9495c8b273512ca792d
  indexed: true
  sha256: 3f3e9a23db74793d2aa8a31ea18f5ff82b1e907782148e651a075ffc090a7506
  source_range:
    end: 294576783
    part: 5
    start: 277780598
  summary: 计划使用 153 和 154 两机、`DP2 × TP16 × EP32` 拉起完整 QuaRot 主权重与 GQA DSpark draft，`max_model_len` 设为 16K，以验证 8K 输入/1K 输出的采信率；旧的两套对照服务已确认属于该验证目录并正常停止，32 张卡已释放。两机完整权重约 1.4T 且可读，GQA draft 一致；当前 head 配套 vLLM 0.27.1，但 K3 自定义算子的适配头和绑定层相对旧验证源码有变化。尚处于在 153 编译当前 head、再同步产物到 154 的准备阶段，尚未报告服务成功拉起、采信率或 NPU 运行结果。
  title: 153/154 双机完整权重验证准备
- block_id: 3910cc07e75f6546e14e784fe4bf2ac1dd9375b3ba4003e54a67a87901d40732
  indexed: true
  sha256: ce4d607f3110c36c507bc57858059a17f96316b44ff232b79df1dbcdb182f809
  source_range:
    end: 307043472
    part: 0
    start: 294576783
  summary: 构建阶段曾出现 KDA 头文件 include 路径错误，修复后 `vllm_ascend_C`、算子注册和双机哈希检查通过。启动阶段先后暴露 CANN `PYTHONPATH` 被覆盖、未设置 `VLLM_VERSION=0.27.1`、`VLLM_PLUGINS=ascend` 过滤 `ascend_model` 等问题；这些导致 `acl`、PCP 和错误 K3 模型类相关失败，修正后 32-rank HCCL、QuaRot 权重和 GQA DSpark 均可加载。后续 `gpu_memory_utilization=0.80` 导致 KV cache 为 `-7.19 GiB`，将参数提高到 `0.95` 后服务越过 KV 分配并完成 graph capture，但这不等于整套运行时验证已完成。
  title: 0.27.1 启动链路与环境问题定位
- block_id: 5d6571458d42b2bc7532cf57f27b11c8fe53c29755ba9d29dbeebfbd78e3d516
  indexed: true
  sha256: c83ea8c8c7d73aa71efbd26a2f4008237d6863b186193066cdd2d931dfeea65c
  source_range:
    end: 307043472
    part: 1
    start: 294576783
  summary: runbook 曾加入多层 preflight、源码哈希和 revision marker，随后按用户要求收缩为启动脚本内固定 CANN、`PYTHONPATH`、`VLLM_VERSION`、插件列表、拓扑参数和常见错误处理，删除独立门禁与哈希审计。修正后的双机 `DP2×TP16×EP32` 服务完成 345 个目标 shard、QuaRot optional shard 和 GQA DSpark 加载，单 DP 8K+1K 请求采信率为 75.55%/78.66%，未复现“采信率为 0”。两 DP 同时各一请求时聚合约 52.75%至 58.20%，低于历史四机同协议 76.81%，且低侧会在 engine 间交换；这提示 DP 并发调度、同步或路由问题仍未定性。
  title: 轻量 runbook 与单路采信率验证
- block_id: 6a46e2766da13b23c90f67756107d4d809419bf3d2a4bb64b77b2b653ee1b0ee
  indexed: true
  sha256: 3c2814ef1e71aa32a84aa8e5f8f0a31de87a199662fa2ef516ac685a6d5cd528
  source_range:
    end: 307043472
    part: 2
    start: 294576783
  summary: 在冷 prefix、8K 输入和固定 1K 输出下，每 DP 2 并发（总并发 4）四个请求均成功，但聚合采信率仅 28.54%，engine0/1 为 24.64%/33.30%；每 DP 4 并发（总并发 8）未形成有效采信率，DSpark `propose_draft_token_ids()` 的 hidden-state `torch.cat` 申请 552 MiB 时 OOM，服务退出后已恢复。当前最可疑边界是同一 engine 内 batch size 从 1 增至 2 时的 DSpark 行映射或状态推进，而非已证实的模型精度问题；旧四机 DP4 每 engine 一请求约 73%仍只是历史对照。下一步计划使用已释放的 155/156 做 DP4/TP16/EP64 的单请求与双请求对照，但最新检查发现 155 仍有 vLLM worker、156 后 8 卡仍有 vLLM-OmniDiff，尚未确认可安全使用。
  title: 多并发采信率下降与后续四机定位
- block_id: 37f5a83dc085fb45d8766adb5fac40e1960d4922f98d39ee9f7dff3e13469ffd
  indexed: true
  sha256: 7d10dfe8c6f7c3ff88056510e298585f3ed57c245d1342d77d18a9e86c0fee68
  source_range:
    end: 307043472
    part: 3
    start: 294576783
  summary: 四机 external-DP 拓扑已按 TP16/DP4/EP64 拉起；旧路由 header 曾造成 HTTP 200 但空流，修正后同一 DP、同一 prompt 的采信率仍会在约 79% 与 25%–33% 间翻转，最终 token hash 也变化。代码检查发现 K3 的 1 个 attention group 与 3 个 Mamba group 使用不同 block ID，但清零记录只覆盖 attention group；不过补齐所有 group 的清零后，首条请求仍仅 25%，因此“复用页未清零”已被否定并应回退。版本边界也得到纠正：vLLM `6e448d0...` 就是固定 `v0.27.1` tag，`cdc4824a` 才是曾用于 A/B 的后续 main commit；旧的正常基线实际使用了 `cdc4824a` 与 vllm-ascend `3053909a`。
  title: 四机复现与状态复用假设被否定
- block_id: b7714d2edbc19b20b01aa13281e2b63357bd33006fbbc8b77f275860edc72cf3
  indexed: true
  sha256: f1560eedd3a98a3937700aabfd9a627b5a5168ff0512561dec2371236db0f464
  source_range:
    end: 307043472
    part: 4
    start: 294576783
  summary: 正式验证已恢复到固定 vLLM `v0.27.1`（commit `6e448d0ea9bf...`）、`VLLM_VERSION=0.27.1` 和固定 import 路径，未设置 `VLLM_ROOT_OVERRIDE`；此前把源码 hash 误称为动态 main 的判断已纠正。与曾经正常的 `3053909a`/`cdc4824a` 组合正向比较后，共享专家串行化、GQA residual materialization 及 QuaRot 矩阵计算本身尚未显示为丢失，主要可疑点是提交 `43b14dbe7`、`0bafe6ed3` 将 target-owned QuaRot 状态从 proposer 移到 draft modeling/loader。当前日志只有共享 target 权重而无 QuaRot 对齐日志，提示 rotation 状态可能为空，但 draft 注册类和 `quant_config` 继承关系仍需核实，不能把这一假设当作最终根因。
  title: 正向 diff 收敛到 QuaRot 生命周期变化
- block_id: ea19dd921f961897b1a8abc5f28a274011b8f8349fd42908addf06bfa11003ae
  indexed: true
  sha256: c0510b64bd7e6bf349e4f858eab22ab6e68fbbb6ed6bc2bd2231514fe9a56923
  source_range:
    end: 307043472
    part: 5
    start: 294576783
  summary: 真人检视明确要求将 `_maybe_load_quarot_rotation` 与 `_prepare_unrotated_shared_layer` 移入 modeling 的 `load_weights`；因此仍由 proposer 回调 `prepare_shared_layer` 的方案 2 可能只完成形式迁移，职责边界仍有评审风险。当前判断方案 1 更符合意见：modeling 在加载阶段完成 `fc/context_proj`、embedding 和 lm_head 的基底转换，使其成为 draft 自有权重，并移除 proposer 中的 QuaRot 特例。实现仍需确认能否通过现有 loader 和 safetensors index 精确取得目标 embedding/lm_head，避免改成手工扫描完整 checkpoint 的新 hack。
  title: 检视意见支持将 QuaRot 收口到 load_weights
- block_id: 2ac806e27364970fa7e41844b904836bd414a91a789ac3db53ee2baa5a802ceb
  indexed: true
  sha256: d7aa45efd2004f827fb819aace3c1a6cf41f9901502d38ba087098a7423ae467
  source_range:
    end: 320741584
    part: 0
    start: 307043472
  summary: 方案 1 将 QuaRot 的 embedding/lm_head 读取、旋转转换和 draft 自有权重状态移入 modeling/load_weights，并删除 proposer 侧 hook；初版单请求采信率曾降至 24.23%–25.79%，后确认边界权重分片、旋转方向和数值误差均对齐，低值受采样波动影响。DP0 两并发当前 58.17%，历史 58.35%，相关 UT 为 46 passed；另发现 vLLM 版本固定、CANN/glibc 容器和启动环境会影响复现。后续 TPOT 首轮异常被初步归因于新并发 shape 的冷路径，#13954/#14915 合入及后续硬件验证仍在进行。
  title: QuaRot 边界权重对齐与采信率验证
- block_id: e354a391f24a6f459fafc981c6926fe922f4730c6819f34fae16c1209b8cd3bd
  indexed: true
  sha256: b1f09dc890be35fc2da31ab25e320c970440e41b84a893609feb85f3cd5b760d
  source_range:
    end: 320741584
    part: 1
    start: 307043472
  summary: '按父 PR 的长度×并发矩阵复现时，2K c4 首轮 TPOT 为 31.24 ms、TTFT 约 34 秒，随后同服务热态 c2/c4 恢复至约 19.38/23.00 ms；同序热态 c1/c2/c4 为 17.09/19.26/27.99 ms，质量通过，说明首轮异常不能直接判定为稳定算子回退。#13954 的 KDA fused operator 与 #14915 的 MLA 增量已合入并完成 rebase，但构建先后暴露旧 binding 重复定义、schema 15/20 参数不一致及非幂等打包等问题，均在独立集成目录中修正。定向测试最终达到 108 passed，AscendC fused KDA 单卡数值用例 1 passed；远端连接和构建期间还遇到旧容器 CANN/glibc、SSH agent 等环境问题。'
  title: TPOT 首轮冷路径与算子合入
- block_id: d666c06d74039e01b937ceb2f712f4fbc6b1f696943c0c6f801f1a800b686341
  indexed: true
  sha256: ce4e0c783d95ae8f06c316213672b38429943d25bfe7f1c3fa944e61931d77f9
  source_range:
    end: 320741584
    part: 2
    start: 307043472
  summary: 父 PR 更新至 e895710c1 后重新部署 QuaRot 权重 + GQA DSpark：首次失败是 draft 错误继承 target 的 EP64 parallel_config，修复后已能通过 draft 配置校验并加载 target/draft 权重。第二次失败源于部署遗漏伴随库 libvllm_ascend_kernels.so，导致 custom op 整体禁用；补齐产物后第三次服务完成权重加载，但 KV coordinator 触发 `block_size must be divisible by hash_block_size`，尚未进入 curl“你好”100 token 或采信率请求。当前最新未完成事项是定位父 PR 的 KV 分组块大小契约并做最小修复，不能据此宣称采信率或功能验证通过。
  title: 最新父 PR 采信率验证受启动断言阻塞
- block_id: 39e9acda1fd3d40601cb306ed42b9eed4026298901ca9c302b5ebeac66eba8d1
  indexed: true
  sha256: 1c101e5422db620418e17dc80ecc0d7a165c73d322d24b65c83df99d3f66a0b0
  source_range:
    end: 320741584
    part: 3
    start: 307043472
  summary: 双机诊断确认此前 KV 分组失败源于 GQA block_size=128 与 K3 MLA/Mamba manager block_size=384 的不当相等约束；修复提交 `72f4a62d2` 后，K3 分组回归用例通过，但四机最终验证尚未完成。新 `[REDACTED_SECRET]` 权重被确认是 5-token、1M 上下文的 full-causal MLA DSpark；启动中先后暴露 K3 配置基类校验、QuaRot metadata 丢失及训练期 `confidence_head` 未过滤等兼容问题，已按上游逻辑处理。当前验证口径仍是 MRV1 + greedy + `num_speculative_tokens=5`，未设置 speculative `max_model_len`；服务正在加载 345 个 target 分片，尚无最终精度、采信率或性能结果。
  title: Block5 MLA DSpark 适配与 MRV1 greedy 基线准备
- block_id: 24baf7efdf3cb07a8c6f6d6ea59c2bb01967b02287b3f3ffa208c7cc10febe1a
  indexed: true
  sha256: 2624411ef7a6607cb474cc630899f6eed2b586249f3c462646dc21169fea1b4d
  source_range:
    end: 320741584
    part: 4
    start: 307043472
  summary: Block5 的每请求 verification 长度为 6，但后续核对认为不应把 graph gear 强制为 6 与 TP16 的 LCM；ACLGraph 可将 6、12 等真实 token 数向上取到 TP 对齐档位，并通过 virtual-request/FIA padding 补齐。此前提出的 `math.lcm(6,16)=48` 方案因此被撤回，r4 改用 `[16,32,48,64,80,96]`，并已通过 CPU 行为测试覆盖 `6/12→16`、`18/30→32` 及 `[6,6,4]` 类虚拟请求布局。四机 r4 已进入 target 权重加载，进度曾到 313/345，尚未完成真实 KV、graph capture/replay、精度或性能验证。
  title: Block5 MRV1 SP gear 从 LCM 修正为 TP 向上取档
- block_id: 2abc9dded55a70f78e8c0875ff604a981646d0405228ada2a98d20441c6a0c80
  indexed: true
  sha256: ef41de47582bf3260ed0f27cf8f1abacc6d6bdb321552530fc4b4478c0fa7f61
  source_range:
    end: 334169226
    part: 0
    start: 320741584
  summary: 曾尝试移除重复的 `gear % 6 == 0` 约束、使用 LCM 档位并拆分 NoPE/RoPE metadata builder，局部测试通过，但随后发现 prefix-cache 的 `128 % 384` 断言，dummy 权重又因单机 OOM 未能完成复现。后续分析认为问题来自 draft 识别时替换 `VllmConfig.model_config`，触发 `refresh_block_size()` 将共享 cache 配置从 384 改为 128；相关 128 hash 粒度修复被否定，转而清理 config 替换、KV grouping 特判和其他排障弯路。
  title: 回退 LCM 与定位 KV block 污染
- block_id: 9a15a874d7400f0d798cd7972134cc2801d3c4486722e4244194f40ac08ca7ae
  indexed: true
  sha256: e6d30de4a195545139b8adc16dbf8f8d5687bbe7b58d45cebc3684d2637f89f8
  source_range:
    end: 334169226
    part: 1
    start: 320741584
  summary: 将 draft 关闭 MLAPO 收敛为使用正式 `runner_type="draft"`，删除 draft `model_config` 替换、cache 防御复制、128/384 KV 特判及过时测试，并完成 rebase、提交历史清理和定向测试；153 上真实依赖的 82 项测试通过。单机 TP16 使用完整 896 专家权重在模型实例化阶段因约 60.48 GiB OOM，改用 16 专家裁剪权重后服务 ready、HTTP 请求和并发烟测通过，但裁剪 target 与完整 draft 的低采信率不具备精度结论。
  title: 清理算子 PR 接入与单机容量限制
- block_id: b1bb5cb167f6ca1a4080315b3bdf89969c82c10d43fa7fcec8fd19a8be379682
  indexed: true
  sha256: 64dbc584c8b72dfc61c8973e96f4807638679b66155e96f236b7991578ff3262
  source_range:
    end: 334169226
    part: 2
    start: 320741584
  summary: 16 专家单机验证确认 KV 分组为 24 个 target 加 3 个 draft 共用 attention group、Mamba 为三组，48/96 两档图完成且输出链路可用；四机 EP64 随后成功加载完整 QuaRot 权重并 ready，显存和 activation 均在预期范围。完整权重短请求及并发测试的 DSpark 采信率仅约 1.27%–1.5%，后续发现旧进程/源码挂载和启动环境曾导致结果不代表 clean head，并定位到 `VLLM_PLUGINS=ascend` 错误过滤 general plugins；移除该白名单后 clean head 单机重新 ready，裁剪权重仍仅作为功能烟测，四机完整权重验证在材料结尾仍进行中。
  title: 完整四机启动与采信率问题追踪
- block_id: c133a0b707a7189b4f86fa6b068a45500e6c3122a4f7a9ffa12ec20c784a2960
  indexed: true
  sha256: e17e62f080f833677212ad239aa70d3f4d98478b3142806cdb8e4fe4e1279c48
  source_range:
    end: 334169226
    part: 3
    start: 320741584
  summary: '定位并修复 ModelSlim 的 DSpark `WeightsMapper("" -> "model.")` 错将 `optional` 映射为 `model.optional` 的问题；这导致 draft 无法读取 `optional.quarot`，错误共享目标模型的旋转后 embedding/lm_head。修复后单机裁剪权重采信率由约 1% 提升至 46.0%，四机完整权重由 2.5% 提升至 33.2%/37.5%，相关 UT 通过并更新 PR #14454；但 GQA 修复后尚未重新用权重做实机 smoke。随后在 0.26 分支验证 PR #14768：GQA 固定 8K/1K 为 884/994=88.93%，接近历史 89.67%；新 MLA Block5 已完成 QuaRot 加载路径，但验证期间还遇到 JSON 转义、Gloo 网卡、运行库和图档位等环境问题，尚未完成最终采信率结论。'
  title: QuaRot 元数据映射修复及 GQA/MLA 实测
- block_id: ebd9ca7ff5c69c8b94833a78baf16f7fb932452e004ecd42acf92de11ac844f4
  indexed: true
  sha256: 7b7f8eecdbd3f57d27a181d86c71b4de57f65d752066187b456bcbd9eb0e90b2
  source_range:
    end: 334169226
    part: 4
    start: 320741584
  summary: 'PR #14768 已在最新 `releases/v0.26.0rc` 范围内核对，现有 0.26 MLA/K3DSpark 路径和相关 83 项测试通过；GQA 实机固定 8K/1K 结果为 884/994=88.93%、AL 7.225，与历史基线基本同水平。新 MLA Block5 已识别为 `K3DSparkModel`，并确认 `context_proj` 反旋转、draft-owned embed/lm_head 和 raw MLA stream 均进入正确路径，但尚未完成请求采信率验证。启动过程中确认了 Gloo 网卡、native-compat 库和图尺寸的环境约束；Block5 的 5-token block 加当前 token 要求 `[48,96]` 图档位，最后一次验证仍在按该档位重拉，不能视为成功。'
  title: 'PR #14768 的 0.26 GQA/新 MLA 兼容性验证'
- block_id: dfa7f7552fd119fc0824ffa5323075e54d9bc4d9839fc1793f5500d874331cdd
  indexed: true
  sha256: a08b24c8b7aca6335c921bc150b1ea476d27b330c35e73e8df69649f048fbbb6
  source_range:
    end: 347586150
    part: 0
    start: 334169226
  summary: PR14768 先在单机 TP16、16 专家裁剪权重上完成启动与请求检查，但该结果不能作为精度结论；随后确认需使用 153–156 四机完整专家权重。MLA Block5 的关键最小适配是读取 checkpoint 的 `dflash_config.causal`，去除无必要的 `block_size` 运行时限制；SP 开启、共享专家 TP 开启且共享专家多流关闭时，单机路径曾成功捕获 `[48,96]` 图并消除 MC2 满核卡死，但四机 MLA 最终精度尚未完成验证。
  title: PR14768 四机验证纠偏与 MLA/GQA 适配
- block_id: 7725aff1a4b2755188ad706b8d20f8a5ddbf90b452ce89e8bafb1843c09efc8e
  indexed: true
  sha256: a5de05d5ebee6eee6df24528d52ff59bf6f9927d62e65feb28d84b3b678b5cb8
  source_range:
    end: 347586150
    part: 1
    start: 334169226
  summary: 四机 DP4×TP16×EP64 的完整专家 GQA 服务已启动并完成 8K/1K 请求；早期单 DP 结果 AL 3.018 不足以判定退化。严格复用 0.27 的 prompt hash、greedy、7 draft token、四 DP 并发和冷缓存口径后，0.26 首轮 AL 4.0714、采信率 43.88%，但第二轮恢复至 AL 6.3190、采信率 75.99%，接近 0.27 稳态 6.37–6.45；首轮冷请求存在 DP 间分化，第三轮未完成。按用户要求已停止四机服务并确认 153–156 无 NPU 运行进程。
  title: PR14768 四机 GQA 复现与机器释放
- block_id: 33ef75f298b8cc541ca53cb3a62008c3561c616f3c5b54480be7f9c0ca0f814a
  indexed: true
  sha256: 2834f0c4e486734eda1abebb86cc300da9917b750c1c61cd69095b02c57192cc
  source_range:
    end: 347586150
    part: 2
    start: 334169226
  summary: PR14454 先修复 ModelSlim Ruff 格式问题及 310P `CATLASS_UNIFIED_CORE` 路径误用 `Fixpipe<float,float>` 的编译失败，提交 `9e37b8963` 后三平台 csrc 构建通过。后续 CPU、MLA、量化和配置兼容性失败被逐项修复，涉及显存计量、Mamba block-table 宽度、DSpark 权重版本、CompilationConfig、`custom_ops`、g_proj、FA-quant 和缺省 `use_mla_rope`，最新已知提交为 `8b9a0f3ee`。截至最后一次心跳，PR 可合并但新 head 尚未生成新的完整 E2E run；旧 head 的失败已知为已修复问题，不能宣称全部 CI 通过，仍需继续监控。
  title: PR14454 CI 修复与持续监控
- block_id: 7d050818337ca902fd6260665c5724edaee5b27c5b1afcfe2bc88ca850fc06ed
  indexed: true
  sha256: a3aa5f6e3723dd626eb3788aba44ad8b7c560b940a72f6bd0aa527d08ff97678
  source_range:
    end: 347586150
    part: 3
    start: 334169226
  summary: 'PR #14454 先因当前 head 未触发 E2E 而通过刷新提交重新排队；随后 A3-800I 日志据报暴露 DeepSeek V4 indexer 复用 MLA 类型但缺少对应 impl，以及 SFA 被过宽判断为使用 MLA RoPE，相关修复提交曾通过 ruff、py_compile 和 pre-commit。后续讨论认为 identity cos/sin 只是占位而非 no-RoPE 的正确实现，已据报改为 no-RoPE 使用 cos/sin=None、删除 identity getter、混组断言及其 UT；这些结果主要是对话中的报告，CI 当时仍在运行，不能视为最终验证成功。'
  title: PR14454：重推后发现 MLA 类型判断与 no-RoPE 占位逻辑问题
- block_id: dbf8532896c9bda48786debf0b95be403347bffd5de991fd2d05feda87a5ed96
  indexed: true
  sha256: 0d901d00735eb51d429ae972572751c8e923d603cf41b65f96984f01fb6ed73f
  source_range:
    end: 347586150
    part: 4
    start: 334169226
  summary: 讨论确认 K3 no-RoPE 应保留 use_mla_rope 和 target/draft 分组，但不应依赖 identity cos/sin；随后反馈指出 UniformTypeKVCacheSpecs 在分组后丢失 MambaSpec.num_speculative_blocks，导致 133120/384 从应有 354 列变成 347 列，接近满载时可能出现 shape (2,)→shape (0,)。据报提交 88398526b 改为按能力检测补充 inner-spec 委托，并将测试改为 354 列边界，同时审查 v1、v2、310P、scheduler、worker、proposer 和 connector；本地静态/pre-commit 检查据报通过，但运行态 CI 尚未完成。
  title: PR14454：清理 identity RoPE 后修复 Mamba speculative block 容量
- block_id: b50514eaad14ed5c55b469791076b12c305abd3af87026189bf7749a4b59b8e7
  indexed: true
  sha256: f4cd3474892db08651ceaef55c6b6560541e85bea9d5433b5447a6aef662ee2c
  source_range:
    end: 347586150
    part: 5
    start: 334169226
  summary: '截至 2026-08-27 03:21，PR #14454 最新已知 head 为 88398526b，报告称无合并冲突，pre-commit、DCO、文档检查及部分 CPU/310P 检查通过。NPU 测试仍在运行或排队，当前没有报告代码失败，也没有进行新的代码修改；因此容量修复和整体 PR 仍缺少完整 E2E 结果。'
  title: PR14454：容量修复后的 CI 持续运行
- block_id: 6b728191b012d6f38bb7bfbd5e0defc32696708a0aca3b1de5a88fdc7a023431
  indexed: true
  sha256: 296da08ad412782ec0144723612770ac67f0485efe3db5061b724057e1b52cbd
  source_range:
    end: 361906433
    part: 0
    start: 347586150
  summary: 'PR14454 的 CI 曾因 uc-manager 拉取 spdlog 的网络问题失败，重跑后 42 项成功、6 项跳过，但 GitHub 合入状态一度仍为 BLOCKED；随后移除了 kimi-k3-execution-parity nightly 用例。手动触发的 nightly 尚未完成，但日志已显示 vLLM 0.27.1 的 BF16 MoE forward 临时创建 SituAndMul 时触发 `AssertionError: Current vLLM config is not set`，尚未执行缓存命中或输出精度断言。后续分析确认上游路由 MoE 通常直接调用底层 SiTU 计算，而普通 MLP/共享专家复用初始化实例；当前纯张量修复的 UT 通过，但未完成整模 NPU 验证。'
  title: PR14454 CI 收敛及 nightly SiTU 根因定位
- block_id: dabb9edc44c97e86eaecbb62a6fea2d7df5e7931a7ced939967cdf5bd851480d
  indexed: true
  sha256: 370a16ab2a204d81d0cca948db02e39715b8fcabef24009480464cc806f4ffcf
  source_range:
    end: 361906433
    part: 1
    start: 347586150
  summary: 上游 vLLM 0.27.1 的 `SituAndMul` 已注册为 CustomOp，但路由 MoE 的 `if SITU` 直接调用底层算子，不创建实例；因此 Ascend 侧应保持函数式分发，普通 MLP 才复用已有激活实例。进一步分析发现，PR 中以 `computed >= prompt_len` 作为入图条件会拒绝正常 PD handoff 的 `N-1` 状态，使一个 DP rank 的 NONE 扩散到整个 DP/EP group；CPU 对照复现了 0.26 与当前实现的差异，但现场具体触发请求仍未确认。随后修复了入图判定、accepted-token 快照重排读取错误和 DCP 重复扩容，并报告父 PR/子 PR 定向测试通过；这些结果不等于实机 PD 精度或性能验证。
  title: SiTU 调用方式与 K3 graph eligibility 回归
- block_id: b6458b7b8377e17b22c7606d4ba7601ce18dc4cd4696576f67dc5b08544d5c3c
  indexed: true
  sha256: 4875f0c7f2b8783ccc2d525f22d3bff757e3c0ddba85b0c5798d90d23de4563f
  source_range:
    end: 361906433
    part: 2
    start: 347586150
  summary: '历史对照显示，prompt 完成才能入图的限制曾在 0.23 被回退，后于 0.26 实验提交及 hybrid KDA graph-state 整理中重新带回；#12255 的等价修复尚未进入官方 main，但已由当前父 PR/KDA 子 PR 携带。父 PR #14454 与运行时子 PR 已 rebase 并推送，基线 head 为 `5e327cb7f6afe8dc81aefdfc706ec7400d7b3af5`，当时无冲突但多项检查仍排队。随后 E2E pre-commit 暴露了明确代码类型错误：`moe_mlp.py:725` 将 `float | None` 的 beta 传给要求 `float` 的 `_apply_situ`；这是需要修复的 mypy 问题，不能归因于 runner 的通用提示，后续修复/推送结果在材料中尚未给出。'
  title: 框架历史修复、rebase 与新 head 的 mypy 失败
- block_id: 20dc3be4ace919c78f19807f5ffdd3014eb5400bc34b04264110dec345e42003
  indexed: true
  sha256: de5b8846ee5a38a7517a92f3af609a01c6d4a28450804bab8728ebc231c3564e
  source_range:
    end: 361906433
    part: 3
    start: 347586150
  summary: 'PR14454 曾出现测试隔离导致的 MagicMock 日志路径 `FileExistsError`，修复 UT 后通过 67 项组合回归；DSpark DeepSeek-V4 acceptance 曾因第二位置 70.09% 低于 71% 通过线失败，但同一 head 的 attempt=2 复跑成功，未据此确认代码根因。随后确认 #14878 引入的 `cos/sin=None` 切片问题并以四处最小修改修复，相关 CPU 测试通过；又确认 A5 缺少 `FusedRMSNormGated` Ascend 注册，已接通现有融合 kernel，A3 数值测试通过，但 A5 性能收益尚未实测，KDA 子 PR 曾有冲突待处理。'
  title: PR14454 多轮 CI、DSpark 失败复跑及 MLA/A5 修复记录
- block_id: 5ab5c50afc492b5a865b294ce5e2669f4d5dc533470cb77b022a4a3a0d92b668
  indexed: true
  sha256: 7cca6e5eea68480ecdc02f5924fcc0489b4a31414a68367ca5e6562be7e1ab7b
  source_range:
    end: 361906433
    part: 4
    start: 347586150
  summary: 用户要求先设计、暂不写代码或提交：在一个非 nightly 的 A2 普通 PR E2E 测试文件中覆盖 MLA 老版、MLABlock5 和 GQA 的 DSpark 变体。计划构造减层、减专家的权重并验证服务可启动及推理 2 个 token 不报错，层数和专家数可缩减至 16 或更小；随后用户提出可使用 dummy、避免真实加载权重以降低测试成本。该设计仍待用户评估确认，尚未实现或验证。
  title: A2 普通 PR E2E 用例设计待评估
- block_id: c402ec5bc0b10b0b8da9ae3ba18ca3466b2ff3849d860d8793718b5b9a389b5e
  indexed: true
  sha256: c4afc963e07627580066ba09d0136e617955b754c7719301b786965d123d5b7b
  source_range:
    end: 378692514
    part: 0
    start: 361906433
  summary: 已按 A3 单机 16 卡、dummy 减层减专家方案实现并验证多组 DSpark 功能；旧 MLA、GQA＋W4A8、Block5、DP2/TP8、MTP 图片和 P8/D8 均曾暴露或解决具体问题，不能把中间失败当作最终成功。DP2/TP8 的 MC2 `x_active_mask` 行数错误参考旧 FlashComm 的 DP 补齐语义修复并通过定向检查；P/D 退出时的 `corrupted size vs. prev_size` 可脱离 vLLM/K3 在 `TransferEngine` 导入中复现，未纳入 vllm-ascend 绕行修复。最终六组回归据报道全部通过并提交 `8c8deeddd`，但后续矩阵压缩和 CI 结果另有更新。
  title: A3 TP16 DSpark 六组实现与阻塞定位
- block_id: 9de5643ee75f2a9016610489fbaeae74b2e252c37a08abe558218b497c4f379a
  indexed: true
  sha256: a338d187e1edcbcdb009d76465aa0ef1c3de9e2b60b397941db9dd768e2ad9dc
  source_range:
    end: 378692514
    part: 1
    start: 361906433
  summary: GQA＋W4A8 TP16、Block5、DP2/TP8、MTP 文本图片及 P8/D8 的阻塞分别涉及量化描述、MC2 mask 补齐、共享 `lm_head`/多模态接口和 Ascend decoder tensor 返回接口；修复后相关功能据报道通过。P/D 的 Mooncake allocator 堆损坏仍被确认是外部 `mooncake-transfer-engine-npu==0.3.11.post1` 可独立复现的问题，未修改 VA 代码。六组被压成四组后实测 5 分 57 秒，仍未低于 5 分钟；随后清理 30 个 UT 文件中的低价值重复检查，保留数值、KV 容量、旋转权重和 P/D 状态覆盖，提交 `ba14b852d`。
  title: DSpark 阻塞修复、四组压缩与提交
- block_id: 203a0f9a3053cf655579c0d3affc95797b8f9087c4a5447a341df73c428f97a9
  indexed: true
  sha256: be5c29d6c3eff70b86fc1d077d404780be831815c821936ca23396769273e8aa
  source_range:
    end: 378692514
    part: 2
    start: 361906433
  summary: '四组压缩回归全部通过但耗时为 5 分 57 秒；CPU UT 的裸 `__new__` fixture 因缺少 `supports_mm_inputs` 失败，补 `False` 后定向文件 23 项通过。进一步审视 30 个 UT 文件，删除源码字符串、重复构造/参数转发和冗余 mock，净减少 1162 行，定向验证为 490 passed、5 skipped，并提交 `ba14b852d`。该 head 的 EAGLE A2 首位 acceptance 仅 0.3294 对 golden 0.43，attempt 1 失败但 attempt 2 成功且未复现；PR #14454 随后已合并，子 PR 已关闭，父 PR 描述已移除子 PR 矩阵。'
  title: UT 清理、EAGLE 临界复跑与 PR 合并
- block_id: 49e22e92d00bfc9388098d05dff00d70a12132f8fe96a47a1991474fdb9b6e02
  indexed: true
  sha256: e68cee540b08e0cc29fa5e8b574bc870540c4745eed15a89f6484a9d551f2fec
  source_range:
    end: 378692514
    part: 3
    start: 361906433
  summary: '围绕 PR #14454 合入版本在 153/154 双机上的 GQA 采信率复现，确认历史 8K 的 58.17% 仅是 DP0，四 DP 汇总为 31.18%；同脚本当前 DP0/DP1 为 34.68%/59.37%，复跑还出现 DSpark hidden states 申请 552 MiB 的 OOM及后续 Gloo 断连。32K/1K 单并发在保持 `max_num_batched_tokens=8192`、prefix 命中 93.75% 的条件下，总采信率为 73.90%，低于历史 80.41%，某输入交叉复测降至 17.25%，但尚未证明根因。代码差异重点收窄到 KDA prefill、融合 RMSNorm gate、Mamba accepted-count 快照/状态回写；QuaRot modeling 加载、MLAPO、概率采样等未被确认是本次路径原因，后续计划整理四机复现数据与交接包。'
  title: 双机 GQA 采信率异常与 32K 差异定位
- block_id: f4790846396726f10437db431970bd8792372107b6e6bcaca6b1cb50b04dd8cb
  indexed: true
  sha256: 47f2d1999e3644472570edb9e20e397c44a4da53601469b3a9400122d7f68358
  source_range:
    end: 378692514
    part: 4
    start: 361906433
  summary: 已整理四机非旋转/旋转权重扫描、2K–256K 临界点复测、0.26/0.27 GQA 复验及 43 组 token 输入；多轮冷缓存 8K 合计采信率为 66.71%、76.70%、77.79%，部分标为冷启动的记录实际 prefix counter 已显示 93.75% 命中。新集群发现异常仅在大于 96K 且命中 prefix cache 时复现，初步对比未见最终合入后 K3 KV 分组、MRV1 布局或 DSpark 文件再次变化，但历史节点到最终版改过 Mamba 状态写回、block-table 容量，vLLM 版本切换也可能改变缓存管理。基线已修正为最后一次采信率正常的 GPQA 128K 记录；已知 8 月 23 日 GPQA 仅有 17.49% 中途采信率，不能把 179/193 的答题正确数当作采信率正常，真正基线仍待找回。
  title: 四机交接资料与长序列 prefix cache 调查
- block_id: 10574f0768149bbc4c86838f0ccdcd2d4c718baf4b4084b27b49b3dcb89d6227
  indexed: true
  sha256: 245220a713a0f14ee7cc98abfe54ed4123b9b626f9c03444831bb7ae48dfaf52
  source_range:
    end: 392527998
    part: 0
    start: 378692514
  summary: 围绕 vLLM `cdc4824a` 与 `v0.27.1` 的四机 128K/1K、GQA DSpark 对照展开，固定完整旋转量化权重、每 DP 单并发、缓存预热和单档图捕获。启动期间出现 `libvllm_ascend_kernels.so` 缺失、`_C_ascend.moe_gating_top_k` 未注册、Qwen3.5 辅助函数缺失以及 155 节点显存被外部服务占用等失败尝试，均被保留且未计入结果；后来通过隔离目录、清理进程和有限兼容处理重新启动。该阶段尚未产生 vLLM 对照采信率，仍处于加载、通信和图捕获过程。
  title: 四机 vLLM 版本对照启动与兼容问题
- block_id: d359c4569ec93ba3e7b6388339d7cdf151dbe59157583b931a4086697b150847
  indexed: true
  sha256: edc8a9c0c85387fa137ee8c6ea4999daa231793d76730b6df3304968a619d70b
  source_range:
    end: 392527998
    part: 1
    start: 378692514
  summary: '`v0.27.1` 两轮总采信率为 55.12% 和 50.75%，缓存命中率均为 94.63%；逐 DP 低值会在不同轮次迁移，不能归因于固定 DP。`cdc4824a` 经历 `confidence_head=None`、`num_prefill_lookahead`、`calculate_kv_scales` 和 Mamba `TEMPORAL_TILES` 等接口兼容失败及隔离适配后，两轮采信率为 86.25% 和 62.65%，加权结果 72.86%，平均接收长度 6.10；`v0.27.1` 加权结果为 52.87%、平均接收长度 4.70。两版缓存命中率一致，但旧版第二轮 DP2 仅 31.91%，因此不能认定版本升级是唯一原因，也未证明物理 KV 损坏。'
  title: vLLM 两版本四机采信率实测
- block_id: 34ac6f3d71bc1c7ee58757d953c4d434e13601354bb8479d3ced1d20b70b18c6
  indexed: true
  sha256: a3360c6811230a2031ed00e3206ba63ad6afe81519ba6e4c9892c726aa42a8a8
  source_range:
    end: 392527998
    part: 2
    start: 378692514
  summary: 四机恢复了 `3053909aa + cdc4824a` 的历史源码、启动参数和旧算子，但发现版本选择残留、155/156 缺少 `aclnnDequantSituQuant`、遗留 `_C` 扩展触发 `aclnnRecurrentKdaGetWorkspaceSize` 段错误等环境问题；重编绑定和恢复 vendor 后才继续。完整历史栈五轮 128K/1K 请求全部返回，但采信率分别为 39.28%、71.79%、47.70%、51.69% 和加权 50.67%，因此该历史版本也不是稳定正常基线。材料先误称历史版本“不带 fused KDA prefill”，随后更正为：历史版本仍带旧版 AscendC `chunk_kda_fwd` prefill，后续 `6c039a1b6` 才引入更融合的 gate、状态布局和 prefill 路径；最新 main + `v0.27.1` 的回退组已准备但尚未产生结果。
  title: 历史 3053909aa 复现及 fused KDA 回退实验
- block_id: 6de59280b413c83b964be9ed4f72dbdf9f03d331e9fddf1fa65afeedc87f120f
  indexed: true
  sha256: dcb146c30a49bf43e1f4869a155b8b1358f31f906d310d09f5d5b5b8063a0f12
  source_range:
    end: 392527998
    part: 3
    start: 378692514
  summary: 诊断请求显示四个 DP 命中 124,032 token，69 层 KDA 恢复的初始状态与预热写入状态逐元素一致，且缓存命中率异常不能单独解释全部低采信率：同一用例冷缓存下也出现约 30% 的低值。首层真实张量中 Q/K/V、beta、初始状态和累计 gate 一致，但 `raw_gate` 最大差异为 0.0078125，来源被追到 BF16 `f_a` 投影在不同批次形状下的舍入差异；替换 replay 中的 gate 可使状态和输出逐位对齐，但尚未证明足以解释全部退化。进一步的新旧 KDA 对比定位到第一处中间差异在块内相关矩阵 `aqk/akk`，随后传播到 `w/u/v_new/h/final_state`；当前待用 FP32/FP64 参考公式判断新旧实现哪一侧偏离，并关联长前缀采信率。
  title: 正向数据流定位：缓存状态一致，KDA 数值差异出现于 aqk/akk
- block_id: 8184b6d150694a611600a495898d9ce54a5bad93d67fccaf8991ef7261f9027a
  indexed: true
  sha256: 5dd358d0fa890748fffcf3fc7fef9fe3f18174087ad2367baaade702faf1fb3c
  source_range:
    end: 407033261
    part: 0
    start: 392527998
  summary: 在 `0a023b094`/vLLM `v0.27.1` 上启动 TP16×DP2、EP32 的32专家双机验证时，先后遇到 custom-op 包选择错误和 `libopapi.so` 搜索路径问题，修正后 profile 才通过。历史问题被重新关联到 DP idle dummy 调用 `_dummy_run(1)` 推进 GDN/KDA recurrent state；14454 保留了 builder 的零长度处理，却漏掉了 ModelRunner/Worker 的调用链。修复 `skip_gdn_state_update` 后，单 DP 128K cold/warm 追加3轮均输出一致、采信率约96.13%–97.49%，但首轮曾有 cold/warm 开头偏移，因此仍保留不确定性；随后启动四机完整权重验证。
  title: 双机32专家复现与 idle dummy 状态污染线索
- block_id: 1ddc6e0a71c7ac443962ad69edb57ca02e6370fce6f224a3234136925ddc1479
  indexed: true
  sha256: 76437dfe372889240cc784d27d46ca80c4acbf4c0b54433019925d8e429c0c31
  source_range:
    end: 407033261
    part: 1
    start: 392527998
  summary: '双机修复版中，修复前 DP1 的128K warm 请求曾从首 token 变为重复 `1.1.1...`，reset 后恢复；修复后连续多轮 cold/warm 1024 token 一致，支持“idle dummy 推进 GDN/KDA 状态”这一候选根因，但不是形式化证明。修复按0.26提交 `3cd7136db` 恢复显式 idle 语义，保留 dummy token 参与通信，不增加全卡同步；同时确认 builder 无法仅凭形状或 block id 区分真实1-token decode与 idle dummy。四机 TP16×DP4/EP64 验证已启动，PR #15253 已创建并包含该最小修复。'
  title: idle-dummy 修复的双机 A/B 与四机验证启动
- block_id: 57076afea31d66385043f3934e9151a5e21530fbb085aaa9ff782bc5aef47c40
  indexed: true
  sha256: 37c308500ba0f53b0bb9afaa25d10374b00f13293b5055ee4f33aa8eb47bf357
  source_range:
    end: 407033261
    part: 2
    start: 392527998
  summary: '四机完整权重服务成功 ready，前两轮历史128K脚本的聚合采信率为85.40%和87.00%，各 DP 未再出现旧的27%–46%塌陷；第3轮中155的 EngineCore 被外部 Docker exec 以137/SIGKILL终止，故四机验证未完成，不能将该轮视为功能失败或成功。PR #15253 后续 CI 唯一失败被定位为 Mooncake 端口39783占用，已触发 failed-only rerun，但 runner 长时间排队。之后开始参考 #14985 支持 DSpark 入图，固定叠加 #15253，保留 N→(1+N) 图桶、capture metadata 和动态 context-KV 出图逻辑；154 容器定向 UT 为55/55通过，但真实 GQA 启动曾因 `PYTHONPATH` 覆盖丢失 ACL 路径，随后又受153–156 SSH 握手前被远端关闭阻塞，GQA/MLA-Block5 实机验证尚未完成。'
  title: 四机采信率初步结果、外部终止与 DSpark 入图准备
- block_id: f2f303ba13fa1d7ca3f11323b3fe1ebfac1e2d9a403c409b81ef93eb99395ea0
  indexed: true
  sha256: 2a9a86f64246fe37d3c4cf36c64060aa66a4312f889d00d806757b88722fac9d
  source_range:
    end: 407033261
    part: 3
    start: 392527998
  summary: '单机 GQA 与 MLA-Block5 曾暴露权重描述符不匹配、ChunkedPrefill 元数据、KeyError: 40 以及 block-table 行数不足等问题，随后通过旋转量化权重、SpecDecoding 标记、固定 48 档 attention tensor 和 max_batch_size buffer 修正。单机两种模型均完成 ACLGraph capture/replay 与真实请求；四机 GQA 使用 DP4/TP16/EP64，修复了 PATH 缺少 ninja、源码目录遮蔽标准库 platform、DP1–3 必须 headless 等启动问题，并完成权重加载、draft graph capture 和 API 启动。运行进程实证加载了 vllm_ascend_C、libcust_opapi.so、CANN 9.1 libascendcl.so 及相关 ACL/HCCL 环境变量，但该块末尾仅记录开始服务端 replay 核验，不能单独据此确认四机 GQA 的最终采信率。'
  title: DSpark 单机与四机 GQA 入图验证
- block_id: 85ac7547c8f215c8746b290cd395c9b7363a0bb984318cdaf2a82e4d4cd02818
  indexed: true
  sha256: 955fa687c2872d52580585329f7a9b038791ee208959fbebe6b253c570fa5301
  source_range:
    end: 407033261
    part: 4
    start: 392527998
  summary: '后续材料声称四机 GQA 与 MLA-Block5 均完成 4 个 DP 的 ACLGraph replay，8 次 9.6K 输入正常返回、prefix-cache hit 约 42%，并报告 51 个定向测试通过及 Ruff、格式检查和 diff 检查通过；这些是来源中的报告结果，应保留其验证口径和不确定性。随后创建了 vllm-project/vllm-ascend#15334，并开始按 #14454 评论区的 external-8k-c2、8K+1K、每 DP 2 并发等历史参数做采信率对齐；CI 曾因标题 `[Feat]` 不符合仓库规则失败，改为 `[Feature]`，不能将其误认为代码失败。最新状态是四个独立 API 已用当前 PR 源码启动并进入完整权重加载/profile/capture，历史口径的重放与对齐结果尚未给出。'
  title: 四机验证完成与 PR 对齐测试启动
- block_id: a4b22d40c047744bebd78da3f2ad323a02df91db6de8cad2de0575141d71fd93
  indexed: true
  sha256: 17390105adef931a590f0be7bbdd58dae4ae73dfaf694be2f361c4f789ce489e
  source_range:
    end: 423811224
    part: 0
    start: 407033261
  summary: '四机环境曾因 `nsenter` 不继承 `LD_LIBRARY_PATH`、CANN `set -u` 和 `libop_plugin_atb.so` 缺失而失败，改用容器原生环境后完成 64-rank HCCL、权重加载和图捕获。GQA DSpark 在 DP 不均衡或图档位切换时出现 `index 98..111 exceeds bounds 98`，排查指向真实 token 数、padding token、query slot 与 graph bucket 语义混用；单机固定 14 请求未必触发，问题更可能与多 DP 局部 batch 切换有关。后续请求改为使用 GQA DSpark + QuaRot 旋转量化权重，并按 #14454 的 30ms mean TPOT 口径补跑性能，但本块末尾仍处于环境和复现准备阶段。'
  title: GQA DSpark 图边界越界与四机重跑准备
- block_id: 79c656e80f0a99cb77dba54b10fd9003d29de0633de564f296c8730aa1117a22
  indexed: true
  sha256: f3cb2bf031f2a9a7edbbf53763097b4d59f638bfa95d0b3b00a0449de355c093
  source_range:
    end: 423811224
    part: 1
    start: 407033261
  summary: '对比显示 K3 GQA 与 GLM GQA DSpark 都采用 anchor-first、7 draft token；GLM 现有用例因 `enforce_eager=True` 未覆盖 draft ACLGraph padding 路径，因此共性问题尚未被其通过结果证实。诊断定位为选图应按 `batch×(7+1)`、KV/attention 的 `num_actual_tokens` 仍按真实 `batch×7`，修复后 32 请求转换连续三轮 32/32 成功，相关 UT 报告为 49 passed，但这些是来源中的阶段性验证。随后准备按 #15334 口径跑四机矩阵时，因新源码目录未链接含 `dequant_situ_quant` 的 `vllm_ascend_C` 扩展而在 profile 阶段失败，需修正运行时算子产物后再测试。'
  title: DSpark padding 契约修复与四机性能验证准备
- block_id: eb27e7e50826ef95d98b77cf9e27534a4d7c5bd0e76a72af2b2129604c54fde3
  indexed: true
  sha256: ae34c628b4f18b4da088f41be0b9d8e28476ca7d62288c0c2a6d0dc1879cd000
  source_range:
    end: 423811224
    part: 2
    start: 407033261
  summary: '来源最终声称四机 QuaRot GQA DSpark 矩阵完成：2K–256K、固定 1K 输出、95% 公共前缀、30ms 平均 TPOT 门槛，共 29 个波次和 464 个请求；报告的最大每-DP并发为 2K/8、4K/4、8K/8、16K/4、32K/2、64K/2、128K/2、256K/1，并称已贴入 #15334，但完整指标仍应视为待复核的来源数据。新的任务要求以 #15334 和 #15253 为基线完善 #15336，只保留其 csrc tiling 下沉能力，丢弃非 csrc Python 实现，正规替换 GQA/MLA 的 FIA，并先完成单算子数值对齐和单机验证后再重跑性能。当前已在隔离目录接通设备侧 metadata/AICPU `actual_seq_lengths` 路径，尚未报告单算子或单机验证通过。'
  title: '#15334 四机性能结果与 #15336 正规接入启动'
- block_id: 38c89fbeee940c5923ac2cb7f625768b8533fa41bea208f51367b80259308e03
  indexed: true
  sha256: 0052b03a103c5548d8d87ae9ca31491dd92e5cc70f9713657be7be2875195694
  source_range:
    end: 423811224
    part: 3
    start: 407033261
  summary: 首轮构建受 macOS AppleDouble `._*` 元数据误读、binding 产物复制路径错误及隔离环境缺少既有 `dequant_situ_quant` 算子影响；清理元数据、修正路径并复用完整 custom-op 构建后，两个 FIA 算子成功编译注册，GQA/MLA × FP16/BF16 四组数值对齐和旧算子交叉用例通过。#15253 所需的 Mamba speculative block 容量修复已在当前代码中，`133120 / 384 + 7` 实测为 354 列，因此未重复加补丁；若服务仍出现 347 列，仍需核查实际加载版本或其他 sizing/handoff 路径。GQA 与 MLA Block5 均完成 16 卡单机启动、profile/graph capture 和真实 128-token 请求，报告的采信率分别为 87.3% 与 85.6%，但首条空 JSON 请求产生的 HTTP 400 错误必须与模型/算子错误区分；四机首轮曾因 154 缺少 `set_env.bash` 中止，补齐并通过四节点算子可见性预检后已重新进入组网和权重加载，四机性能结果尚未完成。
  title: FIA 算子下沉：编译、注册与 GQA/MLA 单机验证
- block_id: 7dc46c386be89fa098aa5370a7146413d3c01b27ebad780d03ef4a88f632014b
  indexed: true
  sha256: d39cb7246544cf91783632fa09cd3f9a29918eb81485e5a6ee80b971759876bd
  source_range:
    end: 440607495
    part: 0
    start: 423811224
  summary: 四机启动环境、旧 vendor 路径、HCCL 初始化和算子符号问题均曾导致失败，随后修正并完成完整权重、8 档图捕获及真实请求验证。按用户建议，metadata AICPU 已从图内移到 builder，由 builder 持有固定 `int32[1024]` 输出 buffer；GQA/MLA 单算子和图回放曾 7/7 通过。额外引入固定 `int64` length buffer 后触发 507018/AICPU 22007，已回退该额外改动，最终服务重验证当时仍在进行，不能据此宣称最终四机结果。
  title: DSA式 metadata builder 集成与回退
- block_id: 5ca443110bb6c263415d582cd4d50344ec9acd3b6a2114a744eb9839eb7962ad
  indexed: true
  sha256: 39a79106f12c1c5760449a705100759f9943d69c05beb7de632f4ff4d0d858c3
  source_range:
    end: 440607495
    part: 1
    start: 423811224
  summary: '对照 DSA v1 的 SAS/QLI metadata 后确认 FIA metadata 输出契约为 `int32[1024]`，而长度输入 ABI 为 `int64`；固定输出 buffer 必须保留，长度输入需遵守 call-local 生命周期，不能靠 dtype 转换偶然产生新存储。最终单机 GQA/MLA、16 卡图回放和真实请求通过，并已补充 #15334 四节点脱敏启动命令、更新 #15336；四机同口径性能 sweep 首轮在 154 的 2K 图回放触发 FIA sink `MTE accesses an invalid GM address`，因此首轮没有可用性能表。随后按请求数独立缓存稳定的 q/kv length buffer 修复跨档位问题，之后性能数据才显示明确收益，但仍需保留各档位退化与 CI gate 标签限制。'
  title: ABI 生命周期核对与四机性能启动
- block_id: 758dc5e5b723c7f235e78e2fec420392ee049198f45376d3d1b4205d8e29e675
  indexed: true
  sha256: 871bf4ca69efb9d67399eb1a326b1d5298f18759976adcaf7668b28768125ef0
  source_range:
    end: 440607495
    part: 2
    start: 423811224
  summary: 固定 `int64` 版本先在第二个图档位因 MLA metadata 的 batch=16 与 block_table batch=14 不匹配而失败，进一步定位为 ACLNN 静态 descriptor 复用同一 backing storage；按请求数使用独立且地址稳定的 buffer 后，单算子 8/8、单机 112/128 图、四机 8 档 capture/replay 和 624/624 请求通过。四机 2K–256K sweep 报告 4K、16K、32K 高并发 TPOT/ITL 与容量收益，同时保留 64K/c4 和长序列未过 30ms 的退化或未通过情况；#15336 已更新并贴出 MLA/GQA 退化输入，ci-gate 仍因缺少 `ready-precise` 标签失败。最新任务转为审计 tiling 下沉后 K3 的 `seqlen`/`num_accepted_tokens` 消费链，判断 model runner 的 D2H 是否可删；当前仅确认需覆盖 scheduler、图档位、KV/prefix-cache 和 speculative decode，尚未形成删除结论。
  title: 跨图档位 descriptor 修复、性能收益与后续 D2H 审计
- block_id: 61bc34dc4764b64864062985ab55dcaf805d37eedf063ba40c2e25aaa88485ef
  indexed: true
  sha256: b8d75768c68d9fafa9ed5f5026a1c3b075967241be1687da7e2d7e1b53d5142e
  source_range:
    end: 440607495
    part: 3
    start: 423811224
  summary: 审计显示，GQA/MLA sink fast path 的 `seq_lens` D2H 可移除，但 metadata builder 仍有 `.tolist()`、CPU list 到 NPU 的绕路；应先完成 builder 设备化，再按 builder capability 缩小 model runner 的精确 CPU 同步范围。`num_accepted_tokens` 不能整体删除：GDN 已直接消费设备 Tensor，但 Mamba 仍依赖 NPU→CPU planning→NPU 回路，需设备化 accepted-count 重排、state-copy metadata 和 `accept_token_bias` 后才能改动。用户要求按此边界实施，并先在 153、154 单机验证，确认无问题后再进行四机验证；截至材料结尾尚未修改代码或完成这些验证。
  title: '#15336：区分 attention tiling 与 Mamba 状态同步'
- block_id: 53d95339303f13e5b1475339a8c836892e314122a57bbce4fba5ff7a44169265
  indexed: true
  sha256: 0248c3dd2ff49277aa6dc16ce605db44504766d66d71151e8f18bd65df957f1c
  source_range:
    end: 457401492
    part: 0
    start: 440607495
  summary: FIA/GQA/MLA sink 路径改为消费 device 侧长度，fallback、CP/PCP、compressed attention 和 Mamba 原有同步边界曾被保留；153/154 的定向 UT、GQA 与 MLA Block5 单机请求均报告通过。四机环境经历旧 runtime 缺模块、156 HBM 未回收、进程组清理和 NFS 加载等问题后，GQA 完成 8K 并发、32K 单并发验证；但四机 MLA 的 uneven-DP 图捕获仍反复出现 FIA sink 校验错误，未形成成功结论。
  title: GQA/MLA sink 下沉与四机 GQA 验证
- block_id: b60f68974fe9907f1a7aa08e117e6de77024e8471d9aed4054dce852ac4ac99e
  indexed: true
  sha256: 021e1dd648710f272b656d1464c9c87ab6f8419b257c3e77c01a66f49d52111f
  source_range:
    end: 457401492
    part: 1
    start: 440607495
  summary: 在暂停 MLA DSpark 后，Mamba `num_accepted_tokens` 从 postprocess 到下一轮 precopy 改为设备侧传递，复用既有 fused postprocess/precopy 和 event 依赖；同步调度、310P fallback、非 align 路径仍保留 CPU 契约。153/154 各报告 52 个定向测试通过，并完成 8K→128、跨 384-token block 的 8K→512、并发请求和 prefix-hit 单机回归；这些结果属于该批次报告性验证，后续四机性能 sweep 仍需区分暖态、长尾与组合收益。
  title: Mamba accepted-token D2H 消除与单机回归
- block_id: 3215e5e8d11482262b37ba0b51833e4e1a30ee3bbdf46f68d76556cbbcbffb3c
  indexed: true
  sha256: c42e43ca284ae3f8fefbdccbfca5575b34cae429cf1f5937b74dd672e41c157b
  source_range:
    end: 457401492
    part: 2
    start: 440607495
  summary: '四机 GQA sweep 共报告 528/528 请求完成，覆盖 2K–256K 及多并发；2K/C4 首轮 34.61 ms 经暖态复测为 17.03 ms，256K 最大达标并发为每 DP 2，8K/16K 等长度仍有容量和长尾波动。相对 #15334“仅 DSpark 入图”基线，组合改动（DSpark 入图、tiling 下沉、Mamba D2H 消除）的 ITL p50/p90 平均改善被报告为 6.31%/6.74%，但不是单独 D2H 收益，且数据发布、rebase 和 PR 状态均为来源中的报告。另一个 session 的调查发现关闭 prefix cache 时 NaN 已在采样前 hidden states 出现，混合批次的新 prefill（第 7 层、73 个请求）整段 NaN，而已有 decode 正常；根因仍未完成确认。'
  title: 四机性能 sweep、ITL 对比与 NaN session 调查
- block_id: ff3250a1dc6c151404a3cd4d96ac5df37c6b77bcd4f91f957f799f9a68d2c515
  indexed: true
  sha256: 781891b00fe9e260d8f018504c8b560c6e85ffc15240c6a6d779d1876245f1bc
  source_range:
    end: 458174164
    part: 0
    start: 457401492
  summary: 独立复现显示，有限输入、零初始状态下，`chunk_kda_fwd` 在 127/128 条变长序列时输出和最终状态出现 NaN，而 32 条及以下正常；该现象可在无 prefix cache、DSpark 或服务调度参与时发生。报告指出，Post-WU 按每条序列补齐到 64 token 写临时内存，但分配仅按实际 token 总数计算：127 条输入约需 23.8 MiB，实际约 13.8 MiB，越界可能污染后续 `uSeed` 缓冲区。容量修正后同一失败输入据报告连续两次恢复正常，但完整服务回归仍未完成，因此应保留为已定位的源码错误线索与部分修复结果，而非最终验证结论。
  title: chunk_kda_fwd 变长批次 NaN 与临时内存越界线索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0bc94d24162470fe8ce3fc189b7eb239aa868d074ff53c78706cad0c5c608243
  kind: experience
  material_digest: 578f55eb2d0e505e2b6d2ad8616e36a163b4920ed348d14022154e4a23128e29
  revision: 4da4e42996a8a07338669a018746c29711fe01a4e497e04bf097a96eba5568e5
  schema: mindie-entry/3
  summary: '此前关于 sink builder 去 host 化、Mamba accepted-token 回读边界、153/154 单机门槛、四机推进、GQA 单机/四机请求、Mamba 定向测试和四机性能 sweep 的材料仍为可检索的报告性结果，不能直接升级为独立变更的因果收益；ITL 6%–7% 仅是相对 #15334 的组合对比。MLA 四机 uneven-DP/FIA sink 图捕获错误仍未解决，关闭 prefix cache 的另一条 session 中 prefill hidden states 在采样前出现 NaN、decode 正常，其 KDA 状态或混合批次根因此前未收口。新增独立复现进一步报告 `chunk_kda_fwd` 在 127/128 条变长序列触发 NaN，并定位到按 64 token 补齐写入与按实际 token 分配之间的容量不匹配；修正容量后同一输入两次恢复正常的结果仍属部分验证，完整服务回归、长尾/暖态差异及相关环境清理问题仍需分别核对。'
  title: 任务导航：GQA/Mamba 有报告性验证，MLA 与服务回归未收口；新增 KDA 越界线索
navigation: '此前关于 sink builder 去 host 化、Mamba accepted-token 回读边界、153/154 单机门槛、四机推进、GQA 单机/四机请求、Mamba 定向测试和四机性能 sweep 的材料仍为可检索的报告性结果，不能直接升级为独立变更的因果收益；ITL 6%–7% 仅是相对 #15334 的组合对比。MLA 四机 uneven-DP/FIA sink 图捕获错误仍未解决，关闭 prefix cache 的另一条 session 中 prefill hidden states 在采样前出现 NaN、decode 正常，其 KDA 状态或混合批次根因此前未收口。新增独立复现进一步报告 `chunk_kda_fwd` 在 127/128 条变长序列触发 NaN，并定位到按 64 token 补齐写入与按实际 token 分配之间的容量不匹配；修正容量后同一输入两次恢复正常的结果仍属部分验证，完整服务回归、长尾/暖态差异及相关环境清理问题仍需分别核对。'
schema: mindie-material-task/1
status: complete
task_id: 0bc94d24162470fe8ce3fc189b7eb239aa868d074ff53c78706cad0c5c608243
---

# 任务导航：GQA/Mamba 有报告性验证，MLA 与服务回归未收口；新增 KDA 越界线索

此前关于 sink builder 去 host 化、Mamba accepted-token 回读边界、153/154 单机门槛、四机推进、GQA 单机/四机请求、Mamba 定向测试和四机性能 sweep 的材料仍为可检索的报告性结果，不能直接升级为独立变更的因果收益；ITL 6%–7% 仅是相对 #15334 的组合对比。MLA 四机 uneven-DP/FIA sink 图捕获错误仍未解决，关闭 prefix cache 的另一条 session 中 prefill hidden states 在采样前出现 NaN、decode 正常，其 KDA 状态或混合批次根因此前未收口。新增独立复现进一步报告 `chunk_kda_fwd` 在 127/128 条变长序列触发 NaN，并定位到按 64 token 补齐写入与按实际 token 分配之间的容量不匹配；修正容量后同一输入两次恢复正常的结果仍属部分验证，完整服务回归、长尾/暖态差异及相关环境清理问题仍需分别核对。

## Materials

- [Kimi K3 vLLM 0.27 Ascend 适配与 Draft PR](blocks/b659e28a8300b465cf019c7f10abf1dcc08c4c8d37328d0b929a32f523401e34.md): 围绕 vLLM 0.27.1 原生 Kimi K3 能力，重新实现 Ascend 侧 KDA、no-RoPE MLA、SiTU/MoE、MTP、DSpark、FlashComm、Hybrid KV cache 和 A5 MLA Prolog 等适配，并保留了多处接口、算子装载和 CI 路由失败及修正记录。模型层 dummy 验证曾完成真实一 token eager 前向，代码检查、单测和静态门禁多次通过，但 MTP/DSpark 完整分布式运行、完整 checkpoint、长上下文和性能仍未验证。最终形成依赖算子 PR #14426 的 Draft PR #14454；硬件矩阵因缺少维护者 ready 标签而跳过，不能视为正式硬件验收。
- [单机验证准备与精确源码构建](blocks/8a232c6a33a314e09e6fc56f55d4615ab4fe8660fff93f1be3cb121b6bb5dfb4.md): 为单机真实权重验证，固定 vLLM 0.27.1 与 PR #14454 源码，完成 A3 算子构建并处理 editable 安装、构建依赖下载和容器旧 PYTHONPATH 遮蔽等环境问题。构建产物生成了数百个 kernel binary，但当时尚未完成服务启动或完整功能矩阵；后续验证计划包括真实权重、流式、Prefix Cache、ACLGraph、EP/FlashComm、DSpark 和输出完整性检查。
- [16 专家单机启动中的兼容性修复](blocks/eeea6bb75264c6f2de0852146fed32f69ff46b4d12caad9ce90a0bd3bd34fa90.md): 验证先排除 155 机器，改用 153 单机 16-expert 真实权重，并随后计划使用 153/154 做 P/D 分离；网络网卡绑定、量化映射、Kimi K3 外层/内层别名、MoE 并行补齐、latent projection 和 shared-expert runner 契约等问题已分别暴露并修复，focused UT 达到 58 passed，且 340/340 shard 曾完成加载。最新 profile run 已越过 shared-expert 维度错误，但又触发 AMD Triton attention residual 的 UB overflow；已实现并测试 NPU 组合路径，r9 重启验证尚未完成，不能宣称单机全功能通过或进入四机 GPQA。
- [r13 图捕获通过但语义验收受限](blocks/28d629af37432921a74f9e50b5969de715049df602bbd191aaac1de326d523df.md): 在 153 单机 EP16、FlashComm、131K context、Prefix Cache 和 FULL decode ACLGraph 配置下，修复了 MLA gate 的 16/256 token 分片错配及 KDA beta 未 sigmoid 导致的 NaN；r13 完成 183 秒图捕获并验证无 NaN、无字面量 None，16 并发 replay 不崩。此前 r10 的连续 `!` 已定位为 NaN logits，第 10 层 KDA 是首个非 finite 点；beta 修复后真实 93 层 prefill 恢复正常文本，临时诊断移除且单测为 134 passed。当前 16 专家裁剪权重的文本质量、reasoning/tool/prefix 语义验收仍不达标，不能据此宣称全通过；下一步仅补 ignore_eos Prefix Cache 冷热一致性及接近 131K 的 prefill+decode，完整权重和四机阶段再评估语义能力。
- [153 单机与 DSpark 兼容修复及 P/D 首轮失败](blocks/34b70137c3f079d048990a073ce171c7560a1dc85d0a0def90aaadff15a03257.md): 153 单机 16 专家服务完成 131K 精确边界、Prefix Cache、并发和多模态结构验证；8K prefill 配置避免了此前 MLA `g_proj` 额外申请 578 MiB 导致的 NPU OOM，但裁剪权重的 reasoning/tool 语义未被证明正确。DSpark 期间修复了 shared-expert 3584/7168 宽度、K3 多模态 token index、target/draft KV 分组、proposer metadata、embedding 替换和 FlashComm 分片等 vLLM 0.27 迁移问题，单机最终 7/7 场景与 16 并发通过。同步启动 153/154 P/D 后，基础接口虽通过，但 Prefix Cache 重复前缀输出不一致、真实图片请求首轮 500，尚不能视为 P/D 通过；155、156 未访问。
- [P/D 缓存一致性修复与 c64 EngineCore 崩溃](blocks/500bf61a1e4c0eba970e24014cdaa1d86f1125e824e1585b4d386ea15b907ba3.md): 冷启动与热缓存对照确认，早期输出差异来自 P/D + Prefix Cache 状态一致性回归；按 vLLM 0.27 接口修复后，冷/热缓存四次输出一致，多模态 data URI 请求为 200。32 并发、基础功能、Prefix Cache 和图片路径通过，但 64 并发在第 49 个请求附近触发 TP rank 的 AIVector `invalid GM address / cross-device timeout`，最终 EngineCore 或 P 侧退出；多次尝试的 pointer metadata、Mamba event 顺序、decode-graph 分类和旧 zeroer 移植均未证明能修复，旧 zeroer 反而使 r6 提前出现空响应和 500。r7 启动还经历过 CANN `acl` 环境缺失等实验性启动错误；单机同配置 cold/warm 各 64 全通过，问题进一步限定为 P/D 传输生命周期或缓存交接路径，155 仍未使用。
- [KV cache kernel 定位与 P/D 传输边界实验](blocks/daeff4d49ba553767b9bfaae6063f4d238e8046621b1d102f97a811cca474c75.md): r7c/r8/r9 在两机 P/D 上均重复出现冷 c64 仅 49/64 成功、随后 P 退出，CANN dump 将首个故障定位到 `KvRmsNormRopeCache`；关闭 target graph 后冷 64 全通过但热 64 仅 17 个成功，表明 graph replay 会提前暴露问题，根因仍涉及跨波次 KV/slot 或传输生命周期。0.27 物理 KV group 拓扑核查未证明普通单机 block 复用越界，单机 cold/warm 各 64 共 128/128 成功，因此排查转向 Mooncake/HMA 的状态所有权、对齐基址、远端读取与释放时序。补齐三项 0.27 兼容后，8×16 多轮请求达到 128/128，但直接 c64 仍前 33 个成功、后 31 个 500，说明累计传输或高排队交接问题仍未解决；随后尝试的生产端完成事件同步尚待最终压力验证。
- [P/D 1-token 修复通过但 16-token 压力再次失败](blocks/1cd8acff7e3a0535fc76652dabb533e0b92346c00f88a16a1251791772e2b987.md): 生产端状态可见性修复后，两轮 c64、每请求 1 token 达到 128/128，P/D 全程健康且响应结构检查通过；基础 Chat、SSE、Completion 和 stop 也通过，但裁剪权重的文本语义退化仍不能作为精度结论。更长解码暴露独立问题：32 并发、16 token 通过，64 并发、16 token 时 D 侧退出；单机相同负载两轮 128/128 成功，故障继续指向 P/D 生命周期中的异步 Mamba 状态更新、混合缓存块回收或远端状态接收竞争。针对 D 块回收边界新增同步后，P/D 首轮仅 33/64、P 退出，未形成修复证据；全量功能矩阵、图模式复测、四机 GPQA 和 155 访问仍未完成。
- [混合缓存高位块号边界与反馈提交](blocks/092fbd7ee1a931cd8febd0b350d97885c2fc38b6721d403d820003999a5b449d.md): 定位确认错误注册基址是已有回归保护的隐患，但不是当前崩溃的充分原因；纯目标模型 P/D 在相同条件下 128/128 成功，草稿缓存跳过、全设备栅栏和关闭 Prefix Cache 均未改变 DSpark 失败。混部 `max-num-seqs=64` 首批 0/64 退出，而 16 槽两轮 128/128 稳定；34 并发通过、35 并发失败，故障与 Mamba/KDA 高 block-id 跨过 4096 高度吻合，#13836 的 recurrent KDA 原地更新成为重点排查对象。已提交脱敏的 Codex issue #39119，并向 OpenAI Support 发送反馈；后续定位应保留服务器容器内 `127.0.0.1` 压测、网络安全误报和未验证的算子根因。
- [KDA 高索引证据与工单附件未完成](blocks/2a956f4a20c577561d2b7c3af567990cc14c40641730fd89b0d4528970fd5379.md): `K3_STATE_DEBUG` 显示 recurrent state 为连续布局 `(14125, 6, 128, 128)`，35 请求批次最大索引 4026 可执行，随后请求使用 4137–4144 并触发设备异常；这支持 4096 附近的边界，但尚未证明首个报错来自 causal-conv 还是 #13836 的 recurrent KDA。OpenAI Support 工单 #13359600 已收到包含多次 `cyber_policy` 触发信息的回复，原始日志 ZIP 已压缩并校验，但 Gmail/Chrome 附件上传持续超时，尚未确认日志已成功附到原线程。此前邮件中提及的 issue、工单和会话线索均属来源记录，不应视为安全问题或算子修复已验证。
- [DSpark 逻辑块大小错配修复及单机验证](blocks/a94ef48f79aee4abef9c04088dbd23431730e85db36c7375e3ac9b231daee835.md): 故障证据最终指向 DSpark 的物理页大小 384 与 attention 使用的逻辑块大小 128 不一致，导致高 block ID 下 KvRmsNormRopeCache 写入越界；修复已恢复按 cache group 传递逻辑块大小。相关定向单测 28/28、547 项集合 547 passed/1 skipped，且单机高块位、Prefix Cache、默认请求事件同步、canonical AttnRes 及多模态矩阵均通过，但这些结果不等于完整 P/D 或最终精度闭环。r35 曾在约 20 分钟后收到有序 SIGTERM，原因未确认；测试中还发现并修正了多个 fixture/API 适配问题。
- [Prefix Cache 与 canonical K3 单机回归](blocks/51eb48f45c8f7869d4f7652f78bb553ffb7318fd12ec0da5ed661c64bf036296.md): r36 的无命中长请求和长短混部均通过，r37 的 Prefix Cache 命中矩阵 c4/c16/c64 也通过，未出现 None、空输出、全感叹号或 507035；默认不返回 token IDs 的事件竞态 UT 为 4/4，真实 c16 与两波 c35 也通过。canonical K3 AttnRes 定向测试 14/14，r39 的无命中、命中和混部单机回归全部通过，acceptance 与旧路径接近，但完整定向集合此前暴露的失败主要是测试夹具问题。后续仍需在 153/154 的隔离 P/D 环境完成验证，不能把单机通过推广为端到端成功。
- [P/D Mamba 状态对齐修复与并发污染线索](blocks/172214bd9e3bcca25bf385c19b7456764c894046fe72e48e779ddb80b74127bd.md): 两机 P/D 的冷态对照显示 P 直出、D 直出和 P→D 修复前存在 token 0 分叉，而无 Prefix Cache 时三路 128/128 一致；根因被收窄为 align 模式 load-only step 将 mamba_state_idx 持久化为 -1，导致下一次 forward 漏拷跨块状态。修复后的冷态三路 token 完全一致，共享前缀 16 并发也通过；但低 KV 容量边界的 6 并发中仍有 2 条出现连续“!”。直连 P/D 和单条 P→D 均正常，当前未决假设是并发状态下 384-token 物理父页承载多个 128-token 子槽时引用计数或父页生命周期错误，尚未验证为根因。
- [并发尾部 token 0 退化与同步修复排除](blocks/e7c734a713284d8b8d24a2ba1e161d2f018c97a4d94c3e48a569c25a41af3b87.md): 直连 P 的 6 路长上下文并发也复现连续 `!`，控制组显示问题集中在接近 10000 上限、经过 9984 边界且请求退出后 batch 收缩的尾部；`-1 → token 0` 探针未发现占位符泄漏。曾定位到 accepted-count 异步 D2H 与 batch `condense/swap` 共享行的竞态，并尝试稳定缓冲区、提前 event 同步等修复；部分实验达到 36/36 正确，但 acceptance 从约 0.55–0.59 降至 0.11–0.33，不能视为成功。与 #13277 对照后确认其最终 event 同步已在当前代码中，额外同步修复在第二轮仍复现，因此已撤销；非连续 stride `recurrent_kda` 96 步对照也未复现，当前重点转向 0.27 的动态 draft 宽度、持久 batch 行及跨 wave 状态复用。
- [0.26 正确基线与 0.27 差异对照](blocks/9e670e51da78cfe49d4faf4f22c7714d5a8eb1986f5698037aad055052ae0e4c.md): 已确认 #13277 的 `num_accepted_tokens` 精确 event 同步已移植，额外提前同步不能解决问题；因此不能把当前故障归因于同步修复漏移植。新的已知正确基线是 `releases/v0.26.0rc`，三方对照发现 main 在其之后引入了 DSpark 输入展开的多程序/向量化 kernel，且 0.27 还可能删除了“拒绝 token 数 → MLA `actual_seq_kvlen`”修正链路，同时默认开启 async scheduling。旧 kernel 回退实验尚在进行，MLA 使用未修正 CPU `seq_lens` 的缺口只是待验证嫌疑，不是已证实根因。
- [异步多 draft 长 prefill 的精度退化范围收敛](blocks/c4e60c268165bc375dbe2dc5915af32b12afa085fb150aaa61eec2e02aa22c68.md): 旧 kernel、reject→MLA KV 长度修正、postprocess kernel、额外同步及多种长度探针均未解释 token 0；严格 raw token ID 检查显示，问题只稳定出现在 DSpark + async、K>1、长 prompt 分块并与并发请求混部的场景。target-only、K=1、短 prompt、单请求及 `--no-async-scheduling` 多轮通过，但 P/D connector 冷启动会显著放大失败，表现为后完成请求出现连续 token 0 尾缀。后续又发现 `async_tokens_to_discard` 仅覆盖 reset/preemption，不能解释正常 `max_tokens` 结束，因此该线索被修正为非直接根因。
- [P/D connector 触发条件与 cache-layout 候选被逐步排除](blocks/bb4b52895701c37d2c39dd6b1b09069f6ae1f5a38f6e46c80f05003d7cdb2e87.md): 单机冷启动高并发稳定通过，而启用 P/D connector 后出现递增的 token 0 尾部；对齐 storage view、`register_buffer`、每步 tracker、Mooncake 后台线程及若干生命周期假设均未能消除故障。针对 target/draft 物理 KV 分域的 r48/r53 候选虽改变了 capacity（约 479,861 降至 244,548 tokens），但仍为约 2/6 通过，因此不是充分修复。r49 进一步确认原始 `AsyncGPUModelRunnerOutput` 已包含全 0，且 `invalid_req_indices=[]`，但首批尚未 shrink 就已出现异常，排除了 API 解析和简单请求丢弃。
- [NaN logits 指向动态 batch 状态或输入路径，padding 修复失败](blocks/0d9d4104414809116892753c4d0e24ef87076e79aa40a52963bdb641ecf992f6.md): r50 在首次 token 0 时观察到 target 与 bonus logits 的全部 163,840 个词表值均为 NaN，说明 rejection sampler、输出解析和 Mooncake 不是首个坏点。将 target/draft 合并逻辑 cache group 因 target=4、draft=6 heads 的 v0.27 约束不可直接移植；保持逻辑分组并隔离物理 storage 的 r53 仍失败。恢复 0.26 的完整 K3 embedding 在 v0.27 产生 shape mismatch，清零 `split_inputs_tp_to_sp` 的非整除 padding 尾部也未改善；c1/c2/c6 的新观察显示请求通常在同伴完成、batch 删除/压缩后开始连续输出 0，但这一映射假设仍待验证。
- [问题收敛至 batch condense 后的 K3 padded 状态污染](blocks/ee0e3a652905a53a29350ae664f1472038126dc45ee75c0de94f161aa0c2755f.md): 单请求正常；并发时首个请求完成并触发 batch shrink/condense 后，被从高槽位搬到低槽位的请求开始连续输出 0，且 0 出现前 target logits 已整行 NaN。当前必要条件收敛为 DSpark + async + 多请求删除/condense；target-only、关闭 async、K=1、关闭 connector 等对照未复现，先前大量同步、缓存、embedding、padding 等候选均未验证为根因。代码差异的最强候选是 v0.27 清空 block-table 源行后，Ascend FULL graph 仍消费全部 padded metadata，且 padding state ID 被改为物理块 0，导致伪造 KDA/Mamba 状态更新；这是待验证的根因候选，尚非已确认修复。
- [K3 384/128 映射与候选验证](blocks/b491d87763a96bd18496202919536a794663384de09ed50c29f485baf269feeb.md): 排查将主要根因收敛到 v0.27 中 DSpark 把 384-token 物理页按 128-token 逻辑块错误解释，导致 cache index 放大、越界及历史 KV 污染；同时发现 arch35 的 42+ task 空 Vnew 分支可能少归还 workspace 事件。隔离回归和单机矩阵曾报告 135 项、93/93 通过，但 P/D 和真实 Prefix Cache 命中仍待最终验证；期间确认部分旧怀疑（zeroer、KDA gate/g2 丢失）并不成立。另发现 dynamic K、FlashComm 多模态 positions、空序列 chunk_indices、非流式 JSON 分片等未完全闭环的边界问题。
- [状态行生命周期与 proxy 异常出口](blocks/61e2f4e5386ab34e2c0671829043b5d0e74153e4e550edf25ebef872bf17d3bb.md): 正向对照确认 BlockTable.move_row() 在 batch condense 后必须清空源行，当前 Ascend 覆盖实现遗漏该动作，可能使 GDN/Mamba 图 padding 继续引用已释放并复用的 state block。另确认两个 P/D proxy 仍可能吞掉 decoder 或流式响应异常，造成空尾、截断 200、重复内容；拟改为首个 chunk 前允许有界重试，之后异常直接传播，但结果尚未验证。物理页到逻辑块的 metadata/reshape 链路已找到对应实现，zeroer 未获版本差异证据。
- [P/D Prefix Cache 与候选冻结前核对](blocks/06d35f8e78f6e1242bfd6b6b669fa150356fbd3040a2f8b4516d7ef32aaaf0cf.md): 当前新增差异仍集中在两类：混部或 condense 后源行未清可能污染 GDN 状态，以及 proxy 两层吞异常可能产生空尾或重复响应。后续将继续核对 P/D producer/consumer 的 Prefix Cache 判定、状态块归一化和双机路径，确认单机修复不会引入新的分叉。此阶段仍是计划中的核对，尚无新的成功验证结论。
- [0.27 K3 状态边界与单机定向验证](blocks/618e59a1471d5e24d7d5b9793c93a8c5907d785cd2455591f827e96f36ba7188.md): 正向审计将当前差异收敛为 K3 全宽 `g_proj` 的 SP 误判、K=7 padding 行 accepted-token 契约、42-task 空子块事件，以及全 0 draft 的 spec 降级等边界；其中部分属于对齐缺口而非已确认根因。删除了不必要的 `_clear_speculative_conv_tail` cache 写入，恢复 `accepted=1` 等基线语义，并统一 proxy 的 `None` 归一化。r57 单机 Prefix Cache、K7 mixed 和 c64 定向结果通过，42-task 直测返回 0，新增隔离回归共 13 passed；这些结果主要证明无挂、非空和边界行为，尚不构成完整精度结论。
- [GSM8K 基线与 main graph 回归定位](blocks/8edd09ab9c0714a7dbf30e93567f2ee118ab5bd4926ad8ae7d2fafc38842ee17.md): 合成 filler 输入导致的重复短语被排除为精度证据，随后使用同一 16 专家 checkpoint、8192/1024 token 和历史脚本校验；c1 与 0.26 同权重基线一致，唯一一次 c16 为 16/16 完成且无 `None`、空输出、乱码或崩溃。混部审计发现 `kv_both`/connector 需要按请求的 `do_remote_prefill` 分流，并将滞后的 `BalanceScheduler` 副本改为直接调用 vLLM 0.27.1 原生调度器；这些修复已静态检查或 UT 验证，但两机 P/D 尚未完成。r58 仅开启 main `FULL_DECODE_ONLY` graph 后 6/16 命中 `repeated_exclamation`，因此问题暂时收敛到 main ACL graph replay，而非 async、网络或混部；此前把 0.26 balance 路径当作无问题基线的判断已被纠正。
- [ACL Graph 状态算子契约候选](blocks/23d5ceb3d32cf2b2c6504fedd67f9cc2fe1c63b46b216fcd88646f28ffb56759.md): 同权重、同 c16、同请求集下，r57 eager 通过而 r58 main graph 稳定出现后半批重复输出，说明质量回归与图回放路径相关；ACL graph 外壳与 #13277、v0.26 哈希一致，尚未证明是同步修复漏移植。源码差异集中到 v0.27 KDA 的原地 state/别名输出协议、dummy capture 的 spec 元数据，以及 causal-conv 的 `conv_state` schema：`output_final_state=false` 时仍声明必选同址 `final_state`，而实际写入的 `output`/`conv_state` 被标为只读。单算子硬件回归因 153/154 卡被占用尚未执行，因此这些是待验证的合同错误候选，不应升级为已确认根因。
- [K3 FULL graph 状态契约与回归收敛](blocks/7862db17544f71901fdc1266517cc8578dbc8df10367768e340c8f6ea5a0fee0.md): 审计将 K3 FULL graph 风险集中到 causal-conv 与 recurrent-KDA 的 mutation/alias 契约、动态 padding、norm-gate 和状态生命周期；发现并修正了若干测试自身问题，包括 conv state 长度、slot 写冲突及 CPU `index_copy_` 的 int32/int64 差异。隔离编译和 CPU 定向测试多次通过，但 NPU 验证尚未完成；prefix-cache tail、dummy no-op 输出、NaN padding 和 producer prefix-hit 仍有未完成或需继续确认的分支。最后提出 dummy 分支应在无 KDA 工作时直接清零并返回，避免 `0 * NaN` 污染。
- [FULL graph padding 与移植差异审计](blocks/09b0e55b926bd4406849de0a8d34495953c0a3e3a273830dd5f30ed6dc9d4781.md): 正向对照继续以 0.26 高并发基线为参照，重点检查 0.27 将外层 opaque KDA 展开后暴露的 causal-conv、recurrent、norm 和 padding 契约。新增或确认的风险包括 recurrent 空长度行从 `empty` 泄漏未初始化值、c16→c11 replay 的动态 padding 经 gate/norm 后被重新污染，以及 KDA RMSNorm `eps=1e-6`、beta sigmoid 和 native dispatch 等移植差异；其中旧的 speculative tail 清零假设已被参考语义否定并撤回。CPU 验证曾报告 65/65，但随后确认使用了旧隔离副本，因此该结果一度被撤销；后续 23/23 强化回归通过，但对应硬件 ACLGraph 结果仍缺失。
- [NPU schema 反证：finalState 不能为空](blocks/39f118e8ab73f298a47df207371cd94601412d32471c4471d2aa8456222c9ae6.md): 最新精确源码隔离回归报告 65/65 通过，强化 KDA adapter 回归为 23/23；但这些仍主要是 CPU/静态证据，不能替代 NPU ACLGraph 验证。153/154 无真正空闲卡，随后在 153 的隔离构建上运行 schema 用例：动态库路径问题已排除，schema 检查通过，却发现当前 CANN 的 `aclnnRecurrentKda` 会以 `EZ1001` 拒绝空 `finalState`，因此“省略未使用 final state”的修法不成立。当前改为让 CANN 使用不对外暴露的 scratch `final_state`，只保留顶层 `initial_state` mutation；新 recurrent 编译已进入 kernel/protobuf 阶段，但尚未得到重跑结果，r58 服务仍报告 200。
- [算子级修复与完整服务回归前置准备](blocks/4522f22d7ddc626bd4adf29ffaaf8d7a7e86786fa62f098b9fbc7a6ebda24d60.md): recurrent KDA 与 causal-conv 的 NPU 回归、定向 UT 和显存诊断曾通过，最终采用非空、独立且仅占 1 个元素存储的 expanded scratch；但完整 TP16 服务尚未完成验证。随后停止既有服务，尝试构建并部署同时包含两类算子的统一 runtime，整包构建被无关 GMM 的 CANN 9.1 `limited` 字符串/float 配置错误阻断；新 binding 与旧统一 vendor 的 12/12 算子组合检查通过。跨 153/154 的多种 SCP/SFTP/SSH 传输路径曾失败或卡住，固定 tar 包约 61 MB，后改用临时 HTTP，最终 archive SHA 与 runtime manifest 一致。
- [双图服务首请求暴露退化并暂停扩展回归](blocks/b105b9332b5ffba608d27a1c2a273ee201fc7039919a2aa506194c55fb2a47a2.md): 153 上将 FULL 图捕获从 8 张收敛为 `[16,128]` 两张后，服务健康检查为 200，但首请求及 chat、reasoning、raw completions 均稳定输出连续感叹号；因此停止大矩阵和相关服务，未将其视为完整回归成功。当前证据仍缺少同一代码、权重与运行时下的 eager/ACLGraph、target-only/DSpark 对照，且完整 clean vendor rebuild 被无关 GMM 配置阻断。材料随后将诊断边界收窄为 0.26 已验证算子作为 golden、直接比较 0.26/0.27 整机集成，而不是继续扩大算子修改范围。
- [0.26/0.27 同构对照定位到图执行链路](blocks/f237f083b126c802b5a5fdcb75eb1a16608bc3711edde2cd2ef8b0b9445a93f8.md): 隔离快照固定了同一 40-op vendor、权重配置、async scheduling、两张图 `[16,128]` 和相同 prompt IDs；0.26 正常返回 16 个 token，而 0.27 首次请求因 `nan` 序列化返回 400，去掉 `logprobs` 后仍输出连续 `!`，表明不是单独的 API/logprobs 问题。随后两侧 eager 服务使用相同输入仅生成 1 个 token，均返回相同首 token 空格，初步把差异从算子普通 eager 数值收窄到 0.27 的图执行/图输入复用链路。为比较关键节点，双方隔离副本加入统一 dump；首次门控因 padding 长度未命中，后改为服务启动后标记并对真实 forward dump `NaN/Inf/max/mean/RMS`，但后续小文件同步遇到 SSH 连接阻塞，尚未完成新的部署或节点证据。
- [0.27 图回放中 causal-conv 修复无效](blocks/976142f4d54fb5c7cb9d79e53679ebf5edfc77728940971d0eb16e3e561986c0.md): 曾提出 0.27 图模式丢弃 causal-conv 返回值可能导致未初始化张量，并完成单变量部署；但硬件结果显示该修复后 1 token 正常，2/16 token 仍因 NaN logprob 返回 400。compile-only 模式下 1/2/16 token 均正常，因此问题进一步指向 ACLGraph decode/replay，而非 torch.compile 或算子数学实现。尝试增加 opaque/mutation outer custom op 后仍失败，当前怀疑需要真正的 attention graph boundary；该结论仍是实验性定位，尚未完成最终修复验收。
- [split 边界无效，转查 metadata 与 norm 回写](blocks/0dab04044a8f2ac9ad925cfc3f9fc6a2f6c3366242df06df73212e3562a4b1e4.md): 将 KDA op 加入 `splitting_ops` 未改变 1 token 正常、2/16 token 从第二步 NaN 的现象，且 FULL_DECODE_ONLY 会重置该列表，说明模型初始化时追加配置可能过晚。回退 GDN builder 的 168 行未提交改动后结果仍失败，排除了这组改动作为当前精度根因；现有差异重点转向 graph replay 输入同步、`num_accepted_tokens`、DSpark metadata 以及 KDA norm 的别名/`copy_` 回写。最新实验已启动，尝试将 norm 移回 0.26 的 op 外位置，但尚未报告 capture 或回归结果。
- [定位并修复 0.27 FULL 图第二 token NaN](blocks/a16103d91783737c588f739483987b9837c7d208b12976744d6b2861a81d5e85.md): 对照验证显示 target-only 普通 KDA decode 的 1/2/16-token 请求均为 HTTP 200 且 finite，故障限定在 DSpark speculative decode 的 ACLGraph replay 路径；一次调试 dump 曾因不存在的 `self.gdn_query_start_loc` 触发 500，并造成“全 0 输入”的假象。随后确认 0.27 在 FULL 图中将 padding 请求误判为 non-spec prefill，且 `KimiLinearConfig.is_linear_attn` 计算属性未被识别，导致 GDN metadata 索引错位并从前两层产生 NaN；按 0.26 语义移植检测逻辑后，2-token 及固定 1/2/16 回归恢复，均无 NaN/Inf。高并发 Prefix Cache、长上下文和混部回归后来通过，但回归脚本曾因 `serve.pid` 中字符串 `baseline=...` 强制转整数而未实际发请求，修正后才继续。
- [单机与 P/D 回归通过，部署准备经历传输和版本问题](blocks/5e88d08693386e8d782f0d87ecd336cf98db7849bb496b85ad93c78c2e07751c.md): 修复版单机完成 c16、两轮 c35、8K no-hit/prefix-hit 及长短混部回归，分别报告 16/16、70/70、32/32 等结果，health=200 且 fatal=0；针对性单测初次受精简 overlay 缺少 `tests/` 骨架影响，补齐后 GDN 4/4、Kimi 19/19 通过。向 153 复制源码时流式 tar 管道近乎停滞、scp 又卡在 SFTP 握手，均被终止并改用容器间传输；P/D 首次启动还发现安装 metadata 显示 `v0.26.0`，于是停止精确 PGID、显式设置 `VLLM_VERSION=0.27.1` 后重启。P/D 冷热输出与单机参考一致，Mooncake 覆盖和 DSpark 接受率检查通过，固定 1/2/16 也返回 200 且 logprobs finite；随后开始清理未编译的算子、padding、dummy、causal-conv 等实验性改动，但部分 model-runner 全量 UT 因精简包结构和错误 mock 不具备完整验证条件。
- [纠正补丁归属并完成最终源码 P/D 验证](blocks/109e4454cda03964178864bc3a099022f08c83abed4380c75c956722a7ed92ba.md): 审计中纠正了 deferred-reject 判断：由于 K3 DSpark 覆盖基类 `set_inputs_first_pass`，该同步链是 0.26 已验证的正式迁移，恢复后定向测试通过；`balance_schedule` 则收缩为 producer Mamba prefix-hit 的薄适配，proxy 仅保留 0.26 已合入的多模态 content-part 处理，并保留 `<redacted:employee-id:fdcf2fc0fdc5>` 占位。最终源码在两端重启后完成固定 1/2/16、Prefix Cache、流式、completions、多模态、精确 8192→1024 和 32 路长短混部回归；一次 10,545-token 请求返回 HTTP 400，被确认是脚本超出 `max_model_len=10000`，不能算模型失败，改用精确 8192-token 构造后通过。CPU 定向回归为 526 passed、2 skipped，最新 main 审计树为 527 passed、2 skipped；但 `mla_prolog_v3` 与最新 main/独立 operator PR 的归属仍在审计，4 机 GPQA 因 155 未释放尚未完成。
- [0.26/0.27 固定 token A/B 对照完成](blocks/c3fc265f0595ddb795a46e223fac21b6b15d6d6432ba50990739fd68df79068c.md): 模型树已清理掉 operator 源码差异，0.27 单机及 P/D 的功能面、c64、8192→1024 长上下文和 16 长+16 短混合请求均报告通过；但单机曾出现一次低概率空输出，不能视为已关闭。随后按同构配置完成 0.26/0.27 A/B：聊天请求两边均为 112/112，但 tokenizer/rendering 导致 prompt token 数不同；固定 `prompt_token_ids` 的 completions 对照中，0.27 为 112/112，0.26 为 111/112，唯一空文本仍带 16 个有限 logprobs，因此未证明 0.27 更易产生空输出，也未完成 155 上的四机 GPQA 或最终提交推送。
- [六个子 PR 的逐条检视修复进展](blocks/92cbd78bdf44952d53a628b659a30d386190e133ca4ddf9fdbcb4ec03267851d.md): 已按特性维度处理六个非算子子 PR 的检视意见：Hybrid 补充 Qwen3/Qwen3.5 兼容测试，Attention 清理 DSpark 旧链路、收紧接口并修复 g_proj 重复 all-gather，SiTU/MoE 对齐 vLLM 0.27 原生契约，Model 清理不必要的 getattr，DSpark 删除额外策略限制，P/D 完成基线检查。过程中发现并修正了 CRLF、测试夹具缺少 VllmConfig、共享专家入口变化等问题，但部分早期修改后来被证明过度，需以之后的回退和补齐记录为准。
- [大 PR 整合、服务器启动与运行时错误定位](blocks/85e3ad58073edf930e801383e5ec02d83635524e337143c5ca73d1d1fa156dec.md): 六个子 PR 与四个算子提交已重建为十个 signed commit 的大 PR，整合静态检查通过；服务器定向 UT 曾出现 CustomOp 配置夹具问题、算子 tiny-shape 基线失败，以及部署目录缺少已编译算子包等非同类问题。修复配置上下文并设置正确算子路径后，服务完成 TP16/EP、两张 FULL decode 图捕获并 health=200；随后发现 review 中去掉兼容访问后暴露 `initial_state_mode` 缺失，定位为拆 PR 漏掉的 GDN 固定图状态和 DSpark host KV/reject 同步配套修复。Attention 侧定向测试 33/33 通过，DSpark 测试在修正 `additional_config` 夹具后 79/79 通过，但这些结果不等于最终服务验收已完成。
- [整合回归通过后的单机 C16 空输出复现](blocks/e08183e764739067b8996333f71c21677af0e44ee0786c5bc4cbb5634effadad.md): DSpark 后续按检视收窄为仅保留异步拒绝同步和主机侧序列长度链路，去掉初始化期配置拦截；Attention 126 项全过，整合 UT 最终为 596 passed、2 skipped，已知算子 tiny-shape 基线测试被排除。单机功能矩阵虽无 `None`、空响应或卡死，但裁剪权重导致的语义质量差被确认不应作为精度验收依据；更关键的是当前分支冷启动首个并发 C16 仍出现一个空输出，且同批请求轨迹分叉，单请求却稳定。此前一次冷启动缺少 `--async-scheduling`，因此该实验不能归因到代码；最新动作是补回 async 后重跑，并并行检查 Model/P/D 的 pre-commit 失败。
- [子 PR 重建、冲突处理与评论清理](blocks/5f40463790da40ccdd2509062e109aa7583383c23d59af0d449d3e8de1f67154.md): 核实后，#14597 中 Gemini 关于压缩 MLA cache `page_size_bytes` 的判断不适用于当前 vLLM 0.27 虚属性链，已有单测覆盖，相关评论已解释并 resolve。#14426 已 rebase 最新 main，删除零引用的 `kimi_k3_situ_quant_cases.json`，移除自定义 MLA Prolog V3 源码、绑定和测试；KDA 形状意见被判定为将内部 identity-copy barrier 误解为公开布局转换。#14598 恢复使用默认 `torch_npu.npu_mla_prolog_v3`，#14454 重建为 7 个特性 commit；#14600 中 REST 不可见的 pending review comment 已通过 GraphQL 删除，遍历相关 PR 后确认作者为 maoxx241 的残留为 0。
- [block size+1 分类修复与 K3 nightly 方案](blocks/9c9026407c6a7c5eecf9376388bec512fb6cbf4614127720258e3c032f5e42d6.md): 历史问题被重新界定为完整 cache block 后剩 1 个 token 时的 prefill/decode 错分，而不是 attention-residual 的 `N=129` 精度问题；当前 #14598 已包含 `is_prefilling && query_len == 1 && seq_len > 1` 的 GDN metadata 修复。新增回归明确覆盖 `block_size=16` 下 `seq_len=1` 保持 prefill、`seq_len=17` 使用 recurrent-state decode metadata，隔离 CPU metadata 测试为 2 passed；但运行中的 P/D 服务使用更早源码，不能作为该最新场景的实机证明。后续父 PR 需要加入 K3 nightly 看护用例和部署文档，并区分裁剪权重可验证的执行/部署能力、dummy 权重可验证的启动与图捕获，以及必须由完整真实权重承担的 GPQA 语义精度门禁；具体 nightly 实现尚在设计/落代码阶段。
- [153 旋转量化混精度加载与 BF16 ND/NZ 实验](blocks/5a59017940f4d2b469da0132473619c7aea024327485c50d6eafa5feb1a4edef.md): 在 153 上针对 vLLM `58d3918e`、大 PR `1ab5a53c3` 和 QuaRot 16 专家权重复现了 `ModelSlim` 混合精度报错，定位为 K3 七路投影合并后 q/k/v 为 W8A8、g/f_a/b 为 FLOAT，不能沿用统一 fused shard 校验。实现逐步收敛为官方 merged-linear shard loader：量化 q/k/v 一组 GEMM、BF16 g/f_a/b 一组 GEMM，并修复了模块级 loader、普通参数误路由、DSpark `context_kv_proj` 映射及 latent-MoE runner 分发；153 实例最终完成 333 个主模型分片、DSpark、两张 `[16,32]` 图和服务请求回归。真实 `K=7168,N=912` microbenchmark 中，ND 在 M=128 等较大形状更快，NZ 仅在 M=16/32 有约 1–2% 小优势，因此保留库默认 BF16 ND；Grok 代理未实际执行实验，不能计入证据。
- [153 端到端服务验证与 PR 门禁问题](blocks/f36278e3d0188e8fab850331408d3c40083b1c39b60c19af614f8d0ba1e5281f.md): 直接在 153 执行了原计划由 Grok 完成的单卡 ND/NZ 对照，确认首轮 `nz_format=2` 的结果无效，随后获得真实 ND/FRACTAL_NZ 数据并支持 BF16 保持 ND。r9–r11 期间先后修复普通参数误路由、DSpark 权重名兼容和 `moe_comm_method=None`；最终服务通过 profile、KV cache 分配、两张图编译、health 200、1-token/block-size+1、并发和有限性检查，prefix cache 在 769/1537 重复前缀分别命中 384/1152 token，但强制工具场景未生成 tool call，属于能力限制而非崩溃。父 PR 及子 PR 已按职责拆分并更新，部分 `ci-gate` 仍因缺少 `ready-precise`/`ready-all` 标签而受权限阻塞；随后按七个子 PR 合并净差异验证，父 PR 目标仅保留文档。
- [子 PR 清理、LoRA 结论与 latent runner patch](blocks/871a4310f27ca1bb501459a3720e21ef0c10f98ccaf27ed784f553c42fb15cf8.md): #14426 已精确删除 csrc 下 3 个算子 README 和 `recurrent_kda/tests/pta` 的 3 个文件，同时保留正式 API 文档及仓库级 nightly 用例；子 PR 与父 PR rebase 后本地格式门禁、远端 pre-commit/mypy 和文档检查通过，父 PR 仍只差 K3 文档及索引。核对后确认真实 K3 LoRA MoE 未验证，现有改动只有 mock/随机 tensor UT，且真实服务未启用 `--enable-lora`，因此应从 #14599、父 PR 及说明中移除，当前尚未执行远端修改。与之不同，`_KIMI_VENDOR_LATENT_RUNNERS` 被判断为需要保留但应缩窄：K3 显式传入上游 `ROCmLatentMoERunner` 会绕过 Ascend 的 `AscendMoERunner` 注册；该判断来自真实 K3 调用链和此前运行路径，最终实现范围仍需继续确认。
- [LoRA 回退、K3 runner 绕过与 153 实机验证](blocks/1fb2ca624a1e9f5dae86b030e6e2ae1bc56b10015333bdc6f0d616c33f4161dd.md): 确认普通模型的 `runner_cls=None` 会进入 Ascend runner 分发，但 vLLM 0.27 的 K3 显式传入 `ROCmLatentMoERunner`，会绕过旧 patch；因此旧 patch 并非完全失效，`_KIMI_VENDOR_LATENT_RUNNERS` 的最终必要范围仍需继续验证。LoRA MoE 两处改动已从子 PR #14599 和父 PR #14454 回退并推送，focused UT 为 `155 passed, 1 deselected`，被排除的 N=4 对齐测试在独立 main 基线同样失败。153 上真实 93 层 QuaRot 16 专家服务在补设 `GLOO_SOCKET_IFNAME=lo` 后完成权重加载、ACLGraph `[16,32]` 编译及文本、PNG 多模态和 C16 请求；这些结构与完整性检查通过，但裁剪权重输出语义较差，CI 仍有未完成项，且主线程曾发现进程清理状态与 subagent 报告不一致，不能把所有运行环境结论视为完全核实。
- [K3 PR 全量审计：移除过度门禁并复用上游实现](blocks/05606e995cec01b620ab0f659f7d4bdc1bbcb6215c520b9ee964bd7480b51d7b.md): 审计将“仅因未在 153 验证就拒绝”的 `NotImplementedError` 与真正防止静默错误的 shape、地址、状态和物理契约检查区分开；SiTU EPLB、W4A16/antiquant、KDA gate、混合 RoPE metadata 等定向回归已通过，153 历史触发用例也报告通过。后续发现 QuaRot 自定义 loader、K3 全局 latent-runner 特判、重复 MTP/DSpark forward、手工 MLA merge、私有防御检查等应改为复用仓库或 vLLM 上游接口；用户进一步要求所有特性遵循“先复用，只有硬件差异才新增实现”。P/D 验证受 154 使用 `v0.26.0` 混合环境及 SSH 密钥交换失败阻断，不能将旧服务结果包装成当前 0.27 head 的通过；最新 main 已覆盖部分 DSpark/MRV2/GDN 公共逻辑，后续 diff 必须以最新 `origin/main` 重新审计。
- [K3 PR 栈复用审计与 153 初次验证](blocks/0d6cf56d2d3a3c893745f1aff94ee5177ac21b850d6c446e844854175946e7c5.md): K3 PR 栈持续按“优先复用当前仓库和配套 vLLM、仅保留 Ascend 差异”收敛，删除了重复的 MLA、SiTU、注意力残差、加载器及 P/D 逻辑，并将 MoE、DSpark 等实现边界重新审计。153 初次真实启动曾分别遇到 Gloo hostname 解析失败、覆盖 PYTHONPATH 导致 `acl` 丢失、缺少一级算子产物，以及 DSpark 重复注册 MLA；部分问题已通过环境固定或代码调整越过，但服务启动和权重加载尚不等于请求正确。后续出现的 W4A8 `moe_comm_method=None` 和 hybrid KV `scale_dim` 错误推动了 runner 分发与 cache merge 修复，仍需保留未完成验证边界。
- [独立 Ascend MoE 与 153 NaN 阻断](blocks/bbc3cc2b655a0f3c2bebcae523f280077f78f9ec3427c48cc3758a02368f1b7e.md): 针对 AMD `ROCmLatentMoERunner` 继承导致反向 patch 的问题，MoE 改为独立 Ascend 组装层，仅复用通用组件和 `FusedMoEFactory`，由标准 `MoERunner` 插件分发到现有 `AscendMoERunner`；结构 UT 通过，真实加载也越过了原 runner、DSpark 和 hybrid KV 异常。153 服务虽完成权重、两张图和健康启动，但固定请求在 logprobs 路径出现 NaN：127/128/129-token 等用例失败，且后续 32-token 请求可能被污染；关闭 shared-expert overlap、prefix caching、DSpark，或清空 KDA cache 均未解决。155/156 均被 16 卡 DP/EP 服务占用，当前可用对照资源受限。
- [同机 TP8 对照与 KDA 状态线索](blocks/dc28f7c51fb32498c6c6981b60a3db2b024e26af4a059d69b56983ca3b56ee87.md): 153 已重组为前 8 卡 vLLM 0.26、后 8 卡当前 0.27，双方 TP8、attention block size 768、eager 配置和权重对齐；0.26 在固定请求中始终有限，而 0.27 从 127→128/129 或更早的 31→32 临界点开始出现首 token logprob NaN。补齐 0.26 的 `_cann_ops_custom` OPP 包后两边均健康，排除了共同 AscendC 二进制、权重、机器和 block size 等因素；逐层 `.item()` 同步会暂时使问题消失，提示异步时序或持久状态污染，但 KDA 返回同步和 prefill 前清空 KDA cache 都未奏效。进一步结构 dump 显示 0.27 有 14 个 Mamba cache group/5529 blocks，而 0.26 为 3 个 group/943 blocks，NaN 在物理块复用后出现；这仍是当前假设和定位线索，不是已验证根因或完整 0.27 精度结论。
- [attn-residual stride 修复及 TP8 P/D 回归](blocks/d6771e6cd82c04e69438043d68f0354916cd8fc6616b8d2390aa2b7ead3748a7.md): 定位并修复了 0.27 预分配 block_residual 的非连续前缀切片被 Triton kernel 按紧凑布局寻址的问题；单卡连续/非连续回归及原始 8 请求序列均通过，随后同机 TP8 P/D 也完成了 16/16 边界请求和 8192→256 长请求验证，未观察到 None、NaN/Inf 或 HTTP 错误。期间首轮 P/D 失败源于旧脚本设置 VLLM_VERSION=0.27.1、容器 vllm metadata 不一致及 154 库布局差异，后经隔离环境、版本文件和路径修正恢复；PR #14600、#14454 的格式与 mypy 问题已修正，硬件证据仍对应其直接父提交，剩余 CI 红灯是缺少 ready-precise/ready-all 标签导致的门禁失败。
- [GQA DSpark 精度测试被加载阶段缺口阻塞](blocks/ce2e5db094088d5f8e44d2d55fab3d42a2ba41c0e4787e108fbf5d6f600fa60f.md): 计划使用旋转量化 16 专家 QuaRot、GQA DSpark 草稿，在单机与 P/D TP8 下比较确定性请求的 token、logprob、Prefix Cache、C16 和长上下文结果，并打印实际 KVCacheConfig、group spec/block/page/layers 及 tensor 配置；历史分组结论仍仅作待核对参考。实机启动时发现 GQA 草稿仍路由到 deepseek_v4/dspark.py，该实现无条件读取 MLA 专用的 hc_mult，而 Qwen3Config 不含此字段，导致所有 rank 在 KV 初始化前失败；因此本轮尚未得到精度或 KV 分组实测结果。
- [四机验证环境清理与 MLA 启动](blocks/71be1ac4cdea00e0acbeb35b55b0851c02767b8fffab31450279fe01954f57f9.md): 按授权清理了 155/156 的服务及 153/154 的旧实验进程，创建了带 QuaRot 标准描述符的隔离权重视图，并修复 154 staging tree 不一致；原始权重未修改。四机 MLA 以 TP16×DP4×EP64 启动，完整 345 分片和 MLA draft 正在加载，64 个 HCCL rank 尚处于初始化/图捕获阶段，尚未形成精度结论。
- [MLA 历史精度用例首轮回归](blocks/89c2c5464aa9af2b16d3539e1233d3e4c9be3450b62c5a03de23cf2e1573104c.md): 四机 MLA 服务最终达到 64/64 worker、两张 ACLGraph 完成、fatal=0，KV 配置为 20 个逻辑组、5 个物理 tensor、scheduler block=384。短题、384/385 边界、Prefix Cache 冷热清空、A/B1/B2/C、8K c1/c2/c4/c16、c35/c64 和 128K c1/c2 均未复现 None、NaN、连续 `!`、Invalid-GM 或高并发卡死；DSpark 产生并接受了 draft token。GSM8K 串行 no-hit 已开始，后续 16 路并发、GPQA 及 GQA 专项仍未完成；跨 DP 的重复 token 文本存在合法 hash 差异，不能当作精度失败。
- [MLA 补充边界与 GPQA 超时归因](blocks/95d32b0e4302647ae8d790306bd64a6acce02695b2f725e4f0f5bbcaa7eab491.md): GSM8K no-hit 串行 8/8 精确，但 16 路中 case-12 稳定输出 12 而标准答案为 13；历史 0.26/PR13277 也同样输出 12，故保留为既有权重质量边界。MLA 的 128K c1/c2、384/385、4095/4096/4097、hot-rank 图回放和多模态 DSpark smoke 已通过，未见历史坏输出或 KV 冲突；GPQA c64 中集中出现的 21 个 `pred=None` 已确认是客户端三次 `ReadTimeout` 累计约 5403 秒，不是模型返回 None。GPQA 随后改以 c16、7200 秒超时重跑，进度到 16/198 且暂为全对，仍未完成。
- [MLA GPQA 长推理持续回归](blocks/b78a19217925720b431bcaca62ed1aed01869e9dacc54d29c17752f5e26f64c0.md): GPQA c16 继续运行至 47/198，46 题正确、1 题错误；错误主要是可解析的语义选择题差异，未出现 None、NaN、解析失败、请求错误或服务异常。长题最长约 2807 秒，四个 engine 持续生成、无 waiting，日志约 18 tokens/s、KV 占用约 10–13%，因此暂未支持卡死判断。历史精度问题专项大部分已通过，但 GPQA 尚未完成，GQA 以及若干未完成矩阵仍不能宣告通过。
- [GQA 启动与基础矩阵](blocks/d3fd1a70cfea77048c7bb13a7e80732ee21de216bba1d93acb6e402a9853173c.md): GPQA 中途保留到 111/198（104 对、7 个语义错题、请求错误为 0）后暂停，转测 GQA DSpark。GQA 完成 345/345 target、draft、QuaRot 共享权重加载及两档图编译；KV dump 显示 20 组、block_size=384，四个 DP 的 1-token、384/385、边界、Prefix Cache 基础矩阵均报告 ALL_PASS。上述结果是运行记录中的通过结论，不能外推到尚未测试的高并发或完整任务。
- [GQA 高并发静默退化](blocks/13cc2057274a026d83c4de503cf2275e3ab87117288d6a56d23723687a04e336.md): GQA 的 4K、Prefix Cache 冷/暖/重置、128K 和 B2 对照均报告通过，但 8K 同一 DP 高并发出现静默连续 `!`：全 miss c8 约 5/8 正常，c16 约 9/16 正常，而失败 prompt 单独 c1 均正常。一次正文出现 `None` 后经重跑确认是自然语言词汇导致 harness 误判；真正问题被定位为并发状态更新或 prefix-enabled Mamba/KDA 路径，不能把 prefix-on c8/c16 原实现视为通过。
- [A/B 修复与四平面撤回](blocks/995674d497708d1ab62b9399c3ced14ba9a636cbc650ba5d262310ea7d3aac7c.md): 显式 prefix-off 的 c8 为 8/8 正常，c16 的唯一失败后来确认是正文包含 `None.` 的测试器误报，实际为 16/16 正常；late-H2D 补丁的 targeted UT 为 6 passed，并使 prefix-on all-miss c8 达到 8/8、all-miss c16 达到 16/16。可是 warm prefix c16 仍只有 5/16 正常，随后发现 GQA/MLA/Mamba 共享 backing 的 KV 页布局冲突；四平面+late-H2D 的 UT 报告 57 passed，但用户要求释放四机且不带四平面改动，最终仅保留 late-H2D 文件差异，未启动最终组合验证。
- [GQA+MLA 共分组方案首次四机验证](blocks/9950bb334c6041e67df47de898daa27c6190561a1a32d412a4cb364c41034e41.md): `dc9e8375` 在 TP16×DP4×EP64、GQA DSpark、Prefix Cache 和异步调度下完成四机回归；cold/warm 各 16 条请求语义均通过，warm 命中率为 93.75%，未复现重复 `!`、空输出或乱码。旧 harness 的失败来自正文普通单词 `None` 的误报；随后 `/collective_rpc` 的 cache-spec 探查因 `torch.dtype` 安全序列化失败，仅是测试后诊断噪声，运行时 KV group 对象仍未独立 dump 闭环。
- [先单机后四机的精度验收与历史边界覆盖](blocks/8a4294b2f8860118ba6421cb388ad26f26266fb7c6bf4a2f7f43ad33be96a936.md): 单机 16 专家 overlay 在 rotation descriptor 修正后完成 cold/warm 16/16，DSpark 接受率约 70%；随后四机全量权重服务启动成功，并按用户要求扩展到 c16、c35、c64、1-token/42-task、block-size+1、128K 及后继请求等历史边界。旧 15/16 仍被确认是 `None` 文本误报；c35 高容量排队期间持续推进，阶段性结果均未见卡死或异常，但该块结束时四机全面矩阵尚未完全收尾，GPQA 仍跳过。
- [四机全面精度回归通过，但显存 A/B 尚未完成](blocks/0facc9b27017af7c752c7e28fc3ad9ceebaf5bbfd83a7ee22fb636a6342a5ed7.md): 四机历史矩阵最终报告 327 条正式请求通过，覆盖 Prefix Cache、c35/c64 容量压力、128K c1/c2、自然 GSM8K、多模态和工具调用；日志无 ERROR/CRITICAL、traceback、EngineDead、invalid-GM、HCCL timeout 或 NaN。强制 `ignore_eos=True` 的 K3 控制标记被保留为压力输出，不与正常 EOS 的 GSM8K 语义结果混同；服务和产物仍保留在 153–156，GPQA 未执行。后续 PR 整理把 Attention Residual 的任意非连续 stride 兼容改为完整连续 buffer 加 `num_valid_blocks` 契约，并新建 GQA/MLA KV 分组子 PR #14765；针对 0.26/0.27 多约 4.45 GiB 显存的单机 A/B 已锁定 exact commit，但因 153/154 仍有未知占卡进程而暂停，尚未定位最终来源。
- [0.26/0.27 单机显存 A/B 与环境排障](blocks/a190d88b91c89d67a30157d183ec98e91303b056789a248ce3f6b1b92b940f77.md): 在统一 TP16/EP16、DSpark、8192 profile tokens、capture size 16 和同一份 16 专家 QuaRot 权重下，旧 0.27 的 KV 余量为 30.05 GiB，0.26 为 33.40 GiB；0.27 权重反而少约 0.17 GiB，但激活/非权重开销多约 3.52 GiB，graph 均约 0.51 GiB。实验经历了 PGID 命名空间错误、SSH/VPN/代理不可达、PYTHONPATH 与 spawn 的 `acl` 丢失、tokenizer cache 只读、`dequant_situ_quant`/`libcust_opapi.so` 不匹配及单机 EP16 加载全量 896 专家导致 OOM；这些均不能作为版本显存结论。0.27 最终完成 graph 和本机 HTTP 200 请求，但裁剪权重语义不可信；0.26 服务可 ready，MLA prefill 请求因 key tensor 为 `RESERVED` 返回 500，因此运行成功与请求通过均需分开记录。
- [当前父 PR 的残留问题与 residual 显存复核](blocks/a7c9baff11ab7a82a113d7847920162d3c848e50144abb33b854e75dcbb51d84.md): 旧 0.27 对照显示 peak activation 为 4.23 GiB、0.26 为 0.70 GiB；当前父 PR 在隔离副本清理三处 FlashComm v1 残留后，激活峰值降至 2.19 GiB、KV 余量回升至 32.09 GiB，但这不是未经修改 current head 的直接结果。current head 曾暴露 `is_vl_first_layer`、两参数 gather、`maybe_chunk_residual`、256/4096 shape mismatch 等组网同步问题，诊断性删除后才完成 graph 和本机 HTTP 200；这些改动尚未推送，且不能把隔离结果宣称为正式修复。随后已开始单变量实验，将 residual bank 从全容量预分配恢复为 0.26 的 compact 追加方式，以判断剩余约 1 GiB 峰值是否由 residual 管理造成，但实验因 SSH 认证失效中断，最终显存分项和请求结果尚未取得。
- [双机裁剪验证与 profile 卡死定位启动](blocks/f1271dc904acb95230f0e7072a1a937caf3a6e34871c52fdae8f9c80db9a5c86.md): 围绕四机 GQA 全量验证，先清理了 155 上旧 0.26 服务，并以父 PR 全栈启动 DP4×TP16×EP64；r3 在完整权重加载后于 Attention residual Triton 的 8192-token profile 触发 vector core timeout，不能视为验证成功。将 residual kernel 恢复为 0.26 的 N/B/capacity 编译期特化后，单卡同形状算子测试约 2.49 秒完成且与 PyTorch 参考一致，独立提交为 `928a7012e`；但四机 r4 profile 仍在数分钟内持续高负载、未返回，随后按用户要求停止并改用双机 16 专家裁剪权重定位。
- [KV 逻辑分组已通过 TP8/TP16 与真实服务验证](blocks/1e78c0a61316bacda160dcf11ed8f790edd6b6a506680b118afe0fdd4ef9c7e2.md): KV grouping 已从固定 TP16、层数和 page size 改为按 ownership 语义动态分组：full-sequence target/draft attention 合并，Mamba 按比例均衡分组，local/sliding-window 等不同语义回退通用逻辑。TP8 实测使用 block size 768，得到 `[29,23,23,23]`，TP8/TP16、GQA/MLA、动态层数及 scheduler/worker 合约测试通过；双机/四机之外的 TP8 真实裁剪服务也启动并返回非空响应。此前基于错误 vLLM core 添加的 worker helper 已撤回，不能把旧 core 导致的 `None - 1` 问题当成当前 PR 缺陷。
- [四机 residual timeout 后转双机 DFX 定位](blocks/d812c76d128f5a7dc995674aef63ef5a6b88f2f1af59da89453c597c9e9eb0b9.md): 四机 r3 完成 352 shard 加载及 post-load，但在 DP2/TP0 的 `_apply_attn_res_kernel` 发生 vector core timeout，随后出现无效 stream allocator；因此尚未进入精度用例或 GPQA。修复 kernel 编译期特化后启动 r4，仍在 8192-token profile 长时间保持 64 卡 AICore=100% 且未返回，故不能仅凭无 fatal 判定通过。按新的验证顺序，已放弃先测单机显存，改为双机 DP2×TP8×EP16 的 16 专家裁剪权重；双机 profile 约 80 秒仍无 worker 返回，已停止并准备使用临时同步 DFX 逐层定位。
- [SP 共享专家多流卡死的定位与通信时序修复尝试](blocks/44f442ce83b1698c731e50e5b7828acaac048324f152b0ae0a4b6909b22ee8ec.md): 双机验证确认卡死特定于 `SP=true + shared_expert_dp=false + multistream_overlap_shared_expert=true`，而不是通用 SP、HCCL、MoE、权重或 task queue；恢复 `shared_expert_dp=true` 可通过但每卡 KV 降至 10.66 GiB，不能作为最终方案。首个“把输入 TP all-gather 放主流”的修复思路被纠正，目标改为让 TP AllGather/ReduceScatter 保持辅流，并与 routed dispatch/combine 错开；随后按“入口 AllGather→quant→共享专家首个 matmul→与 routed 路径并行→combine 后 ReduceScatter”的时间线实现。focused UT 在容器中由 8 passed 增至 9 passed，但首次 r9 硬件复测失败源于残留进程导致显存不足，尚未证明该版本硬件修复成功。
- [通信卡死已解除，但共享专家多流引入输出不确定性](blocks/b5297848645243404c583d7c4d0cac2bc5ac258dd6da229fafc95ccc1a8cc55c.md): 新时间线的 focused UT 为 `9 passed`；TP16 单机 profile 约 27 秒返回，peak activation 为 0.79 GiB/card，双机 TP8×DP2 约 0.93 GiB/card，均未复现卡死且保持低显存。四机全量权重服务成功启动，profile 和图捕获通过，四机本地 `2+2` 多数返回 4，但出现特殊 token 泄漏；进一步 A/B 显示当前多流配置 7 次中有 2 次泄漏，而 `multistream=false` 在四个 rank、20/20 次稳定返回 4。由此应保留“通信卡死已修复”的结论，但不能宣称精度通过；剩余问题已收窄为共享专家多流计算重叠，尚未完成下一轮针对性时序修正。
- [共享专家时序与 Mamba 网格修复验证](blocks/a9a487f9602ce352867c2807e5771ea9ddaf306cf45bcc7e783cf0c2c8662594.md): 通过错开 shared down-proj 与 routed combine，四机完整权重的高信号用例由旧方案 7 次泄漏 2 次变为 20/20，聚焦回归 62/62 通过；随后将 Ascend Mamba 的 temporal grid 从 `32×138×16=70656` 调整为单 temporal tile，避免 `coreDim > 65535`。单机裁剪权重 C32 为 32/32、无 EngineCore 退出，四机完整权重 C128 为 128/128，且无 `None`、乱码或特殊 token 泄漏。GPQA 后台运行期间仅完成部分题目，曾记录 26/198、146/198、166/198、178/198 等中间状态，最终状态在该块内未给出。
- [GPQA 中间结果与 PR 更新](blocks/390647ca13894afcf156084a11ef31c30bd339132ce983d16f7b6c7cfffed0a1.md): GPQA 曾观察到 draft 采信率 `17.49%`、平均接收长度 `2.224 token/轮`，并在不同阶段出现 92% 左右的中间准确率；进程运行正常但速度较慢，不能将中间快照当作最终 198 题结果。父 PR #14454 及 Attention、SiTU MoE、Hybrid Runtime 子 PR 已更新，明确 GPQA 仍在进行中，并计划进一步优化 CANN/Ascend Triton kernel，而非把 `_TEMPORAL_TILES=1` 视为最终性能方案。
- [四机性能矩阵与 DSpark 接收率回退](blocks/5638a467d96ee4b9d8d6914451c8e47db90f6425adfcd33ad91f3f80f10d1158.md): 按 `[16,32,48,64,80,96,112,128]` 捕获图完成 2K–256K、固定 1K 输出的四机矩阵；最终 r4 48/48 请求质量通过，但 30 ms TPOT 门限下只有部分长度保留 c1，16K 与 64K 的 c2 线性度分别为 0.487 和 0.435。随后用相同历史 8K prompt、清空 prefix cache 做对照，确认当前平均接收长度约 `3.226`、TPOT `36.92 ms`，显著差于 0.26 的 `6.529` 和 `23.20 ms`；初步根因是 0.27 删除了 GQA draft 所需的 materialized hidden-state hook，使 draft logits 偏离，而非 SP 或单轮算子吞吐变慢。
- [GQA materialized auxiliary stream 修复与全量性能矩阵](blocks/098fad8b44b92612705796927390b82cbbb859c5a7be9ddb9d50fa918035911f.md): 报告称恢复了 Qwen3 GQA DSpark 的 materialized attention-residual stream，同时保持 K3 MLA 为 raw stream；固定 8K/1K 两轮结果中，平均接收长度报告为 6.403、TPOT 18.20 ms，但这些仍是来源中的测试记录而非独立核验。随后完成 2K–256K、1K 输出的自适应并发 sweep；32K 因 `literal_none` 误报补测，64K/c4 与更高档位因 TPOT 超过 30 ms 停止，256K/c1 报告为 30.72 ms。
- [精度回归、harness 误报与四机释放](blocks/72cac8db3f00cde6b5c0902ac8c3cc479605fee6c68ffb6636ad37c9519dbcce.md): 来源报告性能矩阵达到 600/600 结构性通过，精度 focused 集为 62/62；短数学集为 128/128 结构通过但语义正则仅 126/128，两个 miss 后续串行复测 9/9，分别涉及 `5/8` 表达形式和输出截断。`literal_none` 将题面合法单词 “none” 误判为输出 None，曾导致 8K/c16 与 32K/c4 提前停止，后者已用修正规则补测；首轮精度 runner 还因 `ModuleNotFoundError` 失败并保留记录。来源最后称已按精确 PGID 停止 153–156 服务、端口不可达且 NPU 无运行进程，但这些是报告结果，不能升级为独立验证。
- [QuaRot 双机前的四机验证与 PR 评论](blocks/07934b2fa3f82958ac7de147578d31c3a2a758f50e258c890831b42e0e8122b9.md): 来源记录先确认普通 W4A8 非 QuaRot，随后找到并修正真实 QuaRot 权重自身的 `global_rotation` 配置，日志进入 `[spec_decode/quarot]` 路径。QuaRot 扫描报告 18 个稳定达标波次，并保留 8K/16K 临界档复测失败、DSpark 接收率和输出轨迹差异等不确定性；精度回归、PR 独立评论和四机释放均为来源陈述。记录还确认 `_finalize_pending_parallel_draft_rejects` 属于后来误带入 0.27 PR 的旧 reject D2H/event 链，计划删除而改用 canonical host sequence length。
- [旧拒绝链清理与评审前整体审计](blocks/20ffea028fa97cc7c9de0c0f1448b99fd0d75474a4816219ef074c63e9a6e9c0.md): 来源记录删除了跨 worker、attention 和 spec-decode 的旧 reject D2H、event 与 finalizer 链，并清理仅为 UT 引入的一次性 helper、不可达 vendor 分支和格式化噪音；算子侧则报告 106 个文件中 98 个与 0.26 实现一致。父 PR 与多个子 PR 的类型检查、pre-commit、mypy、review thread 状态和可合并状态均属于当时报告，不能视为独立验证；153/154 曾因 VPN 不可达而缺少当前 head 的 NPU 回归。随后只读估算认为双机可尝试 TP16×DP2×EP32、短中上下文，但 133K 配置可能容量不足；最新验证在 154 遇到 vLLM 0.26/0.27 镜像不一致，修正后又因漏同步 `_build_info.py` 导致 154 在建引擎前退出，当前尚未形成精度通过结论。
- [双机 TP16×DP2×EP32 精度验证启动受阻](blocks/3cb787806406a363da2a11b01bc93e90f7fa1aa5d1876f46005f128cb8af6bec.md): 来源记录对父 PR 当前 head `cbeac40f2` 进行双机完整 QuaRot 验证准备，确认两机 16 卡、共享权重和自定义算子编译产物，并计划覆盖 `None`、乱码、1-token、Prefix Cache、GQA DSpark 等用例。153 的 PR 当前 head 算子编译报告为 RC=0，双机服务也曾启动并确认 TP16×DP2×EP32，但这只代表启动阶段证据，不代表精度或性能成功。154 因缺少编译生成的 `_build_info.py` 在真正建引擎前退出，153 仍等待 DP peer；后续需要补齐部署产物、重启并重新验证。
- [双机 QuaRot 显存与 DSpark 安全性排查](blocks/25c73fe7a9603229082027d8692ed3399cf6297a1859908d5a479784468c50a3.md): 记录了 TP16×DP2×EP32、EP32、GQA DSpark 双机部署从 headless rank、权重加载到图捕获和真实请求的过程：`gpu_memory_utilization=0.98` 在第二档图捕获时因仅余约 229.79 MiB 而 OOM，`0.97` 完成图捕获但运行态仍偏紧。`0.961` 实测 KV 为 66,859 tokens、图后最差约 1,166 MiB，但 8,192-token prefill 因 DSpark 一次申请约 526 MiB、仅余 389.56 MiB 而 OOM；随后提出固定 2.625 GiB KV 与 `max_num_batched_tokens=2048` 的新配置，但其最终安全性尚未验证。期间还记录了 `num_prefill_lookahead=None` 接口错误、Mamba block capacity 10→17 修复及相关 UT 通过，之后用户要求暂停实验并转向 PR 审计。
- [vllm_ascend 大 PR 清理与 DeepSeek KV 契约争议](blocks/49299291e9850b8acba207a8437fa8d8798cbb97d5778fbf8c123eeee1854a05.md): 该块报告了针对父 PR `vllm_ascend/` 的审计和清理：据助手陈述，删除了仅供测试的字段、重复校验、loader hack、无关格式化及 CRLF 噪声，并声称生成了覆盖 28 个生产文件的全景文档和两个提交；但 CI 仍排队，聚焦 UT 受本机缺少 `torch` 影响，不能把旧结果当作当前 head 的验证。后续审阅聚焦 `kv_cache_interface.py` 的 DeepSeek V4 风险，确认讨论点包括 `compress_ratio` 是否由父类 `MLAAttentionSpec.merge()` 保留、`real_page_size_bytes` 是否覆盖 DSV4/V3.2 特殊布局，以及 K3 自定义 `scale_dim`；这些结论在材料中主要是解释与待核对的审计判断，不应视为已完成的 DeepSeek 运行验证。
- [GQA/MLA DSpark 分组权衡与父 PR 拆分方向](blocks/5d7cd9b7825f37baf84d4305d8accee3024c9f6c1d6f944d0a5ba8b7f0bafad3.md): 材料说明 vLLM 0.27 原生 hybrid grouping 对 K3 的 `24 MLA + 5 draft attention + 69 Mamba` 使用最小 bucket 5，GQA 形成 20 个 KV group/约 20 次 build，而 MLA 合并为 29 个 attention layer 后形成 4 个 group。当前 PR 据称将两类 attention 共用 scheduler ownership、把 Mamba 均衡为 `[23,23,23]`，使 GQA 变为 4 个 group、5 个 builder，但代价是统一物理 stride 带来约 18/116 的空槽；同时材料指出 MLA 的“4 builders”测试按 Python 类型计数可能错误，按 exact spec/backend 应为 5。最后用户提出按投机、PD 分离、prefix cache、Mamba、MLA、modeling、算子、KV cache 重新复用子 PR 或拆父 PR commit，当前仅开始依赖和独立测试边界审计，尚未形成最终拆分方案。
- [K3 大 PR 原子化重组与最新主线同步](blocks/2b409f97d54bb7fbc16a066c0a64f2abc2c059c5920ee696aaeef47b34e81d49.md): 材料记录了将父 PR 拆为 10 个职责清晰的子 PR，并把 Prefix Cache 定位为跨 Mamba、KV、投机和 P/D 的验证契约，而非独立代码 PR；随后各子 PR 曾基于最新 main 重建，DSpark 需要人工处理 breakable ACL graph wrapper 与 K3 drafter 逻辑的冲突。后续同步中报告了 vLLM PCP import、CRLF/Ruff、KV 测试格式等 CI 问题及修补，但部分独立 PR 的失败被归因于主线自身的旧 import 缓存问题，不能视为全部验证成功。最新状态显示父 PR/DSpark 仍需继续处理主线推进后的冲突，尤其是新增概率采样初始化测试、EAGLE3 `_maybe_remove_d2t` 与 QuaRot 共享层逻辑的并存。
- [Review 修复、ModelSlim 归属与 VIT-FIA 权重排障](blocks/82d337c7d2b4a1e3aeeaaf5980e64acfe55f71a22ff40e9a4ebf309a8338c51a.md): 材料报告了 rebase 后的双版本 PCP import 兼容、MLA/父 PR 的静态检查结果，以及 #14768 在 QuaRot source of truth、固定 GQA 拓扑、全局 `UniformTypeKVCacheSpecs` monkey patch 和 TP8 测试方面与 0.27 逻辑并不完全等价。另记录了 VIT-FIA 中 `key/value.contiguous()` 的遗漏补丁归入 #14600，及随后加载阶段因 checkpoint 含 `mm_projector.rot_proj.weight` 而描述符未声明 QuaRot 导致的 `There is no module or parameter named 'mm_projector.rot_proj'`；该问题尚未通过代码修改解决。针对真人 review，#14599 保留 ModelSlim 量化适配并改写标题/描述，不再拆独立 PR；同时声称已处理 SiTU 重复 forward、KV 契约和 P/D proxy 抽象等意见，但硬件回归受现有双机服务占用而未执行。
- [真人 Review 汇总与父 PR 冲突同步](blocks/4ca603a54d21cff96721d861ff829927f0da866b3e8c7d36145cc287e837c914.md): 材料将 25 条真人未解决意见按代码、PR 边界、解释和文档类别整理，重点指出 #14765 的 coordinator 参数、page-size 字段、Ascend MLA spec uniform 限制，以及 #14599 SiTU/ModelSlim、#14602 proxy 抽象等问题；随后报告已回复并解决这些线程，但这些状态仍属于过程记录。用户要求不拆 ModelSlim PR 后，#14599 改为承担 SiTU MoE、共享专家和 K3 ModelSlim 量化适配，并同步父 PR。最新推进中，DSpark #14601 已在新 main 上重放，但父 PR 集成仍遇 DSpark/KV 测试冲突；同时 #14839 暴露 `SimpleNamespace` 缺失导入和 PCP 兼容导入未使用等静态问题，需先修子 PR 再同步父 PR。
- [PR 冲突、Review 收口与低质量测试清理](blocks/6ed019ec695a6d6c55cd22cbc615ee2eec110240bbb4262068a75f0a10c2f529.md): 父 PR #14454 及多个子 PR 曾完成 rebase、冲突解除和真人 Review 回复，但后续审查又发现过时说明、失效 proxy UT、源码字符串断言、重复 helper 及错误的 `k_pe.contiguous()` 等问题，并进行了删除或重构；Gemini 意见被明确排除。旋转量化最初因错误的架构判断未迁移到 modeling，后来确认应由 modeling/load_weights 管理，proposer 只保留通用 hook；47 个定向行为测试通过，但未完成新的整权重 NPU 验证。AscendC 算子侧仅声称复用重复的 `IntegralConstant/IsSame`，未改变 kernel 流水或整体逻辑，不过仍需 A5 clean cross-compile 才能完成更严格的算子验证。
- [旋转量化归属迁移与 Review 再评估](blocks/3074dee34e95e8a2e5da6d4e553065416bda9963e92de91141d53723b64fec4b.md): 确认将 QuaRot 加载、rotation 保存、draft projection 转换及共享 embedding/lm_head 准备迁入 #14600 的 modeling，#14601 的通用 proposer 不再识别 K3/Qwen 或读取 QuaRot 文件；同时修复了 vLLM `ba07e4a` 下的 `WeightsMapper` 契约和一个无意义测试赋值。47 个聚焦测试通过，相关 pre-commit 被报告通过，但 coverage gate 可能因缺少 `ready-precise/ready-all` 标签而失败，不能等同于代码验证成功。另采纳了 `situ_mx_quant_common.h` 的模板去重；历史评论和空 pending review 也被清理，但整权重 NPU 验证因 153–156 有在用服务而未执行。
- [153/154 双机完整权重验证准备](blocks/6a928642daddd4963146c0353f332d7d13ab3741dbfde9495c8b273512ca792d.md): 计划使用 153 和 154 两机、`DP2 × TP16 × EP32` 拉起完整 QuaRot 主权重与 GQA DSpark draft，`max_model_len` 设为 16K，以验证 8K 输入/1K 输出的采信率；旧的两套对照服务已确认属于该验证目录并正常停止，32 张卡已释放。两机完整权重约 1.4T 且可读，GQA draft 一致；当前 head 配套 vLLM 0.27.1，但 K3 自定义算子的适配头和绑定层相对旧验证源码有变化。尚处于在 153 编译当前 head、再同步产物到 154 的准备阶段，尚未报告服务成功拉起、采信率或 NPU 运行结果。
- [0.27.1 启动链路与环境问题定位](blocks/3910cc07e75f6546e14e784fe4bf2ac1dd9375b3ba4003e54a67a87901d40732.md): 构建阶段曾出现 KDA 头文件 include 路径错误，修复后 `vllm_ascend_C`、算子注册和双机哈希检查通过。启动阶段先后暴露 CANN `PYTHONPATH` 被覆盖、未设置 `VLLM_VERSION=0.27.1`、`VLLM_PLUGINS=ascend` 过滤 `ascend_model` 等问题；这些导致 `acl`、PCP 和错误 K3 模型类相关失败，修正后 32-rank HCCL、QuaRot 权重和 GQA DSpark 均可加载。后续 `gpu_memory_utilization=0.80` 导致 KV cache 为 `-7.19 GiB`，将参数提高到 `0.95` 后服务越过 KV 分配并完成 graph capture，但这不等于整套运行时验证已完成。
- [轻量 runbook 与单路采信率验证](blocks/5d6571458d42b2bc7532cf57f27b11c8fe53c29755ba9d29dbeebfbd78e3d516.md): runbook 曾加入多层 preflight、源码哈希和 revision marker，随后按用户要求收缩为启动脚本内固定 CANN、`PYTHONPATH`、`VLLM_VERSION`、插件列表、拓扑参数和常见错误处理，删除独立门禁与哈希审计。修正后的双机 `DP2×TP16×EP32` 服务完成 345 个目标 shard、QuaRot optional shard 和 GQA DSpark 加载，单 DP 8K+1K 请求采信率为 75.55%/78.66%，未复现“采信率为 0”。两 DP 同时各一请求时聚合约 52.75%至 58.20%，低于历史四机同协议 76.81%，且低侧会在 engine 间交换；这提示 DP 并发调度、同步或路由问题仍未定性。
- [多并发采信率下降与后续四机定位](blocks/6a46e2766da13b23c90f67756107d4d809419bf3d2a4bb64b77b2b653ee1b0ee.md): 在冷 prefix、8K 输入和固定 1K 输出下，每 DP 2 并发（总并发 4）四个请求均成功，但聚合采信率仅 28.54%，engine0/1 为 24.64%/33.30%；每 DP 4 并发（总并发 8）未形成有效采信率，DSpark `propose_draft_token_ids()` 的 hidden-state `torch.cat` 申请 552 MiB 时 OOM，服务退出后已恢复。当前最可疑边界是同一 engine 内 batch size 从 1 增至 2 时的 DSpark 行映射或状态推进，而非已证实的模型精度问题；旧四机 DP4 每 engine 一请求约 73%仍只是历史对照。下一步计划使用已释放的 155/156 做 DP4/TP16/EP64 的单请求与双请求对照，但最新检查发现 155 仍有 vLLM worker、156 后 8 卡仍有 vLLM-OmniDiff，尚未确认可安全使用。
- [四机复现与状态复用假设被否定](blocks/37f5a83dc085fb45d8766adb5fac40e1960d4922f98d39ee9f7dff3e13469ffd.md): 四机 external-DP 拓扑已按 TP16/DP4/EP64 拉起；旧路由 header 曾造成 HTTP 200 但空流，修正后同一 DP、同一 prompt 的采信率仍会在约 79% 与 25%–33% 间翻转，最终 token hash 也变化。代码检查发现 K3 的 1 个 attention group 与 3 个 Mamba group 使用不同 block ID，但清零记录只覆盖 attention group；不过补齐所有 group 的清零后，首条请求仍仅 25%，因此“复用页未清零”已被否定并应回退。版本边界也得到纠正：vLLM `6e448d0...` 就是固定 `v0.27.1` tag，`cdc4824a` 才是曾用于 A/B 的后续 main commit；旧的正常基线实际使用了 `cdc4824a` 与 vllm-ascend `3053909a`。
- [正向 diff 收敛到 QuaRot 生命周期变化](blocks/b7714d2edbc19b20b01aa13281e2b63357bd33006fbbc8b77f275860edc72cf3.md): 正式验证已恢复到固定 vLLM `v0.27.1`（commit `6e448d0ea9bf...`）、`VLLM_VERSION=0.27.1` 和固定 import 路径，未设置 `VLLM_ROOT_OVERRIDE`；此前把源码 hash 误称为动态 main 的判断已纠正。与曾经正常的 `3053909a`/`cdc4824a` 组合正向比较后，共享专家串行化、GQA residual materialization 及 QuaRot 矩阵计算本身尚未显示为丢失，主要可疑点是提交 `43b14dbe7`、`0bafe6ed3` 将 target-owned QuaRot 状态从 proposer 移到 draft modeling/loader。当前日志只有共享 target 权重而无 QuaRot 对齐日志，提示 rotation 状态可能为空，但 draft 注册类和 `quant_config` 继承关系仍需核实，不能把这一假设当作最终根因。
- [检视意见支持将 QuaRot 收口到 load_weights](blocks/ea19dd921f961897b1a8abc5f28a274011b8f8349fd42908addf06bfa11003ae.md): 真人检视明确要求将 `_maybe_load_quarot_rotation` 与 `_prepare_unrotated_shared_layer` 移入 modeling 的 `load_weights`；因此仍由 proposer 回调 `prepare_shared_layer` 的方案 2 可能只完成形式迁移，职责边界仍有评审风险。当前判断方案 1 更符合意见：modeling 在加载阶段完成 `fc/context_proj`、embedding 和 lm_head 的基底转换，使其成为 draft 自有权重，并移除 proposer 中的 QuaRot 特例。实现仍需确认能否通过现有 loader 和 safetensors index 精确取得目标 embedding/lm_head，避免改成手工扫描完整 checkpoint 的新 hack。
- [QuaRot 边界权重对齐与采信率验证](blocks/2ac806e27364970fa7e41844b904836bd414a91a789ac3db53ee2baa5a802ceb.md): 方案 1 将 QuaRot 的 embedding/lm_head 读取、旋转转换和 draft 自有权重状态移入 modeling/load_weights，并删除 proposer 侧 hook；初版单请求采信率曾降至 24.23%–25.79%，后确认边界权重分片、旋转方向和数值误差均对齐，低值受采样波动影响。DP0 两并发当前 58.17%，历史 58.35%，相关 UT 为 46 passed；另发现 vLLM 版本固定、CANN/glibc 容器和启动环境会影响复现。后续 TPOT 首轮异常被初步归因于新并发 shape 的冷路径，#13954/#14915 合入及后续硬件验证仍在进行。
- [TPOT 首轮冷路径与算子合入](blocks/e354a391f24a6f459fafc981c6926fe922f4730c6819f34fae16c1209b8cd3bd.md): 按父 PR 的长度×并发矩阵复现时，2K c4 首轮 TPOT 为 31.24 ms、TTFT 约 34 秒，随后同服务热态 c2/c4 恢复至约 19.38/23.00 ms；同序热态 c1/c2/c4 为 17.09/19.26/27.99 ms，质量通过，说明首轮异常不能直接判定为稳定算子回退。#13954 的 KDA fused operator 与 #14915 的 MLA 增量已合入并完成 rebase，但构建先后暴露旧 binding 重复定义、schema 15/20 参数不一致及非幂等打包等问题，均在独立集成目录中修正。定向测试最终达到 108 passed，AscendC fused KDA 单卡数值用例 1 passed；远端连接和构建期间还遇到旧容器 CANN/glibc、SSH agent 等环境问题。
- [最新父 PR 采信率验证受启动断言阻塞](blocks/d666c06d74039e01b937ceb2f712f4fbc6b1f696943c0c6f801f1a800b686341.md): 父 PR 更新至 e895710c1 后重新部署 QuaRot 权重 + GQA DSpark：首次失败是 draft 错误继承 target 的 EP64 parallel_config，修复后已能通过 draft 配置校验并加载 target/draft 权重。第二次失败源于部署遗漏伴随库 libvllm_ascend_kernels.so，导致 custom op 整体禁用；补齐产物后第三次服务完成权重加载，但 KV coordinator 触发 `block_size must be divisible by hash_block_size`，尚未进入 curl“你好”100 token 或采信率请求。当前最新未完成事项是定位父 PR 的 KV 分组块大小契约并做最小修复，不能据此宣称采信率或功能验证通过。
- [Block5 MLA DSpark 适配与 MRV1 greedy 基线准备](blocks/39e9acda1fd3d40601cb306ed42b9eed4026298901ca9c302b5ebeac66eba8d1.md): 双机诊断确认此前 KV 分组失败源于 GQA block_size=128 与 K3 MLA/Mamba manager block_size=384 的不当相等约束；修复提交 `72f4a62d2` 后，K3 分组回归用例通过，但四机最终验证尚未完成。新 `[REDACTED_SECRET]` 权重被确认是 5-token、1M 上下文的 full-causal MLA DSpark；启动中先后暴露 K3 配置基类校验、QuaRot metadata 丢失及训练期 `confidence_head` 未过滤等兼容问题，已按上游逻辑处理。当前验证口径仍是 MRV1 + greedy + `num_speculative_tokens=5`，未设置 speculative `max_model_len`；服务正在加载 345 个 target 分片，尚无最终精度、采信率或性能结果。
- [Block5 MRV1 SP gear 从 LCM 修正为 TP 向上取档](blocks/24baf7efdf3cb07a8c6f6d6ea59c2bb01967b02287b3f3ffa208c7cc10febe1a.md): Block5 的每请求 verification 长度为 6，但后续核对认为不应把 graph gear 强制为 6 与 TP16 的 LCM；ACLGraph 可将 6、12 等真实 token 数向上取到 TP 对齐档位，并通过 virtual-request/FIA padding 补齐。此前提出的 `math.lcm(6,16)=48` 方案因此被撤回，r4 改用 `[16,32,48,64,80,96]`，并已通过 CPU 行为测试覆盖 `6/12→16`、`18/30→32` 及 `[6,6,4]` 类虚拟请求布局。四机 r4 已进入 target 权重加载，进度曾到 313/345，尚未完成真实 KV、graph capture/replay、精度或性能验证。
- [回退 LCM 与定位 KV block 污染](blocks/2abc9dded55a70f78e8c0875ff604a981646d0405228ada2a98d20441c6a0c80.md): 曾尝试移除重复的 `gear % 6 == 0` 约束、使用 LCM 档位并拆分 NoPE/RoPE metadata builder，局部测试通过，但随后发现 prefix-cache 的 `128 % 384` 断言，dummy 权重又因单机 OOM 未能完成复现。后续分析认为问题来自 draft 识别时替换 `VllmConfig.model_config`，触发 `refresh_block_size()` 将共享 cache 配置从 384 改为 128；相关 128 hash 粒度修复被否定，转而清理 config 替换、KV grouping 特判和其他排障弯路。
- [清理算子 PR 接入与单机容量限制](blocks/9a15a874d7400f0d798cd7972134cc2801d3c4486722e4244194f40ac08ca7ae.md): 将 draft 关闭 MLAPO 收敛为使用正式 `runner_type="draft"`，删除 draft `model_config` 替换、cache 防御复制、128/384 KV 特判及过时测试，并完成 rebase、提交历史清理和定向测试；153 上真实依赖的 82 项测试通过。单机 TP16 使用完整 896 专家权重在模型实例化阶段因约 60.48 GiB OOM，改用 16 专家裁剪权重后服务 ready、HTTP 请求和并发烟测通过，但裁剪 target 与完整 draft 的低采信率不具备精度结论。
- [完整四机启动与采信率问题追踪](blocks/b1bb5cb167f6ca1a4080315b3bdf89969c82c10d43fa7fcec8fd19a8be379682.md): 16 专家单机验证确认 KV 分组为 24 个 target 加 3 个 draft 共用 attention group、Mamba 为三组，48/96 两档图完成且输出链路可用；四机 EP64 随后成功加载完整 QuaRot 权重并 ready，显存和 activation 均在预期范围。完整权重短请求及并发测试的 DSpark 采信率仅约 1.27%–1.5%，后续发现旧进程/源码挂载和启动环境曾导致结果不代表 clean head，并定位到 `VLLM_PLUGINS=ascend` 错误过滤 general plugins；移除该白名单后 clean head 单机重新 ready，裁剪权重仍仅作为功能烟测，四机完整权重验证在材料结尾仍进行中。
- [QuaRot 元数据映射修复及 GQA/MLA 实测](blocks/c133a0b707a7189b4f86fa6b068a45500e6c3122a4f7a9ffa12ec20c784a2960.md): 定位并修复 ModelSlim 的 DSpark `WeightsMapper("" -> "model.")` 错将 `optional` 映射为 `model.optional` 的问题；这导致 draft 无法读取 `optional.quarot`，错误共享目标模型的旋转后 embedding/lm_head。修复后单机裁剪权重采信率由约 1% 提升至 46.0%，四机完整权重由 2.5% 提升至 33.2%/37.5%，相关 UT 通过并更新 PR #14454；但 GQA 修复后尚未重新用权重做实机 smoke。随后在 0.26 分支验证 PR #14768：GQA 固定 8K/1K 为 884/994=88.93%，接近历史 89.67%；新 MLA Block5 已完成 QuaRot 加载路径，但验证期间还遇到 JSON 转义、Gloo 网卡、运行库和图档位等环境问题，尚未完成最终采信率结论。
- [PR #14768 的 0.26 GQA/新 MLA 兼容性验证](blocks/ebd9ca7ff5c69c8b94833a78baf16f7fb932452e004ecd42acf92de11ac844f4.md): PR #14768 已在最新 `releases/v0.26.0rc` 范围内核对，现有 0.26 MLA/K3DSpark 路径和相关 83 项测试通过；GQA 实机固定 8K/1K 结果为 884/994=88.93%、AL 7.225，与历史基线基本同水平。新 MLA Block5 已识别为 `K3DSparkModel`，并确认 `context_proj` 反旋转、draft-owned embed/lm_head 和 raw MLA stream 均进入正确路径，但尚未完成请求采信率验证。启动过程中确认了 Gloo 网卡、native-compat 库和图尺寸的环境约束；Block5 的 5-token block 加当前 token 要求 `[48,96]` 图档位，最后一次验证仍在按该档位重拉，不能视为成功。
- [PR14768 四机验证纠偏与 MLA/GQA 适配](blocks/dfa7f7552fd119fc0824ffa5323075e54d9bc4d9839fc1793f5500d874331cdd.md): PR14768 先在单机 TP16、16 专家裁剪权重上完成启动与请求检查，但该结果不能作为精度结论；随后确认需使用 153–156 四机完整专家权重。MLA Block5 的关键最小适配是读取 checkpoint 的 `dflash_config.causal`，去除无必要的 `block_size` 运行时限制；SP 开启、共享专家 TP 开启且共享专家多流关闭时，单机路径曾成功捕获 `[48,96]` 图并消除 MC2 满核卡死，但四机 MLA 最终精度尚未完成验证。
- [PR14768 四机 GQA 复现与机器释放](blocks/7725aff1a4b2755188ad706b8d20f8a5ddbf90b452ce89e8bafb1843c09efc8e.md): 四机 DP4×TP16×EP64 的完整专家 GQA 服务已启动并完成 8K/1K 请求；早期单 DP 结果 AL 3.018 不足以判定退化。严格复用 0.27 的 prompt hash、greedy、7 draft token、四 DP 并发和冷缓存口径后，0.26 首轮 AL 4.0714、采信率 43.88%，但第二轮恢复至 AL 6.3190、采信率 75.99%，接近 0.27 稳态 6.37–6.45；首轮冷请求存在 DP 间分化，第三轮未完成。按用户要求已停止四机服务并确认 153–156 无 NPU 运行进程。
- [PR14454 CI 修复与持续监控](blocks/33ef75f298b8cc541ca53cb3a62008c3561c616f3c5b54480be7f9c0ca0f814a.md): PR14454 先修复 ModelSlim Ruff 格式问题及 310P `CATLASS_UNIFIED_CORE` 路径误用 `Fixpipe<float,float>` 的编译失败，提交 `9e37b8963` 后三平台 csrc 构建通过。后续 CPU、MLA、量化和配置兼容性失败被逐项修复，涉及显存计量、Mamba block-table 宽度、DSpark 权重版本、CompilationConfig、`custom_ops`、g_proj、FA-quant 和缺省 `use_mla_rope`，最新已知提交为 `8b9a0f3ee`。截至最后一次心跳，PR 可合并但新 head 尚未生成新的完整 E2E run；旧 head 的失败已知为已修复问题，不能宣称全部 CI 通过，仍需继续监控。
- [PR14454：重推后发现 MLA 类型判断与 no-RoPE 占位逻辑问题](blocks/7d050818337ca902fd6260665c5724edaee5b27c5b1afcfe2bc88ca850fc06ed.md): PR #14454 先因当前 head 未触发 E2E 而通过刷新提交重新排队；随后 A3-800I 日志据报暴露 DeepSeek V4 indexer 复用 MLA 类型但缺少对应 impl，以及 SFA 被过宽判断为使用 MLA RoPE，相关修复提交曾通过 ruff、py_compile 和 pre-commit。后续讨论认为 identity cos/sin 只是占位而非 no-RoPE 的正确实现，已据报改为 no-RoPE 使用 cos/sin=None、删除 identity getter、混组断言及其 UT；这些结果主要是对话中的报告，CI 当时仍在运行，不能视为最终验证成功。
- [PR14454：清理 identity RoPE 后修复 Mamba speculative block 容量](blocks/dbf8532896c9bda48786debf0b95be403347bffd5de991fd2d05feda87a5ed96.md): 讨论确认 K3 no-RoPE 应保留 use_mla_rope 和 target/draft 分组，但不应依赖 identity cos/sin；随后反馈指出 UniformTypeKVCacheSpecs 在分组后丢失 MambaSpec.num_speculative_blocks，导致 133120/384 从应有 354 列变成 347 列，接近满载时可能出现 shape (2,)→shape (0,)。据报提交 88398526b 改为按能力检测补充 inner-spec 委托，并将测试改为 354 列边界，同时审查 v1、v2、310P、scheduler、worker、proposer 和 connector；本地静态/pre-commit 检查据报通过，但运行态 CI 尚未完成。
- [PR14454：容量修复后的 CI 持续运行](blocks/b50514eaad14ed5c55b469791076b12c305abd3af87026189bf7749a4b59b8e7.md): 截至 2026-08-27 03:21，PR #14454 最新已知 head 为 88398526b，报告称无合并冲突，pre-commit、DCO、文档检查及部分 CPU/310P 检查通过。NPU 测试仍在运行或排队，当前没有报告代码失败，也没有进行新的代码修改；因此容量修复和整体 PR 仍缺少完整 E2E 结果。
- [PR14454 CI 收敛及 nightly SiTU 根因定位](blocks/6b728191b012d6f38bb7bfbd5e0defc32696708a0aca3b1de5a88fdc7a023431.md): PR14454 的 CI 曾因 uc-manager 拉取 spdlog 的网络问题失败，重跑后 42 项成功、6 项跳过，但 GitHub 合入状态一度仍为 BLOCKED；随后移除了 kimi-k3-execution-parity nightly 用例。手动触发的 nightly 尚未完成，但日志已显示 vLLM 0.27.1 的 BF16 MoE forward 临时创建 SituAndMul 时触发 `AssertionError: Current vLLM config is not set`，尚未执行缓存命中或输出精度断言。后续分析确认上游路由 MoE 通常直接调用底层 SiTU 计算，而普通 MLP/共享专家复用初始化实例；当前纯张量修复的 UT 通过，但未完成整模 NPU 验证。
- [SiTU 调用方式与 K3 graph eligibility 回归](blocks/dabb9edc44c97e86eaecbb62a6fea2d7df5e7931a7ced939967cdf5bd851480d.md): 上游 vLLM 0.27.1 的 `SituAndMul` 已注册为 CustomOp，但路由 MoE 的 `if SITU` 直接调用底层算子，不创建实例；因此 Ascend 侧应保持函数式分发，普通 MLP 才复用已有激活实例。进一步分析发现，PR 中以 `computed >= prompt_len` 作为入图条件会拒绝正常 PD handoff 的 `N-1` 状态，使一个 DP rank 的 NONE 扩散到整个 DP/EP group；CPU 对照复现了 0.26 与当前实现的差异，但现场具体触发请求仍未确认。随后修复了入图判定、accepted-token 快照重排读取错误和 DCP 重复扩容，并报告父 PR/子 PR 定向测试通过；这些结果不等于实机 PD 精度或性能验证。
- [框架历史修复、rebase 与新 head 的 mypy 失败](blocks/b6458b7b8377e17b22c7606d4ba7601ce18dc4cd4696576f67dc5b08544d5c3c.md): 历史对照显示，prompt 完成才能入图的限制曾在 0.23 被回退，后于 0.26 实验提交及 hybrid KDA graph-state 整理中重新带回；#12255 的等价修复尚未进入官方 main，但已由当前父 PR/KDA 子 PR 携带。父 PR #14454 与运行时子 PR 已 rebase 并推送，基线 head 为 `5e327cb7f6afe8dc81aefdfc706ec7400d7b3af5`，当时无冲突但多项检查仍排队。随后 E2E pre-commit 暴露了明确代码类型错误：`moe_mlp.py:725` 将 `float | None` 的 beta 传给要求 `float` 的 `_apply_situ`；这是需要修复的 mypy 问题，不能归因于 runner 的通用提示，后续修复/推送结果在材料中尚未给出。
- [PR14454 多轮 CI、DSpark 失败复跑及 MLA/A5 修复记录](blocks/20dc3be4ace919c78f19807f5ffdd3014eb5400bc34b04264110dec345e42003.md): PR14454 曾出现测试隔离导致的 MagicMock 日志路径 `FileExistsError`，修复 UT 后通过 67 项组合回归；DSpark DeepSeek-V4 acceptance 曾因第二位置 70.09% 低于 71% 通过线失败，但同一 head 的 attempt=2 复跑成功，未据此确认代码根因。随后确认 #14878 引入的 `cos/sin=None` 切片问题并以四处最小修改修复，相关 CPU 测试通过；又确认 A5 缺少 `FusedRMSNormGated` Ascend 注册，已接通现有融合 kernel，A3 数值测试通过，但 A5 性能收益尚未实测，KDA 子 PR 曾有冲突待处理。
- [A2 普通 PR E2E 用例设计待评估](blocks/5ab5c50afc492b5a865b294ce5e2669f4d5dc533470cb77b022a4a3a0d92b668.md): 用户要求先设计、暂不写代码或提交：在一个非 nightly 的 A2 普通 PR E2E 测试文件中覆盖 MLA 老版、MLABlock5 和 GQA 的 DSpark 变体。计划构造减层、减专家的权重并验证服务可启动及推理 2 个 token 不报错，层数和专家数可缩减至 16 或更小；随后用户提出可使用 dummy、避免真实加载权重以降低测试成本。该设计仍待用户评估确认，尚未实现或验证。
- [A3 TP16 DSpark 六组实现与阻塞定位](blocks/c402ec5bc0b10b0b8da9ae3ba18ca3466b2ff3849d860d8793718b5b9a389b5e.md): 已按 A3 单机 16 卡、dummy 减层减专家方案实现并验证多组 DSpark 功能；旧 MLA、GQA＋W4A8、Block5、DP2/TP8、MTP 图片和 P8/D8 均曾暴露或解决具体问题，不能把中间失败当作最终成功。DP2/TP8 的 MC2 `x_active_mask` 行数错误参考旧 FlashComm 的 DP 补齐语义修复并通过定向检查；P/D 退出时的 `corrupted size vs. prev_size` 可脱离 vLLM/K3 在 `TransferEngine` 导入中复现，未纳入 vllm-ascend 绕行修复。最终六组回归据报道全部通过并提交 `8c8deeddd`，但后续矩阵压缩和 CI 结果另有更新。
- [DSpark 阻塞修复、四组压缩与提交](blocks/9de5643ee75f2a9016610489fbaeae74b2e252c37a08abe558218b497c4f379a.md): GQA＋W4A8 TP16、Block5、DP2/TP8、MTP 文本图片及 P8/D8 的阻塞分别涉及量化描述、MC2 mask 补齐、共享 `lm_head`/多模态接口和 Ascend decoder tensor 返回接口；修复后相关功能据报道通过。P/D 的 Mooncake allocator 堆损坏仍被确认是外部 `mooncake-transfer-engine-npu==0.3.11.post1` 可独立复现的问题，未修改 VA 代码。六组被压成四组后实测 5 分 57 秒，仍未低于 5 分钟；随后清理 30 个 UT 文件中的低价值重复检查，保留数值、KV 容量、旋转权重和 P/D 状态覆盖，提交 `ba14b852d`。
- [UT 清理、EAGLE 临界复跑与 PR 合并](blocks/203a0f9a3053cf655579c0d3affc95797b8f9087c4a5447a341df73c428f97a9.md): 四组压缩回归全部通过但耗时为 5 分 57 秒；CPU UT 的裸 `__new__` fixture 因缺少 `supports_mm_inputs` 失败，补 `False` 后定向文件 23 项通过。进一步审视 30 个 UT 文件，删除源码字符串、重复构造/参数转发和冗余 mock，净减少 1162 行，定向验证为 490 passed、5 skipped，并提交 `ba14b852d`。该 head 的 EAGLE A2 首位 acceptance 仅 0.3294 对 golden 0.43，attempt 1 失败但 attempt 2 成功且未复现；PR #14454 随后已合并，子 PR 已关闭，父 PR 描述已移除子 PR 矩阵。
- [双机 GQA 采信率异常与 32K 差异定位](blocks/49e22e92d00bfc9388098d05dff00d70a12132f8fe96a47a1991474fdb9b6e02.md): 围绕 PR #14454 合入版本在 153/154 双机上的 GQA 采信率复现，确认历史 8K 的 58.17% 仅是 DP0，四 DP 汇总为 31.18%；同脚本当前 DP0/DP1 为 34.68%/59.37%，复跑还出现 DSpark hidden states 申请 552 MiB 的 OOM及后续 Gloo 断连。32K/1K 单并发在保持 `max_num_batched_tokens=8192`、prefix 命中 93.75% 的条件下，总采信率为 73.90%，低于历史 80.41%，某输入交叉复测降至 17.25%，但尚未证明根因。代码差异重点收窄到 KDA prefill、融合 RMSNorm gate、Mamba accepted-count 快照/状态回写；QuaRot modeling 加载、MLAPO、概率采样等未被确认是本次路径原因，后续计划整理四机复现数据与交接包。
- [四机交接资料与长序列 prefix cache 调查](blocks/f4790846396726f10437db431970bd8792372107b6e6bcaca6b1cb50b04dd8cb.md): 已整理四机非旋转/旋转权重扫描、2K–256K 临界点复测、0.26/0.27 GQA 复验及 43 组 token 输入；多轮冷缓存 8K 合计采信率为 66.71%、76.70%、77.79%，部分标为冷启动的记录实际 prefix counter 已显示 93.75% 命中。新集群发现异常仅在大于 96K 且命中 prefix cache 时复现，初步对比未见最终合入后 K3 KV 分组、MRV1 布局或 DSpark 文件再次变化，但历史节点到最终版改过 Mamba 状态写回、block-table 容量，vLLM 版本切换也可能改变缓存管理。基线已修正为最后一次采信率正常的 GPQA 128K 记录；已知 8 月 23 日 GPQA 仅有 17.49% 中途采信率，不能把 179/193 的答题正确数当作采信率正常，真正基线仍待找回。
- [四机 vLLM 版本对照启动与兼容问题](blocks/10574f0768149bbc4c86838f0ccdcd2d4c718baf4b4084b27b49b3dcb89d6227.md): 围绕 vLLM `cdc4824a` 与 `v0.27.1` 的四机 128K/1K、GQA DSpark 对照展开，固定完整旋转量化权重、每 DP 单并发、缓存预热和单档图捕获。启动期间出现 `libvllm_ascend_kernels.so` 缺失、`_C_ascend.moe_gating_top_k` 未注册、Qwen3.5 辅助函数缺失以及 155 节点显存被外部服务占用等失败尝试，均被保留且未计入结果；后来通过隔离目录、清理进程和有限兼容处理重新启动。该阶段尚未产生 vLLM 对照采信率，仍处于加载、通信和图捕获过程。
- [vLLM 两版本四机采信率实测](blocks/d359c4569ec93ba3e7b6388339d7cdf151dbe59157583b931a4086697b150847.md): `v0.27.1` 两轮总采信率为 55.12% 和 50.75%，缓存命中率均为 94.63%；逐 DP 低值会在不同轮次迁移，不能归因于固定 DP。`cdc4824a` 经历 `confidence_head=None`、`num_prefill_lookahead`、`calculate_kv_scales` 和 Mamba `TEMPORAL_TILES` 等接口兼容失败及隔离适配后，两轮采信率为 86.25% 和 62.65%，加权结果 72.86%，平均接收长度 6.10；`v0.27.1` 加权结果为 52.87%、平均接收长度 4.70。两版缓存命中率一致，但旧版第二轮 DP2 仅 31.91%，因此不能认定版本升级是唯一原因，也未证明物理 KV 损坏。
- [历史 3053909aa 复现及 fused KDA 回退实验](blocks/34ac6f3d71bc1c7ee58757d953c4d434e13601354bb8479d3ced1d20b70b18c6.md): 四机恢复了 `3053909aa + cdc4824a` 的历史源码、启动参数和旧算子，但发现版本选择残留、155/156 缺少 `aclnnDequantSituQuant`、遗留 `_C` 扩展触发 `aclnnRecurrentKdaGetWorkspaceSize` 段错误等环境问题；重编绑定和恢复 vendor 后才继续。完整历史栈五轮 128K/1K 请求全部返回，但采信率分别为 39.28%、71.79%、47.70%、51.69% 和加权 50.67%，因此该历史版本也不是稳定正常基线。材料先误称历史版本“不带 fused KDA prefill”，随后更正为：历史版本仍带旧版 AscendC `chunk_kda_fwd` prefill，后续 `6c039a1b6` 才引入更融合的 gate、状态布局和 prefill 路径；最新 main + `v0.27.1` 的回退组已准备但尚未产生结果。
- [正向数据流定位：缓存状态一致，KDA 数值差异出现于 aqk/akk](blocks/6de59280b413c83b964be9ed4f72dbdf9f03d331e9fddf1fa65afeedc87f120f.md): 诊断请求显示四个 DP 命中 124,032 token，69 层 KDA 恢复的初始状态与预热写入状态逐元素一致，且缓存命中率异常不能单独解释全部低采信率：同一用例冷缓存下也出现约 30% 的低值。首层真实张量中 Q/K/V、beta、初始状态和累计 gate 一致，但 `raw_gate` 最大差异为 0.0078125，来源被追到 BF16 `f_a` 投影在不同批次形状下的舍入差异；替换 replay 中的 gate 可使状态和输出逐位对齐，但尚未证明足以解释全部退化。进一步的新旧 KDA 对比定位到第一处中间差异在块内相关矩阵 `aqk/akk`，随后传播到 `w/u/v_new/h/final_state`；当前待用 FP32/FP64 参考公式判断新旧实现哪一侧偏离，并关联长前缀采信率。
- [双机32专家复现与 idle dummy 状态污染线索](blocks/8184b6d150694a611600a495898d9ce54a5bad93d67fccaf8991ef7261f9027a.md): 在 `0a023b094`/vLLM `v0.27.1` 上启动 TP16×DP2、EP32 的32专家双机验证时，先后遇到 custom-op 包选择错误和 `libopapi.so` 搜索路径问题，修正后 profile 才通过。历史问题被重新关联到 DP idle dummy 调用 `_dummy_run(1)` 推进 GDN/KDA recurrent state；14454 保留了 builder 的零长度处理，却漏掉了 ModelRunner/Worker 的调用链。修复 `skip_gdn_state_update` 后，单 DP 128K cold/warm 追加3轮均输出一致、采信率约96.13%–97.49%，但首轮曾有 cold/warm 开头偏移，因此仍保留不确定性；随后启动四机完整权重验证。
- [idle-dummy 修复的双机 A/B 与四机验证启动](blocks/1ddc6e0a71c7ac443962ad69edb57ca02e6370fce6f224a3234136925ddc1479.md): 双机修复版中，修复前 DP1 的128K warm 请求曾从首 token 变为重复 `1.1.1...`，reset 后恢复；修复后连续多轮 cold/warm 1024 token 一致，支持“idle dummy 推进 GDN/KDA 状态”这一候选根因，但不是形式化证明。修复按0.26提交 `3cd7136db` 恢复显式 idle 语义，保留 dummy token 参与通信，不增加全卡同步；同时确认 builder 无法仅凭形状或 block id 区分真实1-token decode与 idle dummy。四机 TP16×DP4/EP64 验证已启动，PR #15253 已创建并包含该最小修复。
- [四机采信率初步结果、外部终止与 DSpark 入图准备](blocks/57076afea31d66385043f3934e9151a5e21530fbb085aaa9ff782bc5aef47c40.md): 四机完整权重服务成功 ready，前两轮历史128K脚本的聚合采信率为85.40%和87.00%，各 DP 未再出现旧的27%–46%塌陷；第3轮中155的 EngineCore 被外部 Docker exec 以137/SIGKILL终止，故四机验证未完成，不能将该轮视为功能失败或成功。PR #15253 后续 CI 唯一失败被定位为 Mooncake 端口39783占用，已触发 failed-only rerun，但 runner 长时间排队。之后开始参考 #14985 支持 DSpark 入图，固定叠加 #15253，保留 N→(1+N) 图桶、capture metadata 和动态 context-KV 出图逻辑；154 容器定向 UT 为55/55通过，但真实 GQA 启动曾因 `PYTHONPATH` 覆盖丢失 ACL 路径，随后又受153–156 SSH 握手前被远端关闭阻塞，GQA/MLA-Block5 实机验证尚未完成。
- [DSpark 单机与四机 GQA 入图验证](blocks/f2f303ba13fa1d7ca3f11323b3fe1ebfac1e2d9a403c409b81ef93eb99395ea0.md): 单机 GQA 与 MLA-Block5 曾暴露权重描述符不匹配、ChunkedPrefill 元数据、KeyError: 40 以及 block-table 行数不足等问题，随后通过旋转量化权重、SpecDecoding 标记、固定 48 档 attention tensor 和 max_batch_size buffer 修正。单机两种模型均完成 ACLGraph capture/replay 与真实请求；四机 GQA 使用 DP4/TP16/EP64，修复了 PATH 缺少 ninja、源码目录遮蔽标准库 platform、DP1–3 必须 headless 等启动问题，并完成权重加载、draft graph capture 和 API 启动。运行进程实证加载了 vllm_ascend_C、libcust_opapi.so、CANN 9.1 libascendcl.so 及相关 ACL/HCCL 环境变量，但该块末尾仅记录开始服务端 replay 核验，不能单独据此确认四机 GQA 的最终采信率。
- [四机验证完成与 PR 对齐测试启动](blocks/85ac7547c8f215c8746b290cd395c9b7363a0bb984318cdaf2a82e4d4cd02818.md): 后续材料声称四机 GQA 与 MLA-Block5 均完成 4 个 DP 的 ACLGraph replay，8 次 9.6K 输入正常返回、prefix-cache hit 约 42%，并报告 51 个定向测试通过及 Ruff、格式检查和 diff 检查通过；这些是来源中的报告结果，应保留其验证口径和不确定性。随后创建了 vllm-project/vllm-ascend#15334，并开始按 #14454 评论区的 external-8k-c2、8K+1K、每 DP 2 并发等历史参数做采信率对齐；CI 曾因标题 `[Feat]` 不符合仓库规则失败，改为 `[Feature]`，不能将其误认为代码失败。最新状态是四个独立 API 已用当前 PR 源码启动并进入完整权重加载/profile/capture，历史口径的重放与对齐结果尚未给出。
- [GQA DSpark 图边界越界与四机重跑准备](blocks/a4b22d40c047744bebd78da3f2ad323a02df91db6de8cad2de0575141d71fd93.md): 四机环境曾因 `nsenter` 不继承 `LD_LIBRARY_PATH`、CANN `set -u` 和 `libop_plugin_atb.so` 缺失而失败，改用容器原生环境后完成 64-rank HCCL、权重加载和图捕获。GQA DSpark 在 DP 不均衡或图档位切换时出现 `index 98..111 exceeds bounds 98`，排查指向真实 token 数、padding token、query slot 与 graph bucket 语义混用；单机固定 14 请求未必触发，问题更可能与多 DP 局部 batch 切换有关。后续请求改为使用 GQA DSpark + QuaRot 旋转量化权重，并按 #14454 的 30ms mean TPOT 口径补跑性能，但本块末尾仍处于环境和复现准备阶段。
- [DSpark padding 契约修复与四机性能验证准备](blocks/79c656e80f0a99cb77dba54b10fd9003d29de0633de564f296c8730aa1117a22.md): 对比显示 K3 GQA 与 GLM GQA DSpark 都采用 anchor-first、7 draft token；GLM 现有用例因 `enforce_eager=True` 未覆盖 draft ACLGraph padding 路径，因此共性问题尚未被其通过结果证实。诊断定位为选图应按 `batch×(7+1)`、KV/attention 的 `num_actual_tokens` 仍按真实 `batch×7`，修复后 32 请求转换连续三轮 32/32 成功，相关 UT 报告为 49 passed，但这些是来源中的阶段性验证。随后准备按 #15334 口径跑四机矩阵时，因新源码目录未链接含 `dequant_situ_quant` 的 `vllm_ascend_C` 扩展而在 profile 阶段失败，需修正运行时算子产物后再测试。
- [#15334 四机性能结果与 #15336 正规接入启动](blocks/eb27e7e50826ef95d98b77cf9e27534a4d7c5bd0e76a72af2b2129604c54fde3.md): 来源最终声称四机 QuaRot GQA DSpark 矩阵完成：2K–256K、固定 1K 输出、95% 公共前缀、30ms 平均 TPOT 门槛，共 29 个波次和 464 个请求；报告的最大每-DP并发为 2K/8、4K/4、8K/8、16K/4、32K/2、64K/2、128K/2、256K/1，并称已贴入 #15334，但完整指标仍应视为待复核的来源数据。新的任务要求以 #15334 和 #15253 为基线完善 #15336，只保留其 csrc tiling 下沉能力，丢弃非 csrc Python 实现，正规替换 GQA/MLA 的 FIA，并先完成单算子数值对齐和单机验证后再重跑性能。当前已在隔离目录接通设备侧 metadata/AICPU `actual_seq_lengths` 路径，尚未报告单算子或单机验证通过。
- [FIA 算子下沉：编译、注册与 GQA/MLA 单机验证](blocks/38c89fbeee940c5923ac2cb7f625768b8533fa41bea208f51367b80259308e03.md): 首轮构建受 macOS AppleDouble `._*` 元数据误读、binding 产物复制路径错误及隔离环境缺少既有 `dequant_situ_quant` 算子影响；清理元数据、修正路径并复用完整 custom-op 构建后，两个 FIA 算子成功编译注册，GQA/MLA × FP16/BF16 四组数值对齐和旧算子交叉用例通过。#15253 所需的 Mamba speculative block 容量修复已在当前代码中，`133120 / 384 + 7` 实测为 354 列，因此未重复加补丁；若服务仍出现 347 列，仍需核查实际加载版本或其他 sizing/handoff 路径。GQA 与 MLA Block5 均完成 16 卡单机启动、profile/graph capture 和真实 128-token 请求，报告的采信率分别为 87.3% 与 85.6%，但首条空 JSON 请求产生的 HTTP 400 错误必须与模型/算子错误区分；四机首轮曾因 154 缺少 `set_env.bash` 中止，补齐并通过四节点算子可见性预检后已重新进入组网和权重加载，四机性能结果尚未完成。
- [DSA式 metadata builder 集成与回退](blocks/7dc46c386be89fa098aa5370a7146413d3c01b27ebad780d03ef4a88f632014b.md): 四机启动环境、旧 vendor 路径、HCCL 初始化和算子符号问题均曾导致失败，随后修正并完成完整权重、8 档图捕获及真实请求验证。按用户建议，metadata AICPU 已从图内移到 builder，由 builder 持有固定 `int32[1024]` 输出 buffer；GQA/MLA 单算子和图回放曾 7/7 通过。额外引入固定 `int64` length buffer 后触发 507018/AICPU 22007，已回退该额外改动，最终服务重验证当时仍在进行，不能据此宣称最终四机结果。
- [ABI 生命周期核对与四机性能启动](blocks/5ca443110bb6c263415d582cd4d50344ec9acd3b6a2114a744eb9839eb7962ad.md): 对照 DSA v1 的 SAS/QLI metadata 后确认 FIA metadata 输出契约为 `int32[1024]`，而长度输入 ABI 为 `int64`；固定输出 buffer 必须保留，长度输入需遵守 call-local 生命周期，不能靠 dtype 转换偶然产生新存储。最终单机 GQA/MLA、16 卡图回放和真实请求通过，并已补充 #15334 四节点脱敏启动命令、更新 #15336；四机同口径性能 sweep 首轮在 154 的 2K 图回放触发 FIA sink `MTE accesses an invalid GM address`，因此首轮没有可用性能表。随后按请求数独立缓存稳定的 q/kv length buffer 修复跨档位问题，之后性能数据才显示明确收益，但仍需保留各档位退化与 CI gate 标签限制。
- [跨图档位 descriptor 修复、性能收益与后续 D2H 审计](blocks/758dc5e5b723c7f235e78e2fec420392ee049198f45376d3d1b4205d8e29e675.md): 固定 `int64` 版本先在第二个图档位因 MLA metadata 的 batch=16 与 block_table batch=14 不匹配而失败，进一步定位为 ACLNN 静态 descriptor 复用同一 backing storage；按请求数使用独立且地址稳定的 buffer 后，单算子 8/8、单机 112/128 图、四机 8 档 capture/replay 和 624/624 请求通过。四机 2K–256K sweep 报告 4K、16K、32K 高并发 TPOT/ITL 与容量收益，同时保留 64K/c4 和长序列未过 30ms 的退化或未通过情况；#15336 已更新并贴出 MLA/GQA 退化输入，ci-gate 仍因缺少 `ready-precise` 标签失败。最新任务转为审计 tiling 下沉后 K3 的 `seqlen`/`num_accepted_tokens` 消费链，判断 model runner 的 D2H 是否可删；当前仅确认需覆盖 scheduler、图档位、KV/prefix-cache 和 speculative decode，尚未形成删除结论。
- [#15336：区分 attention tiling 与 Mamba 状态同步](blocks/61bc34dc4764b64864062985ab55dcaf805d37eedf063ba40c2e25aaa88485ef.md): 审计显示，GQA/MLA sink fast path 的 `seq_lens` D2H 可移除，但 metadata builder 仍有 `.tolist()`、CPU list 到 NPU 的绕路；应先完成 builder 设备化，再按 builder capability 缩小 model runner 的精确 CPU 同步范围。`num_accepted_tokens` 不能整体删除：GDN 已直接消费设备 Tensor，但 Mamba 仍依赖 NPU→CPU planning→NPU 回路，需设备化 accepted-count 重排、state-copy metadata 和 `accept_token_bias` 后才能改动。用户要求按此边界实施，并先在 153、154 单机验证，确认无问题后再进行四机验证；截至材料结尾尚未修改代码或完成这些验证。
- [GQA/MLA sink 下沉与四机 GQA 验证](blocks/53d95339303f13e5b1475339a8c836892e314122a57bbce4fba5ff7a44169265.md): FIA/GQA/MLA sink 路径改为消费 device 侧长度，fallback、CP/PCP、compressed attention 和 Mamba 原有同步边界曾被保留；153/154 的定向 UT、GQA 与 MLA Block5 单机请求均报告通过。四机环境经历旧 runtime 缺模块、156 HBM 未回收、进程组清理和 NFS 加载等问题后，GQA 完成 8K 并发、32K 单并发验证；但四机 MLA 的 uneven-DP 图捕获仍反复出现 FIA sink 校验错误，未形成成功结论。
- [Mamba accepted-token D2H 消除与单机回归](blocks/b60f68974fe9907f1a7aa08e117e6de77024e8471d9aed4054dce852ac4ac99e.md): 在暂停 MLA DSpark 后，Mamba `num_accepted_tokens` 从 postprocess 到下一轮 precopy 改为设备侧传递，复用既有 fused postprocess/precopy 和 event 依赖；同步调度、310P fallback、非 align 路径仍保留 CPU 契约。153/154 各报告 52 个定向测试通过，并完成 8K→128、跨 384-token block 的 8K→512、并发请求和 prefix-hit 单机回归；这些结果属于该批次报告性验证，后续四机性能 sweep 仍需区分暖态、长尾与组合收益。
- [四机性能 sweep、ITL 对比与 NaN session 调查](blocks/3215e5e8d11482262b37ba0b51833e4e1a30ee3bbdf46f68d76556cbbcbffb3c.md): 四机 GQA sweep 共报告 528/528 请求完成，覆盖 2K–256K 及多并发；2K/C4 首轮 34.61 ms 经暖态复测为 17.03 ms，256K 最大达标并发为每 DP 2，8K/16K 等长度仍有容量和长尾波动。相对 #15334“仅 DSpark 入图”基线，组合改动（DSpark 入图、tiling 下沉、Mamba D2H 消除）的 ITL p50/p90 平均改善被报告为 6.31%/6.74%，但不是单独 D2H 收益，且数据发布、rebase 和 PR 状态均为来源中的报告。另一个 session 的调查发现关闭 prefix cache 时 NaN 已在采样前 hidden states 出现，混合批次的新 prefill（第 7 层、73 个请求）整段 NaN，而已有 decode 正常；根因仍未完成确认。
- [chunk_kda_fwd 变长批次 NaN 与临时内存越界线索](blocks/ff3250a1dc6c151404a3cd4d96ac5df37c6b77bcd4f91f957f799f9a68d2c515.md): 独立复现显示，有限输入、零初始状态下，`chunk_kda_fwd` 在 127/128 条变长序列时输出和最终状态出现 NaN，而 32 条及以下正常；该现象可在无 prefix cache、DSpark 或服务调度参与时发生。报告指出，Post-WU 按每条序列补齐到 64 token 写临时内存，但分配仅按实际 token 总数计算：127 条输入约需 23.8 MiB，实际约 13.8 MiB，越界可能污染后续 `uSeed` 缓冲区。容量修正后同一失败输入据报告连续两次恢复正常，但完整服务回归仍未完成，因此应保留为已定位的源码错误线索与部分修复结果，而非最终验证结论。

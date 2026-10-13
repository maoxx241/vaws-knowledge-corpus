---
blocks:
- block_id: 8e48137c07e26dda8bc4b0c9ded0273b8a7dbec9f1d78188ae9710d067202f55
  indexed: true
  sha256: 0c102255d2fc4e146039677e053587194ec7eb790312279ff2163f158c984bfd
  source_range:
    end: 10913763
    part: 0
    start: 0
  summary: 当前候选方案为 vLLM 0.26 原有 raw-page 机制新增第 4 条连续 lane（GQA-V），形成三类 page、四条 lane；四平面静态测试 43/43 通过，但 r5 无 DFX 的 c32 仍稳定失败。r6 中 step2 的 64/64 TP rank 停在首个 KDA 前 TP all-gather，shared-expert 多流 collective 竞跑被列为强候选，但 DFX watchdog 对现场有扰动，不能把该位置直接视为根因。既有验收矩阵覆盖卡死、精度、Prefix Cache hit/miss 和采信率，要求保留失败、hash、日志及每轮 manifest，且未完成的远端正反向验证不能标为成功。
  title: 四平面方案与 c32 卡死基线
- block_id: 676d1c48d3ca0ee37775bac9c08398653b4cdcd05853b4fd870057b2cd18b737
  indexed: true
  sha256: 9443493ee8a2ebf08b4c8b126c5ce92bad925f347c442ba0c42b77d126715f30
  source_range:
    end: 10913763
    part: 1
    start: 0
  summary: 静态审计确认 DSpark 短请求路径不走 zeroer、默认 384 对齐场景不触发 Prefix CoW，ACLGraph 也不是短 c32 的直接来源；但 plane-major CoW helper 仍是 prefix_match_unit<384 时的潜在通用缺陷。r7 的 async-off 冷启动仅完成 3/32，随后 29 条请求均卡在首轮 execute_model，说明关闭 async 不是修复，也纠正了“仅少量请求入队”及 draft/output-copy 为必要原因的判断。分析进一步把重点收窄到首个 target forward 内的 DP metadata rendezvous、shared-expert/EP64 collective 顺序和实际 cudagraph mode；r8 正在以 async-on、shared-expert multistream 关闭进行对照，结果尚未完成。
  title: 静态排除 zeroer/CoW 并聚焦首个 target execute
- block_id: 01ab1717eb6a39b90ca962f04316f1c86d9dda9cd2760e52c40607ba91f2c389
  indexed: true
  sha256: 3237b1ff245a9973ee6fdba812b290f467899911bf3cca8df32771fcba84c602
  source_range:
    end: 10913763
    part: 2
    start: 0
  summary: 针对 K3 将 TP all-gather 放入 shared-expert side stream、可能与默认流 EP64/MC2 竞跑的假设，建议在默认流先完成 shared TP all-gather，再进入 routed collective，同时保留 shared MLP overlap；需要 async-on/multistream-off、pre-gather 和原 side-gather 三腿对照，且批次、路由及源码哈希必须一致。r7 已排除首个 execute 前的 draft GQA、Mamba post-sample 和 output-copy 为当前 H0 必要原因，但 shared-expert stream、DP metadata 和 FULL graph 仍未被实验闭环。验收脚本已审查并修补，纳入 reset/no-warm miss、双 warmup hit、token IDs、严格退出码和采信率判定；本块末尾的具体修改文件列表不完整，且明确未发送远程推理请求、未修改生产源码。
  title: shared-expert pre-gather 候选与验收脚本状态
- block_id: 164bab5d427234858b656f60e20a31333ba6ec24913e4aa7109776e039c26799
  indexed: true
  sha256: 9071d9daa25cd90df3a5432de3ac35a585743532b7cbd6707ed4596467be03cb
  source_range:
    end: 10913763
    part: 3
    start: 0
  summary: 材料给出四平面版本的三轮 hit/miss 验收脚本，包含精确 warmup/replay token 数、output 证据、DP/跨轮采信率、源码与数据 SHA 校验；这些是执行约束，不是已完成的验证。历史审计将旧三平面 compact GQA stride 与 MLA block 发生物理 backing 冲突联系起来，并指出四平面共享 scheduler physical block ID 后可避免该公式碰撞，但仍需复现历史压力链确认。随后列出两个未证实的 Mamba 风险：metadata H2D lifetime（`prepare_inputs_event`、`copy_bufs.offset`）以及 `global_stream` event 等待过晚导致 accepted-count/block-table 生命周期竞态；已有 A/B 数值并未证明它们是历史 `all!` 根因。
  title: 四平面验收矩阵与历史 backing collision 审计
- block_id: 9a2058849d15608365e547b6d7de05b73af6b0dfb7146df6bfa7bbdf6040c953
  indexed: true
  sha256: d0b79f93bb0efed742001ae6074edf2ad81927fa558641c9665b2af94e031081
  source_range:
    end: 10913763
    part: 4
    start: 0
  summary: 材料先提出 `global_bs=0`、`x_active_mask`、MC2/ALLGATHER 及 `[7,7,7,8]` 与 `[8,8,8,8]` 的严格对照，但 r9 后续显示四 DP 都是首个 batch=1 成功、随后 `7/7/7/7` 卡住，因此 DP skew、dummy batch、TASK_QUEUE、Prefix hit 和 MC2 首挂均不再是必要条件。device event 将首个未完成点定位到 layer 0 的 KDA `attention_done` 之前；输入是 7 个 singleton、`cu_seqlens=[0,1,...,7]`，而不是普通 token batch。建议的下一步是对 TP gather、causal conv、`kda_gate_cumsum`、`chunk_kda_fwd` 加细粒度事件，并以单序列 7-token 对比 7 个 1-token；dummy fence 方案已撤销为主因。
  title: 从 MC2 mixed-mask 假设转向 KDA 多序列首 token
- block_id: 48424acff8119c161450475cb562b6ef8551ad7cb94c0a3b3e1527b1a8c3ea41
  indexed: true
  sha256: 22272cfa11355a7ca52e62a4247427f742313470160d9808b59f3e73bec5b331
  source_range:
    end: 10913763
    part: 5
    start: 0
  summary: 单算子证据显示 `chunk_gated_delta_rule_fwd_h` 在 40 个 head-task 内完成，7×1 token×6 heads 的 42 tasks 稳定使 NPU0 AICore 100% 并在 `torch.npu.synchronize()` 无限等待；`kda_gate_cumsum` 及较小边界正常。相同 bf16、`cu_seqlens`、initial state、head 数和 varlen 元数据的 Triton 等价实现首次编译加执行约 6.22 秒后正常完成，输出 finite、形状一致，初步把范围收敛到 AscendC H kernel 的跨 40-task 调度/资源复用，而非该输入的 KDA 数学。后续仍需用隔离 AscendC 结果做数值校验、完成整条 Triton `chunk_kda` 服务侧 A/B，并在独立 source/build 目录通过 `build_aclnn.sh` 仅编译该算子，保留 40/41/42 task 回归；这些还不是生产修复验证。
  title: AscendC H kernel 超过 40 task 卡死，Triton 对照通过
- block_id: dd71a37ee676ad619991c9a9697b478fcd84a5de22c32e92e7200486ce5428bf
  indexed: true
  sha256: 395cd0ea8775c133ff5ee2009ff97e0da1610a40588c90f4775222c63132eae2
  source_range:
    end: 10913763
    part: 6
    start: 0
  summary: 材料记录了两条并行路线：完整 Triton `chunk_kda` 在 7×1×6、42 tasks 用例完成且输出 finite；独立 AscendC 修复据报告将原先 task41 卡死扩展为 task40/41/42/80/96 完成，并与拆分 golden 的 h、v_new、final_state bitwise 一致。机制假设指向空 Vnew subblock 未归还 HardEvent，补丁保留显式 MTE2→V→MTE2 桥，但单算子结果、构建过程、服务候选和四机准备仍需与真实服务精度和 cache 场景区分；多处记录明确未启动请求、未占用 NPU或未修改既有目录。Triton 服务候选复用现有 `l2norm_fwd`、保持 canonical `[H,V,K]` state，并已完成静态/CPU 定向检查；四机启动和 H0 客户端准备阶段曾遇到 SSH 认证、长时间 worker/load/compile/capture 等环境事件，但记录中的真实 H0 请求结果在本块内尚未出现。
  title: AscendC H 修复与 Triton 候选推进
- block_id: 1b46f9fe3ef3113d8f7480120b10ea53bffd2f6e48451e1c4c056c733b93d274
  indexed: true
  sha256: 88f0bfa4f30ef5e47dd4fbd4ac6f48fe2ebdec0beaa2b0d4ecb372e03129e363
  source_range:
    end: 10913763
    part: 7
    start: 0
  summary: 材料报告 baseline full OPP 可由精确替换 4 个 `ChunkGatedDeltaRuleFwd_H_*.o` 及 4 个 SHA JSON 组成 8 文件运行时变更，且保留 `libcust_opapi.so` 和全局配置；四机 native launcher、manifest 和启停审计也已准备，但尚未部署或启动 native 服务。Triton r12 据报告已完成四机 ready、H0 32/32 加四个 tail、H1a 20/20 加 tail、H1b 68/68 加 tail，以及 P0 答案 42/72/12/63，期间记录了 DP3 输出 hash 差异这一未吞并的精度细节。Prefix 场景中 P1 canonical 896 miss 据报告为 4/4、cached=0、hits=0，并已预期进入 hit 对照；128K P3 runner 仅完成部署、离线合成检查和门禁准备，尚无真实执行结论，且后续需保留 poison、三轮 drift、逐 DP metrics 与 prompt/output token ID 校验。
  title: Triton 四机 H0/H1/P0 与 Prefix runner 准备
- block_id: cf975b01e3ed71014289bf53f5417b744ffd50095fbb9c5b6ebc6af67a373881
  indexed: true
  sha256: f23ece50145914d9dca52babd00a35d94ba6cdccf9b437c24de331c358e6ed71
  source_range:
    end: 19031226
    part: 0
    start: 10913763
  summary: P1 的 miss 4/4 正常，但 canonical 与 natural control 的 hit384 均 4/4 从首 token 起输出 token 0/全 `!`；acceptance=100% 被判定为虚高，不能代表质量成功。r13 仅增加 Mamba zero/lifecycle 修复，部署前 preflight 通过且最终服务就绪，但其已报告的实验结果仍显示首 token 问题未解决，因而 fresh-state 未清零不是当前 P1 的直接根因。证据进一步指向通用 cached restore/source/view 链；已准备 r14 的 16-rank DFX，但相关定点数据尚未给出。另有 FullAttention trim 候选把 `[2,3,2]` 修为 `[2,1,2]`，仅 CPU 测试通过且未部署，不能解释首 token 异常。
  title: Prefix hit 首-token 损坏与 r13 zeroer 反证
- block_id: 0770691626cfb34acec84da2b6761682ffe18040ca83ece122684ffbc45592b4
  indexed: true
  sha256: ffa5242b9e86021d40550c1473e79efd126e6279a177c5b6adc19e14fe99f0b4
  source_range:
    end: 19031226
    part: 1
    start: 10913763
  summary: 静态链路确认 896 请求首段被 Eagle 截为 384，第二段执行 `prev=0→curr=2` 的 Mamba copy；因此 miss 正常、hit 损坏不能简单归因于是否执行该 copy。四平面地址的 4 KiB residue 随 N mod 4 变化，但 block stride、uint8 masked copy 和既有 block-ID 情况尚不足以支持纯 alignment 根因；建议的 N=5164 A/B 及 raw tensor 指针回读只是未完成的判别方案。r13 已被其他流程运行，相关记录强调未启动、停止或修改服务的只读约束。
  title: 384→3×128 replay 与 plane alignment 候选审计
- block_id: 618d872a5f2d43e16cfd5d168476c44ab1e155e85eb7d86095e63c8df6375423
  indexed: true
  sha256: c9dd562bda3eadb7d730ee2c7f802f1bf007459215e6d024a3902f5b90d2c1e5
  source_range:
    end: 19031226
    part: 2
    start: 10913763
  summary: r14 DFX 从只抓 global rank 0 修正为 DP0 的 TP0–15，并扩展到 14 个 Mamba group、69 个 KDA 层、MLA block table/slot mapping、原始 view stride/storage offset、边界检查和 execute-epoch completeness；CPU UT、正反 verifier 和四机 preflight 据报告通过。早期 staging 曾因 macOS `._*` AppleDouble 文件导致 tree hash 不一致，随后以 `COPYFILE_DISABLE=1` 重建并完成独立切换；r13 已 TERM 退出，r14 四机最终 READY、health=200、64/64 worker、fatal=0。材料最后只记录了固定的 `reset→miss896→reset→warm768→hit896` DFX 实验开始，尚未提供 manifest、首分叉或根因结论。
  title: r14 全 TP DFX 完善、验证与 READY 状态
- block_id: 34a498b7fcba1592febc906074a81d68c8db29187c03aaefc18be79ffa934481
  indexed: true
  sha256: 1b0425fd1d572c4ecc190c7335c64211d48da046013e9bde335eac82f60bbe69
  source_range:
    end: 19031226
    part: 3
    start: 10913763
  summary: r14 首次 DFX 运行在真正模型请求前因 `/reset_prefix_cache` HTTP 500 失败；后确认首个 `execute_model` 中 rank gate 调用 `get_current_vllm_config()` 时缺少 config context，DP0 出现 `Current vLLM config is not set`，DP1–3 为 `Connection closed by peer`，结果标记为 `invalid_experiment`，无 DFX manifest。修复将 DP/TP/global rank metadata 在 runner 初始化期冻结并让 execute/finalize 只读缓存；隔离 CPU 测试报告 9 passed，部署与 PRE­FLIGHT_ONLY 完成后以新 run-id 启动四机，64/64 worker 已完成初始化和权重加载，compile、KV profile 及 FULL decode graph capture 正在推进，当前未报告新的 fatal 或实际请求结果。
  title: r14 rank-gate 修复部署并进入 64 卡 capture
- block_id: 8d1cad8da3f731a9e5bdc39c9df779689fe9731eedd5df83ccd0ccd8e836f925
  indexed: true
  sha256: a0acc6dfb108979d35c2851f8782fe3921e0570a0a3ceda177c17d2a796871f8
  source_range:
    end: 23551508
    part: 0
    start: 19031226
  summary: r14 rankfix 的三次请求均返回 200，但 DFX 因外部 X-Request-ID 与 vLLM 内部 request id 映射不一致而得到 0/48 manifest；离线修正比较口径后，插桩下各项 cache 内容 exact，但 token0 在同步插桩下消失，分类为 timing-sensitive，不能视为修复。单请求一次 warm 的 hit384 正常，而两次相同 warm 后 hit384 稳定变为首 token 0、全 `!`，且当前 frozen source 未包含历史 `e32ece64a` async-copy 修复。材料提出并冻结了两条候选：`e32ece64a` 的 pointer-table H2D 时序修复，以及上游 `f51193b9a` V1 fused GPU pre-copy 的 Ascend 适配；二者当时均为候选或待验证方案，未宣称成功。
  title: r14/r15 基线复现并锁定异步生命周期方向
- block_id: ae412bd2af2feb07c842d98d116f88b39238ad7bdce9616c66085b44f063fa53
  indexed: true
  sha256: a86197e83ae5ec5dae32f437bfd634df83406da0e5f94f2638ec84c2c5bd4091
  source_range:
    end: 23551508
    part: 1
    start: 19031226
  summary: r15 B2 DFX 在两次相同 canonical 768 warm 后复现了 hit 首 token 0，但同步 DFX 下 cache dump 全部 exact，支持时序敏感性而非直接证明修复；随后离线 verifier 将物理 block ID 差异按字节 hash 正确归类为 `cache_content_exact`。`e32` 单文件候选四机部署完成并绑定 1024-token、SSE、golden hash 与 P1 门禁；系统 fused-precopy 候选也完成四机隔离部署、CPU 合同测试和 preflight，但 direct-op 与端到端结果尚未完成。材料明确保留失败、未部署、未发请求和 P1 未运行状态，不能将测试或部署准备升级为成功。
  title: e32 负向 A/B 与 fused-precopy 候选准备
- block_id: 46265288cf158043407812c83580ade054e21471bd169a74e6a3b961aca9f1fc
  indexed: true
  sha256: 91bfa1422686b510401f75979a1ca05d7c65f543a19049844282db6e36a35054
  source_range:
    end: 23551508
    part: 2
    start: 19031226
  summary: e32 四机 B2 明确失败：miss/warm 正常，但 hit 返回 1024 个全为 `!` 的 token0 结果；`source_before_copy` 在实际 state copy 前已 nonfinite，且 copy 后 2208/2208 精确复制，说明仅保护 pinned ptr-table H2D 生命周期不足。系统 fused GPU pre-copy 的单算子 bias 0/1/2/7、非连续 block 与 padded stride 均 bitwise 通过，但端到端 B2 仍完全复现，fused kernel 只是忠实复制已损坏的 source；P1 因 B2 fail-closed 未运行。当前证据把下一 seam 收敛到第二次 warm 后 `postprocess_mamba/global_stream` 完成事件、cache publish/free 或 source-state 所有权与复用生命周期；计划中的 completion-event wait 仍是待验证假设，不是已证实修复。
  title: e32 与 fused GPU pre-copy 均未修复 B2
- block_id: c54bd6c9fde88d61cd4eae6be1f31356189f3bfa6d73c1fa430ccfa65bffb830
  indexed: true
  sha256: eb010accca5f05570367d21814b73036b8e72692a5b9bcaf32a13a761475b153
  source_range:
    end: 23551508
    part: 3
    start: 19031226
  summary: 四机 async-off preflight/READY 完成，正常并发且不 reset 的 8 道 GSM8K 均答对、prefix hits=0、preemption=0；但 B2 的 896 命中384仍出现首 token 0、后续全0、文本全 `!`，因此排除了“仅由 async scheduling 触发”，P1未运行。纯四平面曾形成隔离提交，但此前 push 未触达远端；后续又发现全局 helper 可能误命中未覆盖的 cache specs，需补全结构条件和负向 UT。当前拟用 async-off 的单 DP0、out1 最小复现，沿时间线全量 dump scheduler block、refcount/free/reuse/COW/zero、worker block/page/slot、Mamba/四平面视图及设备内容，以定位 warm publish 到 hit 读取之间的损坏动作；这仍是分析计划，尚无新验证结论。
  title: async-off仍复现hit损坏，no-hit GSM8K正常；转向最小全链路DFX
- block_id: f6df7d93b6c369b5f0a989ba458aed9ae658caed0be57ce2338efdd42d22aa07
  indexed: true
  sha256: fcdfcd352d7915d1e881bbcc103c05222844ae6a54dfd9b150718f8e332c4631
  source_range:
    end: 23551508
    part: 4
    start: 19031226
  summary: 四平面候选从冻结父节点 `0d72522f6` 生成，最终本地提交 `391fc18e...` 仅含3个生产文件和2个测试文件；Ruff、format、diff-check、AST/pycompile及隔离 CPU UT（8 passed）通过，但未 push。一次 push 因 zsh refspec 解析失败且未触达远端，随后审查发现识别 helper 只检查 MLA+Mamba+GQA 存在、未确认所有 cache specs 完整覆盖，可能导致全局 monkey patch 误扩页。待补充“全部 specs 属于该布局且限定 Ascend MLA 类型”的结构条件与负向 UT，再重新复核发布。
  title: 四平面提交已修复格式门禁但未推送，helper 泛化风险待补测
- block_id: 9deeaf30386ab08448c9d9fb5167a5b3c948c333faba873d84ba5e7f4b5fe2b7
  indexed: true
  sha256: c250be8fb1252b1be5f8c1fdde3444df2b8317afb642e62f536b9dbc9c4d634f
  source_range:
    end: 32575533
    part: 0
    start: 23551508
  summary: 结构误命中已在 commit `842b030f...` 中修复并 amend，未 push；helper 现在要求 MLA 全为 `AscendMLAAttentionSpec`、无 leftover、Mamba 为 `align`，并以 3 个负向 UT 验证返回 `None` 且不改 page size。随后冻结了 core/plugin 的 host 事件与 worker JSONL 取证方案，覆盖 block ownership、hash、refcount、free/reuse/COW/zero、slot、Mamba 和 384→3×128 映射；r18 client/verifier 通过静态、合成和 CPU 验证，但尚未部署或发请求。四机 staging 曾因 `git archive` mode manifest 不一致 fail-closed，修正后 verifier-only 复核通过；这些结果证明了取证协议和 host 门禁，但没有证明原始设备侧问题已修复。
  title: PR13277 四平面修复与全链路 DFX 方案冻结
- block_id: 50b966a1476f410e4800f86bf935c34f786d9607f2286517acad71a3d21a9378
  indexed: true
  sha256: f6208b2b8fabc89a5ed10a9c17ee89a97cfebde7b047970bfe311adaaf835a27
  source_range:
    end: 32575533
    part: 1
    start: 23551508
  summary: r18-v1 因 wrapper 错用 r15 model ID 在首轮序列中止，空 DFX 和日志被保留；clientfix1 随后锁定 r18 served model、禁止旧值，并以全新 v2 根重启。r18-v2 四机 READY 后，修正序列成功，但启用 device clone/sync 的采集时 hit 恢复为 `39214 / " Let's"`，而 host ownership、hash/ref/free、zero/COW、slot `80/80`、16 rank manifest 和 finite 检查均通过。离线 verifier 修正了把 384 native block、跨 TP 物理 ID及 sampled token误报为错误的门禁，得到 `timing_sensitive_healed_under_synchronous_dfx_host_ownership_preserved_device_captures_finite`；这只能说明同步观测改变了 completion ordering，不能声称 kernel 当时未完成或根因已定位。
  title: r18-v2 运行与离线复核：同步采集会治愈命中异常
- block_id: dba99a68fddfd7e26753551a2b660dc67861dab616c125d2a7f0a6db2aee604b
  indexed: true
  sha256: 471a1413eb13e63bbffb65c8f4829a84b6d1b11d0edd4c5cf447ce928f4bff96
  source_range:
    end: 32575533
    part: 2
    start: 23551508
  summary: 独立 verifier 新 verdict 为 `all_passed=true`、`validation_errors=[]`，保留旧 verdict；记录的 warmA 时间线是 postprocess enqueue 后约 `3,282,350 ns` publish、`842,114,790 ns` free、`2,732,524,860 ns` hit lookup，并明确 `device_completion_not_observed=true`。随后 r19 只加入既有 `num_accepted_tokens_event` completion fence，关闭所有 DFX，标准 B2 仍复现 hit 全 token0，反证“scheduler 太早接管”或单纯 postprocess fence 不是充分修复。当前坏数据已知在 hit 前出现，Mamba source 与 MLA K nonfinite 而 V 正常，范围收窄至 graph/internal stream 完成关系、物理地址/OOB、共享四平面 alias 或 postprocess kernel 写错块，但尚无可提交修复。
  title: completion fence A/B 反证并收窄到设备侧写入边界
- block_id: 9b9e8ea5bcd77b81dc50c679fea637e9ba8f11114d96982cdd4849fc70e15f6d
  indexed: true
  sha256: 3997929c81d4d84a6d6a5b5b84d7439f6b09bf5fe16005f8b8fa1b82e6b3d594
  source_range:
    end: 32575533
    part: 3
    start: 23551508
  summary: 为一次实验区分 producer 完成、输入元数据、draft 覆盖、postprocess 写入和 hit copy 传播，R20 固定 7 个单变量 case，并补采 forward 前/后、draft 后、postprocess 前/后及 hit 实际拷贝后的同一物理块指纹。设计已修正为只有 trace 分支做 D2D 采样，同步分支不分配、不拷贝、不落盘；首尾还要独立复现 token0/all-`!`，否则整轮无效。源码复核已撤销“zeroer 遍历 page-size 后提前 return”的误读，相关实现据称已覆盖全部 bucket；因此目前只是候选实验协议和物理 alias 审计，尚未产生根因、修复、压测或推送证据。
  title: R20 七分支诊断实验设计尚未执行
- block_id: 8941efe96034bcb6e97d96e78a10ece2b630ca3484beeafd059fd8932f8b18b5
  indexed: true
  sha256: 8ead24203d1c94aed4621b27f0b510ecce5377f6681e1ebd71f24a4e8fd3b40a
  source_range:
    end: 39384400
    part: 0
    start: 32575533
  summary: R20 诊断候选完成提交，commit `449378d6890a…`，本地 UT、静态检查和 verifier 通过，但当时尚未部署或发请求。随后四机部署和服务启动预检通过；客户端先后暴露旧 R18 summary 门禁和临时文件覆盖两个工具问题，clientfix2 修正后仍未执行有效实验。R20 最终进入 READY，之后在首个 case 前因采集器错误假设 19 个 KV group 而退出，因此这批材料不能证明根因。
  title: R20 诊断封板与启动前修正
- block_id: fbe5f8014dafd190f4690d9a96c64ca6fbebadb590004d90b5bd042bd6d3ff72
  indexed: true
  sha256: 3c1b993375ec8a424e95e02454df970f6431ceb25c5981ef1946beb0854a137c
  source_range:
    end: 39384400
    part: 1
    start: 32575533
  summary: R20 首次实验因诊断器把运行时 20 个 KV group 错当成必须等于 19 而 fail-close；修正为保留 MLA 0–4、draft GQA 5、Mamba 6–19 后，r2 仍在首个 case 前让服务退出，因此两轮都没有有效根因数据。随后删除约 2200 行 block/snapshot/JSON 采集框架，提交 R21 `25586888…`，仅保留 10 个 request-ID 单变量干预 case 和 30 个请求。R21 已启动但材料末尾仍在加载模型，尚无实验结论；此前历史总结还指出必须先执行 `miss896/out1024` 的长状态前置，否则 control 可能被错误“heal”。
  title: R20 采集失败与 R21 极简干预
- block_id: c4db7ceb5a1691cb52ba8c5f88f6eaa4cb245e14b0adf5ea2fc05db1640effb0
  indexed: true
  sha256: 8ac8037053e26f1403cf31ebe40b3db683efc93a6d56df8c34e3ce82435374c6
  source_range:
    end: 39384400
    part: 2
    start: 32575533
  summary: 'R21 第一轮 10 个 case 全健康，后确认因缺少 canonical B2 的 `miss896/out1024` 前置而无效；补足长 miss 后的观察指向 DSpark draft 后 global-stream postprocess 缺少 side-stream join，但仍应视为待完整验证的诊断结论。为先保证 PR 功能和精度，device-sync 兜底已提交至 PR #13277，head 为 `5711d743c`；随后根据历史记录补交 `chunk_gated_delta_rule_fwd_h` 的 AscendC Vnew epilogue 修复，修复 41-task 空 Vnew AIV 提前 return 未归还 E0 HardEvent 的卡死问题，当前 PR head 为 `ad6e3072a`。这些提交和单算子验证记录已报告，但材料末尾尚未给出完整 B2、性能或 CI 最终结果。'
  title: 长 miss 条件、device-sync 兜底与 AscendC 修复
- block_id: 9b1deb24a457c53a9d693e1f149643beb22dd68ec9afae8699279b8521177328
  indexed: true
  sha256: efb9c39c8dcbf4b9a03ddc2f8f60942e5824c331fce19be67eabc04ffe7373cc
  source_range:
    end: 39403463
    part: 0
    start: 39384400
  summary: '已确认 `chunk_gated_delta_rule_fwd_h` 在 41 task 及真实 42 task 场景卡死，根因是第三波 ping-pong slot 中空的第二个 AIV subblock 提前返回而未归还 E0 HardEvent；按状态等待并设置 `V_MTE2` 后再发送 `vec1Done` 的修复，使 40/41/42/80/96 task、真实 7×1×6 及 golden 数值对比通过。该修复后来形成 `ad6e3072ae9facc65863e1a7c2b97ff5341c07ce` 并确认位于 PR #13277 head，但材料也指出此前候选曾是未提交的 dirty worktree；因此应保留这一历史状态差异。AscendC 修复只解释并发 prefill 卡死，不能解释 Triton prefix hit384 输出 token0/`!` 的精度问题；PR 的完整 B2、精度、性能和 CI 结论仍需谨慎，不能仅据已有检查宣称全部验证成功。'
  title: AscendC HardEvent 修复与 Prefix Cache 问题区分
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e522faeee691a40cae892394d0b2c2d856c37cd769d9fa75a43897a54d80240e
  kind: experience
  material_digest: da34051b0b0ca1db6631a0a380437f489cdb478bd0b07b8a1ade1f5caeb57b4f
  revision: 2c8a0b1e5e4a083034d853a42d14d3e2dda61545d40ae41be66ce2eb95a25385
  schema: mindie-entry/3
  summary: '此前关于 async scheduling、host block/slot/ownership、e32/fused pre-copy 和单纯 postprocess completion fence 的排除仍可参考；R20 两轮采集受 19-vs-20 KV group 与 active-block 假设影响，不能作为模型结论。canonical `reset → miss896/out1024 → reset` 前置仍是后续 control 与最小干预矩阵的必要条件，缺失时结果可能假正常。当前生产变更包括 PR #13277 的 device-sync 兜底 `5711d743c` 与 `chunk_gated_delta_rule_fwd_h` AscendC HardEvent 修复 `ad6e3072a`；前者有性能代价，side-stream event/wait 高性能替代和 DSpark draft 归因仍未完成验证。新增证据确认 AscendC 修复可消除特定并发 prefill 卡死并与 golden 一致，但不解释 prefix hit384 精度异常；完整 canonical B2、精度、性能及最终 CI 状态仍不可宣称已验证。'
  title: 当前任务导航：生产兜底与 AscendC 卡死修复已提交，但 Prefix Cache 精度及完整 B2 仍未闭环
navigation: '此前关于 async scheduling、host block/slot/ownership、e32/fused pre-copy 和单纯 postprocess completion fence 的排除仍可参考；R20 两轮采集受 19-vs-20 KV group 与 active-block 假设影响，不能作为模型结论。canonical `reset → miss896/out1024 → reset` 前置仍是后续 control 与最小干预矩阵的必要条件，缺失时结果可能假正常。当前生产变更包括 PR #13277 的 device-sync 兜底 `5711d743c` 与 `chunk_gated_delta_rule_fwd_h` AscendC HardEvent 修复 `ad6e3072a`；前者有性能代价，side-stream event/wait 高性能替代和 DSpark draft 归因仍未完成验证。新增证据确认 AscendC 修复可消除特定并发 prefill 卡死并与 golden 一致，但不解释 prefix hit384 精度异常；完整 canonical B2、精度、性能及最终 CI 状态仍不可宣称已验证。'
schema: mindie-material-task/1
status: complete
task_id: e522faeee691a40cae892394d0b2c2d856c37cd769d9fa75a43897a54d80240e
---

# 当前任务导航：生产兜底与 AscendC 卡死修复已提交，但 Prefix Cache 精度及完整 B2 仍未闭环

此前关于 async scheduling、host block/slot/ownership、e32/fused pre-copy 和单纯 postprocess completion fence 的排除仍可参考；R20 两轮采集受 19-vs-20 KV group 与 active-block 假设影响，不能作为模型结论。canonical `reset → miss896/out1024 → reset` 前置仍是后续 control 与最小干预矩阵的必要条件，缺失时结果可能假正常。当前生产变更包括 PR #13277 的 device-sync 兜底 `5711d743c` 与 `chunk_gated_delta_rule_fwd_h` AscendC HardEvent 修复 `ad6e3072a`；前者有性能代价，side-stream event/wait 高性能替代和 DSpark draft 归因仍未完成验证。新增证据确认 AscendC 修复可消除特定并发 prefill 卡死并与 golden 一致，但不解释 prefix hit384 精度异常；完整 canonical B2、精度、性能及最终 CI 状态仍不可宣称已验证。

## Materials

- [四平面方案与 c32 卡死基线](blocks/8e48137c07e26dda8bc4b0c9ded0273b8a7dbec9f1d78188ae9710d067202f55.md): 当前候选方案为 vLLM 0.26 原有 raw-page 机制新增第 4 条连续 lane（GQA-V），形成三类 page、四条 lane；四平面静态测试 43/43 通过，但 r5 无 DFX 的 c32 仍稳定失败。r6 中 step2 的 64/64 TP rank 停在首个 KDA 前 TP all-gather，shared-expert 多流 collective 竞跑被列为强候选，但 DFX watchdog 对现场有扰动，不能把该位置直接视为根因。既有验收矩阵覆盖卡死、精度、Prefix Cache hit/miss 和采信率，要求保留失败、hash、日志及每轮 manifest，且未完成的远端正反向验证不能标为成功。
- [静态排除 zeroer/CoW 并聚焦首个 target execute](blocks/676d1c48d3ca0ee37775bac9c08398653b4cdcd05853b4fd870057b2cd18b737.md): 静态审计确认 DSpark 短请求路径不走 zeroer、默认 384 对齐场景不触发 Prefix CoW，ACLGraph 也不是短 c32 的直接来源；但 plane-major CoW helper 仍是 prefix_match_unit<384 时的潜在通用缺陷。r7 的 async-off 冷启动仅完成 3/32，随后 29 条请求均卡在首轮 execute_model，说明关闭 async 不是修复，也纠正了“仅少量请求入队”及 draft/output-copy 为必要原因的判断。分析进一步把重点收窄到首个 target forward 内的 DP metadata rendezvous、shared-expert/EP64 collective 顺序和实际 cudagraph mode；r8 正在以 async-on、shared-expert multistream 关闭进行对照，结果尚未完成。
- [shared-expert pre-gather 候选与验收脚本状态](blocks/01ab1717eb6a39b90ca962f04316f1c86d9dda9cd2760e52c40607ba91f2c389.md): 针对 K3 将 TP all-gather 放入 shared-expert side stream、可能与默认流 EP64/MC2 竞跑的假设，建议在默认流先完成 shared TP all-gather，再进入 routed collective，同时保留 shared MLP overlap；需要 async-on/multistream-off、pre-gather 和原 side-gather 三腿对照，且批次、路由及源码哈希必须一致。r7 已排除首个 execute 前的 draft GQA、Mamba post-sample 和 output-copy 为当前 H0 必要原因，但 shared-expert stream、DP metadata 和 FULL graph 仍未被实验闭环。验收脚本已审查并修补，纳入 reset/no-warm miss、双 warmup hit、token IDs、严格退出码和采信率判定；本块末尾的具体修改文件列表不完整，且明确未发送远程推理请求、未修改生产源码。
- [四平面验收矩阵与历史 backing collision 审计](blocks/164bab5d427234858b656f60e20a31333ba6ec24913e4aa7109776e039c26799.md): 材料给出四平面版本的三轮 hit/miss 验收脚本，包含精确 warmup/replay token 数、output 证据、DP/跨轮采信率、源码与数据 SHA 校验；这些是执行约束，不是已完成的验证。历史审计将旧三平面 compact GQA stride 与 MLA block 发生物理 backing 冲突联系起来，并指出四平面共享 scheduler physical block ID 后可避免该公式碰撞，但仍需复现历史压力链确认。随后列出两个未证实的 Mamba 风险：metadata H2D lifetime（`prepare_inputs_event`、`copy_bufs.offset`）以及 `global_stream` event 等待过晚导致 accepted-count/block-table 生命周期竞态；已有 A/B 数值并未证明它们是历史 `all!` 根因。
- [从 MC2 mixed-mask 假设转向 KDA 多序列首 token](blocks/9a2058849d15608365e547b6d7de05b73af6b0dfb7146df6bfa7bbdf6040c953.md): 材料先提出 `global_bs=0`、`x_active_mask`、MC2/ALLGATHER 及 `[7,7,7,8]` 与 `[8,8,8,8]` 的严格对照，但 r9 后续显示四 DP 都是首个 batch=1 成功、随后 `7/7/7/7` 卡住，因此 DP skew、dummy batch、TASK_QUEUE、Prefix hit 和 MC2 首挂均不再是必要条件。device event 将首个未完成点定位到 layer 0 的 KDA `attention_done` 之前；输入是 7 个 singleton、`cu_seqlens=[0,1,...,7]`，而不是普通 token batch。建议的下一步是对 TP gather、causal conv、`kda_gate_cumsum`、`chunk_kda_fwd` 加细粒度事件，并以单序列 7-token 对比 7 个 1-token；dummy fence 方案已撤销为主因。
- [AscendC H kernel 超过 40 task 卡死，Triton 对照通过](blocks/48424acff8119c161450475cb562b6ef8551ad7cb94c0a3b3e1527b1a8c3ea41.md): 单算子证据显示 `chunk_gated_delta_rule_fwd_h` 在 40 个 head-task 内完成，7×1 token×6 heads 的 42 tasks 稳定使 NPU0 AICore 100% 并在 `torch.npu.synchronize()` 无限等待；`kda_gate_cumsum` 及较小边界正常。相同 bf16、`cu_seqlens`、initial state、head 数和 varlen 元数据的 Triton 等价实现首次编译加执行约 6.22 秒后正常完成，输出 finite、形状一致，初步把范围收敛到 AscendC H kernel 的跨 40-task 调度/资源复用，而非该输入的 KDA 数学。后续仍需用隔离 AscendC 结果做数值校验、完成整条 Triton `chunk_kda` 服务侧 A/B，并在独立 source/build 目录通过 `build_aclnn.sh` 仅编译该算子，保留 40/41/42 task 回归；这些还不是生产修复验证。
- [AscendC H 修复与 Triton 候选推进](blocks/dd71a37ee676ad619991c9a9697b478fcd84a5de22c32e92e7200486ce5428bf.md): 材料记录了两条并行路线：完整 Triton `chunk_kda` 在 7×1×6、42 tasks 用例完成且输出 finite；独立 AscendC 修复据报告将原先 task41 卡死扩展为 task40/41/42/80/96 完成，并与拆分 golden 的 h、v_new、final_state bitwise 一致。机制假设指向空 Vnew subblock 未归还 HardEvent，补丁保留显式 MTE2→V→MTE2 桥，但单算子结果、构建过程、服务候选和四机准备仍需与真实服务精度和 cache 场景区分；多处记录明确未启动请求、未占用 NPU或未修改既有目录。Triton 服务候选复用现有 `l2norm_fwd`、保持 canonical `[H,V,K]` state，并已完成静态/CPU 定向检查；四机启动和 H0 客户端准备阶段曾遇到 SSH 认证、长时间 worker/load/compile/capture 等环境事件，但记录中的真实 H0 请求结果在本块内尚未出现。
- [Triton 四机 H0/H1/P0 与 Prefix runner 准备](blocks/1b46f9fe3ef3113d8f7480120b10ea53bffd2f6e48451e1c4c056c733b93d274.md): 材料报告 baseline full OPP 可由精确替换 4 个 `ChunkGatedDeltaRuleFwd_H_*.o` 及 4 个 SHA JSON 组成 8 文件运行时变更，且保留 `libcust_opapi.so` 和全局配置；四机 native launcher、manifest 和启停审计也已准备，但尚未部署或启动 native 服务。Triton r12 据报告已完成四机 ready、H0 32/32 加四个 tail、H1a 20/20 加 tail、H1b 68/68 加 tail，以及 P0 答案 42/72/12/63，期间记录了 DP3 输出 hash 差异这一未吞并的精度细节。Prefix 场景中 P1 canonical 896 miss 据报告为 4/4、cached=0、hits=0，并已预期进入 hit 对照；128K P3 runner 仅完成部署、离线合成检查和门禁准备，尚无真实执行结论，且后续需保留 poison、三轮 drift、逐 DP metrics 与 prompt/output token ID 校验。
- [Prefix hit 首-token 损坏与 r13 zeroer 反证](blocks/cf975b01e3ed71014289bf53f5417b744ffd50095fbb9c5b6ebc6af67a373881.md): P1 的 miss 4/4 正常，但 canonical 与 natural control 的 hit384 均 4/4 从首 token 起输出 token 0/全 `!`；acceptance=100% 被判定为虚高，不能代表质量成功。r13 仅增加 Mamba zero/lifecycle 修复，部署前 preflight 通过且最终服务就绪，但其已报告的实验结果仍显示首 token 问题未解决，因而 fresh-state 未清零不是当前 P1 的直接根因。证据进一步指向通用 cached restore/source/view 链；已准备 r14 的 16-rank DFX，但相关定点数据尚未给出。另有 FullAttention trim 候选把 `[2,3,2]` 修为 `[2,1,2]`，仅 CPU 测试通过且未部署，不能解释首 token 异常。
- [384→3×128 replay 与 plane alignment 候选审计](blocks/0770691626cfb34acec84da2b6761682ffe18040ca83ece122684ffbc45592b4.md): 静态链路确认 896 请求首段被 Eagle 截为 384，第二段执行 `prev=0→curr=2` 的 Mamba copy；因此 miss 正常、hit 损坏不能简单归因于是否执行该 copy。四平面地址的 4 KiB residue 随 N mod 4 变化，但 block stride、uint8 masked copy 和既有 block-ID 情况尚不足以支持纯 alignment 根因；建议的 N=5164 A/B 及 raw tensor 指针回读只是未完成的判别方案。r13 已被其他流程运行，相关记录强调未启动、停止或修改服务的只读约束。
- [r14 全 TP DFX 完善、验证与 READY 状态](blocks/618d872a5f2d43e16cfd5d168476c44ab1e155e85eb7d86095e63c8df6375423.md): r14 DFX 从只抓 global rank 0 修正为 DP0 的 TP0–15，并扩展到 14 个 Mamba group、69 个 KDA 层、MLA block table/slot mapping、原始 view stride/storage offset、边界检查和 execute-epoch completeness；CPU UT、正反 verifier 和四机 preflight 据报告通过。早期 staging 曾因 macOS `._*` AppleDouble 文件导致 tree hash 不一致，随后以 `COPYFILE_DISABLE=1` 重建并完成独立切换；r13 已 TERM 退出，r14 四机最终 READY、health=200、64/64 worker、fatal=0。材料最后只记录了固定的 `reset→miss896→reset→warm768→hit896` DFX 实验开始，尚未提供 manifest、首分叉或根因结论。
- [r14 rank-gate 修复部署并进入 64 卡 capture](blocks/34a498b7fcba1592febc906074a81d68c8db29187c03aaefc18be79ffa934481.md): r14 首次 DFX 运行在真正模型请求前因 `/reset_prefix_cache` HTTP 500 失败；后确认首个 `execute_model` 中 rank gate 调用 `get_current_vllm_config()` 时缺少 config context，DP0 出现 `Current vLLM config is not set`，DP1–3 为 `Connection closed by peer`，结果标记为 `invalid_experiment`，无 DFX manifest。修复将 DP/TP/global rank metadata 在 runner 初始化期冻结并让 execute/finalize 只读缓存；隔离 CPU 测试报告 9 passed，部署与 PRE­FLIGHT_ONLY 完成后以新 run-id 启动四机，64/64 worker 已完成初始化和权重加载，compile、KV profile 及 FULL decode graph capture 正在推进，当前未报告新的 fatal 或实际请求结果。
- [r14/r15 基线复现并锁定异步生命周期方向](blocks/8d1cad8da3f731a9e5bdc39c9df779689fe9731eedd5df83ccd0ccd8e836f925.md): r14 rankfix 的三次请求均返回 200，但 DFX 因外部 X-Request-ID 与 vLLM 内部 request id 映射不一致而得到 0/48 manifest；离线修正比较口径后，插桩下各项 cache 内容 exact，但 token0 在同步插桩下消失，分类为 timing-sensitive，不能视为修复。单请求一次 warm 的 hit384 正常，而两次相同 warm 后 hit384 稳定变为首 token 0、全 `!`，且当前 frozen source 未包含历史 `e32ece64a` async-copy 修复。材料提出并冻结了两条候选：`e32ece64a` 的 pointer-table H2D 时序修复，以及上游 `f51193b9a` V1 fused GPU pre-copy 的 Ascend 适配；二者当时均为候选或待验证方案，未宣称成功。
- [e32 负向 A/B 与 fused-precopy 候选准备](blocks/ae412bd2af2feb07c842d98d116f88b39238ad7bdce9616c66085b44f063fa53.md): r15 B2 DFX 在两次相同 canonical 768 warm 后复现了 hit 首 token 0，但同步 DFX 下 cache dump 全部 exact，支持时序敏感性而非直接证明修复；随后离线 verifier 将物理 block ID 差异按字节 hash 正确归类为 `cache_content_exact`。`e32` 单文件候选四机部署完成并绑定 1024-token、SSE、golden hash 与 P1 门禁；系统 fused-precopy 候选也完成四机隔离部署、CPU 合同测试和 preflight，但 direct-op 与端到端结果尚未完成。材料明确保留失败、未部署、未发请求和 P1 未运行状态，不能将测试或部署准备升级为成功。
- [e32 与 fused GPU pre-copy 均未修复 B2](blocks/46265288cf158043407812c83580ade054e21471bd169a74e6a3b961aca9f1fc.md): e32 四机 B2 明确失败：miss/warm 正常，但 hit 返回 1024 个全为 `!` 的 token0 结果；`source_before_copy` 在实际 state copy 前已 nonfinite，且 copy 后 2208/2208 精确复制，说明仅保护 pinned ptr-table H2D 生命周期不足。系统 fused GPU pre-copy 的单算子 bias 0/1/2/7、非连续 block 与 padded stride 均 bitwise 通过，但端到端 B2 仍完全复现，fused kernel 只是忠实复制已损坏的 source；P1 因 B2 fail-closed 未运行。当前证据把下一 seam 收敛到第二次 warm 后 `postprocess_mamba/global_stream` 完成事件、cache publish/free 或 source-state 所有权与复用生命周期；计划中的 completion-event wait 仍是待验证假设，不是已证实修复。
- [async-off仍复现hit损坏，no-hit GSM8K正常；转向最小全链路DFX](blocks/c54bd6c9fde88d61cd4eae6be1f31356189f3bfa6d73c1fa430ccfa65bffb830.md): 四机 async-off preflight/READY 完成，正常并发且不 reset 的 8 道 GSM8K 均答对、prefix hits=0、preemption=0；但 B2 的 896 命中384仍出现首 token 0、后续全0、文本全 `!`，因此排除了“仅由 async scheduling 触发”，P1未运行。纯四平面曾形成隔离提交，但此前 push 未触达远端；后续又发现全局 helper 可能误命中未覆盖的 cache specs，需补全结构条件和负向 UT。当前拟用 async-off 的单 DP0、out1 最小复现，沿时间线全量 dump scheduler block、refcount/free/reuse/COW/zero、worker block/page/slot、Mamba/四平面视图及设备内容，以定位 warm publish 到 hit 读取之间的损坏动作；这仍是分析计划，尚无新验证结论。
- [四平面提交已修复格式门禁但未推送，helper 泛化风险待补测](blocks/f6df7d93b6c369b5f0a989ba458aed9ae658caed0be57ce2338efdd42d22aa07.md): 四平面候选从冻结父节点 `0d72522f6` 生成，最终本地提交 `391fc18e...` 仅含3个生产文件和2个测试文件；Ruff、format、diff-check、AST/pycompile及隔离 CPU UT（8 passed）通过，但未 push。一次 push 因 zsh refspec 解析失败且未触达远端，随后审查发现识别 helper 只检查 MLA+Mamba+GQA 存在、未确认所有 cache specs 完整覆盖，可能导致全局 monkey patch 误扩页。待补充“全部 specs 属于该布局且限定 Ascend MLA 类型”的结构条件与负向 UT，再重新复核发布。
- [PR13277 四平面修复与全链路 DFX 方案冻结](blocks/9deeaf30386ab08448c9d9fb5167a5b3c948c333faba873d84ba5e7f4b5fe2b7.md): 结构误命中已在 commit `842b030f...` 中修复并 amend，未 push；helper 现在要求 MLA 全为 `AscendMLAAttentionSpec`、无 leftover、Mamba 为 `align`，并以 3 个负向 UT 验证返回 `None` 且不改 page size。随后冻结了 core/plugin 的 host 事件与 worker JSONL 取证方案，覆盖 block ownership、hash、refcount、free/reuse/COW/zero、slot、Mamba 和 384→3×128 映射；r18 client/verifier 通过静态、合成和 CPU 验证，但尚未部署或发请求。四机 staging 曾因 `git archive` mode manifest 不一致 fail-closed，修正后 verifier-only 复核通过；这些结果证明了取证协议和 host 门禁，但没有证明原始设备侧问题已修复。
- [r18-v2 运行与离线复核：同步采集会治愈命中异常](blocks/50b966a1476f410e4800f86bf935c34f786d9607f2286517acad71a3d21a9378.md): r18-v1 因 wrapper 错用 r15 model ID 在首轮序列中止，空 DFX 和日志被保留；clientfix1 随后锁定 r18 served model、禁止旧值，并以全新 v2 根重启。r18-v2 四机 READY 后，修正序列成功，但启用 device clone/sync 的采集时 hit 恢复为 `39214 / " Let's"`，而 host ownership、hash/ref/free、zero/COW、slot `80/80`、16 rank manifest 和 finite 检查均通过。离线 verifier 修正了把 384 native block、跨 TP 物理 ID及 sampled token误报为错误的门禁，得到 `timing_sensitive_healed_under_synchronous_dfx_host_ownership_preserved_device_captures_finite`；这只能说明同步观测改变了 completion ordering，不能声称 kernel 当时未完成或根因已定位。
- [completion fence A/B 反证并收窄到设备侧写入边界](blocks/dba99a68fddfd7e26753551a2b660dc67861dab616c125d2a7f0a6db2aee604b.md): 独立 verifier 新 verdict 为 `all_passed=true`、`validation_errors=[]`，保留旧 verdict；记录的 warmA 时间线是 postprocess enqueue 后约 `3,282,350 ns` publish、`842,114,790 ns` free、`2,732,524,860 ns` hit lookup，并明确 `device_completion_not_observed=true`。随后 r19 只加入既有 `num_accepted_tokens_event` completion fence，关闭所有 DFX，标准 B2 仍复现 hit 全 token0，反证“scheduler 太早接管”或单纯 postprocess fence 不是充分修复。当前坏数据已知在 hit 前出现，Mamba source 与 MLA K nonfinite 而 V 正常，范围收窄至 graph/internal stream 完成关系、物理地址/OOB、共享四平面 alias 或 postprocess kernel 写错块，但尚无可提交修复。
- [R20 七分支诊断实验设计尚未执行](blocks/9b9e8ea5bcd77b81dc50c679fea637e9ba8f11114d96982cdd4849fc70e15f6d.md): 为一次实验区分 producer 完成、输入元数据、draft 覆盖、postprocess 写入和 hit copy 传播，R20 固定 7 个单变量 case，并补采 forward 前/后、draft 后、postprocess 前/后及 hit 实际拷贝后的同一物理块指纹。设计已修正为只有 trace 分支做 D2D 采样，同步分支不分配、不拷贝、不落盘；首尾还要独立复现 token0/all-`!`，否则整轮无效。源码复核已撤销“zeroer 遍历 page-size 后提前 return”的误读，相关实现据称已覆盖全部 bucket；因此目前只是候选实验协议和物理 alias 审计，尚未产生根因、修复、压测或推送证据。
- [R20 诊断封板与启动前修正](blocks/8941efe96034bcb6e97d96e78a10ece2b630ca3484beeafd059fd8932f8b18b5.md): R20 诊断候选完成提交，commit `449378d6890a…`，本地 UT、静态检查和 verifier 通过，但当时尚未部署或发请求。随后四机部署和服务启动预检通过；客户端先后暴露旧 R18 summary 门禁和临时文件覆盖两个工具问题，clientfix2 修正后仍未执行有效实验。R20 最终进入 READY，之后在首个 case 前因采集器错误假设 19 个 KV group 而退出，因此这批材料不能证明根因。
- [R20 采集失败与 R21 极简干预](blocks/fbe5f8014dafd190f4690d9a96c64ca6fbebadb590004d90b5bd042bd6d3ff72.md): R20 首次实验因诊断器把运行时 20 个 KV group 错当成必须等于 19 而 fail-close；修正为保留 MLA 0–4、draft GQA 5、Mamba 6–19 后，r2 仍在首个 case 前让服务退出，因此两轮都没有有效根因数据。随后删除约 2200 行 block/snapshot/JSON 采集框架，提交 R21 `25586888…`，仅保留 10 个 request-ID 单变量干预 case 和 30 个请求。R21 已启动但材料末尾仍在加载模型，尚无实验结论；此前历史总结还指出必须先执行 `miss896/out1024` 的长状态前置，否则 control 可能被错误“heal”。
- [长 miss 条件、device-sync 兜底与 AscendC 修复](blocks/c4db7ceb5a1691cb52ba8c5f88f6eaa4cb245e14b0adf5ea2fc05db1640effb0.md): R21 第一轮 10 个 case 全健康，后确认因缺少 canonical B2 的 `miss896/out1024` 前置而无效；补足长 miss 后的观察指向 DSpark draft 后 global-stream postprocess 缺少 side-stream join，但仍应视为待完整验证的诊断结论。为先保证 PR 功能和精度，device-sync 兜底已提交至 PR #13277，head 为 `5711d743c`；随后根据历史记录补交 `chunk_gated_delta_rule_fwd_h` 的 AscendC Vnew epilogue 修复，修复 41-task 空 Vnew AIV 提前 return 未归还 E0 HardEvent 的卡死问题，当前 PR head 为 `ad6e3072a`。这些提交和单算子验证记录已报告，但材料末尾尚未给出完整 B2、性能或 CI 最终结果。
- [AscendC HardEvent 修复与 Prefix Cache 问题区分](blocks/9b1deb24a457c53a9d693e1f149643beb22dd68ec9afae8699279b8521177328.md): 已确认 `chunk_gated_delta_rule_fwd_h` 在 41 task 及真实 42 task 场景卡死，根因是第三波 ping-pong slot 中空的第二个 AIV subblock 提前返回而未归还 E0 HardEvent；按状态等待并设置 `V_MTE2` 后再发送 `vec1Done` 的修复，使 40/41/42/80/96 task、真实 7×1×6 及 golden 数值对比通过。该修复后来形成 `ad6e3072ae9facc65863e1a7c2b97ff5341c07ce` 并确认位于 PR #13277 head，但材料也指出此前候选曾是未提交的 dirty worktree；因此应保留这一历史状态差异。AscendC 修复只解释并发 prefill 卡死，不能解释 Triton prefix hit384 输出 token0/`!` 的精度问题；PR 的完整 B2、精度、性能和 CI 结论仍需谨慎，不能仅据已有检查宣称全部验证成功。

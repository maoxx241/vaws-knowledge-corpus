---
blocks:
- block_id: 543d3af0e5d9f6249fc53d021bbd22638e91eb8441fa72bcab8868bb54568709
  indexed: true
  sha256: 4861335161484a06a5868727a933d58562b9d70fb7b6f700e45a21d7ad3a558c
  source_range:
    end: 13606139
    part: 0
    start: 0
  summary: 只读审计发现，四机原部署未包含 PR `258ce514`，共享权重是旧 GQA/Qwen3 DSpark，而非 PR 新增的 MLA/K3DSpark 权重；因此早期“低接收率”无法直接归因于该 PR。后续将目标收敛为旧 GQA 权重 + MRV1 greedy，重点修复 `Qwen3DSparkModel` 配置归一化及 aux hidden state 的 materialized residual 语义，同时保留 MLA raw capture；MRV2、probabilistic sampling 和 block verification 仍是不完整路径。四机验证尚未完成：153/155 曾被外部任务占用，单机 EP16/TP16 装载又因每卡专家数量过多 OOM，当前只完成了源码审计、测试与容量分析，未形成精度或接收率成功证据。
  title: PR13071 与 GQA DSpark 定位
- block_id: 68771359f41f16b1c74a16af1b5504bfbc2baab825658236c05d42373feb60bd
  indexed: true
  sha256: 4014c51fa3206ce12a57d8cadca516bc3b4a3a41a92d153278e67b743b5d4cef
  source_range:
    end: 13606139
    part: 1
    start: 0
  summary: 建立了 GQA DSpark 的验收框架：greedy speculative decoding 需与 target-only 逐 token 一致，短答案、GPQA-Diamond、DP4、prefix cache 和逐位置 survival rate 均有明确门槛；历史仅有 `121/128` 通过、重复 `!`、缺失 closing marker 等失败证据，不能当作已验证基线。模型卡只公开 GSM8K Mean acceptance length `5.6660`，缺少完整评测协议，因此 `5.1–6.2`、`<4.8` 等属于临时工程校准带，不是官方范围；建议采用 SpecForge 风格 GSM8K 5-shot `/v1/completions`、固定 revision 与采样参数，并保存所有协议变量。已完成本地 `benchmark-k3-dspark-gsm8k.py` 及自检，但尚未在真实四机上取得结果。
  title: GQA 质量门与 GSM8K 校准方案
- block_id: c261c2c917446d5e29626199d61cc008ccdcaef9c13a4ac2f6d3f39acc0e876b
  indexed: true
  sha256: 1badb980f97b70947998ad736bd8c6155011e31921dd4d48b6afb1f3bfd456d5
  source_range:
    end: 13606139
    part: 2
    start: 0
  summary: DP1 接收率应通过同一进程 `/metrics` 的 before/after Counter 差分计算，不使用 reset；关键指标是 `vllm:spec_decode_num_drafts_total`、`...draft_tokens_total`、`...accepted_tokens_total` 及七个 `accepted_tokens_per_pos_total`，并校验 `A=ΣP[i]`、位置单调性和 `AL=1+A/D`。测试协议要求关闭 prefix cache、单请求、warmup 后采样，保存 64/128 条快照，并检查 logger 连续性与 `drafts_created` 不变。容量核算显示单机 EP16 需要承载 56 个专家并容易 OOM；layer-prefetch 的 `group=3,num=1` 或 `group=3,num=2`、以及 EP32 的约 33.7% offload 是可行候选，但尚未产生精度或接收率验证结果。
  title: Metrics 差分与 EP16 容量约束
- block_id: e35f350c9d00ca0cb16b37cd88f642a67d5134eb4ad86c8d30045b5658bba085
  indexed: true
  sha256: 972b3f9ee91862f96100d8177506f805f9d6a0a1406da3c14901e21f6ac6ed55
  source_range:
    end: 13606139
    part: 3
    start: 0
  summary: DP2 因原持有者重启而取消，改在 156 进行 exact-PR、EP16、TP16 的单机隔离装载；运行日志确认旧 GQA draft 归一化为 `Qwen3DSparkModel`，并使用 materialized residual stream，但尚未取得接收率结果。首次完整加载在 `weight_nz_mode=1` 的 offload 后处理阶段因 `TransData ChooseCompileInfo Failed` 失败，根因是 FRACTAL_NZ 权重被通用路径直接搬回 CPU；改为 `weight_nz_mode=0` 仅保留 ND 存储布局后重试。审计还指出 draft 实际无量化声明、外层 quant config 可能共享污染，以及 `enable_reduce_sample=true`、MRV2、错误 draft TP 或非 greedy 请求可能静默绕过 Markov head；这些分析和 fail-fast 保护不等于真实精度成功。
  title: 单机 GQA 装载、配置核查与 NZ 崩溃定位
- block_id: 2020ad54eb5887898aa2c7650097ba2bae03551c3065eb43a6d5e76a6f6c61be
  indexed: true
  sha256: ccc151382103f48da95dea84854469eecc2bffdacef1dfb2baeb4d73358d2ceb
  source_range:
    end: 13606139
    part: 4
    start: 0
  summary: '`weight_nz_mode=0` 重试曾推进至 352/352 shard 并进入后处理，但这只是绕过单机 offload 崩溃的实验路径，不能作为最终性能结论；NZ/ND 可能引入 kernel 和末位 logit 差异，target-only 与 DSpark 必须使用相同设置。代码复审要求配置 guard 在父类初始化前执行，并提醒 MRV2 DSpark 不应被全局拒绝；当前 materialized capture 修复仅覆盖 MRV1。随后确认单机完整权重路线因主机 OOM（`VLLM::Worker_TP` 被内核杀死）不成立，相关服务退出且日志保留；四机验证应使用完整权重、无 offload、MRV1、prefix-off/eager，并分别建立 target-only 与 DSpark 基线。'
  title: 单机重试、NZ 布局限制与四机前置审查
- block_id: 0cf4f736f7adb4be15832d498ee8b4b96e05062939b455b29da25c4b3106f7d5
  indexed: true
  sha256: 5911807c566045aa4ee4d77e527de1184870bf7ced3839527ef520730c0c24b8
  source_range:
    end: 13606139
    part: 5
    start: 0
  summary: 153 的 `lala-153-k3-main` 是外部占用的 TP16、DP1、5 层 Kimi K3 服务；按用户授权精确终止后，153–156 的 vLLM/NPU 进程和相关端口均已释放，单机路线不再用于精度验证。155 的 release 容器后来恢复，但审计发现其默认 runtime 为 `ascend`，而 154/156 使用 `runc`；排除 runtime 后镜像、挂载、设备和源码内容一致，因此启动前必须显式核对并统一 runtime、源码/权重 SHA、PID/PGID 和端口。后续建议顺序是四机完整权重 target-only，再运行相同配置的 DSpark，单独检查短答 parity、GSM8K 数据集 SHA、accuracy、7-position metrics 与 mean acceptance length；已有 42 个定向测试通过，但真实四机精度和接收率仍未被证明。
  title: 四机资源释放、容器同构与验证门禁
- block_id: 09647410620567ffa1f6c87161131c5216ba219a72459ec23fd0d4eaddc8e933
  indexed: true
  sha256: c520b72450779ca033cff6db0cc4a88192cad46f7e536dbf2d89f967a40598e6
  source_range:
    end: 13606139
    part: 6
    start: 0
  summary: 四机容器预检确认 runtime、网络接口、端口、源码/权重哈希及 NPU 状态一致，并锁定 SGLang legacy GQA 对照；但此前启动曾受 155 的 transformers 版本差异和 V1 元数据差异影响，修复后才进入完整 64-rank、352 文件权重加载。target-only 最终在 KV-cache profiling 的 KDA `o_norm` 处复现 32 对 512 的形状错误，尚未进入请求、精度或接收率验证；根因归因于 full-rank `g_proj` 被误套用 TP16 sequence all-gather。最小修复已通过 `git diff --check` 和 `py_compile`，但本机 pytest 因缺少 `torch` 未收集运行，远端四机回归仍未完成。
  title: 四机预检与完整权重基线失败
- block_id: 33f2e3a496efa909bdbdef6c3e4399bef0d99e3203066d949a2033a3c41d83e5
  indexed: true
  sha256: af0ac052b1eb63be4dbead9ddc05ebaed60a77e7ca89ce4edf9f283cf37df5ba
  source_range:
    end: 13606139
    part: 7
    start: 0
  summary: 适配路线改为以 PR12950 提交 `93b073de` 为基线，仅移植 legacy GQA DSpark 所需的配置归一化、materialized residual capture、Qwen3 DSpark 识别、greedy/7-token fail-fast、Kimi media placeholder fallback 及新版 Mamba postprocess ABI。PR12950 已包含 `g_proj`、KDA、FC1、量化和 MRV1 等修复，故未重复移植 linear 代码，也未引入 MLA、MRV2 或 shared-expert 改动。静态检查、ruff、10 个文件的 `py_compile` 和 manifest 校验通过，但服务、远端运行、专门 Triton/Kimi fallback UT 及真实精度/接收率验证尚未完成；用户明确要求不重新编译算子，后续复用哈希一致的现有制品。
  title: 转向 PR12950 基线的 GQA DSpark 适配
- block_id: 615c9cc8b9226c810c57cdeb4665e7064c66b81d1b03649a8c9fb7924c2e90ca
  indexed: true
  sha256: e8f703523cb24e10d1146199cbc89116dbfaaf054108a645f2804793fb2cdbfc
  source_range:
    end: 23652462
    part: 0
    start: 13606139
  summary: PR12950 的最小 DSpark 适配完成静态收口，相关检查和 92 个定向 UT 通过，并在四机完成代码、core、算子、权重及环境 preflight；这些结果不等于完整运行成功。四机 target-only 服务完成权重与 KV profile，但首个 chat 请求返回 HTTP 500，后定位为 K3 XTML parser 不接受 legacy 空工具尾段，而非 Transformers 或 tokenizer 错误；parser 临时补丁随后已撤销。上游 Kimi frontend 对齐基线记录为 vLLM main `a0c092ee…`、Rust frontend `2899dca…` 和 Python frontend `f5a7cce…`，但 modeling、多模态 preprocess、parser、tokenizer 对齐被明确延后，当前优先 DSpark。
  title: PR12950 基线与 target-only 门禁
- block_id: 498c9a9ea5028ec66ed37d59dfdf904fbaa0fc9b81e998e92f0bfd445c2d6f4c
  indexed: true
  sha256: ea2d5454fc85fde11ade22dae63249c0958b1bfc6dc87d39c928fd139a07ecb1
  source_range:
    end: 23652462
    part: 1
    start: 13606139
  summary: target-only GSM8K 分片最终为 64/64 正确且可解析；四机 MRV1、GQA、gamma 7、greedy、prefix-off DSpark 服务也完成 target 352 shards、draft 1 shard、materialized GQA capture 和 Ready 门禁。首条 DSpark 结果答案正确但接收率仅 24.23%，mean acceptance length 2.696，逐位置接收率从 82.1% 快速降至 0%，与记录的 SGLang 量化基线约 66.66% 存在明显差距；因此低接收率被保留为未解决问题，不能视为统计口径或成功。静态审计暂未证明 hidden capture、aux 顺序、共享 embedding/lm_head 或 KDA/conv commit 的确定性错误；建议按首轮与后续轮次区分，并以 `L=x+1`、`L=1…8` 做状态 oracle。
  title: DSpark 首轮低接收率与 SGLang 对照
- block_id: 03bac1d691c5279bc436ad208f2a0c438f283d20bb26fc884b1c0cc19bd99ee8
  indexed: true
  sha256: 057eef0c714a5532752041eaec38050405bf1598d3eadf7d752792ad91db55eb
  source_range:
    end: 23652462
    part: 2
    start: 13606139
  summary: SGLang oracle 多次未能产出可用接收率对照：首次因 `npu_grouped_matmul(per_token_scale=[None])` 与 torch_npu schema 不兼容退出，随后发现 DeepEP internode 路径写死 `use_quant=False`；尝试 `alltoall` 又暴露 `Buffer.dispatch` 的 `quant_mode` API 不匹配。最终路线收紧为 exact image 的 `low_latency`、chunk 512、dispatcher dtype `auto`，但材料只确认启动配置和失败原因，未提供成功 oracle 结果。vLLM 的纯 Python DSpark probe 已完成、默认关闭并通过静态检查，门控变量为 `VLLM_ASCEND_DSPARK_PROBE_STEPS=8`，用于记录 FC/hidden norm、Markov、target greedy、match bits 和 accepted count；本机缺少 Torch/Pytest，尚未运行 NPU UT，也未证明 probe 已解决低接收率。
  title: SGLang oracle 失败与 DSpark 探针
- block_id: c7acf6419c335586ad1f4406ecaa8eab57e8689032b0eb1a456a483b8c604288
  indexed: true
  sha256: 35602ca8a37445a65261214b498df5cc9b42439062e05c0c45c53f4b6b1ad23f
  source_range:
    end: 23652462
    part: 3
    start: 13606139
  summary: 审计指出 DeepEP 2026.7.4 的 normal dispatch 需要显式 `quant_mode`，现有 SGLang 调用漏传，导致 `hidden_states_scale=None`/`per_token_scale=[None]`；建议采用仅 NPU Python 兼容补丁，保留 `auto`，而 low-latency 旁路仅是有限正确性方案且随后在 graph capture 阶段失败。相关只读复核还认为 DSpark 的 RoPE 抽样改动和当前配置未见确定性静态错误，但发现 Markov bias 仍全量 FP32 统计；K3 parser/tokenizer/frontend 应以后续上游 Python 提交为基线，临时 XTML parser 修改已回退，补丁可应用不等于已编译或测试成功。
  title: DeepEP normal dispatch 量化参数与上游前端审计
- block_id: 9226639d2eb05d6cecaba48db5e7568064ff78427e41de7cc05d71b49be0b91d
  indexed: true
  sha256: cc52a1e0513bfca315a255b57233dad9c0eb3b2c49d0b144f33cca00b7c9e3f8
  source_range:
    end: 23652462
    part: 4
    start: 13606139
  summary: 只读审计确认上游 K3 modeling/config 尚未完整接入 Ascend，不能整体替换 PR12950 的文本模型、KDA、旧 GQA DSpark 或 A3 projector；可优先按文件语义对齐标准 `image` processor、MM preprocess、renderer placeholder，并保留 Ascend 特有路径。vLLM 与 SGLang 同提示首块 anchor/首个 draft 对齐，但第二个 draft 即分叉；embedding/lm_head、非因果标志、逐层 K norm、KV 提交覆盖和现成 fused 路径已被排除为确定根因，当前高概率差异转向 Q/K RMSNorm+NeoX RoPE 的 BF16 舍入时机。
  title: 上游 K3 多模态边界与 DSpark 首块分叉定位
- block_id: a0fc27b4149ad295fbe2bc15ad0f7480bf929900b47a2581b1fe7fe05b7879ba
  indexed: true
  sha256: 728cf2f067adc9a854172624b55aa557d6e9b47afcb32d0aaea82d854b344ec2
  source_range:
    end: 23652462
    part: 5
    start: 13606139
  summary: 当前 vLLM probe 明确运行在纯 eager：顶层 `--enforce-eager` 关闭 torch.compile/CUDAGraph，DSpark 也未执行 GraphFusion 或 ACLGraph replay，且 head_dim=64 不满足现有 QKNorm+RoPE fusion pattern。vLLM separate 与仓内 fused 在 BF16 输出逐元素一致，但 SGLang 使用 FP32 连续完成 Q/K RMSNorm、affine 和 NeoX RoPE 后才一次 cast BF16，已观测到相对两次 BF16 路径的 Q/K 最大差；后续首块实验范围收窄为仅替换 5 层 query Q/K，context K、V、attention 和 cache 保持不变，但 SGLang `/health` 会消耗 probe 请求，结果需按 anchor=646 识别。
  title: Eager row-probe 与 FP32 Q/K 对照实验
- block_id: 92d831578aa0fca9c37b4c1a1c24d394e80a0af710faf278af6eb90e1bf4618a
  indexed: true
  sha256: f66d35c207cf04153382f658dd054b8a8401c969992a0eb8be1a4ab5d9f867fc
  source_range:
    end: 35456841
    part: 0
    start: 23652462
  summary: 实验曾因 warmup 参数门禁失败，随后在容器内完成四机加载；首轮服务健康但未立即发请求。严格 `1/sqrt` 的 FP32 Q/K helper 已逐值命中 SGLang（anchor=646、positions=720–726），但 layer0 attention output 仍接近旧 vLLM，接收率保持 95/392（24.23%），因此 query Q/K 不是低接收率根因。后续诊断范围收窄到 context hidden→KV、实际 attention 长度及缓存读取；探针和临时 monkeypatch 仍属诊断性改动，且曾有宿主机误启动导致四 rank 在加载前失败的记录。
  title: Q/K FP32 对齐未提升接收率
- block_id: 48d91e5ca962dbc0ea46c5dfa1136b4eb5f4a9f6264eed20c81dc2e9bd4c597c
  indexed: true
  sha256: e2e9d4dab9461bd6894f65724eae4838aeec8d5fc0c418a5414271789d3d13ef
  source_range:
    end: 35456841
    part: 1
    start: 23652462
  summary: 本地 context KV、cache、metadata 和手算 attention 探针通过 `py_compile`、`git diff --check`，但尚未完成 NPU 运行验证；逐层与宽 GEMM 差异仅 K RMS 7.3e-5、V RMS 1.8e-5，缓存回读逐元素一致，vLLM 实际为 `q=7, kv=727`。SGLang 对照首次因残留 vLLM 进程占用显存而统一 NPU OOM，精确清理后重新启动；后续设计确认 SGLang 可能通过两次加 7 使 FIA 使用 `C+14`，但该方案仍需 A/B，不应视为正式修复。此前关于 CPU `seq_lens=720`、逐层投影和若干 attention/cache 路径的排除结论均属于当前探针范围，不能替代最终运行证据。
  title: SGLang 734 KV 假设与探针对照
- block_id: d16384f3313f0ff641ca30f1156160e746680fa31948e4691b061dcf46915817
  indexed: true
  sha256: 2baa78cd746caeaa4c011856d1bef2bfbcfafec407fedfda90361804f42e44f1
  source_range:
    end: 35456841
    part: 2
    start: 23652462
  summary: SGLang warmup 探针确认首轮 `context=6`、draft query=7 时 FIA 使用 `kv=20`，即 `C+14`；同请求 SGLang 接收率为 47.22%，而 vLLM 的核心故障进一步定位为 manager/page size 384 与展开 block table/FIA 逻辑 block size 128 混用，导致 query slot 从应有的 4944–4950 写成 13392–13398。Python-only 修复让 slot、FIA 读取和手算 attention 对齐，固定 offset=5 请求答案正确，接收率从 24.23% 升至 62.76%，但该结果仍是单请求门禁，不等同于完整生产验证。四机服务和容器内单测 29/29 已通过；ACLGraph `FULL_DECODE_ONLY` 的后续验证与探针清理/生产化收口尚未在材料中给出完成结果。
  title: Hybrid block size 混用导致 slot 错位
- block_id: 85f61ba53317af56e719b640b28e43cec4826468dd89d28dd5c769d65aae096c
  indexed: true
  sha256: 71319d3d25b9de9c2c9a8366336a909cb96e3cea6e1f1fd1158ea85a5e644efb
  source_range:
    end: 35456841
    part: 3
    start: 23652462
  summary: 四机 ACLGraph FULL_DECODE_ONLY 已完成捕获并健康运行，但 offset=5 请求出现 `[646,0,0,...]`、虚高接收率和 logprobs 序列错位；eager 基线正常，因此未判定图模式通过。诊断将问题收敛到 target speculative verify 的 KDA/GDN metadata、dummy row 和静态 buffer 边界，并记录了 `d4ed5fe86` 与 `0e5adb7b6` 两个历史修复仍需谨慎验证。后续建议用 target-only A/B 和 SGLang 的 padding、state、scatter 语义继续区分根因。
  title: ACLGraph 首轮故障与 KDA/GDN padding 定位
- block_id: 997c084010a70c9a632b54028c41f348014bd2fd717cb121f20c334547f491f3
  indexed: true
  sha256: e7c7ebc50d750f7f9ba601781d86d2744991fcc312327ee4ae0ba3c836842d80
  source_range:
    end: 35456841
    part: 4
    start: 23652462
  summary: 记录确认两处旧 padding 修复必须组合，且单独应用并未根治连续 `!`/`None`；r7–r10 的在线结果显示改善但仍有 batch-sensitive 失败，r11 不能归因于未实际部署的补丁。SGLang 的关键差异是使用 speculative scratch、接受后提交 persistent state，并对 dummy 输出、hidden/logits 和静态 buffer 显式清零与 scatter；vLLM 曾在 recurrent KDA 的 `empty_like` 尾部保留未初始化值。随后采用 Python-only KDA output zero 对齐方案，保留 `accepted=1`、Kimi state `-1` 与普通 GDN 的 `-1→0` 区分，但明确这不是完整 SGLang state-commit 复刻。
  title: SGLang 对照揭示 state 与输出 padding 差异
- block_id: 8d0b6695ff3f66a8446042e65710a1960ac6bd79ba71f0d91c1e7184a32c32b3
  indexed: true
  sha256: 3b6517fac30284df3fdbef0f45f6c8ed396e472e0dd0649a8ddce681fe7a79f4
  source_range:
    end: 35456841
    part: 5
    start: 23652462
  summary: 'helper UT 已覆盖 real/dummy、mixed batch 及 shape/dtype/device，K=7 metadata UT 覆盖 `[0,8,8]`、accepted `[6,1]` 和 `NULL_BLOCK_ID`；ACLGraph 动态 capture/replay 与 runner dummy state 仍缺独立 UT。KDA output 清零后，同一请求从 `[646,0]` 修复为 `[646,12046]`，8/9/32 token、自然结束 offset=5 以及 raw-completion 四 DP 并发均通过；连续 32 条 GSM8K 为 32/32，无 `0/None/!!!`，接收率 56.80%。其中一次 DP3 chat parser HTTP 500 被定位为 `KimiK3XTMLParseError` 缺少 closing message marker，尚未完成 parser、modeling、多模态 preprocess/tokenizer 的上游对齐或 PR #12950 更新。'
  title: SGLang 对齐修复通过 ACLGraph 连续性门禁
- block_id: 4a91aac4f5fedf4638259799edc1a7723aa77974eedb54b9a535a6e6a3530c8f
  indexed: true
  sha256: 0c000d72bf112460e9d19c416fb0ed2d94d18b81f0643e3b7bb02d0a507c077c
  source_range:
    end: 46651066
    part: 0
    start: 35456841
  summary: 只读审计建议多模态整体切换到上游标准 `image` 路径，并成组调整 config、processor、prompt update、renderer 与 KDA 判定；同时建议整体移植上游 reasoning/tool/parser，保留 pinned core 的注册胶水。记录中保留了未完成验证：四机 HTTP 200 不能证明最终组合成功，本地 pytest 受 pytest/Transformers 版本限制，真实 tokenizer 测试因资产 SHA 不匹配未进入功能测试；另有 `_attn_implementation` 嵌套配置被重置为 `None` 的已复现失败。
  title: K3 上游多模态与 Parser 对齐审计
- block_id: 267fe95e6ac1546848983c15a4ebe5314d043ec25d9f9c5e02d1ac50bf5d5a3a
  indexed: true
  sha256: ef9671b9f469d44e617dcd5f230f7734b6ef88e40281094d3a05b5aaf9c7a56e
  source_range:
    end: 46651066
    part: 1
    start: 35456841
  summary: 审计快照记录 base `3d0f7d6`、HEAD `93b073d`、193 个 tracked diff 文件及当前 dirty/untracked 状态，并将改动按算子、模型组网、ViT、transformers utils、tokenizer/parser、MLA split、共享专家 DP+FC1、PD 分离、共享专家多流和 DSpark 十组归类。多个核心文件必须按 hunk/symbol 拆分，且 DSpark 依赖算子、模型、config 与 FC1/FlashComm 修复；建议依赖顺序与用户原先列出的主题顺序不同，不能机械 cherry-pick 旧版本 DSpark 提交。
  title: 十组功能提交拆分与依赖归属
- block_id: 574297d6ec7f278895d6a40e4ceaa05b333bc85d053c63635ccf99af3889cb89
  indexed: true
  sha256: 87b35eee0822c69415650db145ebffa6745405c63caaba09c51f4029d99eb7fc
  source_range:
    end: 46651066
    part: 2
    start: 35456841
  summary: 草案采用 DP4/TP16/EP64、Model Runner V1、DSpark K=7、prefix cache、`FULL_DECODE_ONLY`、FlashComm1 与共享专家多流开启、共享专家 DP 和 fused MC2 关闭，并以占位符隐藏真实 IP、模型和环境路径。附带 Chat Completions、required tool call 与 Prometheus counter delta 检查，但这些仍是可复现命令草案，不是最终四机成功证明；已有的 `3050/5355=56.96%` 仅属于 prefix cache/共享专家多流关闭的旧组合，不能代表目标组合。
  title: 四机 DSpark 启动脚本与验收命令草案
- block_id: 85b6d087b73b1467ae78bc742e1478f4f002606cae45f160aa237d73f9f7d738
  indexed: true
  sha256: 55174d39c2535173e37e1dd1f591299b2a944e1b09669d5411fcde183c34a640
  source_range:
    end: 46651066
    part: 3
    start: 35456841
  summary: 记录 vLLM Kimi K3 的 DP4/TP16/EP64、DSpark K=7、FULL_DECODE_ONLY、prefix cache、FlashComm1 和共享专家多流配置，以及 Chat、tool call、Completions 和基于 counter delta 的接收率检查方法。材料明确要求不能把旧组合累计值 56.96% 当作最终组合结果，并指出旧服务仍关闭 prefix cache/共享专家多流、使用 auto tokenizer 和自定义模板。随后本地修复、真实 tokenizer 测试和十提交重建曾分别报告通过，但最终组合仍缺少完整请求、缓存命中、图 padding、工具调用、completion、固定 workload 接收率及精度门禁证据。
  title: Kimi K3 四机启动配置与旧组合基线
- block_id: 12e898c8f93dcbf7517e4b4c5d4d30ea1da6b7cc409d98c3c78e73db24af675b
  indexed: true
  sha256: ae70d65ce77e886b4722fee47bbd1b0782ab9384e141f941a07f36ef9c7e3712
  source_range:
    end: 46651066
    part: 4
    start: 35456841
  summary: 本块记录最终源码树、10 个 signed-off commits、静态门禁 95 个 Python 文件及 DSpark/KDA/GDN 共 19 个精确测试通过，并保留了隔离部署中 PYTHONPATH 缺少 pyACL、遗漏 Python 扩展、冻结 archive 缺少 catlass 子模块等失败和修正。用户要求改为完整编译后，全量算子生成和三类产物跨四机哈希/import/custom-op 门禁通过；随后四机以 tokenizer_mode=kimi_k3、prefix cache、共享专家多流和 DSpark K=7 启动，64 workers、权重加载、torch.compile 与四 rank ACLGraph capture-size=16 均报告完成。KDA causal-conv1d 前 transpose 的后续优化被明确延期为第 11 个独立 commit，建议在模型初始化/权重加载阶段缓存合并权重，而不是修改 kernel；其实际验证仍未完成。
  title: 最终十提交树、全量构建与四机启动过程
- block_id: 9d4186673b6f0725ce4c0f3924dfc5a8d0d4a9e103a4f86ff0ee7b3d97c0722f
  indexed: true
  sha256: 20e29ab19259114808599451ad9607a01074bc9a16cae00b25f67c973dfe7531
  source_range:
    end: 46651066
    part: 5
    start: 35456841
  summary: 分析认为运行时主要成本是三路权重 cat 和 FP32→BF16 cast，而非 transpose view 本身；推荐在 KDA 初始化时预分配 kernel-native `[W,C]`、persistent=False buffer，由 q/k/v weight_loader 原地写入对应 slice，并保留 dummy/reload 的 lazy fallback。风险包括 loader 顺序、meta/offload、sleep/wake、ACLGraph data pointer 稳定性、dtype、state_dict 和未来 upstream 合并参数的 shape guard；建议覆盖 CPU 排列/stride、reload、NPU prefill/decode/K=7 verify、ACLGraph profiler、输出、DSpark 接收率和 TPOT。当前没有修改文件或操作四机，十提交树仍为 `ec942305b8782be70aa2e0a10102500789ad2af7`。
  title: KDA 权重预打包优化方案与风险测试
- block_id: 2f7958f636769ccd125159c614c48dd9ae2c663422b2c31379a3822663840815
  indexed: true
  sha256: 3ca0e66e35b126047e44f4979e33b1ac2e08322795800cffa90080a329ee3063
  source_range:
    end: 46651066
    part: 6
    start: 35456841
  summary: 材料报告四 rank 完成 352/352 shards、DSpark K=7、FULL_DECODE_ONLY ACLGraph 和 engine init，health 返回 200，日志未见 fatal/error；但本机 urllib 代理曾造成 502 误报，随后用 curl/容器内 validator 排除客户端因素。短题接收率为 81.51%，中文长生成仅 10.64%，而 GSM8K 5-shot 为 56.866%（32/32 正确），因此中文长 chat 的低接收率仍被视为 workload 分布问题，未单独证明泛化正常。PR 曾通过 gh/git 刷新并经历多次远端 head 重写，当前材料报告主线包含 10 个提交；KDA 预打包优化被保留在独立分支，后续审查发现 direct reload 和 layerwise 生命周期边界，尚未合入。
  title: 四机 DSpark 验收与 PR 刷新记录
- block_id: 3df68a6434273bc09a659318deb0572f241c8b8de8956e9dfaf752727ac293f0
  indexed: true
  sha256: f59fe1ccd2091ba696c14c4f69ea4f1ebf876447816159715f70921e4e514c53
  source_range:
    end: 46651066
    part: 7
    start: 35456841
  summary: vLLM main 与 v0.25.1 的视觉 DP helper 签名不同：main 接受 num_heads，v0.25.1 为零参数；材料报告已按仓库惯例使用 vllm_version_is("0.25.1") 方法内分流，并在隔离 UT 中达到 21/21，但这是局部验证，不等同于完整 CI 或 NPU 验收。KDA 预打包曾改为 quant-method process_weights_after_loading 包装，覆盖普通加载、dummy 和部分 layerwise 场景；审查指出 direct/kernel-format 的 param.copy_() 不触发刷新，且 v0.25.1 finalize 可能把 non-persistent buffer 重新注册为 persistent。基于“不改 AscendC kernel、不新增通用生命周期/patch/state-dict hack”的约束，该额外优化被放弃，PR 保留原始正确热路径；其他动态 deinterleave、cache layout transpose 和 output merge 优化仍未完成。
  title: v0.25.1 兼容与 KDA 生命周期审查
- block_id: e5e9cc209e3d4416fc667fb5e7590e99d8827f6e602f9a7a44bab81ce46b4e79
  indexed: true
  sha256: f3ad5adf148f6e7aba7b5b9d4ff3bb78e5a106d8376191a50f9fb858fbf33db6
  source_range:
    end: 46651066
    part: 8
    start: 35456841
  summary: 材料确认不同 runner 的 pack 时机需分别位于 MRV1/MRV2 初始加载、整 reload finalize 或 layerwise finish_weight_update 之后，并要求同步 NPU、保持 ACLGraph 稳定地址；但 prefetch offload 与后续 reload 仍不兼容。审查认为 causal-conv q/k/v 的静态 transpose/cat/cast 是唯一适合当前提交前移的部分，而 q/k/v deinterleave、[H,V,K]↔[H,K,V] cache transpose、fused QKV 和 output merge 属于需要更大架构或 kernel 契约变化的范围。最终结论是 quant-method 包装方案仍有 direct weight update 静默使用旧 packed weight、v0.25.1 persistence 污染 state_dict 两个 P1 缺口，因此不合入该优化，真实 NPU 收益也尚未得到证明。
  title: KDA 优化边界与最终否决
- block_id: d942df4f5f8dcae17ee1020dbac90613abfa69eb150e51af2ab34715ddcea1d4
  indexed: true
  sha256: d646704af331368270fb3f9b22c47b38ca74c938421fa02cad28f6ad847558fb
  source_range:
    end: 54367307
    part: 0
    start: 46651066
  summary: 只读环境盘点显示四台隔离编译容器资源充足，但首次构建因提前注入 overlay 导致 FastAPI 依赖解析 403，随后 CI 又发现测试 `calls = []` 缺少类型注解；这些均是已记录的失败或环境问题。对照 Qwen3.5 和 DeepSeek 后，报告认为 Kimi KDA 应采用 `replace_parameter(prefer_copy=True)` 管理 packed kernel-format Parameter，而不是 non-persistent buffer；该方向随后完成四机编译、服务切换和多项请求验证，但初始顶层 `import vllm_ascend_C` 门禁曾产生假失败，正确模块路径为 `vllm_ascend.vllm_ascend_C`。
  title: 四机基线、依赖构建失败与 KDA 参数生命周期重评
- block_id: 147aa823191841ff55fdc1b56037cfe5f4a6e849e65daae7bd2a1bf5062308a3
  indexed: true
  sha256: 1a235c30eafca80b65249de861f3fe6d98a2dd1967ddc30a9bb97f1add5ac282
  source_range:
    end: 54367307
    part: 1
    start: 46651066
  summary: 新服务完成 352 个 shard 加载、Torch 图编译和四 rank ACLGraph capture，启动期多次 shared-memory 60 秒提示随后自行收敛；最终报告 health=200，chat、tool call、completion、四 rank 短答案、hot-rank dummy padding 和 prefix cache 均通过。KDA 变更先暴露 partial/missing checkpoint reload 可能把未初始化 packed 参数复制进 ACLGraph storage 的精度风险，后续记录称补充回归测试后 main/v0.25.1 CPU 门禁为 31/31，但兼容边界仍限制为完整 checkpoint reload 或携带 packed 参数的 kernel-format reload。四机完整编译和二进制哈希一致，PR 更新到 `38ad149f3`；新 head 硬件 CI 在材料结束时仍为 11 success、12 running、12 queued、0 failure，不能称全 CI 已绿。
  title: KDA packed Parameter 验证与四机真实服务门禁
- block_id: 3925e9c4df03790d55d84d99371c073d1c14d3dfb436a6cffc7e9c03b08d24a9
  indexed: true
  sha256: fde9b5fe25eb58aef69b89d55c7e7b04d40432264d12b89ac53a5a98044eea8a
  source_range:
    end: 54367307
    part: 2
    start: 46651066
  summary: 四机 KDA 服务的正式 GSM8K 门禁报告为 32/32 正确且可解析、DSpark 接收率 `2654/4690 = 56.5885%`，四节点日志无 traceback、RuntimeError、EngineDead、parser error 或 HTTP 500；但这些结果属于已报告的特定 workload 验证。随后用户要求修复或回复 review、处理共性问题并用四机跑带 ViT 的 case；盘点显示远端 head 为 `4b77a156`、base 为 `releases/v0.26.0rc`，5 个 review threads 均未解决且未过时。已提出的共性修复包括收窄 checkpoint config normalization UT、删除基于 `runner.input_batch.block_table` 的异常探测，以及取消全局固定 Qwen3 DSpark `block_size=7`，但该批材料在完整实现、PR 更新和 ViT 四机验证前结束。
  title: PR 检视修复计划与 ViT 场景验证请求
- block_id: b109a931d15c25da480e89e8a78671272e773c658e5e3f5945198b16f32f2a59
  indexed: true
  sha256: b27e0f665db441737e442cd7af116ab15faf400f6e4a781c80daee6f0fb2e86e
  source_range:
    end: 54367307
    part: 3
    start: 46651066
  summary: 已报告移除固定 Qwen3 DSpark `block_size=7`，改以 draft checkpoint 的 `block_size` 为权威，并仅校验其与 `num_speculative_tokens` 一致；Kimi K3 的 K=7 仍来自 checkpoint 元数据。材料还记录 KDA ACLGraph dummy-tail 清零、Kimi 专用 aux hook、vLLM v0.26.0 兼容性，以及 143 passed/11 skipped 和后续 107/107 回归结果，但这些属于特定检查结果，不能替代完整验证。四机源码、core、Catlass 和编译产物据报一致，旧服务已被替换并启动 ViT 配置；ViT、chat、tool、Prefix Cache 和正式 32 题接收率门禁的最终结果仍未提供。
  title: DSpark 共性修复与 ViT 门禁准备
- block_id: 3dd8498ee5fbe5c2aa07e35c1941ad4d47b3b7b3cdbe54307002af96b0fe6ed8
  indexed: true
  sha256: b1fbe334227cc8c47c4bf4a1d9dd7ffbdd7e4d6e476e9beed58e228bbebb0459
  source_range:
    end: 54367307
    part: 4
    start: 46651066
  summary: 正式 GSM8K 门禁要求额外用 `jq` 检查 32/32 解析和正确、质量与指标通过、Kimi K3 block width 为 7，并将接收率限制在 53.5885%–59.5885%、平均接受长度限制在 4.6612–5.2612；短样本 ViT/chat/tool 只能证明 DSpark 工作，不能替代该结论。四机启动曾因完全覆盖 `PYTHONPATH` 导致 CANN `acl` 导入失败，修正后 64/64 rank 初始化完成，12 vision heads 在 TP16 下回退 vision data parallel，target 352/352 权重也已加载。当前已进入约 195 秒级的 size=16 `FULL_DECODE_ONLY` ACLGraph 捕获，真实图片、chat/tool 及正式 32 题结果仍待执行，不能视为验证成功。
  title: 四机 ViT 服务启动与正式接收率门禁
- block_id: 43780624d0e8b9a6f983dc5c92b3aeaec10ef5ba2dc4b9cb4d81b3bedb21e6ec
  indexed: true
  sha256: 2f0c485f825d09bcebefe62109f757dcf914bb65119680cd5d676e9078ff64b6
  source_range:
    end: 57073432
    part: 0
    start: 54367307
  summary: '四 DP 四机实例完成全量重编、ACLGraph 捕获和请求级验证；此前 vLLM v0.26 的 `image_mode` 重复参数问题通过通用 MediaConnector backport 修复，真实图片 sync/async 回归及相关 UT 通过。图片冷/热请求、普通 chat、named tool call、prefix-cache 计数、GSM8K 32 题和 DSpark K=7 指标均报告通过，但 prefix/DSpark 仅能证明聚合计数增长，不能证明每个 rank 的独立命中；named tool 的 `finish_reason=stop` 是契约预期，先前失败来自脚本断言错误。PR #12950 曾更新至 `6ca068805` 并完成 review resolve，随后 CI 暴露并修复了两处 `AscendMLADCPDecodeMetadata` fixture 对新增必填 `seq_lens_device` 的遗漏，相关测试在 v0.26 与 main 环境各报告 124/124 通过，四机服务保持 health=200。'
  title: 四机 ViT 与 DSpark 门禁完成
- block_id: 4249ec1ed7a78b7d41923617b130105f2c5693ed103e5738d86244440cb718a5
  indexed: true
  sha256: 8813ce41a3f8d70d458929fbdacffcd992c2ffae95429937d58d0eff42e89154
  source_range:
    end: 57073432
    part: 1
    start: 54367307
  summary: CI 失败定位为 MLA split 提交 `302dbc342` 新增必填 `seq_lens_device` 后，`test_mla_cp.py` 漏传、`test_acl_graph.py` 仍用位置参数导致字段错位；生产 builder 并无该问题。修复统一显式传递 `seq_lens_device=seq_lens`，并将 ACLGraph 构造改为关键字参数；另有一个无 Python traceback 的 custom-container cancelled job，被保留为可能独立的 runner/容器问题。修复合入 MLA split 后，新 head 为 `4565c88d6524056fef2f32572b2a1aeb0229a6c1`，pre-commit、mypy、select-tests 与前置矩阵报告通过，新 E2E 当时仍在构建设备 cache，尚未形成最终全量 CI 成功结论。此前注释中“普通 chat 4/4 返回 703”是确定性功能门禁值，与这次 CI 失败无关。
  title: PR CI：MLA fixture 修复后重跑
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a15a57f899dc55fa9960e07701cd41ad4363dbe01db65144c1c7ca839508089a
  kind: experience
  material_digest: 207a9a144fbb482277043c1e65725ca42fed655e4e4f86c181906dd91d52a07c
  revision: ada80463f931e70d23b85b7cc5086c290a7adf0269887159370437d5a3a56bb7
  schema: mindie-entry/3
  summary: 当前材料报告四机 ViT、chat、named tool、GSM8K 和 DSpark 门禁已通过，并修复了 vLLM v0.26 MediaConnector 兼容问题；但聚合指标的 rank 归因、JSON 严格格式及未完成或被取消的 CI job 仍需按证据边界理解。随后 CI 的真实根因是两处 MLA DCP 测试 fixture 未同步必填 `seq_lens_device`，已在新 head `4565c88d6` 修复并完成相关本地/跨版本测试；新 E2E 尚只有前置阶段通过，最终设备矩阵和取消 job 的重跑结果仍未提供，不能宣称 CI 全部成功。历史 review、旧 head 备份、四机服务 health=200、脱敏验证脚本及未验证的计划/假设仍可独立检索。
  title: 'PR #12950：ViT/DSpark 验证完成，CI fixture 修复后等待最终 E2E'
navigation: 当前材料报告四机 ViT、chat、named tool、GSM8K 和 DSpark 门禁已通过，并修复了 vLLM v0.26 MediaConnector 兼容问题；但聚合指标的 rank 归因、JSON 严格格式及未完成或被取消的 CI job 仍需按证据边界理解。随后 CI 的真实根因是两处 MLA DCP 测试 fixture 未同步必填 `seq_lens_device`，已在新 head `4565c88d6` 修复并完成相关本地/跨版本测试；新 E2E 尚只有前置阶段通过，最终设备矩阵和取消 job 的重跑结果仍未提供，不能宣称 CI 全部成功。历史 review、旧 head 备份、四机服务 health=200、脱敏验证脚本及未验证的计划/假设仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: a15a57f899dc55fa9960e07701cd41ad4363dbe01db65144c1c7ca839508089a
---

# PR #12950：ViT/DSpark 验证完成，CI fixture 修复后等待最终 E2E

当前材料报告四机 ViT、chat、named tool、GSM8K 和 DSpark 门禁已通过，并修复了 vLLM v0.26 MediaConnector 兼容问题；但聚合指标的 rank 归因、JSON 严格格式及未完成或被取消的 CI job 仍需按证据边界理解。随后 CI 的真实根因是两处 MLA DCP 测试 fixture 未同步必填 `seq_lens_device`，已在新 head `4565c88d6` 修复并完成相关本地/跨版本测试；新 E2E 尚只有前置阶段通过，最终设备矩阵和取消 job 的重跑结果仍未提供，不能宣称 CI 全部成功。历史 review、旧 head 备份、四机服务 health=200、脱敏验证脚本及未验证的计划/假设仍可独立检索。

## Materials

- [PR13071 与 GQA DSpark 定位](blocks/543d3af0e5d9f6249fc53d021bbd22638e91eb8441fa72bcab8868bb54568709.md): 只读审计发现，四机原部署未包含 PR `258ce514`，共享权重是旧 GQA/Qwen3 DSpark，而非 PR 新增的 MLA/K3DSpark 权重；因此早期“低接收率”无法直接归因于该 PR。后续将目标收敛为旧 GQA 权重 + MRV1 greedy，重点修复 `Qwen3DSparkModel` 配置归一化及 aux hidden state 的 materialized residual 语义，同时保留 MLA raw capture；MRV2、probabilistic sampling 和 block verification 仍是不完整路径。四机验证尚未完成：153/155 曾被外部任务占用，单机 EP16/TP16 装载又因每卡专家数量过多 OOM，当前只完成了源码审计、测试与容量分析，未形成精度或接收率成功证据。
- [GQA 质量门与 GSM8K 校准方案](blocks/68771359f41f16b1c74a16af1b5504bfbc2baab825658236c05d42373feb60bd.md): 建立了 GQA DSpark 的验收框架：greedy speculative decoding 需与 target-only 逐 token 一致，短答案、GPQA-Diamond、DP4、prefix cache 和逐位置 survival rate 均有明确门槛；历史仅有 `121/128` 通过、重复 `!`、缺失 closing marker 等失败证据，不能当作已验证基线。模型卡只公开 GSM8K Mean acceptance length `5.6660`，缺少完整评测协议，因此 `5.1–6.2`、`<4.8` 等属于临时工程校准带，不是官方范围；建议采用 SpecForge 风格 GSM8K 5-shot `/v1/completions`、固定 revision 与采样参数，并保存所有协议变量。已完成本地 `benchmark-k3-dspark-gsm8k.py` 及自检，但尚未在真实四机上取得结果。
- [Metrics 差分与 EP16 容量约束](blocks/c261c2c917446d5e29626199d61cc008ccdcaef9c13a4ac2f6d3f39acc0e876b.md): DP1 接收率应通过同一进程 `/metrics` 的 before/after Counter 差分计算，不使用 reset；关键指标是 `vllm:spec_decode_num_drafts_total`、`...draft_tokens_total`、`...accepted_tokens_total` 及七个 `accepted_tokens_per_pos_total`，并校验 `A=ΣP[i]`、位置单调性和 `AL=1+A/D`。测试协议要求关闭 prefix cache、单请求、warmup 后采样，保存 64/128 条快照，并检查 logger 连续性与 `drafts_created` 不变。容量核算显示单机 EP16 需要承载 56 个专家并容易 OOM；layer-prefetch 的 `group=3,num=1` 或 `group=3,num=2`、以及 EP32 的约 33.7% offload 是可行候选，但尚未产生精度或接收率验证结果。
- [单机 GQA 装载、配置核查与 NZ 崩溃定位](blocks/e35f350c9d00ca0cb16b37cd88f642a67d5134eb4ad86c8d30045b5658bba085.md): DP2 因原持有者重启而取消，改在 156 进行 exact-PR、EP16、TP16 的单机隔离装载；运行日志确认旧 GQA draft 归一化为 `Qwen3DSparkModel`，并使用 materialized residual stream，但尚未取得接收率结果。首次完整加载在 `weight_nz_mode=1` 的 offload 后处理阶段因 `TransData ChooseCompileInfo Failed` 失败，根因是 FRACTAL_NZ 权重被通用路径直接搬回 CPU；改为 `weight_nz_mode=0` 仅保留 ND 存储布局后重试。审计还指出 draft 实际无量化声明、外层 quant config 可能共享污染，以及 `enable_reduce_sample=true`、MRV2、错误 draft TP 或非 greedy 请求可能静默绕过 Markov head；这些分析和 fail-fast 保护不等于真实精度成功。
- [单机重试、NZ 布局限制与四机前置审查](blocks/2020ad54eb5887898aa2c7650097ba2bae03551c3065eb43a6d5e76a6f6c61be.md): `weight_nz_mode=0` 重试曾推进至 352/352 shard 并进入后处理，但这只是绕过单机 offload 崩溃的实验路径，不能作为最终性能结论；NZ/ND 可能引入 kernel 和末位 logit 差异，target-only 与 DSpark 必须使用相同设置。代码复审要求配置 guard 在父类初始化前执行，并提醒 MRV2 DSpark 不应被全局拒绝；当前 materialized capture 修复仅覆盖 MRV1。随后确认单机完整权重路线因主机 OOM（`VLLM::Worker_TP` 被内核杀死）不成立，相关服务退出且日志保留；四机验证应使用完整权重、无 offload、MRV1、prefix-off/eager，并分别建立 target-only 与 DSpark 基线。
- [四机资源释放、容器同构与验证门禁](blocks/0cf4f736f7adb4be15832d498ee8b4b96e05062939b455b29da25c4b3106f7d5.md): 153 的 `lala-153-k3-main` 是外部占用的 TP16、DP1、5 层 Kimi K3 服务；按用户授权精确终止后，153–156 的 vLLM/NPU 进程和相关端口均已释放，单机路线不再用于精度验证。155 的 release 容器后来恢复，但审计发现其默认 runtime 为 `ascend`，而 154/156 使用 `runc`；排除 runtime 后镜像、挂载、设备和源码内容一致，因此启动前必须显式核对并统一 runtime、源码/权重 SHA、PID/PGID 和端口。后续建议顺序是四机完整权重 target-only，再运行相同配置的 DSpark，单独检查短答 parity、GSM8K 数据集 SHA、accuracy、7-position metrics 与 mean acceptance length；已有 42 个定向测试通过，但真实四机精度和接收率仍未被证明。
- [四机预检与完整权重基线失败](blocks/09647410620567ffa1f6c87161131c5216ba219a72459ec23fd0d4eaddc8e933.md): 四机容器预检确认 runtime、网络接口、端口、源码/权重哈希及 NPU 状态一致，并锁定 SGLang legacy GQA 对照；但此前启动曾受 155 的 transformers 版本差异和 V1 元数据差异影响，修复后才进入完整 64-rank、352 文件权重加载。target-only 最终在 KV-cache profiling 的 KDA `o_norm` 处复现 32 对 512 的形状错误，尚未进入请求、精度或接收率验证；根因归因于 full-rank `g_proj` 被误套用 TP16 sequence all-gather。最小修复已通过 `git diff --check` 和 `py_compile`，但本机 pytest 因缺少 `torch` 未收集运行，远端四机回归仍未完成。
- [转向 PR12950 基线的 GQA DSpark 适配](blocks/33f2e3a496efa909bdbdef6c3e4399bef0d99e3203066d949a2033a3c41d83e5.md): 适配路线改为以 PR12950 提交 `93b073de` 为基线，仅移植 legacy GQA DSpark 所需的配置归一化、materialized residual capture、Qwen3 DSpark 识别、greedy/7-token fail-fast、Kimi media placeholder fallback 及新版 Mamba postprocess ABI。PR12950 已包含 `g_proj`、KDA、FC1、量化和 MRV1 等修复，故未重复移植 linear 代码，也未引入 MLA、MRV2 或 shared-expert 改动。静态检查、ruff、10 个文件的 `py_compile` 和 manifest 校验通过，但服务、远端运行、专门 Triton/Kimi fallback UT 及真实精度/接收率验证尚未完成；用户明确要求不重新编译算子，后续复用哈希一致的现有制品。
- [PR12950 基线与 target-only 门禁](blocks/615c9cc8b9226c810c57cdeb4665e7064c66b81d1b03649a8c9fb7924c2e90ca.md): PR12950 的最小 DSpark 适配完成静态收口，相关检查和 92 个定向 UT 通过，并在四机完成代码、core、算子、权重及环境 preflight；这些结果不等于完整运行成功。四机 target-only 服务完成权重与 KV profile，但首个 chat 请求返回 HTTP 500，后定位为 K3 XTML parser 不接受 legacy 空工具尾段，而非 Transformers 或 tokenizer 错误；parser 临时补丁随后已撤销。上游 Kimi frontend 对齐基线记录为 vLLM main `a0c092ee…`、Rust frontend `2899dca…` 和 Python frontend `f5a7cce…`，但 modeling、多模态 preprocess、parser、tokenizer 对齐被明确延后，当前优先 DSpark。
- [DSpark 首轮低接收率与 SGLang 对照](blocks/498c9a9ea5028ec66ed37d59dfdf904fbaa0fc9b81e998e92f0bfd445c2d6f4c.md): target-only GSM8K 分片最终为 64/64 正确且可解析；四机 MRV1、GQA、gamma 7、greedy、prefix-off DSpark 服务也完成 target 352 shards、draft 1 shard、materialized GQA capture 和 Ready 门禁。首条 DSpark 结果答案正确但接收率仅 24.23%，mean acceptance length 2.696，逐位置接收率从 82.1% 快速降至 0%，与记录的 SGLang 量化基线约 66.66% 存在明显差距；因此低接收率被保留为未解决问题，不能视为统计口径或成功。静态审计暂未证明 hidden capture、aux 顺序、共享 embedding/lm_head 或 KDA/conv commit 的确定性错误；建议按首轮与后续轮次区分，并以 `L=x+1`、`L=1…8` 做状态 oracle。
- [SGLang oracle 失败与 DSpark 探针](blocks/03bac1d691c5279bc436ad208f2a0c438f283d20bb26fc884b1c0cc19bd99ee8.md): SGLang oracle 多次未能产出可用接收率对照：首次因 `npu_grouped_matmul(per_token_scale=[None])` 与 torch_npu schema 不兼容退出，随后发现 DeepEP internode 路径写死 `use_quant=False`；尝试 `alltoall` 又暴露 `Buffer.dispatch` 的 `quant_mode` API 不匹配。最终路线收紧为 exact image 的 `low_latency`、chunk 512、dispatcher dtype `auto`，但材料只确认启动配置和失败原因，未提供成功 oracle 结果。vLLM 的纯 Python DSpark probe 已完成、默认关闭并通过静态检查，门控变量为 `VLLM_ASCEND_DSPARK_PROBE_STEPS=8`，用于记录 FC/hidden norm、Markov、target greedy、match bits 和 accepted count；本机缺少 Torch/Pytest，尚未运行 NPU UT，也未证明 probe 已解决低接收率。
- [DeepEP normal dispatch 量化参数与上游前端审计](blocks/c7acf6419c335586ad1f4406ecaa8eab57e8689032b0eb1a456a483b8c604288.md): 审计指出 DeepEP 2026.7.4 的 normal dispatch 需要显式 `quant_mode`，现有 SGLang 调用漏传，导致 `hidden_states_scale=None`/`per_token_scale=[None]`；建议采用仅 NPU Python 兼容补丁，保留 `auto`，而 low-latency 旁路仅是有限正确性方案且随后在 graph capture 阶段失败。相关只读复核还认为 DSpark 的 RoPE 抽样改动和当前配置未见确定性静态错误，但发现 Markov bias 仍全量 FP32 统计；K3 parser/tokenizer/frontend 应以后续上游 Python 提交为基线，临时 XTML parser 修改已回退，补丁可应用不等于已编译或测试成功。
- [上游 K3 多模态边界与 DSpark 首块分叉定位](blocks/9226639d2eb05d6cecaba48db5e7568064ff78427e41de7cc05d71b49be0b91d.md): 只读审计确认上游 K3 modeling/config 尚未完整接入 Ascend，不能整体替换 PR12950 的文本模型、KDA、旧 GQA DSpark 或 A3 projector；可优先按文件语义对齐标准 `image` processor、MM preprocess、renderer placeholder，并保留 Ascend 特有路径。vLLM 与 SGLang 同提示首块 anchor/首个 draft 对齐，但第二个 draft 即分叉；embedding/lm_head、非因果标志、逐层 K norm、KV 提交覆盖和现成 fused 路径已被排除为确定根因，当前高概率差异转向 Q/K RMSNorm+NeoX RoPE 的 BF16 舍入时机。
- [Eager row-probe 与 FP32 Q/K 对照实验](blocks/a0fc27b4149ad295fbe2bc15ad0f7480bf929900b47a2581b1fe7fe05b7879ba.md): 当前 vLLM probe 明确运行在纯 eager：顶层 `--enforce-eager` 关闭 torch.compile/CUDAGraph，DSpark 也未执行 GraphFusion 或 ACLGraph replay，且 head_dim=64 不满足现有 QKNorm+RoPE fusion pattern。vLLM separate 与仓内 fused 在 BF16 输出逐元素一致，但 SGLang 使用 FP32 连续完成 Q/K RMSNorm、affine 和 NeoX RoPE 后才一次 cast BF16，已观测到相对两次 BF16 路径的 Q/K 最大差；后续首块实验范围收窄为仅替换 5 层 query Q/K，context K、V、attention 和 cache 保持不变，但 SGLang `/health` 会消耗 probe 请求，结果需按 anchor=646 识别。
- [Q/K FP32 对齐未提升接收率](blocks/92d831578aa0fca9c37b4c1a1c24d394e80a0af710faf278af6eb90e1bf4618a.md): 实验曾因 warmup 参数门禁失败，随后在容器内完成四机加载；首轮服务健康但未立即发请求。严格 `1/sqrt` 的 FP32 Q/K helper 已逐值命中 SGLang（anchor=646、positions=720–726），但 layer0 attention output 仍接近旧 vLLM，接收率保持 95/392（24.23%），因此 query Q/K 不是低接收率根因。后续诊断范围收窄到 context hidden→KV、实际 attention 长度及缓存读取；探针和临时 monkeypatch 仍属诊断性改动，且曾有宿主机误启动导致四 rank 在加载前失败的记录。
- [SGLang 734 KV 假设与探针对照](blocks/48d91e5ca962dbc0ea46c5dfa1136b4eb5f4a9f6264eed20c81dc2e9bd4c597c.md): 本地 context KV、cache、metadata 和手算 attention 探针通过 `py_compile`、`git diff --check`，但尚未完成 NPU 运行验证；逐层与宽 GEMM 差异仅 K RMS 7.3e-5、V RMS 1.8e-5，缓存回读逐元素一致，vLLM 实际为 `q=7, kv=727`。SGLang 对照首次因残留 vLLM 进程占用显存而统一 NPU OOM，精确清理后重新启动；后续设计确认 SGLang 可能通过两次加 7 使 FIA 使用 `C+14`，但该方案仍需 A/B，不应视为正式修复。此前关于 CPU `seq_lens=720`、逐层投影和若干 attention/cache 路径的排除结论均属于当前探针范围，不能替代最终运行证据。
- [Hybrid block size 混用导致 slot 错位](blocks/d16384f3313f0ff641ca30f1156160e746680fa31948e4691b061dcf46915817.md): SGLang warmup 探针确认首轮 `context=6`、draft query=7 时 FIA 使用 `kv=20`，即 `C+14`；同请求 SGLang 接收率为 47.22%，而 vLLM 的核心故障进一步定位为 manager/page size 384 与展开 block table/FIA 逻辑 block size 128 混用，导致 query slot 从应有的 4944–4950 写成 13392–13398。Python-only 修复让 slot、FIA 读取和手算 attention 对齐，固定 offset=5 请求答案正确，接收率从 24.23% 升至 62.76%，但该结果仍是单请求门禁，不等同于完整生产验证。四机服务和容器内单测 29/29 已通过；ACLGraph `FULL_DECODE_ONLY` 的后续验证与探针清理/生产化收口尚未在材料中给出完成结果。
- [ACLGraph 首轮故障与 KDA/GDN padding 定位](blocks/85f61ba53317af56e719b640b28e43cec4826468dd89d28dd5c769d65aae096c.md): 四机 ACLGraph FULL_DECODE_ONLY 已完成捕获并健康运行，但 offset=5 请求出现 `[646,0,0,...]`、虚高接收率和 logprobs 序列错位；eager 基线正常，因此未判定图模式通过。诊断将问题收敛到 target speculative verify 的 KDA/GDN metadata、dummy row 和静态 buffer 边界，并记录了 `d4ed5fe86` 与 `0e5adb7b6` 两个历史修复仍需谨慎验证。后续建议用 target-only A/B 和 SGLang 的 padding、state、scatter 语义继续区分根因。
- [SGLang 对照揭示 state 与输出 padding 差异](blocks/997c084010a70c9a632b54028c41f348014bd2fd717cb121f20c334547f491f3.md): 记录确认两处旧 padding 修复必须组合，且单独应用并未根治连续 `!`/`None`；r7–r10 的在线结果显示改善但仍有 batch-sensitive 失败，r11 不能归因于未实际部署的补丁。SGLang 的关键差异是使用 speculative scratch、接受后提交 persistent state，并对 dummy 输出、hidden/logits 和静态 buffer 显式清零与 scatter；vLLM 曾在 recurrent KDA 的 `empty_like` 尾部保留未初始化值。随后采用 Python-only KDA output zero 对齐方案，保留 `accepted=1`、Kimi state `-1` 与普通 GDN 的 `-1→0` 区分，但明确这不是完整 SGLang state-commit 复刻。
- [SGLang 对齐修复通过 ACLGraph 连续性门禁](blocks/8d0b6695ff3f66a8446042e65710a1960ac6bd79ba71f0d91c1e7184a32c32b3.md): helper UT 已覆盖 real/dummy、mixed batch 及 shape/dtype/device，K=7 metadata UT 覆盖 `[0,8,8]`、accepted `[6,1]` 和 `NULL_BLOCK_ID`；ACLGraph 动态 capture/replay 与 runner dummy state 仍缺独立 UT。KDA output 清零后，同一请求从 `[646,0]` 修复为 `[646,12046]`，8/9/32 token、自然结束 offset=5 以及 raw-completion 四 DP 并发均通过；连续 32 条 GSM8K 为 32/32，无 `0/None/!!!`，接收率 56.80%。其中一次 DP3 chat parser HTTP 500 被定位为 `KimiK3XTMLParseError` 缺少 closing message marker，尚未完成 parser、modeling、多模态 preprocess/tokenizer 的上游对齐或 PR #12950 更新。
- [K3 上游多模态与 Parser 对齐审计](blocks/4a91aac4f5fedf4638259799edc1a7723aa77974eedb54b9a535a6e6a3530c8f.md): 只读审计建议多模态整体切换到上游标准 `image` 路径，并成组调整 config、processor、prompt update、renderer 与 KDA 判定；同时建议整体移植上游 reasoning/tool/parser，保留 pinned core 的注册胶水。记录中保留了未完成验证：四机 HTTP 200 不能证明最终组合成功，本地 pytest 受 pytest/Transformers 版本限制，真实 tokenizer 测试因资产 SHA 不匹配未进入功能测试；另有 `_attn_implementation` 嵌套配置被重置为 `None` 的已复现失败。
- [十组功能提交拆分与依赖归属](blocks/267fe95e6ac1546848983c15a4ebe5314d043ec25d9f9c5e02d1ac50bf5d5a3a.md): 审计快照记录 base `3d0f7d6`、HEAD `93b073d`、193 个 tracked diff 文件及当前 dirty/untracked 状态，并将改动按算子、模型组网、ViT、transformers utils、tokenizer/parser、MLA split、共享专家 DP+FC1、PD 分离、共享专家多流和 DSpark 十组归类。多个核心文件必须按 hunk/symbol 拆分，且 DSpark 依赖算子、模型、config 与 FC1/FlashComm 修复；建议依赖顺序与用户原先列出的主题顺序不同，不能机械 cherry-pick 旧版本 DSpark 提交。
- [四机 DSpark 启动脚本与验收命令草案](blocks/574297d6ec7f278895d6a40e4ceaa05b333bc85d053c63635ccf99af3889cb89.md): 草案采用 DP4/TP16/EP64、Model Runner V1、DSpark K=7、prefix cache、`FULL_DECODE_ONLY`、FlashComm1 与共享专家多流开启、共享专家 DP 和 fused MC2 关闭，并以占位符隐藏真实 IP、模型和环境路径。附带 Chat Completions、required tool call 与 Prometheus counter delta 检查，但这些仍是可复现命令草案，不是最终四机成功证明；已有的 `3050/5355=56.96%` 仅属于 prefix cache/共享专家多流关闭的旧组合，不能代表目标组合。
- [Kimi K3 四机启动配置与旧组合基线](blocks/85b6d087b73b1467ae78bc742e1478f4f002606cae45f160aa237d73f9f7d738.md): 记录 vLLM Kimi K3 的 DP4/TP16/EP64、DSpark K=7、FULL_DECODE_ONLY、prefix cache、FlashComm1 和共享专家多流配置，以及 Chat、tool call、Completions 和基于 counter delta 的接收率检查方法。材料明确要求不能把旧组合累计值 56.96% 当作最终组合结果，并指出旧服务仍关闭 prefix cache/共享专家多流、使用 auto tokenizer 和自定义模板。随后本地修复、真实 tokenizer 测试和十提交重建曾分别报告通过，但最终组合仍缺少完整请求、缓存命中、图 padding、工具调用、completion、固定 workload 接收率及精度门禁证据。
- [最终十提交树、全量构建与四机启动过程](blocks/12e898c8f93dcbf7517e4b4c5d4d30ea1da6b7cc409d98c3c78e73db24af675b.md): 本块记录最终源码树、10 个 signed-off commits、静态门禁 95 个 Python 文件及 DSpark/KDA/GDN 共 19 个精确测试通过，并保留了隔离部署中 PYTHONPATH 缺少 pyACL、遗漏 Python 扩展、冻结 archive 缺少 catlass 子模块等失败和修正。用户要求改为完整编译后，全量算子生成和三类产物跨四机哈希/import/custom-op 门禁通过；随后四机以 tokenizer_mode=kimi_k3、prefix cache、共享专家多流和 DSpark K=7 启动，64 workers、权重加载、torch.compile 与四 rank ACLGraph capture-size=16 均报告完成。KDA causal-conv1d 前 transpose 的后续优化被明确延期为第 11 个独立 commit，建议在模型初始化/权重加载阶段缓存合并权重，而不是修改 kernel；其实际验证仍未完成。
- [KDA 权重预打包优化方案与风险测试](blocks/9d4186673b6f0725ce4c0f3924dfc5a8d0d4a9e103a4f86ff0ee7b3d97c0722f.md): 分析认为运行时主要成本是三路权重 cat 和 FP32→BF16 cast，而非 transpose view 本身；推荐在 KDA 初始化时预分配 kernel-native `[W,C]`、persistent=False buffer，由 q/k/v weight_loader 原地写入对应 slice，并保留 dummy/reload 的 lazy fallback。风险包括 loader 顺序、meta/offload、sleep/wake、ACLGraph data pointer 稳定性、dtype、state_dict 和未来 upstream 合并参数的 shape guard；建议覆盖 CPU 排列/stride、reload、NPU prefill/decode/K=7 verify、ACLGraph profiler、输出、DSpark 接收率和 TPOT。当前没有修改文件或操作四机，十提交树仍为 `ec942305b8782be70aa2e0a10102500789ad2af7`。
- [四机 DSpark 验收与 PR 刷新记录](blocks/2f7958f636769ccd125159c614c48dd9ae2c663422b2c31379a3822663840815.md): 材料报告四 rank 完成 352/352 shards、DSpark K=7、FULL_DECODE_ONLY ACLGraph 和 engine init，health 返回 200，日志未见 fatal/error；但本机 urllib 代理曾造成 502 误报，随后用 curl/容器内 validator 排除客户端因素。短题接收率为 81.51%，中文长生成仅 10.64%，而 GSM8K 5-shot 为 56.866%（32/32 正确），因此中文长 chat 的低接收率仍被视为 workload 分布问题，未单独证明泛化正常。PR 曾通过 gh/git 刷新并经历多次远端 head 重写，当前材料报告主线包含 10 个提交；KDA 预打包优化被保留在独立分支，后续审查发现 direct reload 和 layerwise 生命周期边界，尚未合入。
- [v0.25.1 兼容与 KDA 生命周期审查](blocks/3df68a6434273bc09a659318deb0572f241c8b8de8956e9dfaf752727ac293f0.md): vLLM main 与 v0.25.1 的视觉 DP helper 签名不同：main 接受 num_heads，v0.25.1 为零参数；材料报告已按仓库惯例使用 vllm_version_is("0.25.1") 方法内分流，并在隔离 UT 中达到 21/21，但这是局部验证，不等同于完整 CI 或 NPU 验收。KDA 预打包曾改为 quant-method process_weights_after_loading 包装，覆盖普通加载、dummy 和部分 layerwise 场景；审查指出 direct/kernel-format 的 param.copy_() 不触发刷新，且 v0.25.1 finalize 可能把 non-persistent buffer 重新注册为 persistent。基于“不改 AscendC kernel、不新增通用生命周期/patch/state-dict hack”的约束，该额外优化被放弃，PR 保留原始正确热路径；其他动态 deinterleave、cache layout transpose 和 output merge 优化仍未完成。
- [KDA 优化边界与最终否决](blocks/e5e9cc209e3d4416fc667fb5e7590e99d8827f6e602f9a7a44bab81ce46b4e79.md): 材料确认不同 runner 的 pack 时机需分别位于 MRV1/MRV2 初始加载、整 reload finalize 或 layerwise finish_weight_update 之后，并要求同步 NPU、保持 ACLGraph 稳定地址；但 prefetch offload 与后续 reload 仍不兼容。审查认为 causal-conv q/k/v 的静态 transpose/cat/cast 是唯一适合当前提交前移的部分，而 q/k/v deinterleave、[H,V,K]↔[H,K,V] cache transpose、fused QKV 和 output merge 属于需要更大架构或 kernel 契约变化的范围。最终结论是 quant-method 包装方案仍有 direct weight update 静默使用旧 packed weight、v0.25.1 persistence 污染 state_dict 两个 P1 缺口，因此不合入该优化，真实 NPU 收益也尚未得到证明。
- [四机基线、依赖构建失败与 KDA 参数生命周期重评](blocks/d942df4f5f8dcae17ee1020dbac90613abfa69eb150e51af2ab34715ddcea1d4.md): 只读环境盘点显示四台隔离编译容器资源充足，但首次构建因提前注入 overlay 导致 FastAPI 依赖解析 403，随后 CI 又发现测试 `calls = []` 缺少类型注解；这些均是已记录的失败或环境问题。对照 Qwen3.5 和 DeepSeek 后，报告认为 Kimi KDA 应采用 `replace_parameter(prefer_copy=True)` 管理 packed kernel-format Parameter，而不是 non-persistent buffer；该方向随后完成四机编译、服务切换和多项请求验证，但初始顶层 `import vllm_ascend_C` 门禁曾产生假失败，正确模块路径为 `vllm_ascend.vllm_ascend_C`。
- [KDA packed Parameter 验证与四机真实服务门禁](blocks/147aa823191841ff55fdc1b56037cfe5f4a6e849e65daae7bd2a1bf5062308a3.md): 新服务完成 352 个 shard 加载、Torch 图编译和四 rank ACLGraph capture，启动期多次 shared-memory 60 秒提示随后自行收敛；最终报告 health=200，chat、tool call、completion、四 rank 短答案、hot-rank dummy padding 和 prefix cache 均通过。KDA 变更先暴露 partial/missing checkpoint reload 可能把未初始化 packed 参数复制进 ACLGraph storage 的精度风险，后续记录称补充回归测试后 main/v0.25.1 CPU 门禁为 31/31，但兼容边界仍限制为完整 checkpoint reload 或携带 packed 参数的 kernel-format reload。四机完整编译和二进制哈希一致，PR 更新到 `38ad149f3`；新 head 硬件 CI 在材料结束时仍为 11 success、12 running、12 queued、0 failure，不能称全 CI 已绿。
- [PR 检视修复计划与 ViT 场景验证请求](blocks/3925e9c4df03790d55d84d99371c073d1c14d3dfb436a6cffc7e9c03b08d24a9.md): 四机 KDA 服务的正式 GSM8K 门禁报告为 32/32 正确且可解析、DSpark 接收率 `2654/4690 = 56.5885%`，四节点日志无 traceback、RuntimeError、EngineDead、parser error 或 HTTP 500；但这些结果属于已报告的特定 workload 验证。随后用户要求修复或回复 review、处理共性问题并用四机跑带 ViT 的 case；盘点显示远端 head 为 `4b77a156`、base 为 `releases/v0.26.0rc`，5 个 review threads 均未解决且未过时。已提出的共性修复包括收窄 checkpoint config normalization UT、删除基于 `runner.input_batch.block_table` 的异常探测，以及取消全局固定 Qwen3 DSpark `block_size=7`，但该批材料在完整实现、PR 更新和 ViT 四机验证前结束。
- [DSpark 共性修复与 ViT 门禁准备](blocks/b109a931d15c25da480e89e8a78671272e773c658e5e3f5945198b16f32f2a59.md): 已报告移除固定 Qwen3 DSpark `block_size=7`，改以 draft checkpoint 的 `block_size` 为权威，并仅校验其与 `num_speculative_tokens` 一致；Kimi K3 的 K=7 仍来自 checkpoint 元数据。材料还记录 KDA ACLGraph dummy-tail 清零、Kimi 专用 aux hook、vLLM v0.26.0 兼容性，以及 143 passed/11 skipped 和后续 107/107 回归结果，但这些属于特定检查结果，不能替代完整验证。四机源码、core、Catlass 和编译产物据报一致，旧服务已被替换并启动 ViT 配置；ViT、chat、tool、Prefix Cache 和正式 32 题接收率门禁的最终结果仍未提供。
- [四机 ViT 服务启动与正式接收率门禁](blocks/3dd8498ee5fbe5c2aa07e35c1941ad4d47b3b7b3cdbe54307002af96b0fe6ed8.md): 正式 GSM8K 门禁要求额外用 `jq` 检查 32/32 解析和正确、质量与指标通过、Kimi K3 block width 为 7，并将接收率限制在 53.5885%–59.5885%、平均接受长度限制在 4.6612–5.2612；短样本 ViT/chat/tool 只能证明 DSpark 工作，不能替代该结论。四机启动曾因完全覆盖 `PYTHONPATH` 导致 CANN `acl` 导入失败，修正后 64/64 rank 初始化完成，12 vision heads 在 TP16 下回退 vision data parallel，target 352/352 权重也已加载。当前已进入约 195 秒级的 size=16 `FULL_DECODE_ONLY` ACLGraph 捕获，真实图片、chat/tool 及正式 32 题结果仍待执行，不能视为验证成功。
- [四机 ViT 与 DSpark 门禁完成](blocks/43780624d0e8b9a6f983dc5c92b3aeaec10ef5ba2dc4b9cb4d81b3bedb21e6ec.md): 四 DP 四机实例完成全量重编、ACLGraph 捕获和请求级验证；此前 vLLM v0.26 的 `image_mode` 重复参数问题通过通用 MediaConnector backport 修复，真实图片 sync/async 回归及相关 UT 通过。图片冷/热请求、普通 chat、named tool call、prefix-cache 计数、GSM8K 32 题和 DSpark K=7 指标均报告通过，但 prefix/DSpark 仅能证明聚合计数增长，不能证明每个 rank 的独立命中；named tool 的 `finish_reason=stop` 是契约预期，先前失败来自脚本断言错误。PR #12950 曾更新至 `6ca068805` 并完成 review resolve，随后 CI 暴露并修复了两处 `AscendMLADCPDecodeMetadata` fixture 对新增必填 `seq_lens_device` 的遗漏，相关测试在 v0.26 与 main 环境各报告 124/124 通过，四机服务保持 health=200。
- [PR CI：MLA fixture 修复后重跑](blocks/4249ec1ed7a78b7d41923617b130105f2c5693ed103e5738d86244440cb718a5.md): CI 失败定位为 MLA split 提交 `302dbc342` 新增必填 `seq_lens_device` 后，`test_mla_cp.py` 漏传、`test_acl_graph.py` 仍用位置参数导致字段错位；生产 builder 并无该问题。修复统一显式传递 `seq_lens_device=seq_lens`，并将 ACLGraph 构造改为关键字参数；另有一个无 Python traceback 的 custom-container cancelled job，被保留为可能独立的 runner/容器问题。修复合入 MLA split 后，新 head 为 `4565c88d6524056fef2f32572b2a1aeb0229a6c1`，pre-commit、mypy、select-tests 与前置矩阵报告通过，新 E2E 当时仍在构建设备 cache，尚未形成最终全量 CI 成功结论。此前注释中“普通 chat 4/4 返回 703”是确定性功能门禁值，与这次 CI 失败无关。

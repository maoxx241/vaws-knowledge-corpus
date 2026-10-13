---
blocks:
- block_id: ec058b3f78a436f89b66031266f385c9f30e44bd457ceede05da73085f45a626
  indexed: true
  sha256: d598f8bb7cd2e7d593191ae96b0dcd1a8e764fef8eff988838d1f0963254808e
  source_range:
    end: 10869521
    part: 0
    start: 0
  summary: 目标是在 vLLM-Ascend 0.23.0 基础上适配 Kimi K3，涉及 SiTU 激活、无 RoPE MLA 与 gated o_proj、KDA 算子及模型组网，并参考 PR141；PR142 被明确视为不可靠的 fake adapter。过程中确认了 CANN 9.0.1（用户口径称 9.1）的 A3 容器、4 层 dummy 验证范围，以及 KDA prefill/decode 的 AscendC/Triton 分工。已有报告称 SiTU/MoE、KDA 测试和部分 NPU smoke 通过，但真实量化权重端到端执行当时仍未覆盖。
  title: Kimi K3 适配目标与早期验证
- block_id: 576392e2184b93e6fe6adadd9fabcf0731c4a6435239ed8d141c325efa4ca2d2
  indexed: true
  sha256: 95da2edd667a7be4ef589dfb4249b31994fb067aa2786913a5cb36c692382af8
  source_range:
    end: 10869521
    part: 1
    start: 0
  summary: 5 层 ModelSlim 权重被识别为 routed W4A8_DYNAMIC、shared/latent W8A8_DYNAMIC，随后补充了 packed mapping、padded A_log 裁剪分片、无 RoPE 直通及 K3 KDA 层类型修正。BF16 首 forward 的 `aclnnMoeInitRoutingCustom ... not in libopapi.so` 被进一步定位为镜像中 vLLM-Ascend 扩展/ vendor ACLNN 包不匹配，而非源码删除；源码注册、A3 构建清单和 Torch binding 仍存在。独立重编译后的最小算子包已在 A3 完成有限 NPU 调用验证，但完整 vLLM-Ascend 重编译和真实模型首 forward 当时仍在进行。
  title: A3 权重加载与算子环境诊断
- block_id: 7d1537e50b9885d826adf268636ae59e8b8269bae35d8c5545c9c21b2cceb2ce
  indexed: true
  sha256: 071b646b45807c345ad1b7070f5007080f0ef4c269b2fb54184f396caecbfb30
  source_range:
    end: 10869521
    part: 2
    start: 0
  summary: 审计结论继续确认 `moe_init_routing_custom` 未被废弃，旧失败来自预装 vendor 包缺失或版本不一致；完整扩展已报告编译成功，并包含 MoE 与 KDA schema。真实 5 层 ModelSlim 的 19/19 分片和 TP8 权重加载已报告成功，累计回归为 `235 passed, 9 skipped`，但首个 W4A8 MoE forward 在旧环境仍因缺少该 ACLNN 符号失败；这一结果不能视为完整端到端成功。当前 PR 草案包含 PR141 等价的 KDA 提交、Kimi K3 模型/MLA/MoE/量化改动及测试，并明确保留 A3 验证边界、未宣称五层 INT4 最终 rerun 已通过。
  title: 整合审计、测试结果与 PR 草案
- block_id: 74c0eeb3a9d9393d320488479c1d2c580ad3a38ad54e551e37dedf1e4ece35cf
  indexed: true
  sha256: 81a5dd920bb86a69db54b9281298d06bedea4657531992043ad72a5e8083a512
  source_range:
    end: 10869521
    part: 3
    start: 0
  summary: 记录了 K3 在 Ascend/vLLM 0.23 环境中的验证链路：旧 vendor 包导致 `MoeInitRoutingCustom` 缺失，完整重编译后算子可实际调用；W4 权重是每字节两个 int4，转 int32 仅作 ABI view，SiTU 的 scale 需在调用处恢复为 `[E,1,N]`。Qwen3.5 对照定位到 `AscendMLAAttentionSpec` 丢失 `page_size_padded` 及 `merge()` 元数据的问题，并涉及 `l2norm_fwd` 的版本参数差异和 KDA `num_spec` 边界。后续材料报告真实 5 层 INT4、TP8 的 model init、prefill/decode 已退出码 0，但 A5 原生 MXFP4 全权重验证仍明确延期，不能据此宣称所有精度或硬件场景已验证。
  title: K3 W4A8、混合缓存与端到端适配
- block_id: a81f95749851d56ecf4be77d0463a421da38e5719a76b0d1e37e24a190192176
  indexed: true
  sha256: 875f0c58902e2937d5b950adadc22fdd5de1be1b610c7ca490c83870f83f32a8
  source_range:
    end: 10869521
    part: 4
    start: 0
  summary: Qwen3.5 对照指出 K3 的 SiTU 不能复用 SwiGLU 融合路径，需要 GMM1→SiTU→重加权→GMM2，并将 `_restore_per_channel_scale_group_axis` 限定为独立的 W4A8-SiTU fallback；共享专家、routed-latent、权重映射、config 解析和 MXFP4 也有专属差异。后续报告称真实 5 层 INT4 TP8 已覆盖 SiTU MoE、无 RoPE gated MLA、KDA prefill/decode，并生成 token `[161095, 74143]`；官方语义是保留 raw 64D positional slice、禁用 cos/sin 旋转，当前 cos=1/sin=0 仅为接口占位，是否清理仍属待决事项。发布目标被改为 `vllm-ascend/vllm-ascend-kimi-k3`，目标分支材料中称为 `releases/v0.23.0`；拟提交包含 AscendC 算子及完整修改说明，但材料只记录了计划提交 draft PR，未证明 PR 已创建或已合并。
  title: SiTU、无 RoPE 语义与目标 PR
- block_id: 2d0cff659f19238daa589775cbc688504cab011964f20132f66254bd029c261b
  indexed: true
  sha256: b61326a8142cc441d00ed6a4c04ae694da6aac05199f69d6e10263e4e87d8dc3
  source_range:
    end: 17035785
    part: 0
    start: 10869521
  summary: 'K3 适配已报告完成 W4A8 SiTU fallback、Torch Meta 定向 SymInt 修复及 routed transform 的 Dynamo fullgraph 修复；FULL_DECODE_ONLY ACLGraph 在真实 5 层 INT4、TP8、CANN 9.0.1 环境完成 capture/replay，生成 token 为 `[161095, 74143, 85641]`。Eager 与 ACLGraph 的 token、chosen/cumulative logprob 及 top-20 逐项一致，但未比较完整 vocab logits，也未测试 piecewise graph；A5 原生 MXFP4 全量验证仍未完成。相关改动已报告推送至 draft PR #4，后续审查未发现 correctness 阻断项。'
  title: 'K3 ACLGraph、Torch Meta 与 PR #4 验证'
- block_id: 1809314164d3637f7b1a385d0b5d6d323e266623617bd4c03a1c57b5f45867b7
  indexed: true
  sha256: ecee608d61803b779c7a5c2cdd1403385a49c90632ff66d2d12d9d2be3ee67c8
  source_range:
    end: 17035785
    part: 1
    start: 10869521
  summary: K3 使用 `mamba_cache_mode=align`，实际 hybrid block size 为 768；真实顺序请求报告 `num_cached_tokens=[0,1536,1536]`，命中量对应两个 block，并验证了 KDA initial state 恢复与 cold 行清零。FULL_DECODE_ONLY 下 `B_hit/B_cold` 为 `1536/0`，tokens、chosen/cumulative logprob 和 top-20 均完全一致，ACLGraph replay 发生；cache-on eager 与 graph 也一致。独立 cache-off 使用 `none` 模式时长 prefill 输出不同，构成尚未解决的 `align`/`none` chunk-boundary parity 风险，不能宣称跨模式等价；相关 metadata 回归测试报告通过并已更新 PR。
  title: K3 hybrid prefix cache 命中与精度边界
- block_id: 1dd19f655e1d5a3239083c0480e8231baeca5e1073d8906e5f7aca1ef8bfa5be
  indexed: true
  sha256: 7f6f93e7a1506d9f25cfb8164088ef0bbaae5d3d3dcc5555747c3131ac447a87
  source_range:
    end: 17035785
    part: 2
    start: 10869521
  summary: '在 `releases/v0.23.0_ci_new` 加 PR #9 验证 PD 分离时，P 实例首个 1-token prefill 即报 `RuntimeError`/`call aclnnKdaGateCumsum failed`/`EZ1001`，根因定位为自定义算子安装包的 `binary_info_config.json` 与 `aic-ascend910_93-ops-info.json` 未登记 `KdaGateCumsum`、`ChunkKdaFwd`、`KdaLayoutSwap12`；未进入 tiling 或 shape 校验。旧完整包同规格通过且 `.o` SHA256 一致，说明更像打包/launcher 索引问题而非 TP16 shape 或 PR #9 的 Mooncake 逻辑；需清理旧 build 与 `_cann_ops_custom` 后完整重编安装，再做 P→D 的真实 unequal-TP 端到端验证。当前材料未报告完整 PD 链路已通过，且 154 单机无法同时独占 P TP16 与 D DP2×TP8。'
  title: PD 分离中 KdaGateCumsum 安装清单故障
- block_id: be280363cc49d90d910ff1917922c7977cfba750b7d5d9c78a3a0591e521e859
  indexed: true
  sha256: 97d9a00d78bae2298e236e66822d47d6e9fd871f217257d6fbc349a0fd985426
  source_range:
    end: 17035785
    part: 3
    start: 10869521
  summary: '原始 KdaGateCumsum 报错被定位为增量构建未更新 binary_info_config.json，干净重编后 Gate/Chunk/Layout 的 binary 清单为 3/7/3，相关直调、Torch Meta、AscendC 和 PR #9 的 109 个单测通过。P=156/TP16 与 D=154/DP2×TP8 的服务最终健康，首轮 P 启动曾因 kv_port=20001 与 ADXL 20xxx 随机端口窗冲突失败，重启后成功；两条真实 PD 请求及跨 chunk 请求均返回 200，输出与 D 直跑一致。完成计数的重复 DONE warning 被保留为潜在提前释放竞态，但按用户要求暂不修复；FlashComm1 适配的新任务随后要求用减层多模态权重分别测试纯文本和图片输入的开关精度。'
  title: KDA 清单修复与 PD 端到端验证
- block_id: c3bb68d258f6e01704dd8c42479e898028d787ef92d56037978d495753fa6886
  indexed: true
  sha256: 34d05f501b10dcfbb8941689c8224082c9210ce4009d130736b25a670a01b6d5
  source_range:
    end: 17035785
    part: 4
    start: 10869521
  summary: 当前新主线基于最新 v0.23.0_ci_new，针对 KDA 开启 FlashComm1 后的报错，计划使用 5 层真实 INT4/W4A8 减层权重定位并修复；已观察到 Kimi 的输入投影、首层完整 token 布局到分片布局切换，以及多路 block residual 可能与 Qwen3.5 路径存在差异，但这些仍是待现场堆栈确认的假设。可用环境为 156 上的 16 个空闲 NPU，权重结构为 MLA(0)→KDA(1,2,3)→MLA(4)，启动需声明 5 层并使用 ascend 量化。验收矩阵已扩展为纯语言 off/on 和同一图片 off/on 四组，要求固定采样、逐 token 对齐且日志无异常；目前尚未提供这些复现、修复或最终验证结果。
  title: FlashComm1 KDA 适配与四场景精度验收
- block_id: b369d038f91941ce66d3b23381e47505f42ae74cae4393d4b9c78fb29ed59e89
  indexed: true
  sha256: 96fe433dcde729cecacb9a373f78e5576f0cef6f690e60a47598d406b539d3cb
  source_range:
    end: 25431388
    part: 0
    start: 17035785
  summary: 只读审计认为 KDA 在 FlashComm1 下应在 hidden-dependent projection 前仅做一次 token all-gather，VL 首层若输入为全局 multimodal embeddings 则跳过 gather，后续层再 gather；首层 o_proj 后的输出、block_residual 和 prefix_sum 需统一切到 TP-local。审计记录了 AscendC 的 BSND、FP16/BF16、FP32 state 及 cu_seqlens 约束，并指出 off 基线曾因聚合算子包缺少 aclnnCausalConv1d 而失败；定向 UT 通过，但本机 pytest 因缺少 torch 未能执行。后续审查修正了首层判断：不能只用 is_vl_model，应区分完整 VL、language-model-only 和视觉禁用场景；local 行数也应遵循已 padding 后的 floor 语义。
  title: KDA 首层布局与 FlashComm1 审计
- block_id: a58f354a99d0298228eb591b61d48b8cca3339efb52cd89d4bcd2b2d0c42b7fd
  indexed: true
  sha256: 757eddded02b3a0238985dfa590e541ddd12de2d4ac47c7d6fa28b604f606b51
  source_range:
    end: 25431388
    part: 1
    start: 17035785
  summary: TP16/EP、5 层 W4A8、FULL_DECODE_ONLY 的 off 服务最终成功启动，text/image 各重复两次均 HTTP 200 且 32 个 output token IDs 稳定；此前还修复了 K3 processor 在 MultiModalProcessorOnlyCache 命中路径中丢弃 vision_chunks 的问题，加入透传 _call_hf_processor 后相关测试和 cache miss/hit 验证通过。FlashComm-on 初次 profile 暴露全局 2048 tokens 被错误 reshape 为本地 [16,6,128]，随后通过 learned-residual shape anchor 修复，50 个 Kimi 单测、profile 和 graph capture 通过。修复后服务仍只证明了稳定运行，off/on 输出尚未对齐；首 token 偏差进一步定位为 MLA gate 先在 TP-local hidden 上投影再 gather，之后改为先 gather hidden、再执行本地 g_proj，但后续 token 仍有分歧。
  title: K3 多模态启动与 FlashComm-on 首次失败
- block_id: c741df28c5999f1ac315920e30a6368991720d5c0a30ab22912ccdf4dfd36ddb
  indexed: true
  sha256: 3e50ce33f5fd5c1a295958bec2f6dfebfed7fc8ed44b37f7fd5bf487569fe88c
  source_range:
    end: 25431388
    part: 2
    start: 17035785
  summary: 对照 Qwen3.5-VL 和 Kimi K2 后，结论是它们复用框架的首层 global-input、attention 输出 reduce-scatter 和 residual chunk 机制；K3 仅因三维 learned-residual 需要 squeeze/复原并加动态 shape anchor，MLA gate 则必须先 all-gather hidden 再做 g_proj。MLA 顺序修复后首 token 已与 off 对齐，说明原先的 head/token 错配是首步偏差根因，但文本仍在第 7 个 token、图片在第 2 个 token 分叉，且需要用同一最新源码重跑 off 基线，因此尚未验收。对 MoE 的静态复核未发现确定的 FlashComm1 规约错误：A3 selector 走 MC2 或 ALLTOALL，当前证据不支持将剩余差异归因于 MoE。
  title: K3 与 Qwen/Kimi 布局对照及残余精度差异
- block_id: 6722a1fe4295aafeeead0c1ad574814da1a6e59fdabdc80335e9361db7b3ae57
  indexed: true
  sha256: e626e0e04e665705a883b7935c1ee83ce836ebbf56ec24ab2245f2242ac8cf79
  source_range:
    end: 25431388
    part: 3
    start: 17035785
  summary: 确认 FlashComm1 开关会改变 routed/shared expert 的通信与 GEMM 组合，但当前 A3+EP16 使用 MC2/ALLTOALL，未发现 K3 修改改变 fused MoE 语义；ALLGATHER 专家 partial 路径的 RMSNorm 规约顺序问题仍是未来场景的未验证风险。K3 首层应复用框架 token 轴 gather/chunk/pad 逻辑，DeepSeek V4 表明 `[T, block, H]` 等高维 residual 可透明处理；不过 eager off/on 已出现显著分叉，不能归因于 ACLGraph，首个差异位于 attention 后的 MLP 输入。
  title: K3 首层布局与 MoE 语义边界
- block_id: 778a3baf6c52d7d4adb7f64090873505f6bbf1ba5c17b20d60a0cb826ab613b0
  indexed: true
  sha256: 83aba6e0346f19712ceae1651db07b321a97e382f873f8ec184d3cc8cf83b6a3
  source_range:
    end: 25431388
    part: 4
    start: 17035785
  summary: 两台 TP16、相同 eager 配置的 ColumnParallelLinear 输入探针显示，文本和图片请求的视觉输入及 layer 0 KDA 投影输入均逐元素一致，首个差异出现在 attention 后 `mlp.gate_up_proj` 输入，最大绝对差约为 `7.32e-4` 和 `9.77e-4`。这些 BF16/W4A8 量级误差在 recurrent KDA 中被放大并导致 token 分叉，因此仍需区分 padding 后 GEMM 的 M 变化与 all-reduce→reduce-scatter 数值差异，不能直接判定为语义错误或精度通过。
  title: Eager 探针定位到 attention 后差异
- block_id: 2221a41152721ecd5eef5073341b3cc71deb51c89230053f69ea98b5921786eb
  indexed: true
  sha256: 510568e72a68b1409554868613f9cc84411e936afb71626eee8f68cae3547352
  source_range:
    end: 25431388
    part: 5
    start: 17035785
  summary: 四机真实 W4A8、FlashComm1 开启、prefix cache 关闭、FULL_DECODE_ONLY 的 352 分片服务已完成编译、capture 并健康运行，HTTP 200，前 9/10 GPQA 题已报告全对；第 10 题因长思维链后来被中止，不能宣称完整 10 题通过。随后准备切换到 VL+prefix cache+FlashComm，并使用 smooth-8cards 权重；已有两道明确视觉语义题和 launcher 配置，但该新轮次结果尚未完成。
  title: 完整权重 FlashComm 服务完成启动与 GPQA 进展
- block_id: db1c9db61e0b7d10d46499d4de71101bb17ed3fe19635603731aec3bfc906cd1
  indexed: true
  sha256: e2693c776197201cd666656c3e85577eefe5419dd02f400e44ca2b134ac7b5cb
  source_range:
    end: 25431388
    part: 6
    start: 17035785
  summary: '静态检查、AST、Ruff、格式和签名提交均已完成，提交为 `3337f920 feat(model): support Kimi K3 FlashComm1`；改动范围未涉及 fused MoE、SiTU、INT4 packing、scale 恢复或 W4A8 loader。新一轮已切换到 VL、prefix cache、FlashComm1、FULL_DECODE_ONLY 和 smooth-8cards 权重，实际目录被识别为 353 个 safetensor 文件并开始加载；用户要求先跑两道多模态请求再跑完整 198 题 GPQA，但当前材料只到权重加载阶段，尚无端到端验证结果。'
  title: 代码已提交，新权重 VL 验证未完成
- block_id: 7240c944b63e6077b7d3e19d3e0fe4d04562ba28829ba140ea54906a6391ec90
  indexed: true
  sha256: b6ec1867548357eb296ae0a6e031245feb350e4a57ead01a19f3a46b2e59d9c6
  source_range:
    end: 27407674
    part: 0
    start: 25431388
  summary: 报告称 smooth-8cards 的 VL、Prefix Cache、FlashComm、FULL_DECODE_ONLY 配置完成 353/353 权重加载、compile、KV profile 和 2/2 capture，并通过 health=200；两道多模态题也被报告为通过。随后完整 GPQA 报告为 198 题正确 175，准确率 88.38%，无请求错误或缺失，四机日志异常为 0；但与历史结果的差异在无固定 seed、同时存在多项配置变化的条件下不能归因于单一特性。TextVQA-lite 仅完成了 512 条单图数据和 AISBench 方案准备，尚未执行；服务后来按用户要求停止。
  title: Smooth VL 服务启动、GPQA 完成与后续停服
- block_id: 136690cc1c81b7675a3db39bfa93e20b79a66123d019f1397eed8459f749140e
  indexed: true
  sha256: 9eebcb9d6429fdee0b72a1b024929ea8d8670bd147f1d543ab3d30be9c338b8f
  source_range:
    end: 27407674
    part: 1
    start: 25431388
  summary: 四机新轮次使用相同 smooth 权重和 VL 配置，显式关闭 FlashComm1、开启 prefix cache，并使用 FULL_DECODE_ONLY；报告称 353/353 权重、compile、7/7 ACLGraph capture 完成，health=200 且错误扫描为 0。FlashComm-off GPQA 随后启动，初期进度从 13/198 到 93/198，阶段准确率在 89.81%–94.00% 间波动，请求错误和缺失均为 0。对 FlashComm-on 的只读逐题比较截至 163 题显示预测一致 153/163、双方均正确 149/163，但因 temperature=1 且无固定 seed，不能据此证明开关造成或未造成精度差异。
  title: FlashComm-off 服务完成启动并开始 GPQA
- block_id: 0ebe23025402afe399223c314be35302f2af74f44281bbb173ab78fceec17e59
  indexed: true
  sha256: e217a441441b46a13be7afa8dc298291eec64732779752f64492977d40de3f6e
  source_range:
    end: 27407674
    part: 2
    start: 25431388
  summary: FlashComm-off GPQA 继续运行至 197/198，累计正确 174，阶段准确率 88.32%，请求错误、缺失和四机异常仍报告为 0，health=200。最后 1 题仍在正常 decode，Waiting=0；“若答对则为 175/198、与上一轮相同”只是推测，尚未以最终 summary 验证。尾部输出较长，先前按最大输出上限估计最迟约 20 分钟完成，但本批材料未提供最终完成结果。
  title: FlashComm-off GPQA 接近完成，最后长尾仍未闭环
- block_id: d04f7b74dac94fd7f2970dfa4ba739dab715074c876cbd2f5f9cb785456c514c
  indexed: true
  sha256: 4100fde7a7dd3d51f92dd48d0ae16a883fdcbc4d070b8f4e6a872bfab64f3450
  source_range:
    end: 28739258
    part: 0
    start: 27407674
  summary: 此前 smooth 权重的 GPQA FlashComm-off/on 均为 175/198（88.38%），错误和缺失均为 0；配对结果 9:9，McNemar 双侧 p=1.0，但由于 temperature=1.0 且无固定 seed，不能证明逐 token 或数值严格等价。随后按指定权重开启 prefix cache 的 multicards 四机服务曾因 153 的 `kimi-k3-pool` 和 155 的 `zrc_kimi` 外部显存占用两次失败，授权清理后 retry2 完成 64 rank 初始化、352/352 权重加载、torch.compile 和 ACLGraph 7/7，health=200。新的 198 题 GPQA 运行至 70/198、68 题正确时仍无错误或缺失，后续结果见下一块；服务配置与阶段性成功均为当时报告，不能替代完整最终统计。
  title: GPQA A/B 完成与 multicards 新轮次启动
- block_id: a7e28269a8455b809e80ca784d0dd180d4fba42828021cf4afd999a15cac3904
  indexed: true
  sha256: 4197da5b046f9193ee2ad37b2d977258077fe5a590c333444bbc126d39e7420c
  source_range:
    end: 28739258
    part: 1
    start: 27407674
  summary: multicards + prefix cache 的 GPQA 继续生成，最高到 197/198、180 题正确（阶段准确率 91.37%），已落盘请求的错误和缺失均报告为 0。最后一题为 GPQA `id=71`，持续约 9 token/s 并占用约 5.6% KV，尚未出现终止标记；此前关于其可能答对或答错只是推测。用户随后要求直接杀掉服务，四机服务与 runner 均停止，197 条结果和日志保留，但最后一题未完成，因此没有 `summary.json`，也不存在可确认的全集最终准确率。
  title: multicards GPQA 长尾中止，未形成最终统计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8e90b0aa11918b9018cf60750c464c4f7cd40ab770187470a197fe5238b47f5f
  kind: experience
  material_digest: f2b7aaa5f06550adfb9e71b2ecc361b50de9af6509e72472a987b57c6bfabacb
  revision: 48a484a482053abc29e56f91381f4b422a21bdddc57e439cb31692fca624ddd4
  schema: mindie-entry/3
  summary: 历史 smooth 权重 FlashComm-off/on 均报告 175/198（88.38%），配对差异 9:9、McNemar p=1.0，但无固定 seed，不能据此确认严格等价或无精度影响。新的 multicards + prefix cache 轮次在处理外部显存占用后曾完成 64 rank、352/352 权重、ACLGraph 7/7 和 health=200；GPQA 最终停在 197/198、180 正确，最后一题未完成且无最终 summary，因此 91.37% 只是阶段统计，不是全集结果。
  title: Kimi K3 GPQA：历史 A/B 已完成，新一轮 multicards 因长尾中止
navigation: 历史 smooth 权重 FlashComm-off/on 均报告 175/198（88.38%），配对差异 9:9、McNemar p=1.0，但无固定 seed，不能据此确认严格等价或无精度影响。新的 multicards + prefix cache 轮次在处理外部显存占用后曾完成 64 rank、352/352 权重、ACLGraph 7/7 和 health=200；GPQA 最终停在 197/198、180 正确，最后一题未完成且无最终 summary，因此 91.37% 只是阶段统计，不是全集结果。
schema: mindie-material-task/1
status: complete
task_id: 8e90b0aa11918b9018cf60750c464c4f7cd40ab770187470a197fe5238b47f5f
---

# Kimi K3 GPQA：历史 A/B 已完成，新一轮 multicards 因长尾中止

历史 smooth 权重 FlashComm-off/on 均报告 175/198（88.38%），配对差异 9:9、McNemar p=1.0，但无固定 seed，不能据此确认严格等价或无精度影响。新的 multicards + prefix cache 轮次在处理外部显存占用后曾完成 64 rank、352/352 权重、ACLGraph 7/7 和 health=200；GPQA 最终停在 197/198、180 正确，最后一题未完成且无最终 summary，因此 91.37% 只是阶段统计，不是全集结果。

## Materials

- [Kimi K3 适配目标与早期验证](blocks/ec058b3f78a436f89b66031266f385c9f30e44bd457ceede05da73085f45a626.md): 目标是在 vLLM-Ascend 0.23.0 基础上适配 Kimi K3，涉及 SiTU 激活、无 RoPE MLA 与 gated o_proj、KDA 算子及模型组网，并参考 PR141；PR142 被明确视为不可靠的 fake adapter。过程中确认了 CANN 9.0.1（用户口径称 9.1）的 A3 容器、4 层 dummy 验证范围，以及 KDA prefill/decode 的 AscendC/Triton 分工。已有报告称 SiTU/MoE、KDA 测试和部分 NPU smoke 通过，但真实量化权重端到端执行当时仍未覆盖。
- [A3 权重加载与算子环境诊断](blocks/576392e2184b93e6fe6adadd9fabcf0731c4a6435239ed8d141c325efa4ca2d2.md): 5 层 ModelSlim 权重被识别为 routed W4A8_DYNAMIC、shared/latent W8A8_DYNAMIC，随后补充了 packed mapping、padded A_log 裁剪分片、无 RoPE 直通及 K3 KDA 层类型修正。BF16 首 forward 的 `aclnnMoeInitRoutingCustom ... not in libopapi.so` 被进一步定位为镜像中 vLLM-Ascend 扩展/ vendor ACLNN 包不匹配，而非源码删除；源码注册、A3 构建清单和 Torch binding 仍存在。独立重编译后的最小算子包已在 A3 完成有限 NPU 调用验证，但完整 vLLM-Ascend 重编译和真实模型首 forward 当时仍在进行。
- [整合审计、测试结果与 PR 草案](blocks/7d1537e50b9885d826adf268636ae59e8b8269bae35d8c5545c9c21b2cceb2ce.md): 审计结论继续确认 `moe_init_routing_custom` 未被废弃，旧失败来自预装 vendor 包缺失或版本不一致；完整扩展已报告编译成功，并包含 MoE 与 KDA schema。真实 5 层 ModelSlim 的 19/19 分片和 TP8 权重加载已报告成功，累计回归为 `235 passed, 9 skipped`，但首个 W4A8 MoE forward 在旧环境仍因缺少该 ACLNN 符号失败；这一结果不能视为完整端到端成功。当前 PR 草案包含 PR141 等价的 KDA 提交、Kimi K3 模型/MLA/MoE/量化改动及测试，并明确保留 A3 验证边界、未宣称五层 INT4 最终 rerun 已通过。
- [K3 W4A8、混合缓存与端到端适配](blocks/74c0eeb3a9d9393d320488479c1d2c580ad3a38ad54e551e37dedf1e4ece35cf.md): 记录了 K3 在 Ascend/vLLM 0.23 环境中的验证链路：旧 vendor 包导致 `MoeInitRoutingCustom` 缺失，完整重编译后算子可实际调用；W4 权重是每字节两个 int4，转 int32 仅作 ABI view，SiTU 的 scale 需在调用处恢复为 `[E,1,N]`。Qwen3.5 对照定位到 `AscendMLAAttentionSpec` 丢失 `page_size_padded` 及 `merge()` 元数据的问题，并涉及 `l2norm_fwd` 的版本参数差异和 KDA `num_spec` 边界。后续材料报告真实 5 层 INT4、TP8 的 model init、prefill/decode 已退出码 0，但 A5 原生 MXFP4 全权重验证仍明确延期，不能据此宣称所有精度或硬件场景已验证。
- [SiTU、无 RoPE 语义与目标 PR](blocks/a81f95749851d56ecf4be77d0463a421da38e5719a76b0d1e37e24a190192176.md): Qwen3.5 对照指出 K3 的 SiTU 不能复用 SwiGLU 融合路径，需要 GMM1→SiTU→重加权→GMM2，并将 `_restore_per_channel_scale_group_axis` 限定为独立的 W4A8-SiTU fallback；共享专家、routed-latent、权重映射、config 解析和 MXFP4 也有专属差异。后续报告称真实 5 层 INT4 TP8 已覆盖 SiTU MoE、无 RoPE gated MLA、KDA prefill/decode，并生成 token `[161095, 74143]`；官方语义是保留 raw 64D positional slice、禁用 cos/sin 旋转，当前 cos=1/sin=0 仅为接口占位，是否清理仍属待决事项。发布目标被改为 `vllm-ascend/vllm-ascend-kimi-k3`，目标分支材料中称为 `releases/v0.23.0`；拟提交包含 AscendC 算子及完整修改说明，但材料只记录了计划提交 draft PR，未证明 PR 已创建或已合并。
- [K3 ACLGraph、Torch Meta 与 PR #4 验证](blocks/2d0cff659f19238daa589775cbc688504cab011964f20132f66254bd029c261b.md): K3 适配已报告完成 W4A8 SiTU fallback、Torch Meta 定向 SymInt 修复及 routed transform 的 Dynamo fullgraph 修复；FULL_DECODE_ONLY ACLGraph 在真实 5 层 INT4、TP8、CANN 9.0.1 环境完成 capture/replay，生成 token 为 `[161095, 74143, 85641]`。Eager 与 ACLGraph 的 token、chosen/cumulative logprob 及 top-20 逐项一致，但未比较完整 vocab logits，也未测试 piecewise graph；A5 原生 MXFP4 全量验证仍未完成。相关改动已报告推送至 draft PR #4，后续审查未发现 correctness 阻断项。
- [K3 hybrid prefix cache 命中与精度边界](blocks/1809314164d3637f7b1a385d0b5d6d323e266623617bd4c03a1c57b5f45867b7.md): K3 使用 `mamba_cache_mode=align`，实际 hybrid block size 为 768；真实顺序请求报告 `num_cached_tokens=[0,1536,1536]`，命中量对应两个 block，并验证了 KDA initial state 恢复与 cold 行清零。FULL_DECODE_ONLY 下 `B_hit/B_cold` 为 `1536/0`，tokens、chosen/cumulative logprob 和 top-20 均完全一致，ACLGraph replay 发生；cache-on eager 与 graph 也一致。独立 cache-off 使用 `none` 模式时长 prefill 输出不同，构成尚未解决的 `align`/`none` chunk-boundary parity 风险，不能宣称跨模式等价；相关 metadata 回归测试报告通过并已更新 PR。
- [PD 分离中 KdaGateCumsum 安装清单故障](blocks/1dd19f655e1d5a3239083c0480e8231baeca5e1073d8906e5f7aca1ef8bfa5be.md): 在 `releases/v0.23.0_ci_new` 加 PR #9 验证 PD 分离时，P 实例首个 1-token prefill 即报 `RuntimeError`/`call aclnnKdaGateCumsum failed`/`EZ1001`，根因定位为自定义算子安装包的 `binary_info_config.json` 与 `aic-ascend910_93-ops-info.json` 未登记 `KdaGateCumsum`、`ChunkKdaFwd`、`KdaLayoutSwap12`；未进入 tiling 或 shape 校验。旧完整包同规格通过且 `.o` SHA256 一致，说明更像打包/launcher 索引问题而非 TP16 shape 或 PR #9 的 Mooncake 逻辑；需清理旧 build 与 `_cann_ops_custom` 后完整重编安装，再做 P→D 的真实 unequal-TP 端到端验证。当前材料未报告完整 PD 链路已通过，且 154 单机无法同时独占 P TP16 与 D DP2×TP8。
- [KDA 清单修复与 PD 端到端验证](blocks/be280363cc49d90d910ff1917922c7977cfba750b7d5d9c78a3a0591e521e859.md): 原始 KdaGateCumsum 报错被定位为增量构建未更新 binary_info_config.json，干净重编后 Gate/Chunk/Layout 的 binary 清单为 3/7/3，相关直调、Torch Meta、AscendC 和 PR #9 的 109 个单测通过。P=156/TP16 与 D=154/DP2×TP8 的服务最终健康，首轮 P 启动曾因 kv_port=20001 与 ADXL 20xxx 随机端口窗冲突失败，重启后成功；两条真实 PD 请求及跨 chunk 请求均返回 200，输出与 D 直跑一致。完成计数的重复 DONE warning 被保留为潜在提前释放竞态，但按用户要求暂不修复；FlashComm1 适配的新任务随后要求用减层多模态权重分别测试纯文本和图片输入的开关精度。
- [FlashComm1 KDA 适配与四场景精度验收](blocks/c3bb68d258f6e01704dd8c42479e898028d787ef92d56037978d495753fa6886.md): 当前新主线基于最新 v0.23.0_ci_new，针对 KDA 开启 FlashComm1 后的报错，计划使用 5 层真实 INT4/W4A8 减层权重定位并修复；已观察到 Kimi 的输入投影、首层完整 token 布局到分片布局切换，以及多路 block residual 可能与 Qwen3.5 路径存在差异，但这些仍是待现场堆栈确认的假设。可用环境为 156 上的 16 个空闲 NPU，权重结构为 MLA(0)→KDA(1,2,3)→MLA(4)，启动需声明 5 层并使用 ascend 量化。验收矩阵已扩展为纯语言 off/on 和同一图片 off/on 四组，要求固定采样、逐 token 对齐且日志无异常；目前尚未提供这些复现、修复或最终验证结果。
- [KDA 首层布局与 FlashComm1 审计](blocks/b369d038f91941ce66d3b23381e47505f42ae74cae4393d4b9c78fb29ed59e89.md): 只读审计认为 KDA 在 FlashComm1 下应在 hidden-dependent projection 前仅做一次 token all-gather，VL 首层若输入为全局 multimodal embeddings 则跳过 gather，后续层再 gather；首层 o_proj 后的输出、block_residual 和 prefix_sum 需统一切到 TP-local。审计记录了 AscendC 的 BSND、FP16/BF16、FP32 state 及 cu_seqlens 约束，并指出 off 基线曾因聚合算子包缺少 aclnnCausalConv1d 而失败；定向 UT 通过，但本机 pytest 因缺少 torch 未能执行。后续审查修正了首层判断：不能只用 is_vl_model，应区分完整 VL、language-model-only 和视觉禁用场景；local 行数也应遵循已 padding 后的 floor 语义。
- [K3 多模态启动与 FlashComm-on 首次失败](blocks/a58f354a99d0298228eb591b61d48b8cca3339efb52cd89d4bcd2b2d0c42b7fd.md): TP16/EP、5 层 W4A8、FULL_DECODE_ONLY 的 off 服务最终成功启动，text/image 各重复两次均 HTTP 200 且 32 个 output token IDs 稳定；此前还修复了 K3 processor 在 MultiModalProcessorOnlyCache 命中路径中丢弃 vision_chunks 的问题，加入透传 _call_hf_processor 后相关测试和 cache miss/hit 验证通过。FlashComm-on 初次 profile 暴露全局 2048 tokens 被错误 reshape 为本地 [16,6,128]，随后通过 learned-residual shape anchor 修复，50 个 Kimi 单测、profile 和 graph capture 通过。修复后服务仍只证明了稳定运行，off/on 输出尚未对齐；首 token 偏差进一步定位为 MLA gate 先在 TP-local hidden 上投影再 gather，之后改为先 gather hidden、再执行本地 g_proj，但后续 token 仍有分歧。
- [K3 与 Qwen/Kimi 布局对照及残余精度差异](blocks/c741df28c5999f1ac315920e30a6368991720d5c0a30ab22912ccdf4dfd36ddb.md): 对照 Qwen3.5-VL 和 Kimi K2 后，结论是它们复用框架的首层 global-input、attention 输出 reduce-scatter 和 residual chunk 机制；K3 仅因三维 learned-residual 需要 squeeze/复原并加动态 shape anchor，MLA gate 则必须先 all-gather hidden 再做 g_proj。MLA 顺序修复后首 token 已与 off 对齐，说明原先的 head/token 错配是首步偏差根因，但文本仍在第 7 个 token、图片在第 2 个 token 分叉，且需要用同一最新源码重跑 off 基线，因此尚未验收。对 MoE 的静态复核未发现确定的 FlashComm1 规约错误：A3 selector 走 MC2 或 ALLTOALL，当前证据不支持将剩余差异归因于 MoE。
- [K3 首层布局与 MoE 语义边界](blocks/6722a1fe4295aafeeead0c1ad574814da1a6e59fdabdc80335e9361db7b3ae57.md): 确认 FlashComm1 开关会改变 routed/shared expert 的通信与 GEMM 组合，但当前 A3+EP16 使用 MC2/ALLTOALL，未发现 K3 修改改变 fused MoE 语义；ALLGATHER 专家 partial 路径的 RMSNorm 规约顺序问题仍是未来场景的未验证风险。K3 首层应复用框架 token 轴 gather/chunk/pad 逻辑，DeepSeek V4 表明 `[T, block, H]` 等高维 residual 可透明处理；不过 eager off/on 已出现显著分叉，不能归因于 ACLGraph，首个差异位于 attention 后的 MLP 输入。
- [Eager 探针定位到 attention 后差异](blocks/778a3baf6c52d7d4adb7f64090873505f6bbf1ba5c17b20d60a0cb826ab613b0.md): 两台 TP16、相同 eager 配置的 ColumnParallelLinear 输入探针显示，文本和图片请求的视觉输入及 layer 0 KDA 投影输入均逐元素一致，首个差异出现在 attention 后 `mlp.gate_up_proj` 输入，最大绝对差约为 `7.32e-4` 和 `9.77e-4`。这些 BF16/W4A8 量级误差在 recurrent KDA 中被放大并导致 token 分叉，因此仍需区分 padding 后 GEMM 的 M 变化与 all-reduce→reduce-scatter 数值差异，不能直接判定为语义错误或精度通过。
- [完整权重 FlashComm 服务完成启动与 GPQA 进展](blocks/2221a41152721ecd5eef5073341b3cc71deb51c89230053f69ea98b5921786eb.md): 四机真实 W4A8、FlashComm1 开启、prefix cache 关闭、FULL_DECODE_ONLY 的 352 分片服务已完成编译、capture 并健康运行，HTTP 200，前 9/10 GPQA 题已报告全对；第 10 题因长思维链后来被中止，不能宣称完整 10 题通过。随后准备切换到 VL+prefix cache+FlashComm，并使用 smooth-8cards 权重；已有两道明确视觉语义题和 launcher 配置，但该新轮次结果尚未完成。
- [代码已提交，新权重 VL 验证未完成](blocks/db1c9db61e0b7d10d46499d4de71101bb17ed3fe19635603731aec3bfc906cd1.md): 静态检查、AST、Ruff、格式和签名提交均已完成，提交为 `3337f920 feat(model): support Kimi K3 FlashComm1`；改动范围未涉及 fused MoE、SiTU、INT4 packing、scale 恢复或 W4A8 loader。新一轮已切换到 VL、prefix cache、FlashComm1、FULL_DECODE_ONLY 和 smooth-8cards 权重，实际目录被识别为 353 个 safetensor 文件并开始加载；用户要求先跑两道多模态请求再跑完整 198 题 GPQA，但当前材料只到权重加载阶段，尚无端到端验证结果。
- [Smooth VL 服务启动、GPQA 完成与后续停服](blocks/7240c944b63e6077b7d3e19d3e0fe4d04562ba28829ba140ea54906a6391ec90.md): 报告称 smooth-8cards 的 VL、Prefix Cache、FlashComm、FULL_DECODE_ONLY 配置完成 353/353 权重加载、compile、KV profile 和 2/2 capture，并通过 health=200；两道多模态题也被报告为通过。随后完整 GPQA 报告为 198 题正确 175，准确率 88.38%，无请求错误或缺失，四机日志异常为 0；但与历史结果的差异在无固定 seed、同时存在多项配置变化的条件下不能归因于单一特性。TextVQA-lite 仅完成了 512 条单图数据和 AISBench 方案准备，尚未执行；服务后来按用户要求停止。
- [FlashComm-off 服务完成启动并开始 GPQA](blocks/136690cc1c81b7675a3db39bfa93e20b79a66123d019f1397eed8459f749140e.md): 四机新轮次使用相同 smooth 权重和 VL 配置，显式关闭 FlashComm1、开启 prefix cache，并使用 FULL_DECODE_ONLY；报告称 353/353 权重、compile、7/7 ACLGraph capture 完成，health=200 且错误扫描为 0。FlashComm-off GPQA 随后启动，初期进度从 13/198 到 93/198，阶段准确率在 89.81%–94.00% 间波动，请求错误和缺失均为 0。对 FlashComm-on 的只读逐题比较截至 163 题显示预测一致 153/163、双方均正确 149/163，但因 temperature=1 且无固定 seed，不能据此证明开关造成或未造成精度差异。
- [FlashComm-off GPQA 接近完成，最后长尾仍未闭环](blocks/0ebe23025402afe399223c314be35302f2af74f44281bbb173ab78fceec17e59.md): FlashComm-off GPQA 继续运行至 197/198，累计正确 174，阶段准确率 88.32%，请求错误、缺失和四机异常仍报告为 0，health=200。最后 1 题仍在正常 decode，Waiting=0；“若答对则为 175/198、与上一轮相同”只是推测，尚未以最终 summary 验证。尾部输出较长，先前按最大输出上限估计最迟约 20 分钟完成，但本批材料未提供最终完成结果。
- [GPQA A/B 完成与 multicards 新轮次启动](blocks/d04f7b74dac94fd7f2970dfa4ba739dab715074c876cbd2f5f9cb785456c514c.md): 此前 smooth 权重的 GPQA FlashComm-off/on 均为 175/198（88.38%），错误和缺失均为 0；配对结果 9:9，McNemar 双侧 p=1.0，但由于 temperature=1.0 且无固定 seed，不能证明逐 token 或数值严格等价。随后按指定权重开启 prefix cache 的 multicards 四机服务曾因 153 的 `kimi-k3-pool` 和 155 的 `zrc_kimi` 外部显存占用两次失败，授权清理后 retry2 完成 64 rank 初始化、352/352 权重加载、torch.compile 和 ACLGraph 7/7，health=200。新的 198 题 GPQA 运行至 70/198、68 题正确时仍无错误或缺失，后续结果见下一块；服务配置与阶段性成功均为当时报告，不能替代完整最终统计。
- [multicards GPQA 长尾中止，未形成最终统计](blocks/a7e28269a8455b809e80ca784d0dd180d4fba42828021cf4afd999a15cac3904.md): multicards + prefix cache 的 GPQA 继续生成，最高到 197/198、180 题正确（阶段准确率 91.37%），已落盘请求的错误和缺失均报告为 0。最后一题为 GPQA `id=71`，持续约 9 token/s 并占用约 5.6% KV，尚未出现终止标记；此前关于其可能答对或答错只是推测。用户随后要求直接杀掉服务，四机服务与 runner 均停止，197 条结果和日志保留，但最后一题未完成，因此没有 `summary.json`，也不存在可确认的全集最终准确率。

---
blocks:
- block_id: 3f1a462fe8a0a75a229afe1ad11096c26435feb4ac9d950e6bbe2e41336b988b
  indexed: true
  sha256: 18a6ae1582e2f2f487d7f0a8766fcfd443482d55535ccadd11b7ee3df29099c6
  source_range:
    end: 3362767
    part: 0
    start: 0
  summary: 该材料称，cann-recipes-infer 的 Compressor 分为三层：DeepSeek 官方 V4-Flash 的算法与权重、仓库内的模型编排/缓存/PyTorch 绑定，以及 ops-transformer 提供的 aclnnCompressor 与 Ascend kernel；但这些是此前核对结论，仍应视为易错参考索引。材料记录了 C1、C4、C128 的层分布、压缩比例、SWA/稀疏索引/dense history 配套，以及 ori_kv、cmp_kv、cmp_sparse_indices 等张量契约。还保留了不确定或易混淆事项：SGLang offload 路径复用同一 custom op、CANN 9.0 与 8.5 存在 single/split ABI 差异，PyPTO compressor 被描述为参考实现而非默认生产链；相关版本、路径和来源均未独立验证。
  title: DeepSeek V4 Flash Compressor 来源与配套链路
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c246c4f947afae395595c7ad856cd8071c7abf8906a35910f10fe41fd226de7d
  kind: experience
  material_digest: 21ce5d92d4319b946f5fbb99571fb41b2a2d9566ead69bf21b4277885a5fe185
  revision: 54675d882ce439e77411e72fb7352aafa6637e05d5be3ba26aab4b7653e93809
  schema: mindie-entry/3
  summary: 当前材料聚焦于 cann-recipes-infer 中 DeepSeek V4 Flash Compressor 的来源、模型权重与 C1/C4/C128 配置，以及 torch.ops.custom.compressor → aclnnCompressor → ops-transformer kernel 的接入链路。后续检索应区分官方算法/权重、仓库编排与底层 Ascend 算子，并保留 CANN 版本 ABI、SGLang 补丁路径、环境脚本和 PyPTO 默认未启用等未完全验证或可能修正的细节；原始材料仍应独立搜索。
  title: 任务导航：DeepSeek V4 Flash Compressor 参考索引
navigation: 当前材料聚焦于 cann-recipes-infer 中 DeepSeek V4 Flash Compressor 的来源、模型权重与 C1/C4/C128 配置，以及 torch.ops.custom.compressor → aclnnCompressor → ops-transformer kernel 的接入链路。后续检索应区分官方算法/权重、仓库编排与底层 Ascend 算子，并保留 CANN 版本 ABI、SGLang 补丁路径、环境脚本和 PyPTO 默认未启用等未完全验证或可能修正的细节；原始材料仍应独立搜索。
schema: mindie-material-task/1
status: complete
task_id: c246c4f947afae395595c7ad856cd8071c7abf8906a35910f10fe41fd226de7d
---

# 任务导航：DeepSeek V4 Flash Compressor 参考索引

当前材料聚焦于 cann-recipes-infer 中 DeepSeek V4 Flash Compressor 的来源、模型权重与 C1/C4/C128 配置，以及 torch.ops.custom.compressor → aclnnCompressor → ops-transformer kernel 的接入链路。后续检索应区分官方算法/权重、仓库编排与底层 Ascend 算子，并保留 CANN 版本 ABI、SGLang 补丁路径、环境脚本和 PyPTO 默认未启用等未完全验证或可能修正的细节；原始材料仍应独立搜索。

## Materials

- [DeepSeek V4 Flash Compressor 来源与配套链路](blocks/3f1a462fe8a0a75a229afe1ad11096c26435feb4ac9d950e6bbe2e41336b988b.md): 该材料称，cann-recipes-infer 的 Compressor 分为三层：DeepSeek 官方 V4-Flash 的算法与权重、仓库内的模型编排/缓存/PyTorch 绑定，以及 ops-transformer 提供的 aclnnCompressor 与 Ascend kernel；但这些是此前核对结论，仍应视为易错参考索引。材料记录了 C1、C4、C128 的层分布、压缩比例、SWA/稀疏索引/dense history 配套，以及 ori_kv、cmp_kv、cmp_sparse_indices 等张量契约。还保留了不确定或易混淆事项：SGLang offload 路径复用同一 custom op、CANN 9.0 与 8.5 存在 single/split ABI 差异，PyPTO compressor 被描述为参考实现而非默认生产链；相关版本、路径和来源均未独立验证。

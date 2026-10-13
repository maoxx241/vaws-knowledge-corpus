---
blocks:
- block_id: c288c3300ebfdf717a5801ccff3384c7f686d937caf19bdcb9e70e274a224891
  indexed: true
  sha256: 0527dd3ac2fcd6179ee805d611d33530133c963f8d8803dec2d10686173499d7
  source_range:
    end: 3312138
    part: 0
    start: 0
  summary: 材料称，DeepSeek-V4-Flash 的 QuantLightningIndexer 由官方 checkpoint 中的 Indexer/Compressor 权重、沿用 DeepSeek-V3.2 的 LI 设计，以及 `cann/ops-transformer` 提供的核心 kernel 组合而成；`cann-recipes-infer` 负责模型接线、Cache、PyTorch binding 和 GE converter。文中列出 C4 层、`topk=512`、`compress_ratio=4`、Q/K/W 与 scale 的量化路径，以及 QLI、Indexer Compressor、Indexer Cache、Metadata 和 SparseAttnSharedKV 的接口关系。材料明确这是源码和版本链路核对，未进行 NPU 上板执行；版本 pin、镜像 ABI 组合和若干运行时错误（如 `aclnnQuantLightningIndexer not in libopapi.so`、算子不存在）属于配置依据或可能失败表现，不应视为已验证成功。此前关于 TensorMove/DSA 的推测未被沿用，且主 recipe 与单卡 SGLang 路径的版本绑定存在差异。
  title: DeepSeek-V4-Flash QuantLightningIndexer 来源与配套
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: edc44803e0feab76d8fbbb587a1b1ac7e8e1f444f3dd21e8cff3f89e177324cf
  kind: experience
  material_digest: 62e3dc41e6622142c4f272868d76379a0da36e773a3d1b6e6cd6d9cbb8ac35a0
  revision: 25eb88e8c08e4338626f957da9e955aa2502a984c66365f8e6d9c88f78aad662
  schema: mindie-entry/3
  summary: 当前索引聚焦 DeepSeek-V4-Flash 中 QuantLightningIndexer 的权重来源、`ops-transformer` kernel 血缘、`cann-recipes-infer` 接线方式，以及 A3/950 的 INT8、MXFP8、HiF8 配套。需保留这些结论的非验证性质：材料只完成源码与版本链路核对，未完成 NPU 实机执行；镜像/checkout ABI 不匹配可能导致 `aclnnQuantLightningIndexer` 或 `torch.ops.custom.npu_quant_lightning_indexer` 缺失。
  title: QuantLightningIndexer 来源、量化路径与运行时配套
navigation: 当前索引聚焦 DeepSeek-V4-Flash 中 QuantLightningIndexer 的权重来源、`ops-transformer` kernel 血缘、`cann-recipes-infer` 接线方式，以及 A3/950 的 INT8、MXFP8、HiF8 配套。需保留这些结论的非验证性质：材料只完成源码与版本链路核对，未完成 NPU 实机执行；镜像/checkout ABI 不匹配可能导致 `aclnnQuantLightningIndexer` 或 `torch.ops.custom.npu_quant_lightning_indexer` 缺失。
schema: mindie-material-task/1
status: complete
task_id: edc44803e0feab76d8fbbb587a1b1ac7e8e1f444f3dd21e8cff3f89e177324cf
---

# QuantLightningIndexer 来源、量化路径与运行时配套

当前索引聚焦 DeepSeek-V4-Flash 中 QuantLightningIndexer 的权重来源、`ops-transformer` kernel 血缘、`cann-recipes-infer` 接线方式，以及 A3/950 的 INT8、MXFP8、HiF8 配套。需保留这些结论的非验证性质：材料只完成源码与版本链路核对，未完成 NPU 实机执行；镜像/checkout ABI 不匹配可能导致 `aclnnQuantLightningIndexer` 或 `torch.ops.custom.npu_quant_lightning_indexer` 缺失。

## Materials

- [DeepSeek-V4-Flash QuantLightningIndexer 来源与配套](blocks/c288c3300ebfdf717a5801ccff3384c7f686d937caf19bdcb9e70e274a224891.md): 材料称，DeepSeek-V4-Flash 的 QuantLightningIndexer 由官方 checkpoint 中的 Indexer/Compressor 权重、沿用 DeepSeek-V3.2 的 LI 设计，以及 `cann/ops-transformer` 提供的核心 kernel 组合而成；`cann-recipes-infer` 负责模型接线、Cache、PyTorch binding 和 GE converter。文中列出 C4 层、`topk=512`、`compress_ratio=4`、Q/K/W 与 scale 的量化路径，以及 QLI、Indexer Compressor、Indexer Cache、Metadata 和 SparseAttnSharedKV 的接口关系。材料明确这是源码和版本链路核对，未进行 NPU 上板执行；版本 pin、镜像 ABI 组合和若干运行时错误（如 `aclnnQuantLightningIndexer not in libopapi.so`、算子不存在）属于配置依据或可能失败表现，不应视为已验证成功。此前关于 TensorMove/DSA 的推测未被沿用，且主 recipe 与单卡 SGLang 路径的版本绑定存在差异。

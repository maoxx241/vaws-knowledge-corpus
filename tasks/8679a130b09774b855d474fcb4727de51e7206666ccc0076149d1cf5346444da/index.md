---
blocks:
- block_id: 0427dcfabd1238bb5ffab15981c46a729feb9d7efa4da896fee9a45573859041
  indexed: true
  sha256: f73bc904e14cb4c718a0a7582082c57dcba823fdce25e7dbdf45af92667bb4d3
  source_range:
    end: 1078508
    part: 0
    start: 0
  summary: 记录的结论认为，`torch_npu.npu_mla_prolog_v2` 背后的 `aclnnMlaPrologV2WeightNz` 与仓库 `main` 的自研 `MLAPreprocess` 不一致：虽有相近的 MLA 五路计算图，但权重接口、RMSNorm epsilon、shape 上限、量化模式、cache layout、slot mapping 类型、输出契约和空 Tensor 行为均存在差异。该判断来自对官方规格、仓库实现和测试的报告性核对，测试主要验证输出写入与 cache layout，未完成逐 Tensor 精度对比；此外，CANN 9.1 已标记 V2 将废弃并推荐 V3。
  title: CANN 9.1 MLA Prolog V2 与 MLAPreprocess 规格对比
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8679a130b09774b855d474fcb4727de51e7206666ccc0076149d1cf5346444da
  kind: experience
  material_digest: b2db54c52697f76dfa340c68c44ebb84987e45eb512dc20298f58c43f5b389ee
  revision: 32364f3ae1080773083d18ddbe446a31049c9b20e0961f794924608bc5148ba9
  schema: mindie-entry/3
  summary: 当前索引记录了一次报告性对比：两者核心计算图相近，但接口和多项数值、shape、量化、cache 语义不对齐，不能直接替代。需保留其不确定性：现有测试未完成逐 Tensor 精度验证；如继续替换评估，应关注 epsilon、权重拆包、slot mapping、cache layout、动态反量化 scale，并考虑以 V3 为长期目标。
  title: MLA Prolog V2 与 MLAPreprocess 规格核对
navigation: 当前索引记录了一次报告性对比：两者核心计算图相近，但接口和多项数值、shape、量化、cache 语义不对齐，不能直接替代。需保留其不确定性：现有测试未完成逐 Tensor 精度验证；如继续替换评估，应关注 epsilon、权重拆包、slot mapping、cache layout、动态反量化 scale，并考虑以 V3 为长期目标。
schema: mindie-material-task/1
status: complete
task_id: 8679a130b09774b855d474fcb4727de51e7206666ccc0076149d1cf5346444da
---

# MLA Prolog V2 与 MLAPreprocess 规格核对

当前索引记录了一次报告性对比：两者核心计算图相近，但接口和多项数值、shape、量化、cache 语义不对齐，不能直接替代。需保留其不确定性：现有测试未完成逐 Tensor 精度验证；如继续替换评估，应关注 epsilon、权重拆包、slot mapping、cache layout、动态反量化 scale，并考虑以 V3 为长期目标。

## Materials

- [CANN 9.1 MLA Prolog V2 与 MLAPreprocess 规格对比](blocks/0427dcfabd1238bb5ffab15981c46a729feb9d7efa4da896fee9a45573859041.md): 记录的结论认为，`torch_npu.npu_mla_prolog_v2` 背后的 `aclnnMlaPrologV2WeightNz` 与仓库 `main` 的自研 `MLAPreprocess` 不一致：虽有相近的 MLA 五路计算图，但权重接口、RMSNorm epsilon、shape 上限、量化模式、cache layout、slot mapping 类型、输出契约和空 Tensor 行为均存在差异。该判断来自对官方规格、仓库实现和测试的报告性核对，测试主要验证输出写入与 cache layout，未完成逐 Tensor 精度对比；此外，CANN 9.1 已标记 V2 将废弃并推荐 V3。

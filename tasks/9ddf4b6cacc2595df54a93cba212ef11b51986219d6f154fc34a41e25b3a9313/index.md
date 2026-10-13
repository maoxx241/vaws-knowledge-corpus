---
blocks:
- block_id: 7791a9a376904dfe9dfd3482fe163d43ecf4f7fd04ab79a11cf27a9d3cb4bda3
  indexed: true
  sha256: 3bb7c5a2b0cb670699c4d6619bc53d7d817244628c7c0c40e51e0c4886392dcd
  source_range:
    end: 1051312
    part: 0
    start: 0
  summary: 来源报告称，SGLang oracle 的 Q/K 路径使用 FP32 RMSNorm、affine 与 NeoX RoPE，最后才转换为 BF16；vLLM A3 separate 和 non-SIMT fused 则在 RMSNorm 后先转换，并据称通过 rowprobe 观察到 Q/K/V 逐元素一致。报告还描述了 NeoX compact cache `[cos_half | sin_half]` 布局，以及 SGLang context K 的 `apply_k_norm` → `apply_k_rope` two-round 路径；这些均是来源中的检查结论，未在此独立验证。建议性结论是仅替换 5 个 draft layer 的 query Q/K，保持 context K、V、attention 与 cache 不变，但该方案仍应视为未验证方案。
  title: SGLang 与 vLLM Q/K 精度路径对比
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9ddf4b6cacc2595df54a93cba212ef11b51986219d6f154fc34a41e25b3a9313
  kind: experience
  material_digest: 8ffda3e49da9383994866a1444adcbc2615aede85ba4b89d2fe06b8605d88616
  revision: 4575e3abcb0899f114b01452ab49e4c2b77e02a9ae3f3c2df3f08d2732dce237
  schema: mindie-entry/3
  summary: 本批次材料集中于 SGLang oracle、vLLM A3 separate/non-SIMT fused 的 FP32/BF16 转换差异、rowprobe 观察、NeoX RoPE compact cache 布局，以及 context K 的 two-round 流程。当前记录包含一个据称已完成的只读核对和一个建议性的 5 层 query Q/K 替换方案；未提供更早导航内容，因此不补写被省略的上下文，也不将报告结论视为已验证成功。
  title: 当前任务导航：Q/K 精度路径与替换方案
navigation: 本批次材料集中于 SGLang oracle、vLLM A3 separate/non-SIMT fused 的 FP32/BF16 转换差异、rowprobe 观察、NeoX RoPE compact cache 布局，以及 context K 的 two-round 流程。当前记录包含一个据称已完成的只读核对和一个建议性的 5 层 query Q/K 替换方案；未提供更早导航内容，因此不补写被省略的上下文，也不将报告结论视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 9ddf4b6cacc2595df54a93cba212ef11b51986219d6f154fc34a41e25b3a9313
---

# 当前任务导航：Q/K 精度路径与替换方案

本批次材料集中于 SGLang oracle、vLLM A3 separate/non-SIMT fused 的 FP32/BF16 转换差异、rowprobe 观察、NeoX RoPE compact cache 布局，以及 context K 的 two-round 流程。当前记录包含一个据称已完成的只读核对和一个建议性的 5 层 query Q/K 替换方案；未提供更早导航内容，因此不补写被省略的上下文，也不将报告结论视为已验证成功。

## Materials

- [SGLang 与 vLLM Q/K 精度路径对比](blocks/7791a9a376904dfe9dfd3482fe163d43ecf4f7fd04ab79a11cf27a9d3cb4bda3.md): 来源报告称，SGLang oracle 的 Q/K 路径使用 FP32 RMSNorm、affine 与 NeoX RoPE，最后才转换为 BF16；vLLM A3 separate 和 non-SIMT fused 则在 RMSNorm 后先转换，并据称通过 rowprobe 观察到 Q/K/V 逐元素一致。报告还描述了 NeoX compact cache `[cos_half | sin_half]` 布局，以及 SGLang context K 的 `apply_k_norm` → `apply_k_rope` two-round 路径；这些均是来源中的检查结论，未在此独立验证。建议性结论是仅替换 5 个 draft layer 的 query Q/K，保持 context K、V、attention 与 cache 不变，但该方案仍应视为未验证方案。

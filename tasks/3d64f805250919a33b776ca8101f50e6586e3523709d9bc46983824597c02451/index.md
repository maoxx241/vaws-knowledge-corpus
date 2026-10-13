---
blocks:
- block_id: 91a81ee44ed3a5b6ce51730d0f04a7abf2e5b79a2eb09a80fa6060cfbc5e9074
  indexed: true
  sha256: a212c8d7b7f07341ba49618e750dd4b1137e38487b9bc5dd2a7719d756deeca5
  source_range:
    end: 1683788
    part: 0
    start: 0
  summary: 讨论将 `_apply_attention_residual` 作为复合算子时的输入输出规格：`prefix_sum` 为 `[T,7168]`、`block_residual` 为 `[T,R,7168]`、投影权重为 `[1,7168]`、RMSNorm 权重为 `[7168]`，输出为 `[T,7168]`，模型 dtype 主要为 BF16，计算中使用 FP32；`R` 实际随层变化，候选数为 `R+1`。澄清了每 12 层新增 residual 候选，但融合函数大多每层调用，并给出 93 层模型的调用与层级范围分析。随后据报告生成并多次简化一个 Excel 文件，最终声称按 `refs` 风格保留独立配置区和输入输出规格、删除中间 Tensor 与层级变化表；这些文件操作和校验结果属于来源中的报告，未在本索引中提升为独立验证事实。
  title: apply_attention_residual 规格与表格整理
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3d64f805250919a33b776ca8101f50e6586e3523709d9bc46983824597c02451
  kind: experience
  material_digest: af07bc56084625eec358f8fa99fc3fbb91b6ac1246d70944d20e188a8645b07c
  revision: 13b750f34f9d12e3061606e912b7fef92ac6c2a4e8f27cd7e777ec575f34c911
  schema: mindie-entry/3
  summary: 核心内容是 Kimi-K3 中 `_apply_attention_residual` 的张量规格、FP32 计算语义、`R/K` 层间变化及调用频率。后续表格整理据报告已收敛为：配置单独放置，仅展示输入输出规格，采用 `refs` 的简洁样式；原始讨论中的实现路径、文件路径和校验均应按来源独立检索，不能视为本导航已验证。
  title: 当前任务导航：apply_attention_residual 规格与简洁 Excel
navigation: 核心内容是 Kimi-K3 中 `_apply_attention_residual` 的张量规格、FP32 计算语义、`R/K` 层间变化及调用频率。后续表格整理据报告已收敛为：配置单独放置，仅展示输入输出规格，采用 `refs` 的简洁样式；原始讨论中的实现路径、文件路径和校验均应按来源独立检索，不能视为本导航已验证。
schema: mindie-material-task/1
status: complete
task_id: 3d64f805250919a33b776ca8101f50e6586e3523709d9bc46983824597c02451
---

# 当前任务导航：apply_attention_residual 规格与简洁 Excel

核心内容是 Kimi-K3 中 `_apply_attention_residual` 的张量规格、FP32 计算语义、`R/K` 层间变化及调用频率。后续表格整理据报告已收敛为：配置单独放置，仅展示输入输出规格，采用 `refs` 的简洁样式；原始讨论中的实现路径、文件路径和校验均应按来源独立检索，不能视为本导航已验证。

## Materials

- [apply_attention_residual 规格与表格整理](blocks/91a81ee44ed3a5b6ce51730d0f04a7abf2e5b79a2eb09a80fa6060cfbc5e9074.md): 讨论将 `_apply_attention_residual` 作为复合算子时的输入输出规格：`prefix_sum` 为 `[T,7168]`、`block_residual` 为 `[T,R,7168]`、投影权重为 `[1,7168]`、RMSNorm 权重为 `[7168]`，输出为 `[T,7168]`，模型 dtype 主要为 BF16，计算中使用 FP32；`R` 实际随层变化，候选数为 `R+1`。澄清了每 12 层新增 residual 候选，但融合函数大多每层调用，并给出 93 层模型的调用与层级范围分析。随后据报告生成并多次简化一个 Excel 文件，最终声称按 `refs` 风格保留独立配置区和输入输出规格、删除中间 Tensor 与层级变化表；这些文件操作和校验结果属于来源中的报告，未在本索引中提升为独立验证事实。

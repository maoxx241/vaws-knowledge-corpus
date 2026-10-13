---
blocks:
- block_id: 274fa969f06366027c57b8334bac2d68ee0b08d96a49686651b32b4e38849869
  indexed: true
  sha256: bdba919e4a0e5c0b75c32b88550205fa149c2f2f1e3390018d2dec8847dc9321
  source_range:
    end: 1055723
    part: 0
    start: 0
  summary: 一次只读审计据称检查了固定提交范围 `81d3450..4a1857b` 的 179 个文件（`+32512/-812`），并报告 A5 MLA/FIA 长上下文优化、KV Pool layerwise 行为变化及 MegaMoE `QuantType.NONE` 旁路等疑似非 Kimi 夹带；这些属于审计者的判断和建议，不是已验证结论。记录还提到 `W8A8MXFP` shared-expert 扩展、构建脚本清理、KV cache merge 重构及 CRLF 格式问题，并分别提出 split/drop/keep 处置意见；DeepSeek/Qwen 相关命中则被部分认为可能是 Kimi 通用回归载体。审计明确称未修改文件、未启动服务或推送，但这些检查结果仍应作为可检索的、可能不完整且可被后续修正的参考材料。
  title: 179 文件夹带审计与拆分建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7133eaa76d26d4321930dae34260cc3c4f639461e894c4ffa2a978545d7ace1a
  kind: experience
  material_digest: bb4574737e4e9c115bc0f08187710fb919960f75e44fc10448f267967abe6b29
  revision: 6544bdc972a02316c26e28b6861bc9901ed6ef91c509a10e4c8044ee873a8d90
  schema: mindie-entry/3
  summary: 当前唯一材料是对 `81d3450..4a1857b`、179 个文件的只读夹带审计，重点覆盖 A5 MLA/FIA、KV Pool、MegaMoE、MoE 精度扩展、构建清理、KV cache merge 与格式检查。记录包含失败/不确定性边界和 split/drop/keep 建议，不应将任何假设、报告的检查或处置建议提升为已验证成功；后续可按文件、提交、错误/格式术语及 Kimi 依赖链独立检索。
  title: 当前任务导航：固定范围夹带审计参考索引
navigation: 当前唯一材料是对 `81d3450..4a1857b`、179 个文件的只读夹带审计，重点覆盖 A5 MLA/FIA、KV Pool、MegaMoE、MoE 精度扩展、构建清理、KV cache merge 与格式检查。记录包含失败/不确定性边界和 split/drop/keep 建议，不应将任何假设、报告的检查或处置建议提升为已验证成功；后续可按文件、提交、错误/格式术语及 Kimi 依赖链独立检索。
schema: mindie-material-task/1
status: complete
task_id: 7133eaa76d26d4321930dae34260cc3c4f639461e894c4ffa2a978545d7ace1a
---

# 当前任务导航：固定范围夹带审计参考索引

当前唯一材料是对 `81d3450..4a1857b`、179 个文件的只读夹带审计，重点覆盖 A5 MLA/FIA、KV Pool、MegaMoE、MoE 精度扩展、构建清理、KV cache merge 与格式检查。记录包含失败/不确定性边界和 split/drop/keep 建议，不应将任何假设、报告的检查或处置建议提升为已验证成功；后续可按文件、提交、错误/格式术语及 Kimi 依赖链独立检索。

## Materials

- [179 文件夹带审计与拆分建议](blocks/274fa969f06366027c57b8334bac2d68ee0b08d96a49686651b32b4e38849869.md): 一次只读审计据称检查了固定提交范围 `81d3450..4a1857b` 的 179 个文件（`+32512/-812`），并报告 A5 MLA/FIA 长上下文优化、KV Pool layerwise 行为变化及 MegaMoE `QuantType.NONE` 旁路等疑似非 Kimi 夹带；这些属于审计者的判断和建议，不是已验证结论。记录还提到 `W8A8MXFP` shared-expert 扩展、构建脚本清理、KV cache merge 重构及 CRLF 格式问题，并分别提出 split/drop/keep 处置意见；DeepSeek/Qwen 相关命中则被部分认为可能是 Kimi 通用回归载体。审计明确称未修改文件、未启动服务或推送，但这些检查结果仍应作为可检索的、可能不完整且可被后续修正的参考材料。

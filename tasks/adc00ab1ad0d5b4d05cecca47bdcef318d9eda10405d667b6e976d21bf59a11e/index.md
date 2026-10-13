---
blocks:
- block_id: 05f4d9d87d7c0f07595dfc310d32e2408af0dd9cd2984796a21d99bab95e082d
  indexed: true
  sha256: a6e936ebd8931043e985ab8f6452920eb9830166160be017323aa89fe5bb9f83
  source_range:
    end: 3657165
    part: 0
    start: 0
  summary: 讨论认为 v0.23 的 W4A8 CombineV2/FUSED_MC2 可能存在 packed-row 与 token-row 累计单位混用，导致容量提前截断，但“所有 W4A8 MoE 都有精度问题”尚未证实。后续分析修正了“此前配置未走该算子”的解释：DeepSeek/GLM 长期使用相关路径，历史验证未暴露可能与 4096-token、top_k、路由均衡、FlashComm1/shared-expert-DP、padding 及实际 mega_moe_max_tokens 有关；报告仍缺少 per-rank expert rows 和有效 shape 等决定性证据。不能把 max_num_batched_tokens≤4096 视为通用安全线；已报告的临时规避是关闭 FUSED_MC2，或谨慎限制到 2048 并检查每 rank 的 expert_token_nums。
  title: '#14869：W4A8 MoE FUSED_MC2 精度风险与容量边界'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: adc00ab1ad0d5b4d05cecca47bdcef318d9eda10405d667b6e976d21bf59a11e
  kind: experience
  material_digest: a548d95d3ddab6cde15d557cdd54b3906ff22ed0d701e9f7f6608c7a13906bc2
  revision: f6d666e5d9e05e5689ab0a1db3e3763414ca38dfac3c6b8f4a40449d5763ffff
  schema: mindie-entry/3
  summary: '已整理 #14869 关于 v0.23 W4A8 MoE FUSED_MC2 潜在容量 bug 的分析及后续修正。当前结论保留不确定性：源码风险高度可信，但影响范围、4480-token 复现归因和 4096 安全边界均未验证；后续检索应重点查找有效 shape、top_k、每 rank expert_token_nums、运行开关、mega_moe_max_tokens 与独立算子 A/B 结果。'
  title: 当前任务导航：#14869 W4A8 MoE 风险索引
navigation: '已整理 #14869 关于 v0.23 W4A8 MoE FUSED_MC2 潜在容量 bug 的分析及后续修正。当前结论保留不确定性：源码风险高度可信，但影响范围、4480-token 复现归因和 4096 安全边界均未验证；后续检索应重点查找有效 shape、top_k、每 rank expert_token_nums、运行开关、mega_moe_max_tokens 与独立算子 A/B 结果。'
schema: mindie-material-task/1
status: complete
task_id: adc00ab1ad0d5b4d05cecca47bdcef318d9eda10405d667b6e976d21bf59a11e
---

# 当前任务导航：#14869 W4A8 MoE 风险索引

已整理 #14869 关于 v0.23 W4A8 MoE FUSED_MC2 潜在容量 bug 的分析及后续修正。当前结论保留不确定性：源码风险高度可信，但影响范围、4480-token 复现归因和 4096 安全边界均未验证；后续检索应重点查找有效 shape、top_k、每 rank expert_token_nums、运行开关、mega_moe_max_tokens 与独立算子 A/B 结果。

## Materials

- [#14869：W4A8 MoE FUSED_MC2 精度风险与容量边界](blocks/05f4d9d87d7c0f07595dfc310d32e2408af0dd9cd2984796a21d99bab95e082d.md): 讨论认为 v0.23 的 W4A8 CombineV2/FUSED_MC2 可能存在 packed-row 与 token-row 累计单位混用，导致容量提前截断，但“所有 W4A8 MoE 都有精度问题”尚未证实。后续分析修正了“此前配置未走该算子”的解释：DeepSeek/GLM 长期使用相关路径，历史验证未暴露可能与 4096-token、top_k、路由均衡、FlashComm1/shared-expert-DP、padding 及实际 mega_moe_max_tokens 有关；报告仍缺少 per-rank expert rows 和有效 shape 等决定性证据。不能把 max_num_batched_tokens≤4096 视为通用安全线；已报告的临时规避是关闭 FUSED_MC2，或谨慎限制到 2048 并检查每 rank 的 expert_token_nums。

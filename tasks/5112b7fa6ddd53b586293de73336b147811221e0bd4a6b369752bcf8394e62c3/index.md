---
blocks:
- block_id: 55190a0f0ba9c78535cb40116fd5de468253047865433503f9e254c126ec9d88
  indexed: true
  sha256: 7b71e5123e2c663af14279921c4e670d2eda625695e3aaaea6d6d3c7f0036acd
  source_range:
    end: 995544
    part: 0
    start: 0
  summary: '记录了对 vllm-ascend 多个 PR 的 GitHub 操作：PR #14428 针对 `gather_idx` 注释及其与原高级索引/`copy_` 逻辑等价性提交并修改了 `Request changes`；PR #14547 针对独立 `_MC2_DRAFT` 通信域的 HCCL buffer 风险及 `_MEGA_MOE_SUPPORTED` 变量命名提出意见；PR #14763 和 #14755 分别评论了 `getattr` 使用及应在 `enable_custom_op()` 中按 A3 校验 Sparse KV ops。随后对 PR #15416 和 #15315 提交了无正文 `APPROVE`，其中 #15416 回查时整体状态仍为 `REVIEW_REQUIRED`，#15315 显示整体为 `APPROVED`；这些结果均为消息中报告的操作与状态，未独立验证。'
  title: GitHub PR 检视与批准记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5112b7fa6ddd53b586293de73336b147811221e0bd4a6b369752bcf8394e62c3
  kind: experience
  material_digest: 7c165c2f6898dd50b519118c8c2a4d2039767227b9d2b1161780cb7d735a3377
  revision: 8ffe27d17ab05195b386ab088427494cd72db30eaedacb8e47ee361c7197c9e9
  schema: mindie-entry/3
  summary: '可检索内容包括：#14428 的 `gather_idx` 注释/等价性修改要求；#14547 的 MC2 通信域复用建议与 op-transformer 插件变量命名建议；#14763 的 `connector_worker`/`getattr` 条件建议；#14755 的 A3 与 `enable_custom_op()` Sparse KV 校验方案；以及 #15416、#15315 的无评论批准记录。需注意：#14755 的评论先有较泛表述、后改为明确 A3 方案；#15416 虽已报告批准成功，整体状态仍是 `REVIEW_REQUIRED`。'
  title: 当前任务导航：vllm-ascend PR 检视与审批
navigation: '可检索内容包括：#14428 的 `gather_idx` 注释/等价性修改要求；#14547 的 MC2 通信域复用建议与 op-transformer 插件变量命名建议；#14763 的 `connector_worker`/`getattr` 条件建议；#14755 的 A3 与 `enable_custom_op()` Sparse KV 校验方案；以及 #15416、#15315 的无评论批准记录。需注意：#14755 的评论先有较泛表述、后改为明确 A3 方案；#15416 虽已报告批准成功，整体状态仍是 `REVIEW_REQUIRED`。'
schema: mindie-material-task/1
status: complete
task_id: 5112b7fa6ddd53b586293de73336b147811221e0bd4a6b369752bcf8394e62c3
---

# 当前任务导航：vllm-ascend PR 检视与审批

可检索内容包括：#14428 的 `gather_idx` 注释/等价性修改要求；#14547 的 MC2 通信域复用建议与 op-transformer 插件变量命名建议；#14763 的 `connector_worker`/`getattr` 条件建议；#14755 的 A3 与 `enable_custom_op()` Sparse KV 校验方案；以及 #15416、#15315 的无评论批准记录。需注意：#14755 的评论先有较泛表述、后改为明确 A3 方案；#15416 虽已报告批准成功，整体状态仍是 `REVIEW_REQUIRED`。

## Materials

- [GitHub PR 检视与批准记录](blocks/55190a0f0ba9c78535cb40116fd5de468253047865433503f9e254c126ec9d88.md): 记录了对 vllm-ascend 多个 PR 的 GitHub 操作：PR #14428 针对 `gather_idx` 注释及其与原高级索引/`copy_` 逻辑等价性提交并修改了 `Request changes`；PR #14547 针对独立 `_MC2_DRAFT` 通信域的 HCCL buffer 风险及 `_MEGA_MOE_SUPPORTED` 变量命名提出意见；PR #14763 和 #14755 分别评论了 `getattr` 使用及应在 `enable_custom_op()` 中按 A3 校验 Sparse KV ops。随后对 PR #15416 和 #15315 提交了无正文 `APPROVE`，其中 #15416 回查时整体状态仍为 `REVIEW_REQUIRED`，#15315 显示整体为 `APPROVED`；这些结果均为消息中报告的操作与状态，未独立验证。

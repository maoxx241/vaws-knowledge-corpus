---
blocks:
- block_id: fce721f41ca62f213f16b21c64b6eab0f2cb1f29bf3a2291bb8edba2b3964f9a
  indexed: true
  sha256: 0a648a7a94294aae7e177c4786d5cbea6dd9d1831ee0e825944bc4805e1eb317
  source_range:
    end: 3086108
    part: 0
    start: 0
  summary: 只读审计将偶现同步/精度问题的最强候选收窄为 DSpark GDN batch reorder 阈值漏移植：临时分支按 `1+7=8`，而主线修复 `39d1cf9b3` 要求按 `N=7` 并移除 Ascend runner 的强制阈值 override。该假设认为并发阶段不同的 1-token decode 与 8-token verify 可能被错误归入同一区域，混合批先污染 hybrid state、随后纯 8-token `FULL` 首步出现 NaN；但尚未验证为根因。普通 padded block-table 尾行、slot mapping tail、causal conv1d，以及同步调度、PP1、无 DBO 条件下的 KVBlockZeroer 多 in-flight race 被列为当前较弱或不适用路径；建议的最小 A/B 尚未执行。
  title: DSpark 并发 batch reorder 阈值候选根因
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e787a3e7baef879408f1534365de01fb3eb827282df3b3d4dfeac9822cb5652f
  kind: experience
  material_digest: 6e20c075cfb4aeada207798ab0e0eced93ab624ef84b2b2d0b772de2053d1781
  revision: 4ab4f52c0cf1a9821f95a9062698c4269bb10a61ce98abc94206f2f71bc83594
  schema: mindie-entry/3
  summary: 当前唯一新增材料是一次只读审计：重点从 graph block-table/slot-mapping 残留和 causal conv1d 转向 DSpark batch reorder/GDN 阈值语义。可检索 `DSpark`、`GDN`、`calculate_reorder_batch_threshold`、`num_speculative_tokens`、`39d1cf9b3`、`1-token decode`、`8-token verify`、`hybrid state`、`NaN`、`async scheduling`、`DBO` 与 `KVBlockZeroer`；A/B 修改和复跑仍未完成，结论保持为未验证候选。
  title: 当前任务导航：并发同步/精度问题检索索引
navigation: 当前唯一新增材料是一次只读审计：重点从 graph block-table/slot-mapping 残留和 causal conv1d 转向 DSpark batch reorder/GDN 阈值语义。可检索 `DSpark`、`GDN`、`calculate_reorder_batch_threshold`、`num_speculative_tokens`、`39d1cf9b3`、`1-token decode`、`8-token verify`、`hybrid state`、`NaN`、`async scheduling`、`DBO` 与 `KVBlockZeroer`；A/B 修改和复跑仍未完成，结论保持为未验证候选。
schema: mindie-material-task/1
status: complete
task_id: e787a3e7baef879408f1534365de01fb3eb827282df3b3d4dfeac9822cb5652f
---

# 当前任务导航：并发同步/精度问题检索索引

当前唯一新增材料是一次只读审计：重点从 graph block-table/slot-mapping 残留和 causal conv1d 转向 DSpark batch reorder/GDN 阈值语义。可检索 `DSpark`、`GDN`、`calculate_reorder_batch_threshold`、`num_speculative_tokens`、`39d1cf9b3`、`1-token decode`、`8-token verify`、`hybrid state`、`NaN`、`async scheduling`、`DBO` 与 `KVBlockZeroer`；A/B 修改和复跑仍未完成，结论保持为未验证候选。

## Materials

- [DSpark 并发 batch reorder 阈值候选根因](blocks/fce721f41ca62f213f16b21c64b6eab0f2cb1f29bf3a2291bb8edba2b3964f9a.md): 只读审计将偶现同步/精度问题的最强候选收窄为 DSpark GDN batch reorder 阈值漏移植：临时分支按 `1+7=8`，而主线修复 `39d1cf9b3` 要求按 `N=7` 并移除 Ascend runner 的强制阈值 override。该假设认为并发阶段不同的 1-token decode 与 8-token verify 可能被错误归入同一区域，混合批先污染 hybrid state、随后纯 8-token `FULL` 首步出现 NaN；但尚未验证为根因。普通 padded block-table 尾行、slot mapping tail、causal conv1d，以及同步调度、PP1、无 DBO 条件下的 KVBlockZeroer 多 in-flight race 被列为当前较弱或不适用路径；建议的最小 A/B 尚未执行。

---
blocks:
- block_id: c1e05bc858fe31831f39cc9508a601680cfc8e09e5fd348b1cf82cef4233ff05
  indexed: true
  sha256: 754c34416cd8498aad3c0b02efe804d98a78785a7875823eb7f8ed7b5f647f9d
  source_range:
    end: 747689
    part: 0
    start: 0
  summary: 只读审查指出 runtime 声明六类记录而 verifier 仅统计四类，可能触发 `declared actual record counts differ`；需补齐 `target_mla_metadata`、GDN metadata 等计数及唯一键。建议将 GDN 校验拆为物理 destination 与控制语义两部分，并新增按请求独立归一化的 `target_mla_metadata_semantics`，避免跨请求比较 reset 后变化的物理 block ID/hash。记录了生产 `prefill_context_lens` 使用 `sequence_length_after_forward`、metadata 可能未按 batch 切片、关键 `values` 缺失时应 fail closed 等不确定性和剩余风险；这些是审查建议，未验证为成功修复。
  title: Verifier 契约与元数据校验审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 02b8c9f147a94310862fefe6dc9a88f93f2075f6bd5b5180b4e8d228072c1b3a
  kind: experience
  material_digest: a8c7918e84ca2e8dec331b44141c79b5ca8aace64881213db5093d8bce218e04
  revision: c74a2549f6fe7519002c3dcd584ea69122befdb06827059f10114442caac3edf
  schema: mindie-entry/3
  summary: 当前材料聚焦记录计数契约、GDN state/control 语义拆分、MLA metadata 独立校验及分类优先级。重点检索词包括 `declared actual record counts differ`、`target_mla_metadata_semantics`、`gdn_state_destination_semantics`、`prefill_context_lens`、`expected_kernel_page_ids`、fail closed；修复建议和风险均未被确认已实施。
  title: Verifier 与 runtime 元数据校验
navigation: 当前材料聚焦记录计数契约、GDN state/control 语义拆分、MLA metadata 独立校验及分类优先级。重点检索词包括 `declared actual record counts differ`、`target_mla_metadata_semantics`、`gdn_state_destination_semantics`、`prefill_context_lens`、`expected_kernel_page_ids`、fail closed；修复建议和风险均未被确认已实施。
schema: mindie-material-task/1
status: complete
task_id: 02b8c9f147a94310862fefe6dc9a88f93f2075f6bd5b5180b4e8d228072c1b3a
---

# Verifier 与 runtime 元数据校验

当前材料聚焦记录计数契约、GDN state/control 语义拆分、MLA metadata 独立校验及分类优先级。重点检索词包括 `declared actual record counts differ`、`target_mla_metadata_semantics`、`gdn_state_destination_semantics`、`prefill_context_lens`、`expected_kernel_page_ids`、fail closed；修复建议和风险均未被确认已实施。

## Materials

- [Verifier 契约与元数据校验审查](blocks/c1e05bc858fe31831f39cc9508a601680cfc8e09e5fd348b1cf82cef4233ff05.md): 只读审查指出 runtime 声明六类记录而 verifier 仅统计四类，可能触发 `declared actual record counts differ`；需补齐 `target_mla_metadata`、GDN metadata 等计数及唯一键。建议将 GDN 校验拆为物理 destination 与控制语义两部分，并新增按请求独立归一化的 `target_mla_metadata_semantics`，避免跨请求比较 reset 后变化的物理 block ID/hash。记录了生产 `prefill_context_lens` 使用 `sequence_length_after_forward`、metadata 可能未按 batch 切片、关键 `values` 缺失时应 fail closed 等不确定性和剩余风险；这些是审查建议，未验证为成功修复。

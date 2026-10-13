---
blocks:
- block_id: e12dcca7893f7fbdf72d7917c0a2af358749ee4e96db9217b9938f48eb381e2a
  indexed: true
  sha256: 6ab9df90bd6d68d03384c8a41d3e0d87c9d537afbcdda0d7a35650e4f86635d2
  source_range:
    end: 626556
    part: 0
    start: 0
  summary: 只读审计将问题初步归因于 K3 多模态 outer wrapper 的接口缺口：DSpark 的 `use_aux_hidden_state_outputs=True` 首次触发了 `SupportsEagle3`、aux hidden state 转发和 tuple 输出相关检查，因此目前判断并非 DSpark 算法缺陷，但该结论仍是审计判断而非已验证案例。记录中建议在 outer wrapper 显式 delegate 相关接口，并指出 `media_placeholder_token_id` 也可能是公共 speculative proposer 的适配缺口；共享树存在未提交实现，但不能据此视为成功修复。另有后续要求：先验证完整权重服务正常，再继续 5 层 P/D 分离；截断权重目录缺少 `tiktoken.model`，可用显式 `K3_TOKENIZER` 指向完整权重目录，完整权重本身尚待优先确认。
  title: ci-new/K3 多模态接口缺口与完整权重验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 15ebe015c829c4d190e850da6620f67ff4681e78e0b0999e6b607014b65f0505
  kind: experience
  material_digest: 61227d0ebecf26ab866db9878f3818e9d590878c93dec2ea0cc47e568478160a
  revision: 317b7e1d68df8f253d81b285541117ea30d1dc1a3cbf9ae95044a3fb9dc8a9e8
  schema: mindie-entry/3
  summary: 当前索引涵盖 ci-new 中疑似由 K3 多模态 wrapper 接口缺口触发的 speculative/DSpark 路径问题，以及 `media_placeholder_token_id`、`tiktoken.model` 和 tokenizer 配置线索。后续应先验证完整权重服务，再处理 5 层 P/D 分离；现有修复方向和归因均为未完全验证的记录，原始材料仍需独立检索。
  title: K3/ci-new 问题审计与验证顺序
navigation: 当前索引涵盖 ci-new 中疑似由 K3 多模态 wrapper 接口缺口触发的 speculative/DSpark 路径问题，以及 `media_placeholder_token_id`、`tiktoken.model` 和 tokenizer 配置线索。后续应先验证完整权重服务，再处理 5 层 P/D 分离；现有修复方向和归因均为未完全验证的记录，原始材料仍需独立检索。
schema: mindie-material-task/1
status: complete
task_id: 15ebe015c829c4d190e850da6620f67ff4681e78e0b0999e6b607014b65f0505
---

# K3/ci-new 问题审计与验证顺序

当前索引涵盖 ci-new 中疑似由 K3 多模态 wrapper 接口缺口触发的 speculative/DSpark 路径问题，以及 `media_placeholder_token_id`、`tiktoken.model` 和 tokenizer 配置线索。后续应先验证完整权重服务，再处理 5 层 P/D 分离；现有修复方向和归因均为未完全验证的记录，原始材料仍需独立检索。

## Materials

- [ci-new/K3 多模态接口缺口与完整权重验证](blocks/e12dcca7893f7fbdf72d7917c0a2af358749ee4e96db9217b9938f48eb381e2a.md): 只读审计将问题初步归因于 K3 多模态 outer wrapper 的接口缺口：DSpark 的 `use_aux_hidden_state_outputs=True` 首次触发了 `SupportsEagle3`、aux hidden state 转发和 tuple 输出相关检查，因此目前判断并非 DSpark 算法缺陷，但该结论仍是审计判断而非已验证案例。记录中建议在 outer wrapper 显式 delegate 相关接口，并指出 `media_placeholder_token_id` 也可能是公共 speculative proposer 的适配缺口；共享树存在未提交实现，但不能据此视为成功修复。另有后续要求：先验证完整权重服务正常，再继续 5 层 P/D 分离；截断权重目录缺少 `tiktoken.model`，可用显式 `K3_TOKENIZER` 指向完整权重目录，完整权重本身尚待优先确认。

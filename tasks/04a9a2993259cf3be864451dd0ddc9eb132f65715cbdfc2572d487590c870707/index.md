---
blocks:
- block_id: 87ae5c248882c43e0bed49a71cb1fd4618e45536bcfa7171697d196904e10246
  indexed: true
  sha256: 9c1d957798efd8b5cb5f00dba102e6659dca3dad6857fac7379e1db658353450
  source_range:
    end: 839207
    part: 0
    start: 0
  summary: 先发现确定性 blocker：vLLM 0.23 的 `ChatParams.with_defaults()`/`merge_kwargs` 会把显式 `tool_choice="auto"` 当作 unset，可能被服务端 `required` 默认值覆盖，造成 prompt 与 request/parser 模式分叉；最小复现观察到该差异。后续只读复核报告称通过 `kimi_k3:auto` sentinel 使 typed `auto` 穿过 merge，并在 renderer 与 `_effective_chat_template_kwargs` 边界恢复，同时称 `none`、`required`、named 行为未改变。`git diff --check` 和 8 个 Python 文件 AST 检查通过，但因缺少 `torch` 未运行 pytest；该修复结论仍应与前述 blocker、未覆盖的完整测试结果一起检索。
  title: vLLM 0.23 中 auto 语义修复的只读复核
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 04a9a2993259cf3be864451dd0ddc9eb132f65715cbdfc2572d487590c870707
  kind: experience
  material_digest: 579da99fb519a217ce2345857244b0cf4ed25b9bb0a3cb82ec07fa3ca25cc2d5
  revision: d42b0c02dcec63c46845cea0ab1151a6af0b0a639ca7e9b146d1c7efcf1fdf8a
  schema: mindie-entry/3
  summary: 当前材料聚焦 `tool_choice="auto"` 被 `merge_kwargs` 视为 unset 的 blocker，以及随后使用 `kimi_k3:auto` sentinel 在 renderer/有效 kwargs 边界恢复语义的只读复核。已报告 diff/AST 检查通过并用本地 vLLM 0.23 做最小复现，但 pytest 因缺少 `torch` 未执行；保留该测试缺口和前后结论的校正关系。
  title: K3 tool_choice auto 语义与 vLLM 0.23 merge
navigation: 当前材料聚焦 `tool_choice="auto"` 被 `merge_kwargs` 视为 unset 的 blocker，以及随后使用 `kimi_k3:auto` sentinel 在 renderer/有效 kwargs 边界恢复语义的只读复核。已报告 diff/AST 检查通过并用本地 vLLM 0.23 做最小复现，但 pytest 因缺少 `torch` 未执行；保留该测试缺口和前后结论的校正关系。
schema: mindie-material-task/1
status: complete
task_id: 04a9a2993259cf3be864451dd0ddc9eb132f65715cbdfc2572d487590c870707
---

# K3 tool_choice auto 语义与 vLLM 0.23 merge

当前材料聚焦 `tool_choice="auto"` 被 `merge_kwargs` 视为 unset 的 blocker，以及随后使用 `kimi_k3:auto` sentinel 在 renderer/有效 kwargs 边界恢复语义的只读复核。已报告 diff/AST 检查通过并用本地 vLLM 0.23 做最小复现，但 pytest 因缺少 `torch` 未执行；保留该测试缺口和前后结论的校正关系。

## Materials

- [vLLM 0.23 中 auto 语义修复的只读复核](blocks/87ae5c248882c43e0bed49a71cb1fd4618e45536bcfa7171697d196904e10246.md): 先发现确定性 blocker：vLLM 0.23 的 `ChatParams.with_defaults()`/`merge_kwargs` 会把显式 `tool_choice="auto"` 当作 unset，可能被服务端 `required` 默认值覆盖，造成 prompt 与 request/parser 模式分叉；最小复现观察到该差异。后续只读复核报告称通过 `kimi_k3:auto` sentinel 使 typed `auto` 穿过 merge，并在 renderer 与 `_effective_chat_template_kwargs` 边界恢复，同时称 `none`、`required`、named 行为未改变。`git diff --check` 和 8 个 Python 文件 AST 检查通过，但因缺少 `torch` 未运行 pytest；该修复结论仍应与前述 blocker、未覆盖的完整测试结果一起检索。

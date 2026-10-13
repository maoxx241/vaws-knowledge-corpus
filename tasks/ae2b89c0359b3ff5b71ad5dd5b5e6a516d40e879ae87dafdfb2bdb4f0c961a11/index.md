---
blocks:
- block_id: 7fa4f38ee6668c6dfd47b79a86a6015a9406efd3e705fb9d4356285d463a9b57
  indexed: true
  sha256: e8c585cc368932c721933df65d2c7368ada2b99f1e5517448fcbb605cd1c009d
  source_range:
    end: 847845
    part: 0
    start: 0
  summary: '围绕 vllm-ascend PR #13277，修复了 MLA 传递 causal metadata、按 causal 选择 FIA sparse_mode/mask，以及 context_slot_mapping 与 draft layers 数量不一致时抛出 ValueError；后者被说明为当前调用链契约，而 tensor 长度仍可随上下文变化。记录称 Ruff、静态检查和 pre-commit 通过，并创建、推送了提交 7e56d62f8；完整 pytest 因缺少 torch/vllm 未运行，review threads 尚未回复或 resolve，其他 CI 当时仍在运行。'
  title: 'PR #13277 DSpark MLA 检视修复与推送'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ae2b89c0359b3ff5b71ad5dd5b5e6a516d40e879ae87dafdfb2bdb4f0c961a11
  kind: experience
  material_digest: 71028378c3a6e7df6ff0118ee99e16ae15a257c2c441948a7828ccc69e9a9615
  revision: d3904b7bfa030f69ebb67bc11341574e82a9b65edcac68168d656363f788f830
  schema: mindie-entry/3
  summary: '已记录 PR #13277 的修复背景、严格 mapping 校验及提交 7e56d62f8 推送结果；需注意完整 pytest 未运行、review threads 未处理，且相关成功状态来自源对话中的报告，不能视为独立验证。'
  title: '当前任务导航：PR #13277 检视修复'
navigation: '已记录 PR #13277 的修复背景、严格 mapping 校验及提交 7e56d62f8 推送结果；需注意完整 pytest 未运行、review threads 未处理，且相关成功状态来自源对话中的报告，不能视为独立验证。'
schema: mindie-material-task/1
status: complete
task_id: ae2b89c0359b3ff5b71ad5dd5b5e6a516d40e879ae87dafdfb2bdb4f0c961a11
---

# 当前任务导航：PR #13277 检视修复

已记录 PR #13277 的修复背景、严格 mapping 校验及提交 7e56d62f8 推送结果；需注意完整 pytest 未运行、review threads 未处理，且相关成功状态来自源对话中的报告，不能视为独立验证。

## Materials

- [PR #13277 DSpark MLA 检视修复与推送](blocks/7fa4f38ee6668c6dfd47b79a86a6015a9406efd3e705fb9d4356285d463a9b57.md): 围绕 vllm-ascend PR #13277，修复了 MLA 传递 causal metadata、按 causal 选择 FIA sparse_mode/mask，以及 context_slot_mapping 与 draft layers 数量不一致时抛出 ValueError；后者被说明为当前调用链契约，而 tensor 长度仍可随上下文变化。记录称 Ruff、静态检查和 pre-commit 通过，并创建、推送了提交 7e56d62f8；完整 pytest 因缺少 torch/vllm 未运行，review threads 尚未回复或 resolve，其他 CI 当时仍在运行。

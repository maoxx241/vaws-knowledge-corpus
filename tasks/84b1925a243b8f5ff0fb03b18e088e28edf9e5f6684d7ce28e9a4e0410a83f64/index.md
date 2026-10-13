---
blocks:
- block_id: 29c85435000852f6913cc9c6d66e7642e61859cf7168d8b6e541843aaa8782e3
  indexed: true
  sha256: 67f816c24294a50af60887f59ceb70eb534bf24f3f7370c845bcbab584415c51
  source_range:
    end: 548790
    part: 0
    start: 0
  summary: 隔离参考库审查记录称，仓库惯例是用 `vllm_version_is("0.25.1")` 分流，Kimi 在模型内处理 `is_vit_use_data_parallel` 的签名差异，并覆盖 release 可整除、不可整除 fallback 和 main 参数转发路径。此前注释声称 main 与强制 v0.25.1 两条模型 UT 均为 20/20 且四机服务健康，但该记录同时明确本次只读审查未修改文件，后续 split-stage 仍停在 autosquash 测试文件冲突，因此不能将兼容修复或测试结果视为已验证完成。
  title: Kimi vLLM 版本隔离审查与测试状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 84b1925a243b8f5ff0fb03b18e088e28edf9e5f6684d7ce28e9a4e0410a83f64
  kind: experience
  material_digest: 3be46e399bc95809c6b8a21c4b941549efad0371db75d44b69c90c333e094792
  revision: f2c943237eeba2a1e36fc0a4eabbdd7ed19f9569c7f22206dab59e93be13f9ed
  schema: mindie-entry/3
  summary: 核心检索词：`vllm_version_is`、`is_vit_use_data_parallel(num_heads)`、v0.25.1、heads/TP 可整除、fallback、main 参数转发、autosquash 测试文件冲突、20/20、四机 health。当前材料包含相互需区分的测试成功注释、只读审查结论及未完成的冲突状态；后续应保留这些归属与不确定性，并继续核对实际代码、测试和冲突处理结果。
  title: 当前任务导航：Kimi vLLM 兼容性隔离审查
navigation: 核心检索词：`vllm_version_is`、`is_vit_use_data_parallel(num_heads)`、v0.25.1、heads/TP 可整除、fallback、main 参数转发、autosquash 测试文件冲突、20/20、四机 health。当前材料包含相互需区分的测试成功注释、只读审查结论及未完成的冲突状态；后续应保留这些归属与不确定性，并继续核对实际代码、测试和冲突处理结果。
schema: mindie-material-task/1
status: complete
task_id: 84b1925a243b8f5ff0fb03b18e088e28edf9e5f6684d7ce28e9a4e0410a83f64
---

# 当前任务导航：Kimi vLLM 兼容性隔离审查

核心检索词：`vllm_version_is`、`is_vit_use_data_parallel(num_heads)`、v0.25.1、heads/TP 可整除、fallback、main 参数转发、autosquash 测试文件冲突、20/20、四机 health。当前材料包含相互需区分的测试成功注释、只读审查结论及未完成的冲突状态；后续应保留这些归属与不确定性，并继续核对实际代码、测试和冲突处理结果。

## Materials

- [Kimi vLLM 版本隔离审查与测试状态](blocks/29c85435000852f6913cc9c6d66e7642e61859cf7168d8b6e541843aaa8782e3.md): 隔离参考库审查记录称，仓库惯例是用 `vllm_version_is("0.25.1")` 分流，Kimi 在模型内处理 `is_vit_use_data_parallel` 的签名差异，并覆盖 release 可整除、不可整除 fallback 和 main 参数转发路径。此前注释声称 main 与强制 v0.25.1 两条模型 UT 均为 20/20 且四机服务健康，但该记录同时明确本次只读审查未修改文件，后续 split-stage 仍停在 autosquash 测试文件冲突，因此不能将兼容修复或测试结果视为已验证完成。

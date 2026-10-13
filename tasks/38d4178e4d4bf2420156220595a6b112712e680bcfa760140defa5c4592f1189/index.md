---
blocks:
- block_id: f687b0ae4a336033a87a7809a41572d4557981c69770ed24b9a7a2a1a4b764e1
  indexed: true
  sha256: da2106d7ca3e852b801aff4c70ec57c4896bd30f2f119fad0792e7d50c287a25
  source_range:
    end: 539764
    part: 0
    start: 0
  summary: '已记录私密仓库 `maoxx241/vllm-ascend-private` 及 4 个 Draft PR 的创建，其中 PR #3 针对 `releases/v0.23.0` 的 `csrc/**` 内容完成最小修正并推送 commit `6ceb9d3df9f8f65b88ce50c42610859701ca219b`。修正涉及移除 `ASCEND_PLATFORM_310P` 头文件限制、保留 `npu_lightning_indexer_meta` 的 symbolic shape（`SymInt`/`empty_symint`），并确认 `get_masked_input_and_mask` 已存在于 release 基线、未被 PR #3 删除。记录的检查包括 symbolic-meta 用例、定向 Lightning-indexer 检查、`git diff --check` 和 PR scope 检查均通过；未运行 NPU/runtime 或完整 C++ build，也未回复或 resolve GitHub review threads，且 PR #1 的意见仅按 release 基线核对后部分适用。'
  title: 私密 vllm-ascend 仓库与 PR 拆分、修复进展
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 38d4178e4d4bf2420156220595a6b112712e680bcfa760140defa5c4592f1189
  kind: experience
  material_digest: 29eddd548c55d636b2b0b5271f8d8c7ac0d742a834406ea109f3378e7a572172
  revision: f086b9179d5a7df60d4a5194a2a69ff8a474ee16d1dbef01976a58bc2e03c95b
  schema: mindie-entry/3
  summary: '当前可检索上下文集中于 `maoxx241/vllm-ascend-private` 的 4 个 Draft PR：PR #1/#2 面向 main，PR #3/#4 面向 `releases/v0.23.0`，按 `csrc/**` 与非 `csrc` 内容拆分。最新状态是 PR #3 已按 release 基线完成最小修复并保持 `csrc/**` 范围；相关定向检查通过，但未完成 NPU/runtime、完整 C++ build 或 review thread 处理，PR #2 的改动范围问题仍是未完成事项。'
  title: '私密仓库 PR 拆分与 PR #3 修复'
navigation: '当前可检索上下文集中于 `maoxx241/vllm-ascend-private` 的 4 个 Draft PR：PR #1/#2 面向 main，PR #3/#4 面向 `releases/v0.23.0`，按 `csrc/**` 与非 `csrc` 内容拆分。最新状态是 PR #3 已按 release 基线完成最小修复并保持 `csrc/**` 范围；相关定向检查通过，但未完成 NPU/runtime、完整 C++ build 或 review thread 处理，PR #2 的改动范围问题仍是未完成事项。'
schema: mindie-material-task/1
status: complete
task_id: 38d4178e4d4bf2420156220595a6b112712e680bcfa760140defa5c4592f1189
---

# 私密仓库 PR 拆分与 PR #3 修复

当前可检索上下文集中于 `maoxx241/vllm-ascend-private` 的 4 个 Draft PR：PR #1/#2 面向 main，PR #3/#4 面向 `releases/v0.23.0`，按 `csrc/**` 与非 `csrc` 内容拆分。最新状态是 PR #3 已按 release 基线完成最小修复并保持 `csrc/**` 范围；相关定向检查通过，但未完成 NPU/runtime、完整 C++ build 或 review thread 处理，PR #2 的改动范围问题仍是未完成事项。

## Materials

- [私密 vllm-ascend 仓库与 PR 拆分、修复进展](blocks/f687b0ae4a336033a87a7809a41572d4557981c69770ed24b9a7a2a1a4b764e1.md): 已记录私密仓库 `maoxx241/vllm-ascend-private` 及 4 个 Draft PR 的创建，其中 PR #3 针对 `releases/v0.23.0` 的 `csrc/**` 内容完成最小修正并推送 commit `6ceb9d3df9f8f65b88ce50c42610859701ca219b`。修正涉及移除 `ASCEND_PLATFORM_310P` 头文件限制、保留 `npu_lightning_indexer_meta` 的 symbolic shape（`SymInt`/`empty_symint`），并确认 `get_masked_input_and_mask` 已存在于 release 基线、未被 PR #3 删除。记录的检查包括 symbolic-meta 用例、定向 Lightning-indexer 检查、`git diff --check` 和 PR scope 检查均通过；未运行 NPU/runtime 或完整 C++ build，也未回复或 resolve GitHub review threads，且 PR #1 的意见仅按 release 基线核对后部分适用。

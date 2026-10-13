---
blocks:
- block_id: de44915e423633f8dfc8975962ca2f92cf3c10e0c1d4a04c46a056e0c02e74ea
  indexed: true
  sha256: 1b06435ad0863052f640233ebc4408f1deab5353850144a235f21f92f520ab82
  source_range:
    end: 757731
    part: 0
    start: 0
  summary: '用户最初要求将 vllm-ascend 的 ci-new 分支按 csrc/非 csrc 拆分，分别提交到私密仓库的 main 和 release/0.23.0，已报告创建 4 个 Draft PR；其中 PR #1 后续按 3 条 review 意见完成修复并推送 commit `c641ee4561da7babf1a6dddf38db85846fb3cb35`。修复包括移除 310P include guard、完整删除 `get_masked_input_and_mask`、恢复主线 symbolic Meta 实现并移除 shape/numel checks；`check-symbolic-meta`、`py_compile` 和 focused `diff --check` 据报告通过，PR diff 仍仅含 `csrc/**`。这些是来源中的报告而非独立验证结论；PR #1 的 GitHub checks 当时仍在排队，PR #2 的范围问题以及 PR #3/PR #4 的后续状态未在该材料中完成。'
  title: '私密仓库四个 PR 与 PR #1 修复进展'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1c8c6ab456cdf46d44c35baea3c91c9b600ac86f07b6cfcf4b688da02d4df0b7
  kind: experience
  material_digest: 7c42c03c2a99523cdad5c62ada244be88987a2567a54b9a17b24891e1a15e078
  revision: 624d8adffd8d16f01e7167bf1f91adaa63df0cb255cae06f45d05955ad5c52a0
  schema: mindie-entry/3
  summary: '当前材料记录了私密仓库 `maoxx241/vllm-ascend-private` 及 4 个 Draft PR 的创建报告，其中 PR #1 已按 review 修复并推送，检查曾报告通过但 GitHub checks 仍排队。后续重点是继续处理 PR #2 中疑似多余改动，并保留对 PR #3 可复用 PR #1 意见的计划；PR #4 曾待用户 review，其他 PR 的最终合并、验证和修复状态仍未确定。原始消息块仍应独立检索，不应将上述报告视为已核实成功。'
  title: vllm-ascend 私密仓库 PR 任务导航
navigation: '当前材料记录了私密仓库 `maoxx241/vllm-ascend-private` 及 4 个 Draft PR 的创建报告，其中 PR #1 已按 review 修复并推送，检查曾报告通过但 GitHub checks 仍排队。后续重点是继续处理 PR #2 中疑似多余改动，并保留对 PR #3 可复用 PR #1 意见的计划；PR #4 曾待用户 review，其他 PR 的最终合并、验证和修复状态仍未确定。原始消息块仍应独立检索，不应将上述报告视为已核实成功。'
schema: mindie-material-task/1
status: complete
task_id: 1c8c6ab456cdf46d44c35baea3c91c9b600ac86f07b6cfcf4b688da02d4df0b7
---

# vllm-ascend 私密仓库 PR 任务导航

当前材料记录了私密仓库 `maoxx241/vllm-ascend-private` 及 4 个 Draft PR 的创建报告，其中 PR #1 已按 review 修复并推送，检查曾报告通过但 GitHub checks 仍排队。后续重点是继续处理 PR #2 中疑似多余改动，并保留对 PR #3 可复用 PR #1 意见的计划；PR #4 曾待用户 review，其他 PR 的最终合并、验证和修复状态仍未确定。原始消息块仍应独立检索，不应将上述报告视为已核实成功。

## Materials

- [私密仓库四个 PR 与 PR #1 修复进展](blocks/de44915e423633f8dfc8975962ca2f92cf3c10e0c1d4a04c46a056e0c02e74ea.md): 用户最初要求将 vllm-ascend 的 ci-new 分支按 csrc/非 csrc 拆分，分别提交到私密仓库的 main 和 release/0.23.0，已报告创建 4 个 Draft PR；其中 PR #1 后续按 3 条 review 意见完成修复并推送 commit `c641ee4561da7babf1a6dddf38db85846fb3cb35`。修复包括移除 310P include guard、完整删除 `get_masked_input_and_mask`、恢复主线 symbolic Meta 实现并移除 shape/numel checks；`check-symbolic-meta`、`py_compile` 和 focused `diff --check` 据报告通过，PR diff 仍仅含 `csrc/**`。这些是来源中的报告而非独立验证结论；PR #1 的 GitHub checks 当时仍在排队，PR #2 的范围问题以及 PR #3/PR #4 的后续状态未在该材料中完成。

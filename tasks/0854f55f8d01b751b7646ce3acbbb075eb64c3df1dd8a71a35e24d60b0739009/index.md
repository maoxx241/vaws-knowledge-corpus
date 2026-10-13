---
blocks:
- block_id: dd392520be13640c83fa95daa73b50b5036cb845367d83b785bc9699c58cb39f
  indexed: true
  sha256: 9d7d410b631f4bed59e4a14442b2e1358be804c3a890c0145e584608d9326e3c
  source_range:
    end: 1613387
    part: 0
    start: 0
  summary: 历史证据指向 `chunk_gated_delta_rule_fwd_h` 在约 41 个 head-task 后触发的第三波 slot 复用问题：空的第二个 AIV subblock 未归还 E0 HardEvent；记录中的最小修复涉及等待 `MTE3_MTE2` 或 `V_MTE2`、设置 `V_MTE2` 后发送 `vec1Done`。该候选位于指定 AscendC epilogue 文件，记录了源码 SHA256、Git blob 和 kernel-only patch-id，但明确没有形成 Git commit，仍是 dirty worktree 候选，且 `842b030f` 尚未包含它。记录称补丁后多组 task 和四机 Triton A/B 检查通过，但同时指出 Prefix Cache hit384 的精度异常仍存在，因此不能将此候选视为已解决整体 c64 卡死或 Prefix Cache 问题。
  title: AscendC 第三波 task-slot 卡死候选与验证边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0854f55f8d01b751b7646ce3acbbb075eb64c3df1dd8a71a35e24d60b0739009
  kind: experience
  material_digest: eae6b32a0e0658fd008667654f729cead7136eb2a2771f6e89e9f795abdc59c9
  revision: a530332a0ad4e6a8a8c0deb7d6347425dbed779ee56e431b5785f88069563334
  schema: mindie-entry/3
  summary: 优先检索第三波 task-slot 复用、E0 HardEvent、`chunk_gated_delta_rule_fwd_h`、dirty worktree 及 `842b030f`，区分已记录的候选修复与尚未提交的状态。保留并行边界：该候选据称覆盖特定并发 prefill 卡死，但 Prefix Cache hit384 的 token0/`!` 精度问题仍未解释或完成修复。
  title: 当前任务导航：AscendC 卡死候选与 Prefix Cache 精度问题分离
navigation: 优先检索第三波 task-slot 复用、E0 HardEvent、`chunk_gated_delta_rule_fwd_h`、dirty worktree 及 `842b030f`，区分已记录的候选修复与尚未提交的状态。保留并行边界：该候选据称覆盖特定并发 prefill 卡死，但 Prefix Cache hit384 的 token0/`!` 精度问题仍未解释或完成修复。
schema: mindie-material-task/1
status: complete
task_id: 0854f55f8d01b751b7646ce3acbbb075eb64c3df1dd8a71a35e24d60b0739009
---

# 当前任务导航：AscendC 卡死候选与 Prefix Cache 精度问题分离

优先检索第三波 task-slot 复用、E0 HardEvent、`chunk_gated_delta_rule_fwd_h`、dirty worktree 及 `842b030f`，区分已记录的候选修复与尚未提交的状态。保留并行边界：该候选据称覆盖特定并发 prefill 卡死，但 Prefix Cache hit384 的 token0/`!` 精度问题仍未解释或完成修复。

## Materials

- [AscendC 第三波 task-slot 卡死候选与验证边界](blocks/dd392520be13640c83fa95daa73b50b5036cb845367d83b785bc9699c58cb39f.md): 历史证据指向 `chunk_gated_delta_rule_fwd_h` 在约 41 个 head-task 后触发的第三波 slot 复用问题：空的第二个 AIV subblock 未归还 E0 HardEvent；记录中的最小修复涉及等待 `MTE3_MTE2` 或 `V_MTE2`、设置 `V_MTE2` 后发送 `vec1Done`。该候选位于指定 AscendC epilogue 文件，记录了源码 SHA256、Git blob 和 kernel-only patch-id，但明确没有形成 Git commit，仍是 dirty worktree 候选，且 `842b030f` 尚未包含它。记录称补丁后多组 task 和四机 Triton A/B 检查通过，但同时指出 Prefix Cache hit384 的精度异常仍存在，因此不能将此候选视为已解决整体 c64 卡死或 Prefix Cache 问题。

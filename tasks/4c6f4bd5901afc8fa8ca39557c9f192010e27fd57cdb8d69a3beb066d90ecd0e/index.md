---
blocks:
- block_id: fd5125fe4bd92bbdc2ebb8987cd09067bf5f6a96975e3227ae9ad3a6217098c1
  indexed: true
  sha256: 65cdaa0bce377454aefa1a8ecdb29a341e202be189a6511c3f561b2ec646f699
  source_range:
    end: 873127
    part: 0
    start: 0
  summary: 审查认为本轮 A/B 无需新增 model-runner 的 dummy state `-1→0` 修复：`NULL_BLOCK_ID=0`、builder 填充为 0，runner 保留防御性 `fill_(0)`；但该结论是审查判断而非已验证成功。新增 KDA helper 在 recurrent 后用设备侧 `query_start_loc[-1]` 和 `torch.where` 清零未写入的 dummy tail，以避免 `empty_like` 造成的 NaN/垃圾数据；仍存在 ACLGraph capture/replay 动态更新、runner dummy state 独立 UT，以及更广义 graph padding/state-commit 问题未覆盖。
  title: padding-zero A/B 前置条件与 KDA 尾部清零审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4c6f4bd5901afc8fa8ca39557c9f192010e27fd57cdb8d69a3beb066d90ecd0e
  kind: experience
  material_digest: 00d78ad94d7cbceff2b205296cc2ab15a37d356f72af3d78fbe545a98bdb1f26
  revision: b654955abd63926b2cf4ecf897907e9632c9a26d8c322105b170f0616c3d978f
  schema: mindie-entry/3
  summary: 本批材料记录了一次只读审查及其结论：现有 `NULL_BLOCK_ID=0`、builder/runner 的 dummy state 归零链路被认为足以进入 A/B，但未被表述为完整验证。重点检索词包括 `gdn_attn_builder.py`、`model_runner_v1.py`、`kimi_kda.py`、`torch.where`、`empty_like`、ACLGraph capture/replay、`[0,8,16]→[0,8,8]`、K=7、`NULL_BLOCK_ID`；后续仍需保留并核查未完成的 graph 动态更新和 state-commit 风险。
  title: 当前任务导航：padding-zero A/B 与 KDA 尾部清零索引
navigation: 本批材料记录了一次只读审查及其结论：现有 `NULL_BLOCK_ID=0`、builder/runner 的 dummy state 归零链路被认为足以进入 A/B，但未被表述为完整验证。重点检索词包括 `gdn_attn_builder.py`、`model_runner_v1.py`、`kimi_kda.py`、`torch.where`、`empty_like`、ACLGraph capture/replay、`[0,8,16]→[0,8,8]`、K=7、`NULL_BLOCK_ID`；后续仍需保留并核查未完成的 graph 动态更新和 state-commit 风险。
schema: mindie-material-task/1
status: complete
task_id: 4c6f4bd5901afc8fa8ca39557c9f192010e27fd57cdb8d69a3beb066d90ecd0e
---

# 当前任务导航：padding-zero A/B 与 KDA 尾部清零索引

本批材料记录了一次只读审查及其结论：现有 `NULL_BLOCK_ID=0`、builder/runner 的 dummy state 归零链路被认为足以进入 A/B，但未被表述为完整验证。重点检索词包括 `gdn_attn_builder.py`、`model_runner_v1.py`、`kimi_kda.py`、`torch.where`、`empty_like`、ACLGraph capture/replay、`[0,8,16]→[0,8,8]`、K=7、`NULL_BLOCK_ID`；后续仍需保留并核查未完成的 graph 动态更新和 state-commit 风险。

## Materials

- [padding-zero A/B 前置条件与 KDA 尾部清零审查](blocks/fd5125fe4bd92bbdc2ebb8987cd09067bf5f6a96975e3227ae9ad3a6217098c1.md): 审查认为本轮 A/B 无需新增 model-runner 的 dummy state `-1→0` 修复：`NULL_BLOCK_ID=0`、builder 填充为 0，runner 保留防御性 `fill_(0)`；但该结论是审查判断而非已验证成功。新增 KDA helper 在 recurrent 后用设备侧 `query_start_loc[-1]` 和 `torch.where` 清零未写入的 dummy tail，以避免 `empty_like` 造成的 NaN/垃圾数据；仍存在 ACLGraph capture/replay 动态更新、runner dummy state 独立 UT，以及更广义 graph padding/state-commit 问题未覆盖。

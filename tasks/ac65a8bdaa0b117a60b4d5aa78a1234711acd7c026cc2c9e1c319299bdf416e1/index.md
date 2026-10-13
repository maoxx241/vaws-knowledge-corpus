---
blocks:
- block_id: c76397fd8980efd69a7383bb9fd3e6c39342ec79c2cd1113d975f1e34c660af8
  indexed: true
  sha256: b3bd7b1ed0916430ab8e4b3620b300acba0493eaf12c86f5f51c6e743cde110d
  source_range:
    end: 732300
    part: 0
    start: 0
  summary: 只读审计建议在 `model_runner_v1.py` 的 `_update_states` 中、调用父类前同步 `num_accepted_tokens_event`；理由是父类会 remove、condense、reorder 请求行，而 `_prepare_inputs()` 中现有等待可能过晚。建议新增通用同步调度 UT，覆盖同步顺序、async 路径及无 event/禁用条件，但该方案尚未被证明成功。材料还要求剔除 ACL_DIAG、GDN dummy reset、padding 行、非目标平台和 shared-expert multistream 内容，并指出需在干净 HEAD 上复跑 hot-rank0 并发短请求及 prefix cold/warm/reset，以确认最小补丁能独立成立。
  title: model_runner_v1 状态同步候选修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ac65a8bdaa0b117a60b4d5aa78a1234711acd7c026cc2c9e1c319299bdf416e1
  kind: experience
  material_digest: 716d1dd1665c456600e2e3d35606bb90ecc5eaa4353e8ef442ccdd7d3b9274ab
  revision: 162f7a0e914cad9ef14e5bafbd5a3e8cd12bf2b399cfe38d59b3794beda37f54
  schema: mindie-entry/3
  summary: 当前材料聚焦 `model_runner_v1.py` 的 `_update_states` 生命周期竞态：候选是在父类状态变更前同步 event，并以通用 UT 验证调用顺序。该结论是待验证的审计建议，不是成功报告；r10 可能包含同步修复与 r9 reset/no-op/instrumentation 的组合证据，尚需干净 HEAD 复现指定场景。相关排除项包括 ACL_DIAG、GDN dummy reset、padding 行、PR49 Triton、`attn_res.py`、310P 及 shared-expert multistream 内容。
  title: 最小状态同步修复审计导航
navigation: 当前材料聚焦 `model_runner_v1.py` 的 `_update_states` 生命周期竞态：候选是在父类状态变更前同步 event，并以通用 UT 验证调用顺序。该结论是待验证的审计建议，不是成功报告；r10 可能包含同步修复与 r9 reset/no-op/instrumentation 的组合证据，尚需干净 HEAD 复现指定场景。相关排除项包括 ACL_DIAG、GDN dummy reset、padding 行、PR49 Triton、`attn_res.py`、310P 及 shared-expert multistream 内容。
schema: mindie-material-task/1
status: complete
task_id: ac65a8bdaa0b117a60b4d5aa78a1234711acd7c026cc2c9e1c319299bdf416e1
---

# 最小状态同步修复审计导航

当前材料聚焦 `model_runner_v1.py` 的 `_update_states` 生命周期竞态：候选是在父类状态变更前同步 event，并以通用 UT 验证调用顺序。该结论是待验证的审计建议，不是成功报告；r10 可能包含同步修复与 r9 reset/no-op/instrumentation 的组合证据，尚需干净 HEAD 复现指定场景。相关排除项包括 ACL_DIAG、GDN dummy reset、padding 行、PR49 Triton、`attn_res.py`、310P 及 shared-expert multistream 内容。

## Materials

- [model_runner_v1 状态同步候选修复](blocks/c76397fd8980efd69a7383bb9fd3e6c39342ec79c2cd1113d975f1e34c660af8.md): 只读审计建议在 `model_runner_v1.py` 的 `_update_states` 中、调用父类前同步 `num_accepted_tokens_event`；理由是父类会 remove、condense、reorder 请求行，而 `_prepare_inputs()` 中现有等待可能过晚。建议新增通用同步调度 UT，覆盖同步顺序、async 路径及无 event/禁用条件，但该方案尚未被证明成功。材料还要求剔除 ACL_DIAG、GDN dummy reset、padding 行、非目标平台和 shared-expert multistream 内容，并指出需在干净 HEAD 上复跑 hot-rank0 并发短请求及 prefix cold/warm/reset，以确认最小补丁能独立成立。

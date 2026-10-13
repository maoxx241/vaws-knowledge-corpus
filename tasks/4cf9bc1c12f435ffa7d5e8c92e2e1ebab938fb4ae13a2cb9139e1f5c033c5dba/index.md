---
blocks:
- block_id: e4a2ae989e6404f1b0cd26ab0b9bda47c81dbc2a7e1b03f994d34105ceb6ec28
  indexed: true
  sha256: 3b602f12c692c4e81c30cf30a72829522622e0b8c462d5c91847a320ddda6b9d
  source_range:
    end: 2807749
    part: 0
    start: 0
  summary: 记录了使用 Cursor CLI 的 `claude-fable-5-max` 对当前工作树及上游 v0.26.0 进行只读代码审计；现场观察到 DP0–2 为 4 个 cached 批次、DP3 仍为 16 个 new，作为区分 row 复用问题与跨 DP 波次失步的判据。Fable 初步将去除 CPU wait、更深流水与 collective ordinal 失配相关联，但该因果链缺少完整控制流证据，需补充 functional diff；因此尚未验证根因，也未将 balance scheduling 视为修复或 A/B 结果。最终记录称未修改代码、未操作服务，并已将结论及 R8 判读标准交给主 agent。
  title: Fable Max 只读审计与 R6/R8 判据
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4cf9bc1c12f435ffa7d5e8c92e2e1ebab938fb4ae13a2cb9139e1f5c033c5dba
  kind: experience
  material_digest: 9225314ca4aaac3be71bd967479774776788acbea62e27647771e76835783c66
  revision: ee40c2fb499ff209178ce288b4a73c8f2cfad499b94cc609447e9ae86ab341a8
  schema: mindie-entry/3
  summary: 本批次保留了 Cursor/Fable Max 审计、DP 批次差异及 collective/host-device 时序假设。关键未决点是是否存在可证明的 CPU wait 移除导致 collective ordinal 失配的控制流链条；相关性尚未升级为已验证根因，代码和服务也未被修改。
  title: 当前任务导航：分布式批次失步的只读审计
navigation: 本批次保留了 Cursor/Fable Max 审计、DP 批次差异及 collective/host-device 时序假设。关键未决点是是否存在可证明的 CPU wait 移除导致 collective ordinal 失配的控制流链条；相关性尚未升级为已验证根因，代码和服务也未被修改。
schema: mindie-material-task/1
status: complete
task_id: 4cf9bc1c12f435ffa7d5e8c92e2e1ebab938fb4ae13a2cb9139e1f5c033c5dba
---

# 当前任务导航：分布式批次失步的只读审计

本批次保留了 Cursor/Fable Max 审计、DP 批次差异及 collective/host-device 时序假设。关键未决点是是否存在可证明的 CPU wait 移除导致 collective ordinal 失配的控制流链条；相关性尚未升级为已验证根因，代码和服务也未被修改。

## Materials

- [Fable Max 只读审计与 R6/R8 判据](blocks/e4a2ae989e6404f1b0cd26ab0b9bda47c81dbc2a7e1b03f994d34105ceb6ec28.md): 记录了使用 Cursor CLI 的 `claude-fable-5-max` 对当前工作树及上游 v0.26.0 进行只读代码审计；现场观察到 DP0–2 为 4 个 cached 批次、DP3 仍为 16 个 new，作为区分 row 复用问题与跨 DP 波次失步的判据。Fable 初步将去除 CPU wait、更深流水与 collective ordinal 失配相关联，但该因果链缺少完整控制流证据，需补充 functional diff；因此尚未验证根因，也未将 balance scheduling 视为修复或 A/B 结果。最终记录称未修改代码、未操作服务，并已将结论及 R8 判读标准交给主 agent。

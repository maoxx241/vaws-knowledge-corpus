---
blocks:
- block_id: 497e842bfe5ae1f474540599b0b3d335e5a3402efa570f4630a23fa6149bbdca
  indexed: true
  sha256: 70ea2e070aebf617630cb14437449c84a789a05f15604a36ad7bb5f211877d1c
  source_range:
    end: 8993477
    part: 0
    start: 0
  summary: 材料记录了 VAWS 原生客户端 session/worktree 生命周期接入，以及 Claude、Grok、Kimi 的适配边界：Claude 尚需完成适配，Grok 的准备回调仍待确认，Kimi 曾被判断存在回调能力缺口；随后又声称 Kimi Fork 已实现并通过新建、继续、TUI 恢复、`/fork`、`/new`、身份关联和无参 MCP 验收，但真实 GUI 验收、部分 shell context 等早期缺口曾明确未完成。后续材料进一步记录了 Kimi 相关版本与本地 MCP 配置迁移修复，迁移尚未应用本机配置，仅迁移精确 VAWS 标记、`.venv` 和标准模块 provider，保留自定义 provider、环境变量及原生字段；回归报告为 70 项通过、4 项平台跳过，预览识别出三个可更新 provider，均属于报告结果而非独立核验。
  title: VAWS 客户端接入、Kimi 验收与 MCP 迁移
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9ce0819643cad50c49c5ca626dcf0636e63ac0a97a3687391bdce0e547048e12
  kind: experience
  material_digest: 1b6615bd12dc4dea08a70f927933171efa6cc939523183d3120fa29f1e3522c1
  revision: c0831d1650c7d0a138c016715fae4af00ecf77f54593db14254ce366470c4968
  schema: mindie-entry/3
  summary: 当前材料覆盖 VAWS session/worktree、客户端适配能力边界、Kimi Fork 客户端的 reported 实现与验收，以及旧版 MCP 配置的选择性迁移。后续检索应区分“已报告通过”与“尚未完成/未实机验证”：Kimi 结果与 70/4 回归数据来自材料中的最终陈述，而 MCP 迁移明确尚未应用本机；早先关于 Claude、Grok、shell context 和真实 GUI 的缺口及后续修正仍应保留，原始块可独立搜索。
  title: VAWS 多客户端接入、Kimi 验收与配置迁移索引
navigation: 当前材料覆盖 VAWS session/worktree、客户端适配能力边界、Kimi Fork 客户端的 reported 实现与验收，以及旧版 MCP 配置的选择性迁移。后续检索应区分“已报告通过”与“尚未完成/未实机验证”：Kimi 结果与 70/4 回归数据来自材料中的最终陈述，而 MCP 迁移明确尚未应用本机；早先关于 Claude、Grok、shell context 和真实 GUI 的缺口及后续修正仍应保留，原始块可独立搜索。
schema: mindie-material-task/1
status: complete
task_id: 9ce0819643cad50c49c5ca626dcf0636e63ac0a97a3687391bdce0e547048e12
---

# VAWS 多客户端接入、Kimi 验收与配置迁移索引

当前材料覆盖 VAWS session/worktree、客户端适配能力边界、Kimi Fork 客户端的 reported 实现与验收，以及旧版 MCP 配置的选择性迁移。后续检索应区分“已报告通过”与“尚未完成/未实机验证”：Kimi 结果与 70/4 回归数据来自材料中的最终陈述，而 MCP 迁移明确尚未应用本机；早先关于 Claude、Grok、shell context 和真实 GUI 的缺口及后续修正仍应保留，原始块可独立搜索。

## Materials

- [VAWS 客户端接入、Kimi 验收与 MCP 迁移](blocks/497e842bfe5ae1f474540599b0b3d335e5a3402efa570f4630a23fa6149bbdca.md): 材料记录了 VAWS 原生客户端 session/worktree 生命周期接入，以及 Claude、Grok、Kimi 的适配边界：Claude 尚需完成适配，Grok 的准备回调仍待确认，Kimi 曾被判断存在回调能力缺口；随后又声称 Kimi Fork 已实现并通过新建、继续、TUI 恢复、`/fork`、`/new`、身份关联和无参 MCP 验收，但真实 GUI 验收、部分 shell context 等早期缺口曾明确未完成。后续材料进一步记录了 Kimi 相关版本与本地 MCP 配置迁移修复，迁移尚未应用本机配置，仅迁移精确 VAWS 标记、`.venv` 和标准模块 provider，保留自定义 provider、环境变量及原生字段；回归报告为 70 项通过、4 项平台跳过，预览识别出三个可更新 provider，均属于报告结果而非独立核验。

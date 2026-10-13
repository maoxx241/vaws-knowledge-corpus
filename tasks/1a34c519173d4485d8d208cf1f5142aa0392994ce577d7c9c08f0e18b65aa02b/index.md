---
blocks:
- block_id: 257d4bf0782ff2ec2f9bbe22aca00fab8fc6b54a7e31cf4f3d7de66b7afe8918
  indexed: true
  sha256: ae8b981ca057be3367c0ab1ca70801818a292b1ca8ed0f25d822efffe3f2a23a
  source_range:
    end: 16796757
    part: 0
    start: 0
  summary: 记录了 Claude、Grok、Kimi、Cursor 与 Codex 的原生 worktree、Fork、MCP registry、context 注入和 SessionStart hook 适配。部分实现和回归已通过，但 Kimi Windows 路径、客户端能力缺口、真实 GUI 验收、环境选择以及 Fork 暂停与文件复制顺序等仍存在未完成或仅部分确认的事项；多处结果明确标注为只读检查、假设或等待整合。后续 Codex 固定用户级 hook 已实现并通过 19 项测试，但未修改用户信任、配置或会话身份。
  title: 多客户端 Worktree、MCP 与 Codex Hook 适配进展
- block_id: a51b7aa960c4d365d2b3a0ee3ba7dac03742798d45e88edd6c037d75c6052969
  indexed: true
  sha256: f8a2ac16cfd9ce446d3c351950c75fc05702acac20a7c327c69892a7543c71a8
  source_range:
    end: 16796757
    part: 1
    start: 0
  summary: 确认了 `config/mcpServer/reload`、`thread/archive`、`thread/unarchive` 和 `thread/unsubscribe` 的公开参数及宽限行为，但归档成功不能证明 MCP 刷新或 SessionStart 执行。新会话已观察到首条用户消息前自动注入 VAWS context，不过日志未证明环境已选择；Codex 的 Local/Worktree 偏好按主机与 cwd 记忆，公开接口只能逐次指定。进一步发现默认 Worktree 可能从滞后的本地 `main`/`origin/main` 创建，环境可自动发现但不会自动选中，当前材料最后停留在核对 Claude `--worktree` 与 `--resume` 参数组合。
  title: App-server 会话接口与 Codex 默认 Worktree 行为核查
- block_id: dce8bc7f19c1c4881827082549f02682c9242160ebb4032ec33e5f0d45b11cec
  indexed: true
  sha256: a29662f54f28df27a8d20bff9d76dfdc9667796238e19581d23832297ff84053
  source_range:
    end: 17900086
    part: 0
    start: 16796757
  summary: 源码观察显示，Claude 传入 `--worktree` 时会先创建并切换工作树，随后恢复会话；已有 worktree binding 可能导致旧工作树不恢复，因此不应无条件包装普通启动命令。针对新建会话的实测确认原生 cwd 切换成功（HEAD `4a1f28e`、目录干净），但无参 `vaws_session` 仍指向主目录，MCP 在切换前启动且未设置 `VAWS_ENV_RECEIPT`。原因定位为 coordinator 的 `PreToolUse` 未刷新变化后的 cwd；未扩展修复，测试工作树被保留并使主目录出现未追踪的 `.claude/worktrees/`。
  title: Claude Worktree 恢复语义与 VAWS 绑定验收
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1a34c519173d4485d8d208cf1f5142aa0392994ce577d7c9c08f0e18b65aa02b
  kind: experience
  material_digest: 54bef02d21ab37c0bf43446c436cf84c456c8e674561d363ac5d659af97da09d
  revision: a959547d4584b37b650cdc593995aef9ef4a47089f6ff14f886b2b8335a89543
  schema: mindie-entry/3
  summary: 当前材料表明，客户端原生 Worktree 创建可能成功，但 cwd、VAWS 默认源码、MCP 启动时机和环境凭据不会必然同步；Claude 的 `--worktree` 与 `--resume` 组合存在恢复语义风险，不应无条件注入。已记录源码分析、只读观察和一次新建会话验收，但这些结果不代表完整适配：`PreToolUse` 刷新问题未修复，Codex 环境选择与自动初始化边界仍在，测试残留工作树和未设置 `VAWS_ENV_RECEIPT` 需保留为上下文。原始消息块仍需独立检索，不能把建议、计划或报告中的检查升级为普遍成功。
  title: 多客户端会话、Worktree 与 VAWS 生命周期适配索引
navigation: 当前材料表明，客户端原生 Worktree 创建可能成功，但 cwd、VAWS 默认源码、MCP 启动时机和环境凭据不会必然同步；Claude 的 `--worktree` 与 `--resume` 组合存在恢复语义风险，不应无条件注入。已记录源码分析、只读观察和一次新建会话验收，但这些结果不代表完整适配：`PreToolUse` 刷新问题未修复，Codex 环境选择与自动初始化边界仍在，测试残留工作树和未设置 `VAWS_ENV_RECEIPT` 需保留为上下文。原始消息块仍需独立检索，不能把建议、计划或报告中的检查升级为普遍成功。
schema: mindie-material-task/1
status: complete
task_id: 1a34c519173d4485d8d208cf1f5142aa0392994ce577d7c9c08f0e18b65aa02b
---

# 多客户端会话、Worktree 与 VAWS 生命周期适配索引

当前材料表明，客户端原生 Worktree 创建可能成功，但 cwd、VAWS 默认源码、MCP 启动时机和环境凭据不会必然同步；Claude 的 `--worktree` 与 `--resume` 组合存在恢复语义风险，不应无条件注入。已记录源码分析、只读观察和一次新建会话验收，但这些结果不代表完整适配：`PreToolUse` 刷新问题未修复，Codex 环境选择与自动初始化边界仍在，测试残留工作树和未设置 `VAWS_ENV_RECEIPT` 需保留为上下文。原始消息块仍需独立检索，不能把建议、计划或报告中的检查升级为普遍成功。

## Materials

- [多客户端 Worktree、MCP 与 Codex Hook 适配进展](blocks/257d4bf0782ff2ec2f9bbe22aca00fab8fc6b54a7e31cf4f3d7de66b7afe8918.md): 记录了 Claude、Grok、Kimi、Cursor 与 Codex 的原生 worktree、Fork、MCP registry、context 注入和 SessionStart hook 适配。部分实现和回归已通过，但 Kimi Windows 路径、客户端能力缺口、真实 GUI 验收、环境选择以及 Fork 暂停与文件复制顺序等仍存在未完成或仅部分确认的事项；多处结果明确标注为只读检查、假设或等待整合。后续 Codex 固定用户级 hook 已实现并通过 19 项测试，但未修改用户信任、配置或会话身份。
- [App-server 会话接口与 Codex 默认 Worktree 行为核查](blocks/a51b7aa960c4d365d2b3a0ee3ba7dac03742798d45e88edd6c037d75c6052969.md): 确认了 `config/mcpServer/reload`、`thread/archive`、`thread/unarchive` 和 `thread/unsubscribe` 的公开参数及宽限行为，但归档成功不能证明 MCP 刷新或 SessionStart 执行。新会话已观察到首条用户消息前自动注入 VAWS context，不过日志未证明环境已选择；Codex 的 Local/Worktree 偏好按主机与 cwd 记忆，公开接口只能逐次指定。进一步发现默认 Worktree 可能从滞后的本地 `main`/`origin/main` 创建，环境可自动发现但不会自动选中，当前材料最后停留在核对 Claude `--worktree` 与 `--resume` 参数组合。
- [Claude Worktree 恢复语义与 VAWS 绑定验收](blocks/dce8bc7f19c1c4881827082549f02682c9242160ebb4032ec33e5f0d45b11cec.md): 源码观察显示，Claude 传入 `--worktree` 时会先创建并切换工作树，随后恢复会话；已有 worktree binding 可能导致旧工作树不恢复，因此不应无条件包装普通启动命令。针对新建会话的实测确认原生 cwd 切换成功（HEAD `4a1f28e`、目录干净），但无参 `vaws_session` 仍指向主目录，MCP 在切换前启动且未设置 `VAWS_ENV_RECEIPT`。原因定位为 coordinator 的 `PreToolUse` 未刷新变化后的 cwd；未扩展修复，测试工作树被保留并使主目录出现未追踪的 `.claude/worktrees/`。

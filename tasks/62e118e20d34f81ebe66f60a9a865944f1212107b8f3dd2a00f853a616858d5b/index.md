---
blocks:
- block_id: 5950af78c31a4f0ea949bd9405125edf889c9c9fe8ce58a3ac02de5526c065f3
  indexed: true
  sha256: 7c6d031adb8c8d6535800dfb42620481990610c521d93c3f0743ccfa2f08331e
  source_range:
    end: 16777529
    part: 0
    start: 0
  summary: 核实显示，Kimi 的 Bash 支持 `cwd`，Write/Edit 可用跨 workspace 的绝对路径，显式 VAWS sources 可持久绑定；但原生 session 根目录、MCP cwd 和 hook cwd 不会因提示或后续 `cd` 重绑定。报告提出一次 prepare/绑定 sources 后由薄 Skill 或 `AGENTS.md` 引导工作，并保留 resume 沿用原目录、Grok SessionStart 不会提供首条提示等边界。后续实现侧称已统一五客户端启动引导、MCP gateway 路由和旧配置迁移，144 项定向回归通过，但真实客户端最终验收仍未在该阶段完成。
  title: 官方客户端工作目录与启动引导方案
- block_id: fee0563756fdc2411a4621c0f5593cd0081242857036143ce80a27640b00b334
  indexed: true
  sha256: a73d1f3895d035b41ae05ed2b67c6476717ac76d4a1541ab65ac5d0ef48276e1
  source_range:
    end: 16777529
    part: 1
    start: 0
  summary: 补充指出个人 Kimi 补丁曾通过 `KIMI_SESSION_ID`、`KIMI_AGENT_ID` 及 MCP request 字段透传 native identity，官方客户端并不自动提供这些能力；长活 ACP server 从主仓启动时，hook 仍可能回退到旧 bootstrap cwd。可行的低成本方案是 prepare 后显式绑定 worktree sources，Bash 使用 `cwd`、文件工具使用绝对路径、MCP 携带 `context_file`；若要求原生 cwd、项目配置和 MCP 全部一致，仍需 `prepare → chdir → exec` 或宿主在 `session/new` 传 cwd。
  title: Kimi 身份透传与非原生 worktree 方案边界
- block_id: 0f05ac31776a0a245e09dea8eb8dfe4be2d70a7fd851093fe83e4186ed993200
  indexed: true
  sha256: 4e7a6f90bda8dae6c7e645f834c5f6451acee72db3bfc00d33c5c1ba0d3a55a5
  source_range:
    end: 16777529
    part: 2
    start: 0
  summary: 官方 Grok 最终实测首个动作自动准备独立工作目录和固定环境，笔记未写入母仓，三个 MCP 后端及 Stop 候选知识保存成功；但两次只读 shell 漏切目录后才自行纠正，准备还经历并发锁等待，因此不能抹去这些限制。此前官方基线曾因启动指引后执行无关检查而失败，修复颜色化 GitHub JSON 解析后才改善；同时 Claude/Grok/Cursor 旧 matcher 迁移已验证，143 项测试通过。规范检查仍发现知识重试配方无条件重配客户端且缺少 `--client`，以及性能参考强制测量两种 grid 的可收窄指引，均未改文件。
  title: Grok 最终验收、配置迁移与残留指引问题
- block_id: d63825b4552a9d0f936f17366063c0ada9b03ab8fb974aa8d0760055dff89be7
  indexed: true
  sha256: f6ced8182a86b5f811a60a9ccf30e116ac8f9cfaab0576e23d4dfaae38cc3867
  source_range:
    end: 17030688
    part: 0
    start: 16777529
  summary: 对 ascend-triton-workflow 与 vllm-ascend-change-validation 的分析认为，两者主要聚合 manifest、artifact、diff、revision 和证据，不应代表普遍开发流程，也不应承担完成判定。建议保留脚本及一致性检查能力，仅在用户明确要求正式归档或汇总报告时使用；普通开发不应为其补造配置、实验或阶段报告。已观察到入口描述范围过宽、报告可能输出 inconclusive，但脚本不强制 config/manifest，--evidence 可省略；相关判断为建议而非已验证改动，且本批未修改文件或运行测试。
  title: 两项 Skill 应降为可选报告工具
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 62e118e20d34f81ebe66f60a9a865944f1212107b8f3dd2a00f853a616858d5b
  kind: experience
  material_digest: 65775df51025f7891bab0f45c10ccf78cf79db1eff910086338e28f522c267d6
  revision: 9ac34a2f6708999f13f8028ef0ab00a992e4c7ee8e8e7d7671632a7d6f0a1a01
  schema: mindie-entry/3
  summary: 任务重点仍是 prepare 后显式绑定 worktree，并通过共享 AGENTS.md/Skill 引导；Kimi native identity、ACP hook cwd 和 MCP context 仍是未完全覆盖的边界。已有证据支持独立目录、固定环境和三类 MCP 成功，但两次 shell 漏切目录、锁等待及早期基线失败仍需保留，配置迁移和测试大多通过，规范中仍有两处待收窄指引。新增结论是 ascend-triton-workflow 与 vllm-ascend-change-validation 应作为可选报告工具，而非必经 Skill 或完成门槛；需保留 manifest/artifact/revision/diff 证据整理、inconclusive 和缺失证据状态，不把报告聚合结果提升为验证成功，也不因普通开发任务补造实验或报告。
  title: 当前任务导航：显式 worktree 绑定与可选证据报告
navigation: 任务重点仍是 prepare 后显式绑定 worktree，并通过共享 AGENTS.md/Skill 引导；Kimi native identity、ACP hook cwd 和 MCP context 仍是未完全覆盖的边界。已有证据支持独立目录、固定环境和三类 MCP 成功，但两次 shell 漏切目录、锁等待及早期基线失败仍需保留，配置迁移和测试大多通过，规范中仍有两处待收窄指引。新增结论是 ascend-triton-workflow 与 vllm-ascend-change-validation 应作为可选报告工具，而非必经 Skill 或完成门槛；需保留 manifest/artifact/revision/diff 证据整理、inconclusive 和缺失证据状态，不把报告聚合结果提升为验证成功，也不因普通开发任务补造实验或报告。
schema: mindie-material-task/1
status: complete
task_id: 62e118e20d34f81ebe66f60a9a865944f1212107b8f3dd2a00f853a616858d5b
---

# 当前任务导航：显式 worktree 绑定与可选证据报告

任务重点仍是 prepare 后显式绑定 worktree，并通过共享 AGENTS.md/Skill 引导；Kimi native identity、ACP hook cwd 和 MCP context 仍是未完全覆盖的边界。已有证据支持独立目录、固定环境和三类 MCP 成功，但两次 shell 漏切目录、锁等待及早期基线失败仍需保留，配置迁移和测试大多通过，规范中仍有两处待收窄指引。新增结论是 ascend-triton-workflow 与 vllm-ascend-change-validation 应作为可选报告工具，而非必经 Skill 或完成门槛；需保留 manifest/artifact/revision/diff 证据整理、inconclusive 和缺失证据状态，不把报告聚合结果提升为验证成功，也不因普通开发任务补造实验或报告。

## Materials

- [官方客户端工作目录与启动引导方案](blocks/5950af78c31a4f0ea949bd9405125edf889c9c9fe8ce58a3ac02de5526c065f3.md): 核实显示，Kimi 的 Bash 支持 `cwd`，Write/Edit 可用跨 workspace 的绝对路径，显式 VAWS sources 可持久绑定；但原生 session 根目录、MCP cwd 和 hook cwd 不会因提示或后续 `cd` 重绑定。报告提出一次 prepare/绑定 sources 后由薄 Skill 或 `AGENTS.md` 引导工作，并保留 resume 沿用原目录、Grok SessionStart 不会提供首条提示等边界。后续实现侧称已统一五客户端启动引导、MCP gateway 路由和旧配置迁移，144 项定向回归通过，但真实客户端最终验收仍未在该阶段完成。
- [Kimi 身份透传与非原生 worktree 方案边界](blocks/fee0563756fdc2411a4621c0f5593cd0081242857036143ce80a27640b00b334.md): 补充指出个人 Kimi 补丁曾通过 `KIMI_SESSION_ID`、`KIMI_AGENT_ID` 及 MCP request 字段透传 native identity，官方客户端并不自动提供这些能力；长活 ACP server 从主仓启动时，hook 仍可能回退到旧 bootstrap cwd。可行的低成本方案是 prepare 后显式绑定 worktree sources，Bash 使用 `cwd`、文件工具使用绝对路径、MCP 携带 `context_file`；若要求原生 cwd、项目配置和 MCP 全部一致，仍需 `prepare → chdir → exec` 或宿主在 `session/new` 传 cwd。
- [Grok 最终验收、配置迁移与残留指引问题](blocks/0f05ac31776a0a245e09dea8eb8dfe4be2d70a7fd851093fe83e4186ed993200.md): 官方 Grok 最终实测首个动作自动准备独立工作目录和固定环境，笔记未写入母仓，三个 MCP 后端及 Stop 候选知识保存成功；但两次只读 shell 漏切目录后才自行纠正，准备还经历并发锁等待，因此不能抹去这些限制。此前官方基线曾因启动指引后执行无关检查而失败，修复颜色化 GitHub JSON 解析后才改善；同时 Claude/Grok/Cursor 旧 matcher 迁移已验证，143 项测试通过。规范检查仍发现知识重试配方无条件重配客户端且缺少 `--client`，以及性能参考强制测量两种 grid 的可收窄指引，均未改文件。
- [两项 Skill 应降为可选报告工具](blocks/d63825b4552a9d0f936f17366063c0ada9b03ab8fb974aa8d0760055dff89be7.md): 对 ascend-triton-workflow 与 vllm-ascend-change-validation 的分析认为，两者主要聚合 manifest、artifact、diff、revision 和证据，不应代表普遍开发流程，也不应承担完成判定。建议保留脚本及一致性检查能力，仅在用户明确要求正式归档或汇总报告时使用；普通开发不应为其补造配置、实验或阶段报告。已观察到入口描述范围过宽、报告可能输出 inconclusive，但脚本不强制 config/manifest，--evidence 可省略；相关判断为建议而非已验证改动，且本批未修改文件或运行测试。

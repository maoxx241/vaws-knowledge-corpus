---
blocks:
- block_id: 2a667519c238bb24b5c602ca3fa32ac5585b76adce0ed9a4024cf99e6205a503
  indexed: true
  sha256: 1e2ec23e2511ced2c3a4382219e167942c1877fddedc4007b4bb50cca1143be7
  source_range:
    end: 2122729
    part: 0
    start: 0
  summary: 先前已推送 CLI 入口方案，但后续只读调查认为应优先接入 Cursor 原生 worktree setup 与 Claude WorktreeCreate；Cursor 的 sessionStart 是 fire-and-forget，setup 失败会保留旧版本继续创建 Agent，普通 local session 也没有确认的强制 worktree 契约。随后完成 Codex/Cursor 原生 setup 配置接线及 worktree 初始化修复，保留旧环境、gitlink、dirty 状态和 resume 语义，并修复 VAWS 回调先于用户 setup 执行且幂等去重；报告称 43 项和 23 项测试通过，但未进行网络或硬件测试，部分版本/feature flag 行为仍需实际验收。
  title: 原生 Worktree/Session 接入与配置回调修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: dcadd6e236a9bebe81630d4ae149f7a2e1905b8094e7fa4026613331160c8646
  kind: experience
  material_digest: 650d1a38c159481761e598b84d4248d42ffaf81a93780b8a36862bd25abe49a8
  revision: 1cfb0dcdec042fa0368e41be4b9d2bc04dbb109e8d65e7204f083d987329cbcd
  schema: mindie-entry/3
  summary: 上下文从 CLI 启动入口转向各 Agent 工具创建原生 session/worktree 时自动准备版本、依赖和 VAWS 关联。已记录 Cursor/Claude 的接入边界、异步 setup、版本限制、环境收据选择、普通 local session 不应隐式改路径，以及配置回调顺序和幂等修复；此前方案与后续调查存在方向修正，测试结果和“完成”均为报告内容，不等同于全面验证，网络、硬件及部分客户端实际可见性仍未确认。
  title: 当前任务导航：原生 Session/Worktree 更新接入
navigation: 上下文从 CLI 启动入口转向各 Agent 工具创建原生 session/worktree 时自动准备版本、依赖和 VAWS 关联。已记录 Cursor/Claude 的接入边界、异步 setup、版本限制、环境收据选择、普通 local session 不应隐式改路径，以及配置回调顺序和幂等修复；此前方案与后续调查存在方向修正，测试结果和“完成”均为报告内容，不等同于全面验证，网络、硬件及部分客户端实际可见性仍未确认。
schema: mindie-material-task/1
status: complete
task_id: dcadd6e236a9bebe81630d4ae149f7a2e1905b8094e7fa4026613331160c8646
---

# 当前任务导航：原生 Session/Worktree 更新接入

上下文从 CLI 启动入口转向各 Agent 工具创建原生 session/worktree 时自动准备版本、依赖和 VAWS 关联。已记录 Cursor/Claude 的接入边界、异步 setup、版本限制、环境收据选择、普通 local session 不应隐式改路径，以及配置回调顺序和幂等修复；此前方案与后续调查存在方向修正，测试结果和“完成”均为报告内容，不等同于全面验证，网络、硬件及部分客户端实际可见性仍未确认。

## Materials

- [原生 Worktree/Session 接入与配置回调修复](blocks/2a667519c238bb24b5c602ca3fa32ac5585b76adce0ed9a4024cf99e6205a503.md): 先前已推送 CLI 入口方案，但后续只读调查认为应优先接入 Cursor 原生 worktree setup 与 Claude WorktreeCreate；Cursor 的 sessionStart 是 fire-and-forget，setup 失败会保留旧版本继续创建 Agent，普通 local session 也没有确认的强制 worktree 契约。随后完成 Codex/Cursor 原生 setup 配置接线及 worktree 初始化修复，保留旧环境、gitlink、dirty 状态和 resume 语义，并修复 VAWS 回调先于用户 setup 执行且幂等去重；报告称 43 项和 23 项测试通过，但未进行网络或硬件测试，部分版本/feature flag 行为仍需实际验收。

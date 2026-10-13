---
blocks:
- block_id: 6f1285af9da0d4045a7a7fe382a0e58c03ddb9e4ab5b4eb42aa8c462f758dd62
  indexed: true
  sha256: 42ca682b5e7337ead3cbe1b5aaa0f5c5f5831facfefa860ac153ef165acb5a73
  source_range:
    end: 6394028
    part: 0
    start: 0
  summary: 记录了原生客户端 session/worktree 生命周期、VAWS hook、任务上下文与多 matcher 配置的审计和修复；包括 Kimi launcher 优先级、PreToolUse 范围、current-inputs bootstrap、Trae 接线缺失，以及监控脚本和报告类 skill 的迁移。多项测试报告为通过，但真实 GUI 新会话、部分客户端完整原生 worktree 闭环及真实 setup 仍未验收，且曾发现并修复 hook 去重丢失 matcher 条件、Claude 混合组条件遗漏等问题。相关结论包含未提交或仅只读复查的状态，不能将历史报告的验证结果扩大为所有客户端或生产环境成功。
  title: VAWS 客户端接线、Skill 迁移与配置修复索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c62c9317c0ea021aa035b9ef9f200279a2661f13493b873d985adc1e57873ad3
  kind: experience
  material_digest: 3d552c89ba537bfdbb7b8bd564db5c0042bdd151dcd793a671e0f637c380e11a
  revision: 63e56b72fca893ef93cfbff04e3b71344e14d79d3e6aca09d17afd7c642f9d15
  schema: mindie-entry/3
  summary: 本批仅包含一个综合材料块，原始内容涵盖 VAWS 多客户端接线、session/worktree 生命周期、Skill 与监控入口迁移、Trae 移除审计、测试修复及 hook matcher 回归修复。应优先检索：原生 GUI/真实 setup 尚未验收、Claude/Grok/Kimi 的闭环限制、MCP 与 shell 身份边界、current-inputs bootstrap 问题，以及已修复但需区分提交状态的多 matcher 去重和 Claude 条件保留问题；早先“复查通过”或测试通过均不代表所有真实客户端场景已验证。
  title: 当前任务导航
navigation: 本批仅包含一个综合材料块，原始内容涵盖 VAWS 多客户端接线、session/worktree 生命周期、Skill 与监控入口迁移、Trae 移除审计、测试修复及 hook matcher 回归修复。应优先检索：原生 GUI/真实 setup 尚未验收、Claude/Grok/Kimi 的闭环限制、MCP 与 shell 身份边界、current-inputs bootstrap 问题，以及已修复但需区分提交状态的多 matcher 去重和 Claude 条件保留问题；早先“复查通过”或测试通过均不代表所有真实客户端场景已验证。
schema: mindie-material-task/1
status: complete
task_id: c62c9317c0ea021aa035b9ef9f200279a2661f13493b873d985adc1e57873ad3
---

# 当前任务导航

本批仅包含一个综合材料块，原始内容涵盖 VAWS 多客户端接线、session/worktree 生命周期、Skill 与监控入口迁移、Trae 移除审计、测试修复及 hook matcher 回归修复。应优先检索：原生 GUI/真实 setup 尚未验收、Claude/Grok/Kimi 的闭环限制、MCP 与 shell 身份边界、current-inputs bootstrap 问题，以及已修复但需区分提交状态的多 matcher 去重和 Claude 条件保留问题；早先“复查通过”或测试通过均不代表所有真实客户端场景已验证。

## Materials

- [VAWS 客户端接线、Skill 迁移与配置修复索引](blocks/6f1285af9da0d4045a7a7fe382a0e58c03ddb9e4ab5b4eb42aa8c462f758dd62.md): 记录了原生客户端 session/worktree 生命周期、VAWS hook、任务上下文与多 matcher 配置的审计和修复；包括 Kimi launcher 优先级、PreToolUse 范围、current-inputs bootstrap、Trae 接线缺失，以及监控脚本和报告类 skill 的迁移。多项测试报告为通过，但真实 GUI 新会话、部分客户端完整原生 worktree 闭环及真实 setup 仍未验收，且曾发现并修复 hook 去重丢失 matcher 条件、Claude 混合组条件遗漏等问题。相关结论包含未提交或仅只读复查的状态，不能将历史报告的验证结果扩大为所有客户端或生产环境成功。

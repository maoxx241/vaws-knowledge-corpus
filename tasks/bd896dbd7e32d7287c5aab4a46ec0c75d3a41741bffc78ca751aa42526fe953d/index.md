---
blocks:
- block_id: 13c33d5b36dffb6dcdf36a9b4808dee52cc56cbe75ed7dffbe7f8504187ea673
  indexed: true
  sha256: 23307baee33df89d3a97ea8ea936a54a71e3c81858fd3e76032d14e86f7bc12c
  source_range:
    end: 1799595
    part: 0
    start: 0
  summary: 早期方案曾声称已推送个人 Fork、每 5 分钟跟随主仓并完成 153–156 实测，但报告明确未验证真实服务器协作、NPU 缓存验收及算子/推理数值；随后任务转向每个新 session 仅更新一次，以避免工作中途引入新代码。审计和后续修改移除了常驻 watcher、轮询参数及独立 watcher 锁，新增一次性 `prepare`，并以 35 项、后续 37 项更新器测试及 `git diff --check` 作为报告结果。审阅确认恢复目录应保留原环境 receipt 与配置、准备版本交接不循环、源码保护和 GUI hook 不触发更新；同时发现并修正了损坏身份文件可能阻断启动的问题，并指出 detached HEAD 副本再次创建新目录时仍会沿用业务来源，需从主工作区入口创建新会话才能跟随主仓。
  title: 会话启动时一次性更新与恢复环境固定
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bd896dbd7e32d7287c5aab4a46ec0c75d3a41741bffc78ca751aa42526fe953d
  kind: experience
  material_digest: 356932bf5cc0f642d5b3ea6e28de77b128cb9ad3b8f8c88b0ad154081517bdbb
  revision: aa9413ff783aa78add89194a84bb70487f11345bfdc4b2ff1677bdb35f54d8d5
  schema: mindie-entry/3
  summary: 当前任务围绕将主仓同步从常驻 5 分钟 watcher 改为新 session 创建前的一次性准备，并确保已有或恢复目录不被意外更新。重点检索词：`prepare`、`watcher`、`for_session`、`safe_inputs`、`read_receipt`、`state=reused`、detached HEAD、身份文件降级、Git 更新锁、GUI hook；相关结果包含已报告但范围受限的 153–156 实测，以及后续发现的恢复环境固定和启动容错修正。
  title: 会话启动更新与环境恢复索引
navigation: 当前任务围绕将主仓同步从常驻 5 分钟 watcher 改为新 session 创建前的一次性准备，并确保已有或恢复目录不被意外更新。重点检索词：`prepare`、`watcher`、`for_session`、`safe_inputs`、`read_receipt`、`state=reused`、detached HEAD、身份文件降级、Git 更新锁、GUI hook；相关结果包含已报告但范围受限的 153–156 实测，以及后续发现的恢复环境固定和启动容错修正。
schema: mindie-material-task/1
status: complete
task_id: bd896dbd7e32d7287c5aab4a46ec0c75d3a41741bffc78ca751aa42526fe953d
---

# 会话启动更新与环境恢复索引

当前任务围绕将主仓同步从常驻 5 分钟 watcher 改为新 session 创建前的一次性准备，并确保已有或恢复目录不被意外更新。重点检索词：`prepare`、`watcher`、`for_session`、`safe_inputs`、`read_receipt`、`state=reused`、detached HEAD、身份文件降级、Git 更新锁、GUI hook；相关结果包含已报告但范围受限的 153–156 实测，以及后续发现的恢复环境固定和启动容错修正。

## Materials

- [会话启动时一次性更新与恢复环境固定](blocks/13c33d5b36dffb6dcdf36a9b4808dee52cc56cbe75ed7dffbe7f8504187ea673.md): 早期方案曾声称已推送个人 Fork、每 5 分钟跟随主仓并完成 153–156 实测，但报告明确未验证真实服务器协作、NPU 缓存验收及算子/推理数值；随后任务转向每个新 session 仅更新一次，以避免工作中途引入新代码。审计和后续修改移除了常驻 watcher、轮询参数及独立 watcher 锁，新增一次性 `prepare`，并以 35 项、后续 37 项更新器测试及 `git diff --check` 作为报告结果。审阅确认恢复目录应保留原环境 receipt 与配置、准备版本交接不循环、源码保护和 GUI hook 不触发更新；同时发现并修正了损坏身份文件可能阻断启动的问题，并指出 detached HEAD 副本再次创建新目录时仍会沿用业务来源，需从主工作区入口创建新会话才能跟随主仓。

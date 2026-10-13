---
blocks:
- block_id: aa10ca66d082bf4af2e8c52abb67ba359c1c24ec5bd4219c42bd23b71a2ecfad
  indexed: true
  sha256: 714203bd4c9316d16017f2a80d7771a3d29aeb8b99158ef70acfcff97daeac2b
  source_range:
    end: 1056112
    part: 0
    start: 0
  summary: 记录了个人 Fork、自动更新、留言收取、编译缓存复用及 NPU 资源管理等实现与验收声称；其中 153–156 被报告为通过，但未完成算子数值或推理验证，且此前报告明确未验证真实服务器协作、NPU 缓存和完整更新传播。随后讨论将每 5 分钟 watcher 改为每个新 Session 创建目录前执行一次 upstream 准备，以避免工作期间引入新代码；已有目录和恢复会话应保留自身代码与环境，GUI 工作目录仍由原生客户端管理。最终据称更新了 9 个合同文档，删除周期 watcher 设计并保留 `check / prepare / apply`、`state.json`、`logs` 及脏源、业务分支和失败回退边界；仅报告 `git diff --check` 通过，未提交。
  title: 工作区更新策略从周期轮询转为会话级准备
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 929e54ae04ae831324056ebc4652cd8592b20b99f58e2c1ddc5e7c4a18ba2a65
  kind: experience
  material_digest: f7fcc87684f06fc79e1bf2a5d2f46c86cd263e165ef85fa6821ed10584649e12
  revision: a63d9f7873844793819bfdca6606499d160148a19c9d5c708dccaa5a817312c5
  schema: mindie-entry/3
  summary: 当前重点是将更新时机限定为新 CLI 编辑目录创建前的一次准备，避免周期 watcher 在工作中带入新代码；已有目录与恢复会话保持原状态，GUI 工作目录不由 Agent 强制创建。相关实现说明涉及 `check / prepare / apply`、环境 receipt、`state.json` 和 `logs`；已报告文档更新及格式检查通过，但不要将历史实测或方案描述扩展为尚未验证的完整成功。
  title: 当前任务导航：按会话准备工作区
navigation: 当前重点是将更新时机限定为新 CLI 编辑目录创建前的一次准备，避免周期 watcher 在工作中带入新代码；已有目录与恢复会话保持原状态，GUI 工作目录不由 Agent 强制创建。相关实现说明涉及 `check / prepare / apply`、环境 receipt、`state.json` 和 `logs`；已报告文档更新及格式检查通过，但不要将历史实测或方案描述扩展为尚未验证的完整成功。
schema: mindie-material-task/1
status: complete
task_id: 929e54ae04ae831324056ebc4652cd8592b20b99f58e2c1ddc5e7c4a18ba2a65
---

# 当前任务导航：按会话准备工作区

当前重点是将更新时机限定为新 CLI 编辑目录创建前的一次准备，避免周期 watcher 在工作中带入新代码；已有目录与恢复会话保持原状态，GUI 工作目录不由 Agent 强制创建。相关实现说明涉及 `check / prepare / apply`、环境 receipt、`state.json` 和 `logs`；已报告文档更新及格式检查通过，但不要将历史实测或方案描述扩展为尚未验证的完整成功。

## Materials

- [工作区更新策略从周期轮询转为会话级准备](blocks/aa10ca66d082bf4af2e8c52abb67ba359c1c24ec5bd4219c42bd23b71a2ecfad.md): 记录了个人 Fork、自动更新、留言收取、编译缓存复用及 NPU 资源管理等实现与验收声称；其中 153–156 被报告为通过，但未完成算子数值或推理验证，且此前报告明确未验证真实服务器协作、NPU 缓存和完整更新传播。随后讨论将每 5 分钟 watcher 改为每个新 Session 创建目录前执行一次 upstream 准备，以避免工作期间引入新代码；已有目录和恢复会话应保留自身代码与环境，GUI 工作目录仍由原生客户端管理。最终据称更新了 9 个合同文档，删除周期 watcher 设计并保留 `check / prepare / apply`、`state.json`、`logs` 及脏源、业务分支和失败回退边界；仅报告 `git diff --check` 通过，未提交。

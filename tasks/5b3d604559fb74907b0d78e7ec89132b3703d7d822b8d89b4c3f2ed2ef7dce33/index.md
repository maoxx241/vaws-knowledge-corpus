---
blocks:
- block_id: 13d0e43ce9929f88f7be8e44a6a2e6ead83a8dc4f79405338e76ed19a536a74c
  indexed: true
  sha256: e43dd3fcbb33b4771f56ec4d9f1411ccea0b6c2b2e41e5b1af6206861a186e45
  source_range:
    end: 5545622
    part: 0
    start: 0
  summary: 只读核验显示，官方 Kimi 顶层没有 `--cwd`/`--work-dir`，Bash 的 `cwd`、文件工具绝对路径和显式 sources 可支持在独立 worktree 编辑，但不会重绑定 native session root、MCP 或 hook cwd；可靠边界是创建会话前准备并传入 cwd，或由 launcher 先切目录再启动。后续建议采用共享 `AGENTS.md` 启动指引和现有 UserPromptSubmit 的 `context_file`，并注意官方 hook、identity 透传、用户 Stop 项保留及 malformed marker 边界。三个测试文件已更新，定向测试报告为 28 passed、`diff --check` 通过；实现文件未改、未运行真实 session 或模型。
  title: Kimi 工作目录切换、启动指引与测试迁移
- block_id: 8b8b8ea4ff85c35b24f52fd4d90d9fcd704f8bd794cf06ad2580e701567181ff
  indexed: true
  sha256: 2846c09fd413694e8af7f4e2fb437b58d4b03d8d6dabd0c989b7f65892db3b75
  source_range:
    end: 5545622
    part: 1
    start: 0
  summary: 三份 current 合同已补充统一启动、固定 runtime/receipt、共享知识状态、gateway 路由、编辑路径、resume 及 Windows/WSL 边界；这些更新未提交，检查通过，但没有宣称新客户端实测或跨版本并发兼容性。对验证 Skill 的判断是保留报告脚本作为可选工具，继续聚合 artifact、revision 和原始证据；应收窄为用户要求正式归档或汇总报告时使用，普通开发任务无需补造 config/manifest、实验或父 task。
  title: 固定运行环境合同与验证 Skill 范围
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5b3d604559fb74907b0d78e7ec89132b3703d7d822b8d89b4c3f2ed2ef7dce33
  kind: experience
  material_digest: faf189e9c9711f517f8b3e6645d5f1ca0782e7f1e532c0656154d6ed0ae563d1
  revision: 830c4cf67b107413a4c97ce4d3f9c5b2e118d474f1c5b6f56b670ac56e989b4a
  schema: mindie-entry/3
  summary: 主线已从“能否原生切换会话 cwd”收敛为：会话创建边界负责准备和 cwd，Skill/AGENTS.md 与现有 hook 负责启动提示，worktree 编辑依赖 Bash `cwd`、绝对路径和显式 sources。已有测试与合同更新属于未提交的本地工作，28 个定向测试通过；仍未验证真实 session、新客户端、跨版本并发兼容性或模型行为。后续修订应保留用户 hook/Stop、MCP identity 与长活 ACP hook cwd 等边界，并将正式验证报告工具限定为可选流程。
  title: 当前任务导航：Kimi 启动与固定环境合同
navigation: 主线已从“能否原生切换会话 cwd”收敛为：会话创建边界负责准备和 cwd，Skill/AGENTS.md 与现有 hook 负责启动提示，worktree 编辑依赖 Bash `cwd`、绝对路径和显式 sources。已有测试与合同更新属于未提交的本地工作，28 个定向测试通过；仍未验证真实 session、新客户端、跨版本并发兼容性或模型行为。后续修订应保留用户 hook/Stop、MCP identity 与长活 ACP hook cwd 等边界，并将正式验证报告工具限定为可选流程。
schema: mindie-material-task/1
status: complete
task_id: 5b3d604559fb74907b0d78e7ec89132b3703d7d822b8d89b4c3f2ed2ef7dce33
---

# 当前任务导航：Kimi 启动与固定环境合同

主线已从“能否原生切换会话 cwd”收敛为：会话创建边界负责准备和 cwd，Skill/AGENTS.md 与现有 hook 负责启动提示，worktree 编辑依赖 Bash `cwd`、绝对路径和显式 sources。已有测试与合同更新属于未提交的本地工作，28 个定向测试通过；仍未验证真实 session、新客户端、跨版本并发兼容性或模型行为。后续修订应保留用户 hook/Stop、MCP identity 与长活 ACP hook cwd 等边界，并将正式验证报告工具限定为可选流程。

## Materials

- [Kimi 工作目录切换、启动指引与测试迁移](blocks/13d0e43ce9929f88f7be8e44a6a2e6ead83a8dc4f79405338e76ed19a536a74c.md): 只读核验显示，官方 Kimi 顶层没有 `--cwd`/`--work-dir`，Bash 的 `cwd`、文件工具绝对路径和显式 sources 可支持在独立 worktree 编辑，但不会重绑定 native session root、MCP 或 hook cwd；可靠边界是创建会话前准备并传入 cwd，或由 launcher 先切目录再启动。后续建议采用共享 `AGENTS.md` 启动指引和现有 UserPromptSubmit 的 `context_file`，并注意官方 hook、identity 透传、用户 Stop 项保留及 malformed marker 边界。三个测试文件已更新，定向测试报告为 28 passed、`diff --check` 通过；实现文件未改、未运行真实 session 或模型。
- [固定运行环境合同与验证 Skill 范围](blocks/8b8b8ea4ff85c35b24f52fd4d90d9fcd704f8bd794cf06ad2580e701567181ff.md): 三份 current 合同已补充统一启动、固定 runtime/receipt、共享知识状态、gateway 路由、编辑路径、resume 及 Windows/WSL 边界；这些更新未提交，检查通过，但没有宣称新客户端实测或跨版本并发兼容性。对验证 Skill 的判断是保留报告脚本作为可选工具，继续聚合 artifact、revision 和原始证据；应收窄为用户要求正式归档或汇总报告时使用，普通开发任务无需补造 config/manifest、实验或父 task。

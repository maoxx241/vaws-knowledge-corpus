---
blocks:
- block_id: f206b1df2681d1716f1b42ac3c5ba3d7f0e89322f677066c87bb299c6008628e
  indexed: true
  sha256: 6cb6b488126dc56a17d1da2e71f9dc7a5bdb6b2338c9c7918ffd5b654f8df545
  source_range:
    end: 15148652
    part: 0
    start: 0
  summary: '对 PR #66 的首轮 review 发现 6 项问题：NPU 占用探测失败时未 fail-closed、benchmark native digest 门禁可绕过、pending 非 poll 操作会产生 host 副作用、profiling 的 MLA/Dense-MoE 参数估算错误，以及 PyYAML 依赖契约缺失；合并态 CI 还出现过期 Claude skill shim。随后这些问题被修复并补充回归测试，`370ebc7` 的 Python 3.12 测试与四项 GitHub CI 均通过，但未重新进行 NPU 硬件验证。'
  title: 'PR #66 首轮全量审查与六项修复'
- block_id: 81212ae22a64e8e18f7d14ab13fe20f957543ffcccc8df0277f83253bc43fa8b
  indexed: true
  sha256: 2f729866af8bfb31bae812bed185572efd2f1361bc9371a6e2a02a6d3a949b6c
  source_range:
    end: 15148652
    part: 1
    start: 0
  summary: 对新 head `45a25bf` 的复审复现了两个 P1：并发 stop 请求会被旧快照覆盖，截断远端 JSON 会被错误标为终态 `failed`，导致进程和 NPU lease 失去监督；随后以单调合并控制标志和可重试的异常分类修复，并通过 coordinator、remote-dev、session 及对抗回归测试。README 中日期、机器号、模型名和验收流水等一次性内容被删除，顶层中英文 README 与 main 一致，仅组件文档保留真实的 `--client kimi` 配置契约；最终 head `ddde3f3` CI 全绿、可合并，并 squash 合入为 `0c468446`，但未将旧硬件结果视为新 head 的验证证据。
  title: 'PR #66 监督器竞态修复、README 清理与合入'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b6566e3e6bcb5ab01c360762534cc381efec65fc05bb0f67159fba2c2600f37e
  kind: experience
  material_digest: 85d7dc9901cb55094b84e0f74cc0713f414e4a44d181b9e0c2bcb1adc10ba110
  revision: 430dfa71a583cfcb8af30fbba5d74c203be4d64f3259df5f23286a2bc7e00f21
  schema: mindie-entry/3
  summary: '任务经历了首轮全量 review、六项控制面与 profiling 修复、第二轮复审发现并修复两个监督器 P1 竞态/异常分类缺陷，以及 README 内容清理。最终 PR #66 在 `ddde3f3` 上四项 CI 全绿、合并树无冲突并已 squash 合入为 `0c468446`；顶层 README 未保留本次 PR 的时点性验收信息，组件 README 仅保留长期有效的 Kimi 客户端配置契约。硬件/NPU 验证边界仍需保留，旧 head 的硬件证据不代表最终 head。'
  title: 'PR #66：修复、复审、文档清理与合入完成'
navigation: '任务经历了首轮全量 review、六项控制面与 profiling 修复、第二轮复审发现并修复两个监督器 P1 竞态/异常分类缺陷，以及 README 内容清理。最终 PR #66 在 `ddde3f3` 上四项 CI 全绿、合并树无冲突并已 squash 合入为 `0c468446`；顶层 README 未保留本次 PR 的时点性验收信息，组件 README 仅保留长期有效的 Kimi 客户端配置契约。硬件/NPU 验证边界仍需保留，旧 head 的硬件证据不代表最终 head。'
schema: mindie-material-task/1
status: complete
task_id: b6566e3e6bcb5ab01c360762534cc381efec65fc05bb0f67159fba2c2600f37e
---

# PR #66：修复、复审、文档清理与合入完成

任务经历了首轮全量 review、六项控制面与 profiling 修复、第二轮复审发现并修复两个监督器 P1 竞态/异常分类缺陷，以及 README 内容清理。最终 PR #66 在 `ddde3f3` 上四项 CI 全绿、合并树无冲突并已 squash 合入为 `0c468446`；顶层 README 未保留本次 PR 的时点性验收信息，组件 README 仅保留长期有效的 Kimi 客户端配置契约。硬件/NPU 验证边界仍需保留，旧 head 的硬件证据不代表最终 head。

## Materials

- [PR #66 首轮全量审查与六项修复](blocks/f206b1df2681d1716f1b42ac3c5ba3d7f0e89322f677066c87bb299c6008628e.md): 对 PR #66 的首轮 review 发现 6 项问题：NPU 占用探测失败时未 fail-closed、benchmark native digest 门禁可绕过、pending 非 poll 操作会产生 host 副作用、profiling 的 MLA/Dense-MoE 参数估算错误，以及 PyYAML 依赖契约缺失；合并态 CI 还出现过期 Claude skill shim。随后这些问题被修复并补充回归测试，`370ebc7` 的 Python 3.12 测试与四项 GitHub CI 均通过，但未重新进行 NPU 硬件验证。
- [PR #66 监督器竞态修复、README 清理与合入](blocks/81212ae22a64e8e18f7d14ab13fe20f957543ffcccc8df0277f83253bc43fa8b.md): 对新 head `45a25bf` 的复审复现了两个 P1：并发 stop 请求会被旧快照覆盖，截断远端 JSON 会被错误标为终态 `failed`，导致进程和 NPU lease 失去监督；随后以单调合并控制标志和可重试的异常分类修复，并通过 coordinator、remote-dev、session 及对抗回归测试。README 中日期、机器号、模型名和验收流水等一次性内容被删除，顶层中英文 README 与 main 一致，仅组件文档保留真实的 `--client kimi` 配置契约；最终 head `ddde3f3` CI 全绿、可合并，并 squash 合入为 `0c468446`，但未将旧硬件结果视为新 head 的验证证据。

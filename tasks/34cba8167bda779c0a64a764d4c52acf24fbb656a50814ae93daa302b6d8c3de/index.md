---
blocks:
- block_id: 91e177ccac36ffbc05c82a133b158df08c7e0410f91f43257ed75380b5404a92
  indexed: true
  sha256: a8056fe9bc678d52831c5271968a8ebe2010e41fdef206630fe8ac7f9f6996cf
  source_range:
    end: 7992249
    part: 0
    start: 0
  summary: 真实 Claude Code/Grok CLI 会话验证了 VAWS session identity、resume/parent context 关联，以及 153 remote-dev 的 CPU 文件和命令读写；Grok 的 nested `use_tool` envelope 曾导致 `context_file` 注入错误层级并返回 `VAWS context is required`，随后通过仅注入内层 `tool_input` 修复并真实复测成功。随后完成 source worktree、catlass/googletest pinned HEAD 和未提交 probe 准备，并在 154 对 A 任务执行多次真实 NPU HTTP 验收；A、A2 及 supervised v1/v2 均按受控 stop/finish 回收，记录了 timeout 参数误用、submodule metadata 未接管等不完整或修正事项。
  title: Claude/Grok 会话与 A 任务生命周期验收
- block_id: bd50bc2012470efa0660382b2fe3f9117e62609c42681e2c54773c77ce1b3c49
  indexed: true
  sha256: 12c86c73a81fdf20242bdf4f3a886949e481f1cea9a027429e59b649bf61f53b
  source_range:
    end: 7992249
    part: 1
    start: 0
  summary: 只读 review 曾提出 `activate()` 配合 `completion_confirmed=True` 可能接受 supervisor 已消失的 guard，并据此暂缓启动；随后确认对 `process_guard_busy()` 返回值的理解读反，marker 消失时 `if not ...` 会正确拒绝 activation，该问题不构成阻断且未修改实现。最终 supervised-final-v1/v2 在 154 完成两次真实 HTTP 实算、daemon ancestry/status 核验，并分别通过 stop/finish 回收，lease 已释放、runtime 已返回、remote processes 为空；B/C、SSH 和源码未操作。
  title: supervisor guard 误判更正与最终受控回收
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 34cba8167bda779c0a64a764d4c52acf24fbb656a50814ae93daa302b6d8c3de
  kind: experience
  material_digest: eb503268c847ad3e42b59cf0cc8605e76139de50abdd2455ed5f5be80f43459c
  revision: 90e8c789c9651a002f763949707505264f7798183f59d0ff8679941996fb7438
  schema: mindie-entry/3
  summary: 材料涵盖 Claude/Grok native session 与 remote-dev 验收、Grok nested `use_tool` 的 `context_file` 修复，以及 154 上 A 任务的 worktree/probe 准备、v1/v2/v3 实算和受控生命周期。需保留的修正是 supervisor guard 阻断判断曾被误读后纠正；已报告的 timeout、submodule metadata、缺失知识库匹配及未运行 unittest 等限制仍属上下文，不能提升为已验证成功。
  title: 当前任务导航：会话修复、NPU 生命周期与 A 任务回收
navigation: 材料涵盖 Claude/Grok native session 与 remote-dev 验收、Grok nested `use_tool` 的 `context_file` 修复，以及 154 上 A 任务的 worktree/probe 准备、v1/v2/v3 实算和受控生命周期。需保留的修正是 supervisor guard 阻断判断曾被误读后纠正；已报告的 timeout、submodule metadata、缺失知识库匹配及未运行 unittest 等限制仍属上下文，不能提升为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 34cba8167bda779c0a64a764d4c52acf24fbb656a50814ae93daa302b6d8c3de
---

# 当前任务导航：会话修复、NPU 生命周期与 A 任务回收

材料涵盖 Claude/Grok native session 与 remote-dev 验收、Grok nested `use_tool` 的 `context_file` 修复，以及 154 上 A 任务的 worktree/probe 准备、v1/v2/v3 实算和受控生命周期。需保留的修正是 supervisor guard 阻断判断曾被误读后纠正；已报告的 timeout、submodule metadata、缺失知识库匹配及未运行 unittest 等限制仍属上下文，不能提升为已验证成功。

## Materials

- [Claude/Grok 会话与 A 任务生命周期验收](blocks/91e177ccac36ffbc05c82a133b158df08c7e0410f91f43257ed75380b5404a92.md): 真实 Claude Code/Grok CLI 会话验证了 VAWS session identity、resume/parent context 关联，以及 153 remote-dev 的 CPU 文件和命令读写；Grok 的 nested `use_tool` envelope 曾导致 `context_file` 注入错误层级并返回 `VAWS context is required`，随后通过仅注入内层 `tool_input` 修复并真实复测成功。随后完成 source worktree、catlass/googletest pinned HEAD 和未提交 probe 准备，并在 154 对 A 任务执行多次真实 NPU HTTP 验收；A、A2 及 supervised v1/v2 均按受控 stop/finish 回收，记录了 timeout 参数误用、submodule metadata 未接管等不完整或修正事项。
- [supervisor guard 误判更正与最终受控回收](blocks/bd50bc2012470efa0660382b2fe3f9117e62609c42681e2c54773c77ce1b3c49.md): 只读 review 曾提出 `activate()` 配合 `completion_confirmed=True` 可能接受 supervisor 已消失的 guard，并据此暂缓启动；随后确认对 `process_guard_busy()` 返回值的理解读反，marker 消失时 `if not ...` 会正确拒绝 activation，该问题不构成阻断且未修改实现。最终 supervised-final-v1/v2 在 154 完成两次真实 HTTP 实算、daemon ancestry/status 核验，并分别通过 stop/finish 回收，lease 已释放、runtime 已返回、remote processes 为空；B/C、SSH 和源码未操作。

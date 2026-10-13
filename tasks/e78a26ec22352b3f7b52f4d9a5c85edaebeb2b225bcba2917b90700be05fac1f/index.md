---
blocks:
- block_id: 8ef5814a8606e7853b9c4aa72c721b8bac76943d85f2a8a5cc4e4b7a5716d5ed
  indexed: true
  sha256: c22ed41cbbbea7e8ffee502814d6d10ca16468dac4768e8d69ce3f93747de512
  source_range:
    end: 626176
    part: 0
    start: 0
  summary: 只读分析报告称，四节点在下一轮真实多并发 replay 时同时首次出现 NaN；此前 dummy 输出均为 finite=False，且 dummy 使用 `tokens=16, reqs=2`、`num_prefills=2`，spec 字段多为 `None`，可能未覆盖 graph capture 绑定的持久 spec buffers。报告据相同 data pointer、未命中的 reset 条件及期间无真实 runtime 更新，推断 FULL graph 仍重放遗留 spec 分支，但明确日志未直接 dump dummy replay 当下的隐藏持久 buffers，因此该因果链仍是有证据边界的分析而非已验证案例。报告排除了“只有 padding 行导致”的修法方向，提出在 `_dummy_run` 的 FULL graph replay 前无条件重置 GDN builder 的持久 spec graph inputs；未修改文件、服务或进程。
  title: FULL graph dummy replay 与 NaN 因果分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e78a26ec22352b3f7b52f4d9a5c85edaebeb2b225bcba2917b90700be05fac1f
  kind: experience
  material_digest: def58bd8bc68d01547977e927bd58b0740c77e74e423acbd23b144a90bb783f3
  revision: 82d70ca38d88b830a8d898df3cdce4f3fe5cdb10d3fa508ef757a64ad37e1ab0
  schema: mindie-entry/3
  summary: 现有材料是一份只读、非验证性的因果分析：四节点同步出现 NaN，重点涉及 dummy metadata、持久 spec buffers、graph replay、padding 与 reset 条件。保留的关键不确定性是隐藏持久 buffers 未在 dummy replay 当下直接导出；建议方向是无条件重置持久 spec inputs，但尚未实施或验证。原始 block 可独立检索，未提供的更早上下文不作记忆性补充。
  title: 当前任务：检索 FULL graph dummy replay 的 NaN 分析
navigation: 现有材料是一份只读、非验证性的因果分析：四节点同步出现 NaN，重点涉及 dummy metadata、持久 spec buffers、graph replay、padding 与 reset 条件。保留的关键不确定性是隐藏持久 buffers 未在 dummy replay 当下直接导出；建议方向是无条件重置持久 spec inputs，但尚未实施或验证。原始 block 可独立检索，未提供的更早上下文不作记忆性补充。
schema: mindie-material-task/1
status: complete
task_id: e78a26ec22352b3f7b52f4d9a5c85edaebeb2b225bcba2917b90700be05fac1f
---

# 当前任务：检索 FULL graph dummy replay 的 NaN 分析

现有材料是一份只读、非验证性的因果分析：四节点同步出现 NaN，重点涉及 dummy metadata、持久 spec buffers、graph replay、padding 与 reset 条件。保留的关键不确定性是隐藏持久 buffers 未在 dummy replay 当下直接导出；建议方向是无条件重置持久 spec inputs，但尚未实施或验证。原始 block 可独立检索，未提供的更早上下文不作记忆性补充。

## Materials

- [FULL graph dummy replay 与 NaN 因果分析](blocks/8ef5814a8606e7853b9c4aa72c721b8bac76943d85f2a8a5cc4e4b7a5716d5ed.md): 只读分析报告称，四节点在下一轮真实多并发 replay 时同时首次出现 NaN；此前 dummy 输出均为 finite=False，且 dummy 使用 `tokens=16, reqs=2`、`num_prefills=2`，spec 字段多为 `None`，可能未覆盖 graph capture 绑定的持久 spec buffers。报告据相同 data pointer、未命中的 reset 条件及期间无真实 runtime 更新，推断 FULL graph 仍重放遗留 spec 分支，但明确日志未直接 dump dummy replay 当下的隐藏持久 buffers，因此该因果链仍是有证据边界的分析而非已验证案例。报告排除了“只有 padding 行导致”的修法方向，提出在 `_dummy_run` 的 FULL graph replay 前无条件重置 GDN builder 的持久 spec graph inputs；未修改文件、服务或进程。

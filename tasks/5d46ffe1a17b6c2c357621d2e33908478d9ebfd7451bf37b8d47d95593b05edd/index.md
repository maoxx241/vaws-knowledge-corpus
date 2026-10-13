---
blocks:
- block_id: b0d949fd9589ef969a8c3f0f55683e85e11e76c610ff9fbedd0c3465145d4d4b
  indexed: true
  sha256: b61ec07b29d2fba05f6ef4c4dfa08834812df1de9815bc5a2e61ba00c1c0737d
  source_range:
    end: 830955
    part: 0
    start: 0
  summary: 报错收敛为 KDA 核心输出 32 个 TP-local token，而通用 SequenceColumnParallelOp 令 full-rank `g_proj` all-gather 成 512 个 token，最终在 `o_norm` 触发 32 vs 512。修复将 sequence parallel 限定为 `output_size == num_attention_heads` 的 head-wise gate，full-rank gate 改走普通 ColumnParallel，并补充相关分派与测试覆盖。源码比对、`git diff --check` 和 `py_compile` 已通过；本机 pytest 因未安装 `torch` 未开始收集，仍需容器内补跑，远端未修改。
  title: KDA g_proj token-layout 修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5d46ffe1a17b6c2c357621d2e33908478d9ebfd7451bf37b8d47d95593b05edd
  kind: experience
  material_digest: 6d17b947cccac831dc3f3b8c440694193c357191c92ee893c48e22b6cbfa144a
  revision: d8ca6d52cb0ca2af8beaf337a5c85f817791c45476b372ed967e35915e861f66
  schema: mindie-entry/3
  summary: 当前任务聚焦 attention QKV/O、SequenceColumnParallelOp 与 `g_proj` 合约，未涉及 MoE/router/dispatch。已记录 32 vs 512 token 根因及最小修复：head-wise gate 保留 sequence parallel，full-rank gate 禁用；静态检查通过，但 pytest 尚未因缺少 `torch` 完成，容器内验证仍未完成。
  title: KDA g_proj token-layout 修复与待验证状态
navigation: 当前任务聚焦 attention QKV/O、SequenceColumnParallelOp 与 `g_proj` 合约，未涉及 MoE/router/dispatch。已记录 32 vs 512 token 根因及最小修复：head-wise gate 保留 sequence parallel，full-rank gate 禁用；静态检查通过，但 pytest 尚未因缺少 `torch` 完成，容器内验证仍未完成。
schema: mindie-material-task/1
status: complete
task_id: 5d46ffe1a17b6c2c357621d2e33908478d9ebfd7451bf37b8d47d95593b05edd
---

# KDA g_proj token-layout 修复与待验证状态

当前任务聚焦 attention QKV/O、SequenceColumnParallelOp 与 `g_proj` 合约，未涉及 MoE/router/dispatch。已记录 32 vs 512 token 根因及最小修复：head-wise gate 保留 sequence parallel，full-rank gate 禁用；静态检查通过，但 pytest 尚未因缺少 `torch` 完成，容器内验证仍未完成。

## Materials

- [KDA g_proj token-layout 修复](blocks/b0d949fd9589ef969a8c3f0f55683e85e11e76c610ff9fbedd0c3465145d4d4b.md): 报错收敛为 KDA 核心输出 32 个 TP-local token，而通用 SequenceColumnParallelOp 令 full-rank `g_proj` all-gather 成 512 个 token，最终在 `o_norm` 触发 32 vs 512。修复将 sequence parallel 限定为 `output_size == num_attention_heads` 的 head-wise gate，full-rank gate 改走普通 ColumnParallel，并补充相关分派与测试覆盖。源码比对、`git diff --check` 和 `py_compile` 已通过；本机 pytest 因未安装 `torch` 未开始收集，仍需容器内补跑，远端未修改。

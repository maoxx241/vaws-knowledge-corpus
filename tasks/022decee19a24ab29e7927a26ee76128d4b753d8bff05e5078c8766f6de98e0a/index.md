---
blocks:
- block_id: ec086be5b42e5d4372e3de56f2adf449748c1e62786936c2289cc891ab7cce92
  indexed: true
  sha256: b57ec41cf79dc94c26df2dfca8457407748d9deb85cf56e206338db6f676c279
  source_range:
    end: 262404
    part: 0
    start: 0
  summary: 只读监控一个 SGLang run：四个容器尚未退出，352 个权重分片加载曾推进至约 60%，随后完成并进入 target-verify NPU graph capture（`num_tokens_per_bs=8, bs=[16]`）。rank0 的 30000 端口在所述检查时仍未监听，尚未确认服务 ready，也未发现明确异常；这些观察不应视为启动成功。
  title: SGLang 启动监控进展
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 022decee19a24ab29e7927a26ee76128d4b753d8bff05e5078c8766f6de98e0a
  kind: experience
  material_digest: 1c94e3324064ae8f2648e2d471b25173045f071c3a20bc6a96fb72b9e122a399
  revision: 9951500fe5b648c92c5a3c7bea5bc4cb5a8d884722f4a6fda4fbd2bab2780a35
  schema: mindie-entry/3
  summary: 本批材料记录了 SGLang 服务从权重加载到 target-verify NPU graph capture 的只读观察。关键检索词包括 352 个权重分片、四容器、rank0、`/health`、30000 端口、NPU graph capture、`num_tokens_per_bs=8`、`bs=[16]`；截至该记录尚未确认 ready 或成功，端口未监听，后续状态仍需独立检索和核实。
  title: 当前任务导航：SGLang run 启动状态
navigation: 本批材料记录了 SGLang 服务从权重加载到 target-verify NPU graph capture 的只读观察。关键检索词包括 352 个权重分片、四容器、rank0、`/health`、30000 端口、NPU graph capture、`num_tokens_per_bs=8`、`bs=[16]`；截至该记录尚未确认 ready 或成功，端口未监听，后续状态仍需独立检索和核实。
schema: mindie-material-task/1
status: complete
task_id: 022decee19a24ab29e7927a26ee76128d4b753d8bff05e5078c8766f6de98e0a
---

# 当前任务导航：SGLang run 启动状态

本批材料记录了 SGLang 服务从权重加载到 target-verify NPU graph capture 的只读观察。关键检索词包括 352 个权重分片、四容器、rank0、`/health`、30000 端口、NPU graph capture、`num_tokens_per_bs=8`、`bs=[16]`；截至该记录尚未确认 ready 或成功，端口未监听，后续状态仍需独立检索和核实。

## Materials

- [SGLang 启动监控进展](blocks/ec086be5b42e5d4372e3de56f2adf449748c1e62786936c2289cc891ab7cce92.md): 只读监控一个 SGLang run：四个容器尚未退出，352 个权重分片加载曾推进至约 60%，随后完成并进入 target-verify NPU graph capture（`num_tokens_per_bs=8, bs=[16]`）。rank0 的 30000 端口在所述检查时仍未监听，尚未确认服务 ready，也未发现明确异常；这些观察不应视为启动成功。

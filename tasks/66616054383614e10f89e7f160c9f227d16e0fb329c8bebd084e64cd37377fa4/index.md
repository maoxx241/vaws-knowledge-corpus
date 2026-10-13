---
blocks:
- block_id: ad2fb4d941970775d1ce156b6eff10b6848ddb4f46c646aea50ef4990c346c55
  indexed: true
  sha256: b4c89a50b17964684d3720c7c6bf968571684caeef36db407656c84ad3dadf6a
  source_range:
    end: 1785670
    part: 0
    start: 0
  summary: '该 session 基于原始 JSONL、rollout 记录与 Git/worktree 进行了只读审计；截至快照没有完成修复，也没有验证出单一根因。R1 暴露 `UnboundLocalError: preprocess_bufs`，R2 首次确证多 DP hang；R8 的 phase-ledger 排除了 `_sync_metadata_across_dp` CPU collective 调用次数/来源错配，但冻结最终出现在 MoE `_preprocess()` 的 `torch.repeat_interleave`，其 `rtStreamSynchronizeWithTimeout` 可能暴露更早未完成的异步 NPU/collective 工作，尚不能断言该 API 本身有 bug。当前仍缺少具体 hunk、kernel 或严格单变量回退/A-B 证据；worktree 含未提交功能改动与临时 probes，且后续 JSONL 可能继续增长。'
  title: 只读审计：R8 未定位单一根因
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 66616054383614e10f89e7f160c9f227d16e0fb329c8bebd084e64cd37377fa4
  kind: experience
  material_digest: 60d1dae4cca6bae2005cedef28d919c249f35eef392a53fb8890d5aefc67b8af
  revision: ff737076b3314abe942ec28cb7d8015c33bf4b4c593df074134ec8b6e705f6d9
  schema: mindie-entry/3
  summary: 当前唯一索引涵盖从 GQA packing、GPU-only Mamba align 及多轮 R1-R8 验证到最终未决状态的完整线索。重点检索 R2 首次 hang（`preprocess_bufs`、多 DP、API 仍 200）、R8 phase-ledger（ordinal 107、`[64,48,48,16]`、排除 collective 串线）以及 MoE `repeat_interleave` 的 NPU stream 同步栈；保留已被推翻的 R6 CPU all-reduce 解释、失败尝试和未完成的单-hunk/A-B 验证。
  title: 任务导航：Ascend/vLLM 卡死只读审计
navigation: 当前唯一索引涵盖从 GQA packing、GPU-only Mamba align 及多轮 R1-R8 验证到最终未决状态的完整线索。重点检索 R2 首次 hang（`preprocess_bufs`、多 DP、API 仍 200）、R8 phase-ledger（ordinal 107、`[64,48,48,16]`、排除 collective 串线）以及 MoE `repeat_interleave` 的 NPU stream 同步栈；保留已被推翻的 R6 CPU all-reduce 解释、失败尝试和未完成的单-hunk/A-B 验证。
schema: mindie-material-task/1
status: complete
task_id: 66616054383614e10f89e7f160c9f227d16e0fb329c8bebd084e64cd37377fa4
---

# 任务导航：Ascend/vLLM 卡死只读审计

当前唯一索引涵盖从 GQA packing、GPU-only Mamba align 及多轮 R1-R8 验证到最终未决状态的完整线索。重点检索 R2 首次 hang（`preprocess_bufs`、多 DP、API 仍 200）、R8 phase-ledger（ordinal 107、`[64,48,48,16]`、排除 collective 串线）以及 MoE `repeat_interleave` 的 NPU stream 同步栈；保留已被推翻的 R6 CPU all-reduce 解释、失败尝试和未完成的单-hunk/A-B 验证。

## Materials

- [只读审计：R8 未定位单一根因](blocks/ad2fb4d941970775d1ce156b6eff10b6848ddb4f46c646aea50ef4990c346c55.md): 该 session 基于原始 JSONL、rollout 记录与 Git/worktree 进行了只读审计；截至快照没有完成修复，也没有验证出单一根因。R1 暴露 `UnboundLocalError: preprocess_bufs`，R2 首次确证多 DP hang；R8 的 phase-ledger 排除了 `_sync_metadata_across_dp` CPU collective 调用次数/来源错配，但冻结最终出现在 MoE `_preprocess()` 的 `torch.repeat_interleave`，其 `rtStreamSynchronizeWithTimeout` 可能暴露更早未完成的异步 NPU/collective 工作，尚不能断言该 API 本身有 bug。当前仍缺少具体 hunk、kernel 或严格单变量回退/A-B 证据；worktree 含未提交功能改动与临时 probes，且后续 JSONL 可能继续增长。

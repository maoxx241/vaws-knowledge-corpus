# Historical, unverified msprof memory ranges and allocation heuristics

This is a historical reference extracted from consumer Skill documentation. It is not a measured baseline or an acceptance threshold. The source did not identify the model, workload, exact CANN/PyTorch versions, device configuration or raw measurement artifacts behind these values. This migration has not revalidated the claims.

Source: [workspace msprof field guide at eb30d0690edef67e26505a5332cc569de1ed6c59](https://github.com/vllm-ascend-workspace/vllm-ascend-workspace/blob/eb30d0690edef67e26505a5332cc569de1ed6c59/.agents/skills/ascend-memory-profiling/references/msprof_fields.md#L21-L32); allocation patterns are [in the same fixed source](https://github.com/vllm-ascend-workspace/vllm-ascend-workspace/blob/eb30d0690edef67e26505a5332cc569de1ed6c59/.agents/skills/ascend-memory-profiling/references/msprof_fields.md#L88-L91).

## Original 910B4 examples

| Component | What it represents | Typical range (910B4) |
|-----------|-------------------|----------------------|
| APP | All application-level memory (PyTorch allocator + GE) | 10-25 GB |
| HCCL | Communication buffers for collective ops (all-reduce, etc.) | 200-500 MB |
| RUNTIME | CANN runtime internal allocations | 50-100 MB |
| SLOG | System logging buffers | 100-150 MB |
| GE | Graph Engine internal memory | Variable |
| FE | Frontend memory | Small |
| DEVMM | Device Memory Management | Small |
| Others | AICPU, CCE, TBE, TS, etc. | Usually 0 |

## Original allocation patterns

- `aten::empty` with large Size → weight tensor allocation
- `aten::empty` without Release Time → permanent allocation (likely weights or KV cache)
- `aten::matmul` with small Size → activation tensors (transient)

Operator names and missing release timestamps alone do not establish allocation purpose or permanent lifetime. Use the actual capture window, call stack and model execution evidence to assess the original suggestions. Retain the source uncertainty when referring to these examples.

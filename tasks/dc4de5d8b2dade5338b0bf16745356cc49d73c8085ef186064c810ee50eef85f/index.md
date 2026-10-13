---
blocks:
- block_id: e09fe57b168dd11756ef4a8ceda295a5be204f9d94dbc1f45abf5dcf3fe94f09
  indexed: true
  sha256: c281ce72dbcd0c44795f520f63c72cf569865a7c5aebf9e4b9958af30d074278
  source_range:
    end: 1274642
    part: 0
    start: 0
  summary: R6/R9 观察到第二波每个 DP 的 7 个真实单 token 请求在 layer 0 KDA attention_done 前卡死；TASK_QUEUE=0 仍复现，因此 dummy/real 混排、Prefix Cache、host task queue 与 MC2 均未被证明是必要条件，且已撤销 dummy fence/TASK_QUEUE 主因判断。当前尚不能区分 TP gather、causal conv、kda_gate_cumsum 与 chunk_kda_fwd，建议用总 token 相同的 1×7-token 对 7×1-token A/B 及四点 device events 定位。生产候选是为全 singleton prefill 增加“初始化本地 state 后走 recurrent、再写回 cache”的安全 fallback，并补齐 cu=[0,1,...,7]、mixed varlen、生命周期及 TASK_QUEUE=0/1 回归；这些方案仍属待验证修复。
  title: H0 定位为 KDA 多 singleton prefill 卡死
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: dc4de5d8b2dade5338b0bf16745356cc49d73c8085ef186064c810ee50eef85f
  kind: experience
  material_digest: 55830ef42ce285182c015b6d1bc8aff16cb9c5e8a121373b3d1c00f6012ef0d2
  revision: ca1a6f9bda8238c091287fc7234f8da236e44ed3713f4107516727eab8a8fc2c
  schema: mindie-entry/3
  summary: 已确认 H0 在 layer 0 KDA prefill 路径的 attention_done 之前复现，且 TASK_QUEUE=0 不改变结果；dummy fence、异步 host task queue、Prefix Cache 和 MC2 尚非必要因果。下一步优先执行同总 token 的单长序列 vs 7 个单 token 序列 A/B，并在 gather、conv、gate-cumsum、chunk-kda 后加 device events；在 native seam 未切分前，不应把候选 fallback 或 tiling 修复视为成功。
  title: 当前任务导航：定位 KDA 多 singleton prefill 卡死
navigation: 已确认 H0 在 layer 0 KDA prefill 路径的 attention_done 之前复现，且 TASK_QUEUE=0 不改变结果；dummy fence、异步 host task queue、Prefix Cache 和 MC2 尚非必要因果。下一步优先执行同总 token 的单长序列 vs 7 个单 token 序列 A/B，并在 gather、conv、gate-cumsum、chunk-kda 后加 device events；在 native seam 未切分前，不应把候选 fallback 或 tiling 修复视为成功。
schema: mindie-material-task/1
status: complete
task_id: dc4de5d8b2dade5338b0bf16745356cc49d73c8085ef186064c810ee50eef85f
---

# 当前任务导航：定位 KDA 多 singleton prefill 卡死

已确认 H0 在 layer 0 KDA prefill 路径的 attention_done 之前复现，且 TASK_QUEUE=0 不改变结果；dummy fence、异步 host task queue、Prefix Cache 和 MC2 尚非必要因果。下一步优先执行同总 token 的单长序列 vs 7 个单 token 序列 A/B，并在 gather、conv、gate-cumsum、chunk-kda 后加 device events；在 native seam 未切分前，不应把候选 fallback 或 tiling 修复视为成功。

## Materials

- [H0 定位为 KDA 多 singleton prefill 卡死](blocks/e09fe57b168dd11756ef4a8ceda295a5be204f9d94dbc1f45abf5dcf3fe94f09.md): R6/R9 观察到第二波每个 DP 的 7 个真实单 token 请求在 layer 0 KDA attention_done 前卡死；TASK_QUEUE=0 仍复现，因此 dummy/real 混排、Prefix Cache、host task queue 与 MC2 均未被证明是必要条件，且已撤销 dummy fence/TASK_QUEUE 主因判断。当前尚不能区分 TP gather、causal conv、kda_gate_cumsum 与 chunk_kda_fwd，建议用总 token 相同的 1×7-token 对 7×1-token A/B 及四点 device events 定位。生产候选是为全 singleton prefill 增加“初始化本地 state 后走 recurrent、再写回 cache”的安全 fallback，并补齐 cu=[0,1,...,7]、mixed varlen、生命周期及 TASK_QUEUE=0/1 回归；这些方案仍属待验证修复。

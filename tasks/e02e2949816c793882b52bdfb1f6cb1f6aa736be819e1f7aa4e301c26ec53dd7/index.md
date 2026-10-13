---
blocks:
- block_id: 5532ea3fb0a29c5edf7ade33f7691a9400e1d500816483713cbfabfec680742b
  indexed: true
  sha256: 987ab819894e3bcee98cad8e03f9729ed2c29cf537c4b895c95410bf1c3f186c
  source_range:
    end: 328881
    part: 0
    start: 0
  summary: 助手根据当前本地 `vllm-ascend-kimi-k3` checkout（提交 `b5edaf0dc`，据报告比远端少 1 个提交）检查了 `get_mamba_state_shape → mamba_page_size_padded → kv_cache` 路径。报告称两类 state 共享逻辑 block id 和 block-table 关联，但底层 tensor 采用 SoA 布局，先放全部 conv blocks、再放全部 recurrent/SSM blocks；因此不能假设 `block_id * page_size` 对应连续的 `[conv|recurrent]` 数据。该结论是基于当前代码的报告性参考，未在此处独立验证。
  title: KDA recurrent state 与 conv state 的 block/page 布局
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e02e2949816c793882b52bdfb1f6cb1f6aa736be819e1f7aa4e301c26ec53dd7
  kind: experience
  material_digest: 24c5e45ccb35750913516ea51a0aff006647874eb1146157b784e112a0fadee6
  revision: 79e77127249a3d6a667c32aef96acd44bd1af6ecafdc31c9272f75f720671c84
  schema: mindie-entry/3
  summary: 当前批次涉及 KDA/Mamba 中 recurrent state 与 conv state 的关联及物理存储布局：报告区分了共享逻辑 block id 与分区存放的底层 tensor/page，并提示 prefix-cache、offload、block copy 需分别处理两个 state segment。原始材料仍是独立可检索的参考；checkout 状态、代码路径与结论均应视为可能不完整或待验证。
  title: State block/page 布局索引
navigation: 当前批次涉及 KDA/Mamba 中 recurrent state 与 conv state 的关联及物理存储布局：报告区分了共享逻辑 block id 与分区存放的底层 tensor/page，并提示 prefix-cache、offload、block copy 需分别处理两个 state segment。原始材料仍是独立可检索的参考；checkout 状态、代码路径与结论均应视为可能不完整或待验证。
schema: mindie-material-task/1
status: complete
task_id: e02e2949816c793882b52bdfb1f6cb1f6aa736be819e1f7aa4e301c26ec53dd7
---

# State block/page 布局索引

当前批次涉及 KDA/Mamba 中 recurrent state 与 conv state 的关联及物理存储布局：报告区分了共享逻辑 block id 与分区存放的底层 tensor/page，并提示 prefix-cache、offload、block copy 需分别处理两个 state segment。原始材料仍是独立可检索的参考；checkout 状态、代码路径与结论均应视为可能不完整或待验证。

## Materials

- [KDA recurrent state 与 conv state 的 block/page 布局](blocks/5532ea3fb0a29c5edf7ade33f7691a9400e1d500816483713cbfabfec680742b.md): 助手根据当前本地 `vllm-ascend-kimi-k3` checkout（提交 `b5edaf0dc`，据报告比远端少 1 个提交）检查了 `get_mamba_state_shape → mamba_page_size_padded → kv_cache` 路径。报告称两类 state 共享逻辑 block id 和 block-table 关联，但底层 tensor 采用 SoA 布局，先放全部 conv blocks、再放全部 recurrent/SSM blocks；因此不能假设 `block_id * page_size` 对应连续的 `[conv|recurrent]` 数据。该结论是基于当前代码的报告性参考，未在此处独立验证。

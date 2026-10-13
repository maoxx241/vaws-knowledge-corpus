---
blocks:
- block_id: f4570b8e6319c720b3cc2330b6251e5d26256417e368c4dacccfe43c55dee57b
  indexed: true
  sha256: efb4c3bf71638616c4c90ae502506af0cea68ca3097d3104e5d0ee4d824226d7
  source_range:
    end: 1210807
    part: 0
    start: 0
  summary: 用户认为 causal conv1d 未注册原地写不是当前偶现问题根因，助手静态审计提出更接近症状的假设：上一轮异步 D2H 写回 pinned CPU buffer 的 `num_accepted_tokens_cpu`，可能与下一轮 `condense/reorder` 并发访问，且 `_prepare_inputs` 较晚才等待 event；同时 fused postprocess 读取持久 GPU block table 时可能遭遇下一轮 `commit_block_table()` 覆盖。该分析将 recurrent/conv state 错配、accepted count 重排错误和中层 NaN 与 32 并发、FULL size16 切入及 DP3 场景联系起来，但仅是未验证的静态审计结论；未修改文件，也未证明修复建议成功。
  title: 高并发下 accepted-token 与 block table 的跨轮同步竞态假设
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b2548ef88f019df6037db8d924e03af89f0d9240c0fa9f389c5fcf411e4c098d
  kind: experience
  material_digest: 679c3129e75279242001b50fd1918e17f9e0fdc43e26407703f81b5cee991a5d
  revision: 24018c00466940db49839fdbbc320963fe5c08d0eda12dd6110e0859992317d0
  schema: mindie-entry/3
  summary: 重点检索高并发重排、Mamba/KDA align postprocess、`num_accepted_tokens_cpu` event、GPU block-table 覆盖、FULL size16 切入和潜在 double-remap。现有材料记录的是只读静态审计与待验证竞态假设；conv1d 注册、qsl/padding 刷新、DP all-reduce 等被列为排查结果或次要风险，不能视为已验证根因或成功修复。
  title: 当前任务导航：并发 padding/同步问题索引
navigation: 重点检索高并发重排、Mamba/KDA align postprocess、`num_accepted_tokens_cpu` event、GPU block-table 覆盖、FULL size16 切入和潜在 double-remap。现有材料记录的是只读静态审计与待验证竞态假设；conv1d 注册、qsl/padding 刷新、DP all-reduce 等被列为排查结果或次要风险，不能视为已验证根因或成功修复。
schema: mindie-material-task/1
status: complete
task_id: b2548ef88f019df6037db8d924e03af89f0d9240c0fa9f389c5fcf411e4c098d
---

# 当前任务导航：并发 padding/同步问题索引

重点检索高并发重排、Mamba/KDA align postprocess、`num_accepted_tokens_cpu` event、GPU block-table 覆盖、FULL size16 切入和潜在 double-remap。现有材料记录的是只读静态审计与待验证竞态假设；conv1d 注册、qsl/padding 刷新、DP all-reduce 等被列为排查结果或次要风险，不能视为已验证根因或成功修复。

## Materials

- [高并发下 accepted-token 与 block table 的跨轮同步竞态假设](blocks/f4570b8e6319c720b3cc2330b6251e5d26256417e368c4dacccfe43c55dee57b.md): 用户认为 causal conv1d 未注册原地写不是当前偶现问题根因，助手静态审计提出更接近症状的假设：上一轮异步 D2H 写回 pinned CPU buffer 的 `num_accepted_tokens_cpu`，可能与下一轮 `condense/reorder` 并发访问，且 `_prepare_inputs` 较晚才等待 event；同时 fused postprocess 读取持久 GPU block table 时可能遭遇下一轮 `commit_block_table()` 覆盖。该分析将 recurrent/conv state 错配、accepted count 重排错误和中层 NaN 与 32 并发、FULL size16 切入及 DP3 场景联系起来，但仅是未验证的静态审计结论；未修改文件，也未证明修复建议成功。

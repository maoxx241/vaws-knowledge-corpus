---
blocks:
- block_id: 965f9574b3e30db3e7d325582c72b516eee247c6666705ff288096cde19eea8e
  indexed: true
  sha256: 4c26b685f86fa35a867a270376da3aef6b3bdeef4d6d23a28cd4b2b153171169
  source_range:
    end: 706145
    part: 0
    start: 0
  summary: 只读分析提出 K3 的五层 aux hidden capture、`SupportsEagle3` API、固定 7-token DSpark 配置、Qwen3 DSpark 模型 vendoring、公开 registry 注册，以及 scheduler/Mooncake P-side 的最小运行时补丁；同时记录 KDA 状态上限、prefix cache fixed-point 策略和 TP16×DP4×EP64 等测试要求。后续对提交 `fdb5d55034a8bc161771ba9092ce9f1d726b72d5` 的核对称完整生产 diff 为 4 个文件、另有 5 个测试文件，并建议通过包装 `SpeculativeConfig.__init__`、patch `hf_config_override`/`use_eagle`、注册 plugin 模型和拦截 `KVCacheManager.get_computed_blocks` 实现迁移。内容明确属于实现方案和测试清单，尚未证明补丁已应用或测试成功；保留了 Pydantic Literal、`DSparkDraftModel`→`Qwen3DSparkModel`、`mask_token_id`/`pad_token_id`、`parallel_drafting`、`do_remote_decode`、draft KV 恢复等关键 API/环境约束。
  title: K3/DSpark 运行时复刻与验证索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b2e5cc7fac190f6dafd21fbfc3c4109dd15e1045df7b31cb6cea5627970a80ed
  kind: experience
  material_digest: 9a440d261d664fb134fcffa4da049dd59dae4372250ffdfc9ef6250e62babcac
  revision: 800955550024ab355865868370709528f04050054c6718280d008323626405a2
  schema: mindie-entry/3
  summary: 已有材料集中于将 vLLM core 的 DSpark 语义迁移到 vLLM Ascend：包括五层 aux hidden 的下一层 pre-norm capture、固定 `block_size=num_speculative_tokens=7`、Qwen3 DSpark 模型与 loader/registry、scheduler lookahead 及 Mooncake P-side prefix 行为。当前记录仍是只读核对和建议方案，不应视为已实现或验证成功；后续检索应优先关注 `speculative.py`、`qwen3_dspark.py`、Ascend proposer/model runner、`KVCacheManager`、KDA 1–8 接受长度、slot mapping、prefix cache 以及多 DP/TP/EP 场景，并保留失败尝试、未完成测试和后续修正。
  title: 当前任务导航：K3/DSpark 索引与迁移核对
navigation: 已有材料集中于将 vLLM core 的 DSpark 语义迁移到 vLLM Ascend：包括五层 aux hidden 的下一层 pre-norm capture、固定 `block_size=num_speculative_tokens=7`、Qwen3 DSpark 模型与 loader/registry、scheduler lookahead 及 Mooncake P-side prefix 行为。当前记录仍是只读核对和建议方案，不应视为已实现或验证成功；后续检索应优先关注 `speculative.py`、`qwen3_dspark.py`、Ascend proposer/model runner、`KVCacheManager`、KDA 1–8 接受长度、slot mapping、prefix cache 以及多 DP/TP/EP 场景，并保留失败尝试、未完成测试和后续修正。
schema: mindie-material-task/1
status: complete
task_id: b2e5cc7fac190f6dafd21fbfc3c4109dd15e1045df7b31cb6cea5627970a80ed
---

# 当前任务导航：K3/DSpark 索引与迁移核对

已有材料集中于将 vLLM core 的 DSpark 语义迁移到 vLLM Ascend：包括五层 aux hidden 的下一层 pre-norm capture、固定 `block_size=num_speculative_tokens=7`、Qwen3 DSpark 模型与 loader/registry、scheduler lookahead 及 Mooncake P-side prefix 行为。当前记录仍是只读核对和建议方案，不应视为已实现或验证成功；后续检索应优先关注 `speculative.py`、`qwen3_dspark.py`、Ascend proposer/model runner、`KVCacheManager`、KDA 1–8 接受长度、slot mapping、prefix cache 以及多 DP/TP/EP 场景，并保留失败尝试、未完成测试和后续修正。

## Materials

- [K3/DSpark 运行时复刻与验证索引](blocks/965f9574b3e30db3e7d325582c72b516eee247c6666705ff288096cde19eea8e.md): 只读分析提出 K3 的五层 aux hidden capture、`SupportsEagle3` API、固定 7-token DSpark 配置、Qwen3 DSpark 模型 vendoring、公开 registry 注册，以及 scheduler/Mooncake P-side 的最小运行时补丁；同时记录 KDA 状态上限、prefix cache fixed-point 策略和 TP16×DP4×EP64 等测试要求。后续对提交 `fdb5d55034a8bc161771ba9092ce9f1d726b72d5` 的核对称完整生产 diff 为 4 个文件、另有 5 个测试文件，并建议通过包装 `SpeculativeConfig.__init__`、patch `hf_config_override`/`use_eagle`、注册 plugin 模型和拦截 `KVCacheManager.get_computed_blocks` 实现迁移。内容明确属于实现方案和测试清单，尚未证明补丁已应用或测试成功；保留了 Pydantic Literal、`DSparkDraftModel`→`Qwen3DSparkModel`、`mask_token_id`/`pad_token_id`、`parallel_drafting`、`do_remote_decode`、draft KV 恢复等关键 API/环境约束。

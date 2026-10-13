---
blocks:
- block_id: 1c95c435d3e959836ff104068a66e7bddeafa6ab10d35c17a37d75264e0ceb54
  indexed: true
  sha256: 211a6690d4c4da8a2101ac1eba4c4a6292a0a4ae36a0752ef1e625297a1a9707
  source_range:
    end: 1157001
    part: 0
    start: 0
  summary: 审查认为 release/0.23.0 已有可复用的 hybrid prefix-cache 命中断言，但现有 vllm-ascend Qwen3.5 用例仅比较 cache on/off 输出，不能证明真实命中。K3 KDA 应复用 `IsHybrid`、`HasInnerState` 和 `mamba_cache_mode="align"` 路径，不应声明 `supports_mamba_prefix_caching=True`；建议顺序执行相同请求并检查 `SchedulerOutput.num_computed_tokens`、`prefill_has_initial_state` 或 Prometheus 命中指标。推荐补充模型接口及二次请求命中测试，并在 A3 TP8 上验证；本次审查未修改工作树。
  title: K3 Hybrid Prefix Cache 命中验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a1873926fb665168b8f42bfbbd986278352cd59ea4a36b87a4d15fd2b1d9809d
  kind: experience
  material_digest: 57c0ea7c42690a913128af752ad4e2f44037a2d198d4866a76dc799c8b220cd3
  revision: 9b680f5509a7b82484cf278640067fae8f377aa0b99313e6ac77d05c7f6f4fb9
  schema: mindie-entry/3
  summary: 当前任务聚焦 K3 KDA/Qwen3.5 hybrid prefix-cache 的真实命中判据与测试补充。已知输出一致性不足以证明命中，建议使用顺序请求、最终解析的 `cache_config.block_size`、scheduler event 和 recurrent state 恢复信号；PCP/MTP 暂不启用，KDA 的 PCP prefill 支持仍未完成。
  title: Hybrid Prefix Cache 验证导航
navigation: 当前任务聚焦 K3 KDA/Qwen3.5 hybrid prefix-cache 的真实命中判据与测试补充。已知输出一致性不足以证明命中，建议使用顺序请求、最终解析的 `cache_config.block_size`、scheduler event 和 recurrent state 恢复信号；PCP/MTP 暂不启用，KDA 的 PCP prefill 支持仍未完成。
schema: mindie-material-task/1
status: complete
task_id: a1873926fb665168b8f42bfbbd986278352cd59ea4a36b87a4d15fd2b1d9809d
---

# Hybrid Prefix Cache 验证导航

当前任务聚焦 K3 KDA/Qwen3.5 hybrid prefix-cache 的真实命中判据与测试补充。已知输出一致性不足以证明命中，建议使用顺序请求、最终解析的 `cache_config.block_size`、scheduler event 和 recurrent state 恢复信号；PCP/MTP 暂不启用，KDA 的 PCP prefill 支持仍未完成。

## Materials

- [K3 Hybrid Prefix Cache 命中验证](blocks/1c95c435d3e959836ff104068a66e7bddeafa6ab10d35c17a37d75264e0ceb54.md): 审查认为 release/0.23.0 已有可复用的 hybrid prefix-cache 命中断言，但现有 vllm-ascend Qwen3.5 用例仅比较 cache on/off 输出，不能证明真实命中。K3 KDA 应复用 `IsHybrid`、`HasInnerState` 和 `mamba_cache_mode="align"` 路径，不应声明 `supports_mamba_prefix_caching=True`；建议顺序执行相同请求并检查 `SchedulerOutput.num_computed_tokens`、`prefill_has_initial_state` 或 Prometheus 命中指标。推荐补充模型接口及二次请求命中测试，并在 A3 TP8 上验证；本次审查未修改工作树。

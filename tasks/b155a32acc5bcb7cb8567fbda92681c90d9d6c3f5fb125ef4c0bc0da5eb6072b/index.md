---
blocks:
- block_id: cbc5ebb7e52ad30fef90c114bf5d9fa0d0825d11b0d92b593fccf164eda3ca64
  indexed: true
  sha256: 20d9d46f51f016755c9d68ba7726ace9ee2842c8bbc50c1b02493afe4215c989
  source_range:
    end: 2046815
    part: 0
    start: 0
  summary: '材料将旧 dummy-row padding 补丁降为历史线索，建议以同一连续中文请求对照 SGLang，并按 `max_tokens`、并发度、`spec_accept_rate` 和 histogram 区分采样差异、cache/position 漂移与 ACLGraph padding；SGLang issue #32519 仅被描述为可运行性证明，未提供同 workload 接收率。来源报告 GSM8K 56.87%、prefix 冷热门一致及若干对齐结果，并据此暂称中文 10.64% 更像 workload 分布差异，但仍要求直接 A/B 验证；同时提出 KDA causal-conv 的 TP 后 `[W, 3*C_local]` BF16 打包、persistent buffer、lazy fallback、reload 地址稳定及相应 UT 覆盖，均属于建议或待验证事项。'
  title: SGLang 对照与 KDA 预打包建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b155a32acc5bcb7cb8567fbda92681c90d9d6c3f5fb125ef4c0bc0da5eb6072b
  kind: experience
  material_digest: 35cdd7200b01d0b35c78866f6199d47b9c21d4e9ccc9fdce95ef79e3d36c7f61
  revision: 2c3eaa150fafe48df4a4e710a2fa4b5e2d1d020d7bae376aeb5e759bf8b78015
  schema: mindie-entry/3
  summary: 当前材料聚焦于 SGLang/vLLM 同 workload 接收率对照、ACLGraph padding 边界、draft KV/KDA state commit 与 position 漂移排查，以及 KDA causal-conv 预打包实现建议。已保留低接收率“不构成阻断”的暂定判断、中文 10.64% 的不确定性、SGLang issue 证据范围限制和尚未完成的 A/B 验证；原始材料仍可独立检索。
  title: DSpark 检索导航
navigation: 当前材料聚焦于 SGLang/vLLM 同 workload 接收率对照、ACLGraph padding 边界、draft KV/KDA state commit 与 position 漂移排查，以及 KDA causal-conv 预打包实现建议。已保留低接收率“不构成阻断”的暂定判断、中文 10.64% 的不确定性、SGLang issue 证据范围限制和尚未完成的 A/B 验证；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: b155a32acc5bcb7cb8567fbda92681c90d9d6c3f5fb125ef4c0bc0da5eb6072b
---

# DSpark 检索导航

当前材料聚焦于 SGLang/vLLM 同 workload 接收率对照、ACLGraph padding 边界、draft KV/KDA state commit 与 position 漂移排查，以及 KDA causal-conv 预打包实现建议。已保留低接收率“不构成阻断”的暂定判断、中文 10.64% 的不确定性、SGLang issue 证据范围限制和尚未完成的 A/B 验证；原始材料仍可独立检索。

## Materials

- [SGLang 对照与 KDA 预打包建议](blocks/cbc5ebb7e52ad30fef90c114bf5d9fa0d0825d11b0d92b593fccf164eda3ca64.md): 材料将旧 dummy-row padding 补丁降为历史线索，建议以同一连续中文请求对照 SGLang，并按 `max_tokens`、并发度、`spec_accept_rate` 和 histogram 区分采样差异、cache/position 漂移与 ACLGraph padding；SGLang issue #32519 仅被描述为可运行性证明，未提供同 workload 接收率。来源报告 GSM8K 56.87%、prefix 冷热门一致及若干对齐结果，并据此暂称中文 10.64% 更像 workload 分布差异，但仍要求直接 A/B 验证；同时提出 KDA causal-conv 的 TP 后 `[W, 3*C_local]` BF16 打包、persistent buffer、lazy fallback、reload 地址稳定及相应 UT 覆盖，均属于建议或待验证事项。

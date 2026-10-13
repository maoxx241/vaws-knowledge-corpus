---
blocks:
- block_id: a9da2ab7c9faba96e06f9dacf78c98f90d8c7a9ecbf70bfd42384156ca259468
  indexed: true
  sha256: ab7ca6afc9be46760017b30c9899af572904a686137a8771da4cfa24ad267d31
  source_range:
    end: 6310992
    part: 0
    start: 0
  summary: 排查认为低接收率更可能来自 exact image 中 SGLang 与 DeepEP wheel 的 Python API 不匹配，而非权重或 A3 算子本身；关键现象包括 `per_token_scale=[None]`、缺失 `quant_mode` 传参，以及 normal dispatcher 未传递输出 dtype。报告建议在 `_dispatch_core()` 中补充 NPU 的 `quant_mode`，并保留 `auto` 配置以区分 target W4A8 与 unquant draft；但该结论仍是待验证的修复建议，并保留了 low-latency、限制 prefill 等有限旁路及其不等价性。此前还在核对首轮 draft、KDA 状态、`aux_hidden_states` 拼接、残差快照顺序和权重挂接关系，尚未将这些方向确认为根因。
  title: DeepEP INT8 API 对齐与旁路方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 24f513a10f6c787df7ef7aa650e43060d60ad702dd17bff2a62a7cca5bc81e36
  kind: experience
  material_digest: 3c91e9504b052dcb66ebf78f44efd6178639978b351997ed6626ee8daa9bb548
  revision: 844ef63782c181b04210641ee4c080d2286f6c0dc4b962d1254b68c2d654e2e9
  schema: mindie-entry/3
  summary: 已记录一条主要线索：SGLang normal dispatcher 与 DeepEP 2026.7.4 的 `quant_mode`/输出 dtype Python API 可能不一致，导致 `hidden_states_scale=None` 和 `per_token_scale=[None]`；建议的 Python-only 修复及 low-latency 旁路均尚未完成验证。相关未决方向包括首轮 draft 上下文 KV 注入、`aux_hidden_states` 进入 logits processor 的语义、target 词嵌入与 lm_head 挂接、Kimi residual 快照物化，以及 modeling/多模态 preprocess/parser/tokenizer 对齐；不要将已报告的静态闭环或检查过程视为低接收率根因已证实。
  title: 当前任务导航：DSpark 低接收率与 DeepEP INT8 排查
navigation: 已记录一条主要线索：SGLang normal dispatcher 与 DeepEP 2026.7.4 的 `quant_mode`/输出 dtype Python API 可能不一致，导致 `hidden_states_scale=None` 和 `per_token_scale=[None]`；建议的 Python-only 修复及 low-latency 旁路均尚未完成验证。相关未决方向包括首轮 draft 上下文 KV 注入、`aux_hidden_states` 进入 logits processor 的语义、target 词嵌入与 lm_head 挂接、Kimi residual 快照物化，以及 modeling/多模态 preprocess/parser/tokenizer 对齐；不要将已报告的静态闭环或检查过程视为低接收率根因已证实。
schema: mindie-material-task/1
status: complete
task_id: 24f513a10f6c787df7ef7aa650e43060d60ad702dd17bff2a62a7cca5bc81e36
---

# 当前任务导航：DSpark 低接收率与 DeepEP INT8 排查

已记录一条主要线索：SGLang normal dispatcher 与 DeepEP 2026.7.4 的 `quant_mode`/输出 dtype Python API 可能不一致，导致 `hidden_states_scale=None` 和 `per_token_scale=[None]`；建议的 Python-only 修复及 low-latency 旁路均尚未完成验证。相关未决方向包括首轮 draft 上下文 KV 注入、`aux_hidden_states` 进入 logits processor 的语义、target 词嵌入与 lm_head 挂接、Kimi residual 快照物化，以及 modeling/多模态 preprocess/parser/tokenizer 对齐；不要将已报告的静态闭环或检查过程视为低接收率根因已证实。

## Materials

- [DeepEP INT8 API 对齐与旁路方案](blocks/a9da2ab7c9faba96e06f9dacf78c98f90d8c7a9ecbf70bfd42384156ca259468.md): 排查认为低接收率更可能来自 exact image 中 SGLang 与 DeepEP wheel 的 Python API 不匹配，而非权重或 A3 算子本身；关键现象包括 `per_token_scale=[None]`、缺失 `quant_mode` 传参，以及 normal dispatcher 未传递输出 dtype。报告建议在 `_dispatch_core()` 中补充 NPU 的 `quant_mode`，并保留 `auto` 配置以区分 target W4A8 与 unquant draft；但该结论仍是待验证的修复建议，并保留了 low-latency、限制 prefill 等有限旁路及其不等价性。此前还在核对首轮 draft、KDA 状态、`aux_hidden_states` 拼接、残差快照顺序和权重挂接关系，尚未将这些方向确认为根因。

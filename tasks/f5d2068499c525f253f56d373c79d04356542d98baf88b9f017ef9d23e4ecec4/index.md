---
blocks:
- block_id: 2a4d1b2d56ca3f503ce21810ad3e8d0a1053032168430126480f69f6b201e846
  indexed: true
  sha256: 3430c9db8291a02dad71c05407db9e9acb201d059f60090986df616cb2b4fa1f
  source_range:
    end: 1623125
    part: 0
    start: 0
  summary: '记录称 PR #15320 曾完成 QLI metadata 兜底清理，Full v2 精度为 100/107（93.457944%）、算子 E2E 1 passed、Mix 请求 50/50 成功；性能结论为 Compressor-only > Mix > Full v2，但这些是报告中的测试结果，不能视为独立核验。分析认为 `qli_seqused_k.max().item()` 会触发 NPU reduction 后的 D2H 同步，可能造成 Full v2/Mix 的 TPOT 退化，但不能单独解释 Mix 与 Full v2 差异或秒级 TTFT 差异。随后按用户要求将 prefill、decode 和 CP 路径的 `max_seqlen_k` 改为复用 CPU 侧最大长度除以 4，并报告本地检查通过；最新 head 为 `203bb90f34587e7f752fab4f9323a15b997a359b`，新 head 的其余 CI 检查当时仍在排队，`select-tests` 曾 pending。'
  title: 'Full v2 PR #15320：QLI 同步优化与 CP 路径更新'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f5d2068499c525f253f56d373c79d04356542d98baf88b9f017ef9d23e4ecec4
  kind: experience
  material_digest: 8dfd8b3b4dc8da4a8655d81d7f726c0535cef5356b3a78d630b9179eb38c2631
  revision: e4b2af8c07b47ae0a456ae7cc92988bc2a8111871bb1a5faa6ba3cabf416acdf
  schema: mindie-entry/3
  summary: '当前材料集中于 PR #15320 的 QLI metadata 清理、Compressor-only/Mix/Full v2 性能对比，以及通过复用 CPU 侧 `max_seqlen_kv`、`max_seq_lens` 和 `max_local_seq_lens` 除以 4 来避免 `max().item()` D2H 同步的修改。已报告的本地检查和部分 CI 结果与后续仍排队的检查需区分；同步开销导致性能退化仍是可能原因而非已证实结论。'
  title: 'Full v2 PR #15320：性能分析与 CPU 最大长度复用'
navigation: '当前材料集中于 PR #15320 的 QLI metadata 清理、Compressor-only/Mix/Full v2 性能对比，以及通过复用 CPU 侧 `max_seqlen_kv`、`max_seq_lens` 和 `max_local_seq_lens` 除以 4 来避免 `max().item()` D2H 同步的修改。已报告的本地检查和部分 CI 结果与后续仍排队的检查需区分；同步开销导致性能退化仍是可能原因而非已证实结论。'
schema: mindie-material-task/1
status: complete
task_id: f5d2068499c525f253f56d373c79d04356542d98baf88b9f017ef9d23e4ecec4
---

# Full v2 PR #15320：性能分析与 CPU 最大长度复用

当前材料集中于 PR #15320 的 QLI metadata 清理、Compressor-only/Mix/Full v2 性能对比，以及通过复用 CPU 侧 `max_seqlen_kv`、`max_seq_lens` 和 `max_local_seq_lens` 除以 4 来避免 `max().item()` D2H 同步的修改。已报告的本地检查和部分 CI 结果与后续仍排队的检查需区分；同步开销导致性能退化仍是可能原因而非已证实结论。

## Materials

- [Full v2 PR #15320：QLI 同步优化与 CP 路径更新](blocks/2a4d1b2d56ca3f503ce21810ad3e8d0a1053032168430126480f69f6b201e846.md): 记录称 PR #15320 曾完成 QLI metadata 兜底清理，Full v2 精度为 100/107（93.457944%）、算子 E2E 1 passed、Mix 请求 50/50 成功；性能结论为 Compressor-only > Mix > Full v2，但这些是报告中的测试结果，不能视为独立核验。分析认为 `qli_seqused_k.max().item()` 会触发 NPU reduction 后的 D2H 同步，可能造成 Full v2/Mix 的 TPOT 退化，但不能单独解释 Mix 与 Full v2 差异或秒级 TTFT 差异。随后按用户要求将 prefill、decode 和 CP 路径的 `max_seqlen_k` 改为复用 CPU 侧最大长度除以 4，并报告本地检查通过；最新 head 为 `203bb90f34587e7f752fab4f9323a15b997a359b`，新 head 的其余 CI 检查当时仍在排队，`select-tests` 曾 pending。

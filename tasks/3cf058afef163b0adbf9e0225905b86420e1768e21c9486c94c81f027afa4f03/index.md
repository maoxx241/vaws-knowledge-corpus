---
blocks:
- block_id: 3c38a11b09fa1a340cfa43bf62121f1edf3cebce518e238cbe15ff4f99c5e406
  indexed: true
  sha256: 9411d0e6e3576d538b9081f5c93ca9a55d36a0a7ac2097b3b38ca1ce684ad24f
  source_range:
    end: 1375741
    part: 0
    start: 0
  summary: 此前回复声称 Eager 与 FULL_DECODE_ONLY 的 3-token 输出、chosen-token logprob、累计 logprob 及 top-20 结果一致，并报告 Graph 已完成 capture/replay；这些属于来源中的验证陈述，未在本索引中重新核实，且 piecewise graph 未测试。随后对 K3 prefix cache 进行了只读审计，报告其使用 mamba_cache_mode=align、TP8 BF16 预计 768-token block，以及 MLA/KDA state 恢复与 has_initial_state=True 相关路径；同时明确当前缺口是尚未实机确认命中计数，建议用 900–1100 token 公共前缀、不同 suffix，并比较冷运行、命中增量、logprob 及 enforce_eager/FULL_DECODE_ONLY。
  title: ACLGraph 精度对比与 K3 Prefix Cache 审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3cf058afef163b0adbf9e0225905b86420e1768e21c9486c94c81f027afa4f03
  kind: experience
  material_digest: c6448260bb010997e2e3bfe0efcbe09ec70086ad87fd7edbc5eebf17dca36eea
  revision: 1bbad75221307e0faaf5cfd73ccf8f61a0b4fccab0d3e22ab78a100fa096043b
  schema: mindie-entry/3
  summary: 优先执行并记录 prefix cache 的实机测试：确认公共前缀超过 768 tokens 时第二次请求的命中计数增量、输出与冷运行的一致性，以及 chosen-token logprob 容差；覆盖 enforce_eager 与 FULL_DECODE_ONLY。此前 ACLGraph 与 eager 一致性的报告、代码审计结论和测试建议均保留为可检索背景，不应将审计或计划视为已完成的命中验证；piecewise graph 仍未测试。
  title: 当前任务：验证 K3 Prefix Cache 实际命中与一致性
navigation: 优先执行并记录 prefix cache 的实机测试：确认公共前缀超过 768 tokens 时第二次请求的命中计数增量、输出与冷运行的一致性，以及 chosen-token logprob 容差；覆盖 enforce_eager 与 FULL_DECODE_ONLY。此前 ACLGraph 与 eager 一致性的报告、代码审计结论和测试建议均保留为可检索背景，不应将审计或计划视为已完成的命中验证；piecewise graph 仍未测试。
schema: mindie-material-task/1
status: complete
task_id: 3cf058afef163b0adbf9e0225905b86420e1768e21c9486c94c81f027afa4f03
---

# 当前任务：验证 K3 Prefix Cache 实际命中与一致性

优先执行并记录 prefix cache 的实机测试：确认公共前缀超过 768 tokens 时第二次请求的命中计数增量、输出与冷运行的一致性，以及 chosen-token logprob 容差；覆盖 enforce_eager 与 FULL_DECODE_ONLY。此前 ACLGraph 与 eager 一致性的报告、代码审计结论和测试建议均保留为可检索背景，不应将审计或计划视为已完成的命中验证；piecewise graph 仍未测试。

## Materials

- [ACLGraph 精度对比与 K3 Prefix Cache 审计](blocks/3c38a11b09fa1a340cfa43bf62121f1edf3cebce518e238cbe15ff4f99c5e406.md): 此前回复声称 Eager 与 FULL_DECODE_ONLY 的 3-token 输出、chosen-token logprob、累计 logprob 及 top-20 结果一致，并报告 Graph 已完成 capture/replay；这些属于来源中的验证陈述，未在本索引中重新核实，且 piecewise graph 未测试。随后对 K3 prefix cache 进行了只读审计，报告其使用 mamba_cache_mode=align、TP8 BF16 预计 768-token block，以及 MLA/KDA state 恢复与 has_initial_state=True 相关路径；同时明确当前缺口是尚未实机确认命中计数，建议用 900–1100 token 公共前缀、不同 suffix，并比较冷运行、命中增量、logprob 及 enforce_eager/FULL_DECODE_ONLY。

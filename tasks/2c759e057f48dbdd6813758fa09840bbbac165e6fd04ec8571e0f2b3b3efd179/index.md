---
blocks:
- block_id: 8de57714d50a1ce575d33aa6eacc3f1b7c9d3dd137aa802024582cde94396ab4
  indexed: true
  sha256: 1fa956e90f87e02e5b01b06e6fb8f7202e09822aacc20b761718e36be7f27828
  source_range:
    end: 1283391
    part: 0
    start: 0
  summary: 先前记录声称 ACLGraph 与 enforce eager 的 3-token 输出、chosen/cumulative logprob 及 top-20 结果一致，并报告 FULL_DECODE_ONLY capture/replay、KDA 测试和 CANN 9.0.1 环境信息；这些属于来源中的测试结论，未在此重新核验。后续 prefix cache 测试记录称 eager 和 FULL_DECODE_ONLY 均出现命中，`num_cached_tokens` 为 `[0, 1536, 1536]`，reset 后 cold 路径为 0，且 cache-on graph/eager 输出一致；但 fresh cache-off 使用 `mamba_cache_mode=none` 时与 cache-on/`align` 不同，暴露出长 prefill 的 chunk/state continuation 精度风险，因此不能宣称 cache-on/off 跨模式等价。相关术语包括 `Replaying aclgraph`、`reset_prefix_cache()`、768-token block、TP8、KDA、`align`/`none`、logprob 和 top-20 对比；prefix cache 的后续独立排查仍未完成。
  title: ACLGraph 精度与 Prefix Cache 测试记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2c759e057f48dbdd6813758fa09840bbbac165e6fd04ec8571e0f2b3b3efd179
  kind: experience
  material_digest: c5c34b7ed62023290aee83cedcd837af1c4304b2d0d89100b29b6c02725d0d39
  revision: 6805ebb0736e384f8d81c7f3126d96b334831d01f99abdb6f27c019a9af70c63
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，主题是 KDA/ACLGraph 精度对比及 prefix cache 命中验证。已记录的正向结果包括 FULL_DECODE_ONLY replay、prefix hit、state restore、reset 后 cold 重算及 cache-on eager/graph 一致；待继续关注的是 `align` 与 `none` 在长 prefill/state continuation 语义上的差异，以及由此产生的 cache-on/off 精度风险。
  title: 当前任务导航
navigation: 本批仅包含一个材料块，主题是 KDA/ACLGraph 精度对比及 prefix cache 命中验证。已记录的正向结果包括 FULL_DECODE_ONLY replay、prefix hit、state restore、reset 后 cold 重算及 cache-on eager/graph 一致；待继续关注的是 `align` 与 `none` 在长 prefill/state continuation 语义上的差异，以及由此产生的 cache-on/off 精度风险。
schema: mindie-material-task/1
status: complete
task_id: 2c759e057f48dbdd6813758fa09840bbbac165e6fd04ec8571e0f2b3b3efd179
---

# 当前任务导航

本批仅包含一个材料块，主题是 KDA/ACLGraph 精度对比及 prefix cache 命中验证。已记录的正向结果包括 FULL_DECODE_ONLY replay、prefix hit、state restore、reset 后 cold 重算及 cache-on eager/graph 一致；待继续关注的是 `align` 与 `none` 在长 prefill/state continuation 语义上的差异，以及由此产生的 cache-on/off 精度风险。

## Materials

- [ACLGraph 精度与 Prefix Cache 测试记录](blocks/8de57714d50a1ce575d33aa6eacc3f1b7c9d3dd137aa802024582cde94396ab4.md): 先前记录声称 ACLGraph 与 enforce eager 的 3-token 输出、chosen/cumulative logprob 及 top-20 结果一致，并报告 FULL_DECODE_ONLY capture/replay、KDA 测试和 CANN 9.0.1 环境信息；这些属于来源中的测试结论，未在此重新核验。后续 prefix cache 测试记录称 eager 和 FULL_DECODE_ONLY 均出现命中，`num_cached_tokens` 为 `[0, 1536, 1536]`，reset 后 cold 路径为 0，且 cache-on graph/eager 输出一致；但 fresh cache-off 使用 `mamba_cache_mode=none` 时与 cache-on/`align` 不同，暴露出长 prefill 的 chunk/state continuation 精度风险，因此不能宣称 cache-on/off 跨模式等价。相关术语包括 `Replaying aclgraph`、`reset_prefix_cache()`、768-token block、TP8、KDA、`align`/`none`、logprob 和 top-20 对比；prefix cache 的后续独立排查仍未完成。

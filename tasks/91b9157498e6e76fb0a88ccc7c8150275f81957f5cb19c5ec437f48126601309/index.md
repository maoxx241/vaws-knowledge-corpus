---
blocks:
- block_id: 6f9e0c8c713b0104fc4018c857c6c7e348a8de060c1a9acad759eb1e8959001e
  indexed: true
  sha256: f6b91e988c9ece1dba2501ec969cb9a0c597dde87d0fa05805e336e1c357f80a
  source_range:
    end: 887848
    part: 0
    start: 0
  summary: '在 154 单机 TP8、真实 5 层 INT4 权重和提交 f72b35e6 下，测试先后受到未挂载 /weights、未加载 CANN Python 环境导致 ModuleNotFoundError: acl，以及未设置自定义算子环境导致 aclnnMoeInitRoutingCustom 未注册等启动问题影响；这些问题后来被归因并排除，不能作为代码回归结论。修正环境后，eager 与 FULL_DECODE_ONLY（含 Replaying aclgraph）均观察到 cold→hit→reset 后 cold 为 0→1536→0 cached tokens，命中/冷请求 token 与 logprob 对照无差异。该结果仅覆盖 5 层 TP8 fixture，未验证四机完整权重；最终报告称未发现 Prefix Cache 命中问题，但仍应视为此次有限范围测试结果而非普遍保证。'
  title: 154 上 ci-new 5 层 Prefix Cache 验证及环境排障
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 91b9157498e6e76fb0a88ccc7c8150275f81957f5cb19c5ec437f48126601309
  kind: experience
  material_digest: 7dba30b73eb5401c127b9bfd8619684b28096f5e6a7e29f838d503494b5dd6eb
  revision: 2b2cb83210fbe56b72971d9bbf874a6369d588c9be091f9c422a80e2254c9668
  schema: mindie-entry/3
  summary: 当前材料记录了最新 ci-new（f72b35e6）在 154 单机 TP8、5 层 INT4 fixture 上的 Prefix Cache 验证：前置容器、CANN 与自定义算子环境问题经过修正后，eager 和 FULL_DECODE_ONLY 路径均报告命中行为及 token/logprob 对照通过。范围限于该 fixture，四机完整权重未覆盖；原始块仍可独立检索，期间失败尝试和环境归因均已保留。
  title: 154 上 ci-new Prefix Cache 测试索引
navigation: 当前材料记录了最新 ci-new（f72b35e6）在 154 单机 TP8、5 层 INT4 fixture 上的 Prefix Cache 验证：前置容器、CANN 与自定义算子环境问题经过修正后，eager 和 FULL_DECODE_ONLY 路径均报告命中行为及 token/logprob 对照通过。范围限于该 fixture，四机完整权重未覆盖；原始块仍可独立检索，期间失败尝试和环境归因均已保留。
schema: mindie-material-task/1
status: complete
task_id: 91b9157498e6e76fb0a88ccc7c8150275f81957f5cb19c5ec437f48126601309
---

# 154 上 ci-new Prefix Cache 测试索引

当前材料记录了最新 ci-new（f72b35e6）在 154 单机 TP8、5 层 INT4 fixture 上的 Prefix Cache 验证：前置容器、CANN 与自定义算子环境问题经过修正后，eager 和 FULL_DECODE_ONLY 路径均报告命中行为及 token/logprob 对照通过。范围限于该 fixture，四机完整权重未覆盖；原始块仍可独立检索，期间失败尝试和环境归因均已保留。

## Materials

- [154 上 ci-new 5 层 Prefix Cache 验证及环境排障](blocks/6f9e0c8c713b0104fc4018c857c6c7e348a8de060c1a9acad759eb1e8959001e.md): 在 154 单机 TP8、真实 5 层 INT4 权重和提交 f72b35e6 下，测试先后受到未挂载 /weights、未加载 CANN Python 环境导致 ModuleNotFoundError: acl，以及未设置自定义算子环境导致 aclnnMoeInitRoutingCustom 未注册等启动问题影响；这些问题后来被归因并排除，不能作为代码回归结论。修正环境后，eager 与 FULL_DECODE_ONLY（含 Replaying aclgraph）均观察到 cold→hit→reset 后 cold 为 0→1536→0 cached tokens，命中/冷请求 token 与 logprob 对照无差异。该结果仅覆盖 5 层 TP8 fixture，未验证四机完整权重；最终报告称未发现 Prefix Cache 命中问题，但仍应视为此次有限范围测试结果而非普遍保证。

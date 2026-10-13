---
blocks:
- block_id: df700f1d081bde68410cd21bdc4581a6e9252abacdeb1bdaa2ec3e20dd463b01
  indexed: true
  sha256: 7f8fdd6d006311f02974fd3f74d303773f35e349fa5250aab1131a239b0982e7
  source_range:
    end: 2201510
    part: 0
    start: 0
  summary: 只读对照未启动或停止服务，也未改动代码；分析认为 SGLang 与当前 vLLM Ascend 在 target hidden、7-step Markov、verify width 及 KDA/conv 提交公式上基本对齐，尚无直接 off-by-one 证据。关键不确定点是 hidden capture 数值、draft embedding/lm_head 是否真实共享，以及 KDA/conv 实际状态布局；建议用 L=1…8 的串行 oracle 验证，并按 verify round 区分首轮问题与后续状态提交问题。当前 24.23% 基线关闭 prefix-cache，因此 align-copy 不是该结果的关键路径；接收率比较还受 prompt、采样和请求集一致性限制。
  title: SGLang 与 vLLM Ascend speculative decoding 对齐结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 77f6cc9ba4a4956399dde00c2d257f75009b4cdd58f42eb747fbb55534e8248c
  kind: experience
  material_digest: 49a052be7ed24d3408550cdc8c15732083f13fc14afb551c1ee60643403d59fa
  revision: 548fce188bfbc1ead432581882e999027cdcab2cfeedf9ef14ed1d02c0a9672a
  schema: mindie-entry/3
  summary: 已完成 SGLang 精确提交、镜像 digest、draft revision、层号映射和接收率口径的只读对照。当前优先验证 hidden capture 与共享 embedding/lm_head；随后用 L=1…8 oracle 钉死 KDA/conv 状态布局。off-by-one 尚未证实，24.23% 的定量比较仍需相同 prompt、采样和请求集；原始材料中的未完成验证与不确定性应保留。
  title: 当前任务导航：speculative decoding 接收率与状态对齐
navigation: 已完成 SGLang 精确提交、镜像 digest、draft revision、层号映射和接收率口径的只读对照。当前优先验证 hidden capture 与共享 embedding/lm_head；随后用 L=1…8 oracle 钉死 KDA/conv 状态布局。off-by-one 尚未证实，24.23% 的定量比较仍需相同 prompt、采样和请求集；原始材料中的未完成验证与不确定性应保留。
schema: mindie-material-task/1
status: complete
task_id: 77f6cc9ba4a4956399dde00c2d257f75009b4cdd58f42eb747fbb55534e8248c
---

# 当前任务导航：speculative decoding 接收率与状态对齐

已完成 SGLang 精确提交、镜像 digest、draft revision、层号映射和接收率口径的只读对照。当前优先验证 hidden capture 与共享 embedding/lm_head；随后用 L=1…8 oracle 钉死 KDA/conv 状态布局。off-by-one 尚未证实，24.23% 的定量比较仍需相同 prompt、采样和请求集；原始材料中的未完成验证与不确定性应保留。

## Materials

- [SGLang 与 vLLM Ascend speculative decoding 对齐结论](blocks/df700f1d081bde68410cd21bdc4581a6e9252abacdeb1bdaa2ec3e20dd463b01.md): 只读对照未启动或停止服务，也未改动代码；分析认为 SGLang 与当前 vLLM Ascend 在 target hidden、7-step Markov、verify width 及 KDA/conv 提交公式上基本对齐，尚无直接 off-by-one 证据。关键不确定点是 hidden capture 数值、draft embedding/lm_head 是否真实共享，以及 KDA/conv 实际状态布局；建议用 L=1…8 的串行 oracle 验证，并按 verify round 区分首轮问题与后续状态提交问题。当前 24.23% 基线关闭 prefix-cache，因此 align-copy 不是该结果的关键路径；接收率比较还受 prompt、采样和请求集一致性限制。

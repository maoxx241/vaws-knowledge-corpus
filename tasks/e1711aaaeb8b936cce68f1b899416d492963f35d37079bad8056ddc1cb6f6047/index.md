---
blocks:
- block_id: 0b8407d8fb79263ca04dc092ef97c9b4fa8773d122e2414c08e62fffc5c79424
  indexed: true
  sha256: 1bac630f1d415970d8ef9990b66a06de3394d5c2763106230908f2d90537f589
  source_range:
    end: 957417
    part: 0
    start: 0
  summary: 结论认为当前 pinned vLLM 缺少公开的 pre-render request hook，自定义 renderer 无法在不 monkeypatch 的情况下获得完整 ChatCompletionRequest/ResponsesRequest；源码观察涉及 BaseRenderer、ChatParams、OnlineRenderer、Parser.adjust_request 与 ParserManager，且现有 wrapper 漏盖 Responses/batch 并可能造成请求状态不一致。建议不要恢复 renderer adapter 或全局 monkeypatch；若暂留 parsers，应限定为依赖外部已验证 K3 模板的 Chat Completions 输出解析，并明确 reasoning/tool parser 配置、Responses 拒绝边界及 XTML 泄漏或解析失败风险。K3 checkpoint 无 Jinja、GPQA Jinja 不支持 tools/response_format，且私有 thinking 参数与标准 reasoning_effort/enable_thinking 不匹配；完整方案仍需上游调整 request 顺序或增加类型化 pre-render hook，并加入 parser/renderer 兼容性校验，当前不是已验证的完整 serving 实现。
  title: vLLM K3 parser/renderer 能力边界与上游改进建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e1711aaaeb8b936cce68f1b899416d492963f35d37079bad8056ddc1cb6f6047
  kind: experience
  material_digest: e993634b47cb48e90cf923af71a6e543d2f3157a12096ce4ee7068b19181a97b
  revision: dd7339e171b33bf0737dca73b254527f3f0a7c0ab05f518da51dac61add3af21
  schema: mindie-entry/3
  summary: 当前索引聚焦 pinned vLLM 中 K3 renderer/parser 的接口限制、请求状态顺序、Chat/Responses/batch 覆盖范围及 parser 配置风险。保留的未完成项包括无原生 prompt renderer、reasoning_effort 映射不确定、默认 thinking 可能泄漏 XTML，以及需上游提供 pre-render adapter 与统一兼容性校验；原始材料仍可独立检索。
  title: vLLM K3 parser/renderer 调研索引
navigation: 当前索引聚焦 pinned vLLM 中 K3 renderer/parser 的接口限制、请求状态顺序、Chat/Responses/batch 覆盖范围及 parser 配置风险。保留的未完成项包括无原生 prompt renderer、reasoning_effort 映射不确定、默认 thinking 可能泄漏 XTML，以及需上游提供 pre-render adapter 与统一兼容性校验；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: e1711aaaeb8b936cce68f1b899416d492963f35d37079bad8056ddc1cb6f6047
---

# vLLM K3 parser/renderer 调研索引

当前索引聚焦 pinned vLLM 中 K3 renderer/parser 的接口限制、请求状态顺序、Chat/Responses/batch 覆盖范围及 parser 配置风险。保留的未完成项包括无原生 prompt renderer、reasoning_effort 映射不确定、默认 thinking 可能泄漏 XTML，以及需上游提供 pre-render adapter 与统一兼容性校验；原始材料仍可独立检索。

## Materials

- [vLLM K3 parser/renderer 能力边界与上游改进建议](blocks/0b8407d8fb79263ca04dc092ef97c9b4fa8773d122e2414c08e62fffc5c79424.md): 结论认为当前 pinned vLLM 缺少公开的 pre-render request hook，自定义 renderer 无法在不 monkeypatch 的情况下获得完整 ChatCompletionRequest/ResponsesRequest；源码观察涉及 BaseRenderer、ChatParams、OnlineRenderer、Parser.adjust_request 与 ParserManager，且现有 wrapper 漏盖 Responses/batch 并可能造成请求状态不一致。建议不要恢复 renderer adapter 或全局 monkeypatch；若暂留 parsers，应限定为依赖外部已验证 K3 模板的 Chat Completions 输出解析，并明确 reasoning/tool parser 配置、Responses 拒绝边界及 XTML 泄漏或解析失败风险。K3 checkpoint 无 Jinja、GPQA Jinja 不支持 tools/response_format，且私有 thinking 参数与标准 reasoning_effort/enable_thinking 不匹配；完整方案仍需上游调整 request 顺序或增加类型化 pre-render hook，并加入 parser/renderer 兼容性校验，当前不是已验证的完整 serving 实现。

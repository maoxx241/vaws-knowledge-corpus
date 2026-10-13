---
blocks:
- block_id: 7f110fba0e70d42ea97bb04acbc979dc7f352796fd298ed8fadbde2ce0b89722
  indexed: true
  sha256: d3a20dd125eca3be293e1826cb491b12016e05fe6aeca7fdc829db27ce7efb25
  source_range:
    end: 620185
    part: 0
    start: 0
  summary: 记录了该 session 中 GPQA、多模态 smoke、早期 LLM.generate() 三类请求：GPQA 使用 /v1/chat/completions、服务端 chat template、thinking=true 及采样参数，答案后处理由客户端正则提取；多模态 smoke 使用确定性参数、图像 content 数组，token 对齐 probe 另含 return_token_ids/logprobs。后续调查认为 k3-chat-template.jinja 不是权重自带文件，而是参考 K3 tokenizer/Python apply_chat_template() 临时改写的 vLLM 兼容模板；其对单轮 GPQA 文本基本适用，但未完整覆盖 tools、response_format、带 name 消息、历史 reasoning/response 分段和全部多模态渲染。上述来源、文件位置、复制记录和结论均属于该材料中的报告，模板完整等价性及正式 Serving 适用性仍存在不确定性。
  title: K3 请求构造与 chat template 来源
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 71e939b75359039b11221a658a72c7c4f428917a9a284a04069114142af232cc
  kind: experience
  material_digest: 49c4ca02d1e9d0460b05c75eb531284e50cccfed78218a1fb2fd04850210dbad
  revision: 43418006801dc8f29fe541d7b8c5accd3f127600a9924d7be416b086a2797b93
  schema: mindie-entry/3
  summary: 可检索本块以定位 GPQA 与多模态请求体、chat_template_kwargs、采样/后处理参数，以及 k3-chat-template.jinja 的报告来源和已知不完整之处。该索引是易错参考，不代表验证成功；后续应保留早期非 HTTP 路径、参数差异、模板简化限制及任何未完成或待核查结论。
  title: 当前任务导航：K3 请求与模板参考索引
navigation: 可检索本块以定位 GPQA 与多模态请求体、chat_template_kwargs、采样/后处理参数，以及 k3-chat-template.jinja 的报告来源和已知不完整之处。该索引是易错参考，不代表验证成功；后续应保留早期非 HTTP 路径、参数差异、模板简化限制及任何未完成或待核查结论。
schema: mindie-material-task/1
status: complete
task_id: 71e939b75359039b11221a658a72c7c4f428917a9a284a04069114142af232cc
---

# 当前任务导航：K3 请求与模板参考索引

可检索本块以定位 GPQA 与多模态请求体、chat_template_kwargs、采样/后处理参数，以及 k3-chat-template.jinja 的报告来源和已知不完整之处。该索引是易错参考，不代表验证成功；后续应保留早期非 HTTP 路径、参数差异、模板简化限制及任何未完成或待核查结论。

## Materials

- [K3 请求构造与 chat template 来源](blocks/7f110fba0e70d42ea97bb04acbc979dc7f352796fd298ed8fadbde2ce0b89722.md): 记录了该 session 中 GPQA、多模态 smoke、早期 LLM.generate() 三类请求：GPQA 使用 /v1/chat/completions、服务端 chat template、thinking=true 及采样参数，答案后处理由客户端正则提取；多模态 smoke 使用确定性参数、图像 content 数组，token 对齐 probe 另含 return_token_ids/logprobs。后续调查认为 k3-chat-template.jinja 不是权重自带文件，而是参考 K3 tokenizer/Python apply_chat_template() 临时改写的 vLLM 兼容模板；其对单轮 GPQA 文本基本适用，但未完整覆盖 tools、response_format、带 name 消息、历史 reasoning/response 分段和全部多模态渲染。上述来源、文件位置、复制记录和结论均属于该材料中的报告，模板完整等价性及正式 Serving 适用性仍存在不确定性。

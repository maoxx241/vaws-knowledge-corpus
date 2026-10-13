---
blocks:
- block_id: c6ac2301b358c1a680dd20b64d4b8359b759ffdf6d8272ca28469943a4d74aa7
  indexed: true
  sha256: cf9917fd65a6ebb5dc11ebfe91909facba1f47a6402326f2e0104e8453774cb4
  source_range:
    end: 1630720
    part: 0
    start: 0
  summary: 对 vLLM 0.23.0 的 Kimi K3 Chat/Responses、ParserManager/MRO、工具模式、流式与非流式状态机及 tokenizer 证据进行了多轮只读审计；早期发现 Responses 半支持、未跟踪运行时模块、chat_template_kwargs/structured_outputs 契约、工具选择、协议闭合和异常分类等 P0/P1 风险。后续复核确认 Responses 已在生成前拒绝、terminal envelope 已要求 MESSAGE_END，且有效输出路径未发现新的确定性 blocker，但仍记录了 tools + tool_choice:null 的兼容争议、未跟踪 kimi_k3_xtml.py 提交遗漏风险，以及真实 tokenizer 测试可因环境变量缺失而 skip 的证据局限；未修改文件、未运行会写缓存的测试、未访问远端。
  title: K3 XTML Parser 只读审计与收口复核
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 86b822d090c769de0b6880c6bcbd46f71fa6d924b166f79035fb721f560972c6
  kind: experience
  material_digest: b769ab91e91f92f88a6651ab76a18a89b207df0b672b9db76355451d08f0c14f
  revision: 0e2fb9c7c3a12504244cd02b0277132e6d8d7242f164f11bc545efd7ca686fbd
  schema: mindie-entry/3
  summary: 本批材料集中记录 Kimi K3 XTML parser 在 vLLM 0.23.0 上的只读审计、风险演变与收口复核。检索重点：Responses Chat-only 边界、MESSAGE_END/finish reason 协议完整性、none/auto/required/named 工具模式、tool_choice:null、chat_template_kwargs、structured_outputs、未跟踪运行时模块及 tokenizer 测试证据；后续结论应保留已接受设计边界、未完成的提交纳入事项和测试 skip 不等于验证成功。
  title: 当前任务导航：K3 Parser 只读审计
navigation: 本批材料集中记录 Kimi K3 XTML parser 在 vLLM 0.23.0 上的只读审计、风险演变与收口复核。检索重点：Responses Chat-only 边界、MESSAGE_END/finish reason 协议完整性、none/auto/required/named 工具模式、tool_choice:null、chat_template_kwargs、structured_outputs、未跟踪运行时模块及 tokenizer 测试证据；后续结论应保留已接受设计边界、未完成的提交纳入事项和测试 skip 不等于验证成功。
schema: mindie-material-task/1
status: complete
task_id: 86b822d090c769de0b6880c6bcbd46f71fa6d924b166f79035fb721f560972c6
---

# 当前任务导航：K3 Parser 只读审计

本批材料集中记录 Kimi K3 XTML parser 在 vLLM 0.23.0 上的只读审计、风险演变与收口复核。检索重点：Responses Chat-only 边界、MESSAGE_END/finish reason 协议完整性、none/auto/required/named 工具模式、tool_choice:null、chat_template_kwargs、structured_outputs、未跟踪运行时模块及 tokenizer 测试证据；后续结论应保留已接受设计边界、未完成的提交纳入事项和测试 skip 不等于验证成功。

## Materials

- [K3 XTML Parser 只读审计与收口复核](blocks/c6ac2301b358c1a680dd20b64d4b8359b759ffdf6d8272ca28469943a4d74aa7.md): 对 vLLM 0.23.0 的 Kimi K3 Chat/Responses、ParserManager/MRO、工具模式、流式与非流式状态机及 tokenizer 证据进行了多轮只读审计；早期发现 Responses 半支持、未跟踪运行时模块、chat_template_kwargs/structured_outputs 契约、工具选择、协议闭合和异常分类等 P0/P1 风险。后续复核确认 Responses 已在生成前拒绝、terminal envelope 已要求 MESSAGE_END，且有效输出路径未发现新的确定性 blocker，但仍记录了 tools + tool_choice:null 的兼容争议、未跟踪 kimi_k3_xtml.py 提交遗漏风险，以及真实 tokenizer 测试可因环境变量缺失而 skip 的证据局限；未修改文件、未运行会写缓存的测试、未访问远端。

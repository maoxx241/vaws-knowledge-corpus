---
blocks:
- block_id: 7e55ebb7e1e6ee64c69bd2f0c20b20084931a1f4ec3ec7dd18a41f07dbfb2240
  indexed: true
  sha256: 51d32a851657b4e5d0963d0428565bd76b4973e4ca9a54b6879c909c5e70d2de
  source_range:
    end: 1226111
    part: 0
    start: 0
  summary: 用户认为历史上针对 K=7、16-token 图（1 个真实 8-token request 加 1 个零长度 dummy row）的 padding 补丁未解决问题，建议改用 SGLang 做快速对比；该判断和补丁效果均未被验证。助手报告已完成只读对照设计，但未修改文件、未启停服务；计划对齐首轮 8-token verify chain、padding 元数据、cache 映射、KDA 状态及 target hidden/logits，且指出现有 SGLang 探针不覆盖 target KDA/logits。报告中的关键未证实假设是 vLLM 原地复用 state pool，而 SGLang 使用 persistent initial state 加 speculative scratch 并在 accept 后 commit。
  title: SGLang/vLLM 对比定位 K=7 verify 问题
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d0722c7fd7e35fb71f1b20e93c7321065b6e00c99160e3c12db2a94b5c2c6440
  kind: experience
  material_digest: 130f71d7bca9e107e72cbfe8603f9ad55e61bf67c2c56d130e08adb0fca7e041
  revision: 15e4a6d850220b1c7730ae0333d21b2c6931cc704f43c1d888d09a7162e16209
  schema: mindie-entry/3
  summary: 当前批次记录了一个未验证的排查方向：历史 padding 补丁据用户反馈未解决 K=7、16-token 图问题，因此建议用 SGLang 与 vLLM 对比定位。已报告的只读设计涉及 verify chain、padding/cache 映射、KDA 初态与终态、target hidden/logits，以及 ACLGraph replay 时序；尚未执行对比、修改文件或启动服务，现有 SGLang 探针对 target KDA/logits 覆盖不足。历史补丁、实现语义差异和排查设计都应继续作为可检索的假设或报告内容，不应视为已验证修复。
  title: 当前任务导航：K=7 verify 异常的 SGLang/vLLM 对照
navigation: 当前批次记录了一个未验证的排查方向：历史 padding 补丁据用户反馈未解决 K=7、16-token 图问题，因此建议用 SGLang 与 vLLM 对比定位。已报告的只读设计涉及 verify chain、padding/cache 映射、KDA 初态与终态、target hidden/logits，以及 ACLGraph replay 时序；尚未执行对比、修改文件或启动服务，现有 SGLang 探针对 target KDA/logits 覆盖不足。历史补丁、实现语义差异和排查设计都应继续作为可检索的假设或报告内容，不应视为已验证修复。
schema: mindie-material-task/1
status: complete
task_id: d0722c7fd7e35fb71f1b20e93c7321065b6e00c99160e3c12db2a94b5c2c6440
---

# 当前任务导航：K=7 verify 异常的 SGLang/vLLM 对照

当前批次记录了一个未验证的排查方向：历史 padding 补丁据用户反馈未解决 K=7、16-token 图问题，因此建议用 SGLang 与 vLLM 对比定位。已报告的只读设计涉及 verify chain、padding/cache 映射、KDA 初态与终态、target hidden/logits，以及 ACLGraph replay 时序；尚未执行对比、修改文件或启动服务，现有 SGLang 探针对 target KDA/logits 覆盖不足。历史补丁、实现语义差异和排查设计都应继续作为可检索的假设或报告内容，不应视为已验证修复。

## Materials

- [SGLang/vLLM 对比定位 K=7 verify 问题](blocks/7e55ebb7e1e6ee64c69bd2f0c20b20084931a1f4ec3ec7dd18a41f07dbfb2240.md): 用户认为历史上针对 K=7、16-token 图（1 个真实 8-token request 加 1 个零长度 dummy row）的 padding 补丁未解决问题，建议改用 SGLang 做快速对比；该判断和补丁效果均未被验证。助手报告已完成只读对照设计，但未修改文件、未启停服务；计划对齐首轮 8-token verify chain、padding 元数据、cache 映射、KDA 状态及 target hidden/logits，且指出现有 SGLang 探针不覆盖 target KDA/logits。报告中的关键未证实假设是 vLLM 原地复用 state pool，而 SGLang 使用 persistent initial state 加 speculative scratch 并在 accept 后 commit。

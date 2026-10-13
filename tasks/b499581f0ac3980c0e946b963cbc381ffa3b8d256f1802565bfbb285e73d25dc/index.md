---
blocks:
- block_id: b41d94604569487e4bdae047855bb659050d3989b63aa1f845aaf948a7259301
  indexed: true
  sha256: 00af8425a79a1b17d7e82c2c7ff9cfa3c2e62bc9091b518da2abe020e5b70c28
  source_range:
    end: 637849
    part: 0
    start: 0
  summary: 记录了一次只读排查：报告称 target/draft 词表及 BOS/EOS/PAD 一致，DSpark 目录继承 target tokenizer，并使用 `tokenizer-mode kimi_k3` 原生 XTML；但这些结论仍属于该消息中的分析结果，而非独立验证的成功。SpecForge GSM8K raw completions 报告为 32/32 正确、接收率 `2671/4697 = 56.866%`，中文长 chat 报告为 `10.64%`，可能涉及语言、长自由生成或 thinking 条件分布；SGLang issue 只被描述为“能跑”且未提供接收率基线。待核对项包括 raw completions 是否自动加入 BOS、chat `none/max` 的 token 边界、scheduler 输入 IDs、各位置接受直方图，以及 eager 与 ACLGraph 的差异；应保留“raw 正常/chat 低”“ACLGraph 连续 `None`”等尚未证实的诊断分支。
  title: DSpark 接收率与 tokenizer/chat-template 排查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b499581f0ac3980c0e946b963cbc381ffa3b8d256f1802565bfbb285e73d25dc
  kind: experience
  material_digest: f5e0baa768fb315e45e9627080e7b26078a328a6710265457c677fbefbe43db4
  revision: a2e136db645fb0392275cc9539a7d14092afaf39de41cea687f3d5fce62b5fc1
  schema: mindie-entry/3
  summary: 当前批次集中在 tokenizer 继承关系、Kimi K3 原生 XTML、raw/chat 请求的 token 边界，以及 speculative decoding 接收率差异。已有报告的 GSM8K、中文 chat 和 SGLang 对比结果均应视为来源中的观测或判断，不能当作已验证成功；后续检索优先关注 BOS、thinking `none/max`、PAD/DSpark mask、scheduler 实际 IDs、首次拒绝位置及 eager/ACLGraph 对照。
  title: 当前任务导航：DSpark 接收率与输入 token 边界
navigation: 当前批次集中在 tokenizer 继承关系、Kimi K3 原生 XTML、raw/chat 请求的 token 边界，以及 speculative decoding 接收率差异。已有报告的 GSM8K、中文 chat 和 SGLang 对比结果均应视为来源中的观测或判断，不能当作已验证成功；后续检索优先关注 BOS、thinking `none/max`、PAD/DSpark mask、scheduler 实际 IDs、首次拒绝位置及 eager/ACLGraph 对照。
schema: mindie-material-task/1
status: complete
task_id: b499581f0ac3980c0e946b963cbc381ffa3b8d256f1802565bfbb285e73d25dc
---

# 当前任务导航：DSpark 接收率与输入 token 边界

当前批次集中在 tokenizer 继承关系、Kimi K3 原生 XTML、raw/chat 请求的 token 边界，以及 speculative decoding 接收率差异。已有报告的 GSM8K、中文 chat 和 SGLang 对比结果均应视为来源中的观测或判断，不能当作已验证成功；后续检索优先关注 BOS、thinking `none/max`、PAD/DSpark mask、scheduler 实际 IDs、首次拒绝位置及 eager/ACLGraph 对照。

## Materials

- [DSpark 接收率与 tokenizer/chat-template 排查](blocks/b41d94604569487e4bdae047855bb659050d3989b63aa1f845aaf948a7259301.md): 记录了一次只读排查：报告称 target/draft 词表及 BOS/EOS/PAD 一致，DSpark 目录继承 target tokenizer，并使用 `tokenizer-mode kimi_k3` 原生 XTML；但这些结论仍属于该消息中的分析结果，而非独立验证的成功。SpecForge GSM8K raw completions 报告为 32/32 正确、接收率 `2671/4697 = 56.866%`，中文长 chat 报告为 `10.64%`，可能涉及语言、长自由生成或 thinking 条件分布；SGLang issue 只被描述为“能跑”且未提供接收率基线。待核对项包括 raw completions 是否自动加入 BOS、chat `none/max` 的 token 边界、scheduler 输入 IDs、各位置接受直方图，以及 eager 与 ACLGraph 的差异；应保留“raw 正常/chat 低”“ACLGraph 连续 `None`”等尚未证实的诊断分支。

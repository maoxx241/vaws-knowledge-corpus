---
blocks:
- block_id: 85b4a63d3a095048f8367538f3b48f52ece0ff157956e7257781e96820ebb5e8
  indexed: true
  sha256: cce7272b3ab3d258475ef260aad2ff6e1adca711270b6016ff97e6f725b57b47
  source_range:
    end: 369719
    part: 0
    start: 0
  summary: '来源内容称，Kimi K3 的公开口径包括推理强度 `max`、`temperature=1.0`、`top_p=1.0`，MMMU-Pro 遵循官方 protocol，图片置于文本之前；多模态成绩（ZeroBench 除外）据称为 3 次运行平均值，但其中的具体汇总值包含一个已编辑的 `<redacted:username-at-host:63a893da8c61>` 占位符。MMMU-Pro 后处理被描述为优先提取末行精确格式 `Answer: <LETTER>`，再回退到括号、选项字母或选项文本匹配，无法解析时据称随机选择；Kimi K3 专属的 `max_tokens`、seed、repetition penalty 和额外清洗规则仍被标为未披露或待发布，以上均是报告内容而非本索引验证过的结论。'
  title: Kimi K3 在 MMMU-Pro 上的推理与答案后处理线索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 18a09b97fefba6d2ed95832aa0bd477d9ae6e4af11fc57cf915e89b79b91476d
  kind: experience
  material_digest: 448e2e2da710d59dac55193fde26aff4fa57b567cc9fdacc9db42c859518d5ca
  revision: 24cfce1c83013f439554d790a2eeba483775f740a70bc76fa7224f28258a7b6c
  schema: mindie-entry/3
  summary: 当前材料聚焦 Kimi K3 在 MMMU-Pro 上的推理参数、输入顺序、重复运行汇总及答案解析回退规则。关键未决项包括 Kimi K3 专属生成参数和额外清洗规则；来源中还保留了成绩汇总中的编辑占位符，相关内容应作为可检索的未核实参考。
  title: MMMU-Pro 评测后处理与 Kimi K3 设置
navigation: 当前材料聚焦 Kimi K3 在 MMMU-Pro 上的推理参数、输入顺序、重复运行汇总及答案解析回退规则。关键未决项包括 Kimi K3 专属生成参数和额外清洗规则；来源中还保留了成绩汇总中的编辑占位符，相关内容应作为可检索的未核实参考。
schema: mindie-material-task/1
status: complete
task_id: 18a09b97fefba6d2ed95832aa0bd477d9ae6e4af11fc57cf915e89b79b91476d
---

# MMMU-Pro 评测后处理与 Kimi K3 设置

当前材料聚焦 Kimi K3 在 MMMU-Pro 上的推理参数、输入顺序、重复运行汇总及答案解析回退规则。关键未决项包括 Kimi K3 专属生成参数和额外清洗规则；来源中还保留了成绩汇总中的编辑占位符，相关内容应作为可检索的未核实参考。

## Materials

- [Kimi K3 在 MMMU-Pro 上的推理与答案后处理线索](blocks/85b4a63d3a095048f8367538f3b48f52ece0ff157956e7257781e96820ebb5e8.md): 来源内容称，Kimi K3 的公开口径包括推理强度 `max`、`temperature=1.0`、`top_p=1.0`，MMMU-Pro 遵循官方 protocol，图片置于文本之前；多模态成绩（ZeroBench 除外）据称为 3 次运行平均值，但其中的具体汇总值包含一个已编辑的 `<redacted:username-at-host:63a893da8c61>` 占位符。MMMU-Pro 后处理被描述为优先提取末行精确格式 `Answer: <LETTER>`，再回退到括号、选项字母或选项文本匹配，无法解析时据称随机选择；Kimi K3 专属的 `max_tokens`、seed、repetition penalty 和额外清洗规则仍被标为未披露或待发布，以上均是报告内容而非本索引验证过的结论。

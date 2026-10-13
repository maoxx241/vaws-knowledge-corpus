---
blocks:
- block_id: 0f67ea772c5b6621cf2bbadb86bfe0940b615c2a6a2276c2b36c1bfdf8ec9c18
  indexed: true
  sha256: 6175e21d60e86c2d66ffdbea0940e9a77bdc48d78a3f9dc77cd9114b7972101e
  source_range:
    end: 4381176
    part: 0
    start: 0
  summary: '已整理 GSM8K 官方 test 集（1319 条）、5-shot SpecForge 风格 prompt、greedy completion、`/metrics` 计数器差分脚本，以及 PR #14454 四节点启动脚本和非 QuaRot/QuaRot sweep 记录；报告称相关 SHA256、Python AST、Shell 语法、CSV 字段和压缩包内容检查通过。历史固定 gate 为 32/32 正确、4690 个 draft token、2654 个 accepted draft token、56.588%，但这些结果及检查应视为来源中的报告，不能提升为独立验证成功。PR 评论未提供长上下文 prompt 生成器或语料，因此长上下文部分仅是参数与历史结果复现表，包内压缩包路径、README 路径和 SHA256 均含有已保留的 `<redacted:local-path:...>` 占位符；工作树原有冲突未改动，也未启动或停止远程服务。'
  title: DSpark 采信率复现材料整理与边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: eca89de8f1f2e7220957f1a39f141c8892176a341b53cbad4a74019143ca6bef
  kind: experience
  material_digest: 285c7761886ee464a424a31dd9eda7ed9bcf9d3352a9531609804d5750271171
  revision: 28277be90b4eeca5007c2248a162dffce30bd09d96bdc847706bdd39434f24c9
  schema: mindie-entry/3
  summary: '当前唯一新增材料是 DSpark 采信率与 PR #14454 复现资料整理记录，重点检索 GSM8K 数据集、采信率计算、四节点启动脚本、QuaRot 对比、历史 sweep、SHA256/语法校验及长上下文复现边界。材料明确区分可携带的 GSM8K corpus 与缺少原始生成器/语料的长上下文记录；校验通过和复现结果均保持为来源中的报告，不视为已独立验证的成功。'
  title: 当前任务导航：DSpark 采信率材料索引
navigation: '当前唯一新增材料是 DSpark 采信率与 PR #14454 复现资料整理记录，重点检索 GSM8K 数据集、采信率计算、四节点启动脚本、QuaRot 对比、历史 sweep、SHA256/语法校验及长上下文复现边界。材料明确区分可携带的 GSM8K corpus 与缺少原始生成器/语料的长上下文记录；校验通过和复现结果均保持为来源中的报告，不视为已独立验证的成功。'
schema: mindie-material-task/1
status: complete
task_id: eca89de8f1f2e7220957f1a39f141c8892176a341b53cbad4a74019143ca6bef
---

# 当前任务导航：DSpark 采信率材料索引

当前唯一新增材料是 DSpark 采信率与 PR #14454 复现资料整理记录，重点检索 GSM8K 数据集、采信率计算、四节点启动脚本、QuaRot 对比、历史 sweep、SHA256/语法校验及长上下文复现边界。材料明确区分可携带的 GSM8K corpus 与缺少原始生成器/语料的长上下文记录；校验通过和复现结果均保持为来源中的报告，不视为已独立验证的成功。

## Materials

- [DSpark 采信率复现材料整理与边界](blocks/0f67ea772c5b6621cf2bbadb86bfe0940b615c2a6a2276c2b36c1bfdf8ec9c18.md): 已整理 GSM8K 官方 test 集（1319 条）、5-shot SpecForge 风格 prompt、greedy completion、`/metrics` 计数器差分脚本，以及 PR #14454 四节点启动脚本和非 QuaRot/QuaRot sweep 记录；报告称相关 SHA256、Python AST、Shell 语法、CSV 字段和压缩包内容检查通过。历史固定 gate 为 32/32 正确、4690 个 draft token、2654 个 accepted draft token、56.588%，但这些结果及检查应视为来源中的报告，不能提升为独立验证成功。PR 评论未提供长上下文 prompt 生成器或语料，因此长上下文部分仅是参数与历史结果复现表，包内压缩包路径、README 路径和 SHA256 均含有已保留的 `<redacted:local-path:...>` 占位符；工作树原有冲突未改动，也未启动或停止远程服务。

---
blocks:
- block_id: 4d119a9820db8d9a9cbaa590f3e8bf4ee91708f1fd54fd61c8577553d1cf78b8
  indexed: true
  sha256: d4df7fc52098c9d6eb918ac9012728c268c8abf67bfc091bbe44b7ca0a17c262
  source_range:
    end: 308964
    part: 0
    start: 0
  summary: 据报告，`vaws_knowledge/markdown.py`、`corpus_check.py` 和 `distribution/build.py` 的下游逻辑要求合法相对路径、`.md` 后缀及内容 SHA256 记录，但未验证文件名必须含 hash 前缀；原相对路径会直接拼入 URI 并交给 OpenViking `client.write()`。因此，12 位 hex 被判断为贡献层命名约定，而非 Markdown 导入或 OpenViking 适配层的必需格式。该结论是未通过代码修改、测试或联网验证的报告性判断，相关路径与结论仍应视为可检索但未核实的参考信息。
  title: vaws-knowledge 文件名 Hash 前缀要求核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 839ee2df2f05bb12535964be8464fdb2fe53ffae5a856b38e4cda4981afa3ce6
  kind: experience
  material_digest: 43bf458fd96acd80232b87cf4a541031063b9c75591ae8a3933a6b433c426c87
  revision: 7bafee2c1e5d6849dc8d96cba158b1a80a5180733bf469cf84005b7a6b899130
  schema: mindie-entry/3
  summary: 新材料集中讨论 vaws-knowledge 下游是否要求文件名带 hash 前缀：报告称导入与 OpenViking 适配层不要求，但将 12 位 hex 归因于贡献层约定。未修改代码、运行测试或联网；该判断及其证据范围均未独立核实，原始材料应继续单独检索。
  title: 当前任务导航：贡献层命名约定与下游导入要求
navigation: 新材料集中讨论 vaws-knowledge 下游是否要求文件名带 hash 前缀：报告称导入与 OpenViking 适配层不要求，但将 12 位 hex 归因于贡献层约定。未修改代码、运行测试或联网；该判断及其证据范围均未独立核实，原始材料应继续单独检索。
schema: mindie-material-task/1
status: complete
task_id: 839ee2df2f05bb12535964be8464fdb2fe53ffae5a856b38e4cda4981afa3ce6
---

# 当前任务导航：贡献层命名约定与下游导入要求

新材料集中讨论 vaws-knowledge 下游是否要求文件名带 hash 前缀：报告称导入与 OpenViking 适配层不要求，但将 12 位 hex 归因于贡献层约定。未修改代码、运行测试或联网；该判断及其证据范围均未独立核实，原始材料应继续单独检索。

## Materials

- [vaws-knowledge 文件名 Hash 前缀要求核查](blocks/4d119a9820db8d9a9cbaa590f3e8bf4ee91708f1fd54fd61c8577553d1cf78b8.md): 据报告，`vaws_knowledge/markdown.py`、`corpus_check.py` 和 `distribution/build.py` 的下游逻辑要求合法相对路径、`.md` 后缀及内容 SHA256 记录，但未验证文件名必须含 hash 前缀；原相对路径会直接拼入 URI 并交给 OpenViking `client.write()`。因此，12 位 hex 被判断为贡献层命名约定，而非 Markdown 导入或 OpenViking 适配层的必需格式。该结论是未通过代码修改、测试或联网验证的报告性判断，相关路径与结论仍应视为可检索但未核实的参考信息。

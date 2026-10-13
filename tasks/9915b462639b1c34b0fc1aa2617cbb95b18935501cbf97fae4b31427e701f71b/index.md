---
blocks:
- block_id: 61e9576729281da79e27eb53658654789437f490ddf14acf403094ba67b5b4cb
  indexed: true
  sha256: 5da7664172fee60f6ed740f5e48077abf6a05136811d6349d60c764680dad444
  source_range:
    end: 1976729
    part: 0
    start: 0
  summary: 记录了稳定贡献身份与路径语义、pending/v2 的 candidate_keys 和 CAS revision，以及 GitHub Issue reactions 反馈方案；后续反馈改为可累积的独立评论事件，以 author/request_id 去重，并在响应丢失时读回或返回可重试 ID。多次 rebase 均声称工作树 clean、未 push，冲突限于 pin 文件；测试结果从 49/82 项到 114 passed，另有一次 corpus 验收声称 52 篇文档、0 个问题，但这些均为记录中的报告而非本索引验证的成功。保留的限制包括首次创建 Issue 的并发竞争、未执行真实远端写入、未跑全套测试，以及若干路径、提交和 owner pin 约束。
  title: 贡献身份、反馈事件与 rebase 验收记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9915b462639b1c34b0fc1aa2617cbb95b18935501cbf97fae4b31427e701f71b
  kind: experience
  material_digest: 2b95bad61ea9747339a465838e20eaf1f1dfb4adf296346d7f58baaef1e7b728
  revision: 31ff158d91c31aea17f5e4c60b6e7fc9fb97202d60b4adbbe1e2a55c0dec4068
  schema: mindie-entry/3
  summary: 当前材料覆盖稳定身份路径、公开经验反馈、评论事件累计与幂等重试、consumer 工作树 rebase，以及 corpus/pin 检查。后续修订将 reactions 投票改为独立评论事件，并保留历史反馈；rebase 记录显示未 push，验收与测试数字均属报告性结果，不能视为已独立验证。未提供的早期细节仍应通过原始块独立检索。
  title: 贡献系统与反馈机制：实现、修订、同步和验收记录
navigation: 当前材料覆盖稳定身份路径、公开经验反馈、评论事件累计与幂等重试、consumer 工作树 rebase，以及 corpus/pin 检查。后续修订将 reactions 投票改为独立评论事件，并保留历史反馈；rebase 记录显示未 push，验收与测试数字均属报告性结果，不能视为已独立验证。未提供的早期细节仍应通过原始块独立检索。
schema: mindie-material-task/1
status: complete
task_id: 9915b462639b1c34b0fc1aa2617cbb95b18935501cbf97fae4b31427e701f71b
---

# 贡献系统与反馈机制：实现、修订、同步和验收记录

当前材料覆盖稳定身份路径、公开经验反馈、评论事件累计与幂等重试、consumer 工作树 rebase，以及 corpus/pin 检查。后续修订将 reactions 投票改为独立评论事件，并保留历史反馈；rebase 记录显示未 push，验收与测试数字均属报告性结果，不能视为已独立验证。未提供的早期细节仍应通过原始块独立检索。

## Materials

- [贡献身份、反馈事件与 rebase 验收记录](blocks/61e9576729281da79e27eb53658654789437f490ddf14acf403094ba67b5b4cb.md): 记录了稳定贡献身份与路径语义、pending/v2 的 candidate_keys 和 CAS revision，以及 GitHub Issue reactions 反馈方案；后续反馈改为可累积的独立评论事件，以 author/request_id 去重，并在响应丢失时读回或返回可重试 ID。多次 rebase 均声称工作树 clean、未 push，冲突限于 pin 文件；测试结果从 49/82 项到 114 passed，另有一次 corpus 验收声称 52 篇文档、0 个问题，但这些均为记录中的报告而非本索引验证的成功。保留的限制包括首次创建 Issue 的并发竞争、未执行真实远端写入、未跑全套测试，以及若干路径、提交和 owner pin 约束。

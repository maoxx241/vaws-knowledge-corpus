---
blocks:
- block_id: 78b9d30de02833359a05cfddf8597c9d83891f89c17c27bf9ad6ad9fe2e5b44a
  indexed: true
  sha256: ad59eb51fbf5ddb67543c381489a3a94ff1fb7e7e4704a1d3a6eccce29824a71
  source_range:
    end: 2400187
    part: 0
    start: 0
  summary: '来源称依赖审阅发现并离线复现 3 项阻断：#87 忙卡分配绕过、#94 释放竞态/锁泄漏，以及 remote-dev #3 ledger 碰撞；相关报告和修复契约路径均已脱敏。建议保留 #89 修复后的测试并剔除旧 `.remote-dev` 测试；#90 的两个 CI job 各跳过 3 项外部集成，但新 pin 仍待验证。来源明确表示未改已有源码/worktree、未发布或合并，且 Grok 实现尚未验收，因此这些内容不应视为已验证成功。'
  title: 依赖审阅：三项离线复现阻断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: fc0ae70041bf777bbff7b5175a6d4e8f9d8a3b9825fd7fcff2491b4be71c1d72
  kind: experience
  material_digest: 2bbcd933420e17e1302b81edc6e3e6a767ba976eba42d3463013384e5fe862ab
  revision: e8c883960eea3215df05327fe836bece4bfc6fdd20ffc6b96dc2ae3b0ff7a46c
  schema: mindie-entry/3
  summary: 本批次记录了 3 项据称可离线复现的依赖阻断及对应修复契约，并保留了测试清理、CI 外部集成跳过和新 pin 验证等未完成事项。源码/worktree 未改动，未发布或合并，Grok 实现尚未验收；原始块仍可独立检索。
  title: 当前任务导航：依赖审阅与验收准备
navigation: 本批次记录了 3 项据称可离线复现的依赖阻断及对应修复契约，并保留了测试清理、CI 外部集成跳过和新 pin 验证等未完成事项。源码/worktree 未改动，未发布或合并，Grok 实现尚未验收；原始块仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: fc0ae70041bf777bbff7b5175a6d4e8f9d8a3b9825fd7fcff2491b4be71c1d72
---

# 当前任务导航：依赖审阅与验收准备

本批次记录了 3 项据称可离线复现的依赖阻断及对应修复契约，并保留了测试清理、CI 外部集成跳过和新 pin 验证等未完成事项。源码/worktree 未改动，未发布或合并，Grok 实现尚未验收；原始块仍可独立检索。

## Materials

- [依赖审阅：三项离线复现阻断](blocks/78b9d30de02833359a05cfddf8597c9d83891f89c17c27bf9ad6ad9fe2e5b44a.md): 来源称依赖审阅发现并离线复现 3 项阻断：#87 忙卡分配绕过、#94 释放竞态/锁泄漏，以及 remote-dev #3 ledger 碰撞；相关报告和修复契约路径均已脱敏。建议保留 #89 修复后的测试并剔除旧 `.remote-dev` 测试；#90 的两个 CI job 各跳过 3 项外部集成，但新 pin 仍待验证。来源明确表示未改已有源码/worktree、未发布或合并，且 Grok 实现尚未验收，因此这些内容不应视为已验证成功。

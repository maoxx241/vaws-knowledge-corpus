---
blocks:
- block_id: 69e288441ddbcceb998f2503970a2d904cf71fb49624e4ab12cee7a921c831fc
  indexed: true
  sha256: 958822db3ff73b44f23fef54d252cd830db232b735910ff236fdd5da0ab6a0a6
  source_range:
    end: 264865
    part: 0
    start: 0
  summary: '讨论了将 releases/v0.23.0_ci_new 分支中 PR #4 与 PR #6 之间的多个小提交折叠为单个提交；记录认为 PR #4 已合并，重写发布分支历史会影响共享分支，因此建议保留历史或在后续 PR 使用 Squash and merge。随后讨论仓库级合并设置，来源声称已将 vllm-ascend/vllm-ascend-kimi-k3 配置为仅允许 Squash merge，并禁用 merge commit 与 rebase merge；该设置结果在材料中未提供独立验证。'
  title: 整理 Kimi K3 分支提交与合并策略
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e1b17107970410ff19a860846dc00d6f519abba8442534e5ebb63945ea32f554
  kind: experience
  material_digest: 77e301f954af6e64a90e9bd32965dbcc1cac1729ba444f91c5a84992f4ee8131
  revision: 03ea24fb25e6f889c996fb9007b56d099221a2ead1a6fe5ac514e7cefbf83773
  schema: mindie-entry/3
  summary: '材料涉及 PR #4/#6、releases/v0.23.0_ci_new 分支、提交折叠及 GitHub 合并策略设置。保留的关键不确定性是：分支历史重写会影响共享分支，而“仅允许 Squash merge”被报告为已设置但未在材料中独立核实；已合并历史不会因默认策略改变而自动更新。'
  title: 当前任务导航：Git 历史整理与合并策略
navigation: '材料涉及 PR #4/#6、releases/v0.23.0_ci_new 分支、提交折叠及 GitHub 合并策略设置。保留的关键不确定性是：分支历史重写会影响共享分支，而“仅允许 Squash merge”被报告为已设置但未在材料中独立核实；已合并历史不会因默认策略改变而自动更新。'
schema: mindie-material-task/1
status: complete
task_id: e1b17107970410ff19a860846dc00d6f519abba8442534e5ebb63945ea32f554
---

# 当前任务导航：Git 历史整理与合并策略

材料涉及 PR #4/#6、releases/v0.23.0_ci_new 分支、提交折叠及 GitHub 合并策略设置。保留的关键不确定性是：分支历史重写会影响共享分支，而“仅允许 Squash merge”被报告为已设置但未在材料中独立核实；已合并历史不会因默认策略改变而自动更新。

## Materials

- [整理 Kimi K3 分支提交与合并策略](blocks/69e288441ddbcceb998f2503970a2d904cf71fb49624e4ab12cee7a921c831fc.md): 讨论了将 releases/v0.23.0_ci_new 分支中 PR #4 与 PR #6 之间的多个小提交折叠为单个提交；记录认为 PR #4 已合并，重写发布分支历史会影响共享分支，因此建议保留历史或在后续 PR 使用 Squash and merge。随后讨论仓库级合并设置，来源声称已将 vllm-ascend/vllm-ascend-kimi-k3 配置为仅允许 Squash merge，并禁用 merge commit 与 rebase merge；该设置结果在材料中未提供独立验证。

---
blocks:
- block_id: f2f3b30c70fd36947a0b8810940750888b72348de72424f3771cad95dc199f43
  indexed: true
  sha256: 11ecd14d1d1a03a30c57e1091fe0bb7e5d30a7ed3d8b591294658a26909b233a
  source_range:
    end: 4963283
    part: 0
    start: 0
  summary: '从 ops-transformer MR #10462（commit `f8018c86`）迁入 FlashMlaWithKvcache、AICPU metadata 算子及必要的 arch35/common 依赖到 PR #15336，提交为 `c643d46bc`，并通过 fast-forward 推送；自定义 OPP、Torch schema 和实现均限制在 `ascend950`/A5，A2/A3 不注册。静态契约、格式、依赖、容量公式、DCO、冲突检查和 PR 元数据检查通过，但 GitHub pre-commit 当时仍在运行；本机因缺少 `torch` 无法加载 pytest，A5 自定义算子编译和 NPU 实机运行尚未执行，不能视为运行验证。原工作区存在未解决冲突但未改动，迁移在独立干净工作树完成。'
  title: '将 Flash MLA with KV cache 迁移至 PR #15336 的 A5 专用实现'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9db00803eb852cd978e9fe96577198e33bee96b0134e38ae906f4c0e305ecea9
  kind: experience
  material_digest: 437c69e2aefbeaa36d3ca828f6d4211f3712d23be8b99687ca74a95b431ad467
  revision: 420bcc3bb7a422b3a2b803ea4c8ff8ef489ff23234c0f29497d3cc558d15a1b4
  schema: mindie-entry/3
  summary: '已完成从 MR #10462 到 PR #15336 的 A5 专用迁移并推送 `c643d46bc`；静态检查及多项仓库检查通过，但 pre-commit 状态未完结。后续检索应重点关注 GitHub pre-commit 结果、A5 构建与 NPU 实机验证，以及缺少 `torch` 导致的 pytest 未执行；这些未完成项不能由已通过的静态检查替代。'
  title: '当前任务导航：PR #15336 的 A5 Flash MLA 迁移索引'
navigation: '已完成从 MR #10462 到 PR #15336 的 A5 专用迁移并推送 `c643d46bc`；静态检查及多项仓库检查通过，但 pre-commit 状态未完结。后续检索应重点关注 GitHub pre-commit 结果、A5 构建与 NPU 实机验证，以及缺少 `torch` 导致的 pytest 未执行；这些未完成项不能由已通过的静态检查替代。'
schema: mindie-material-task/1
status: complete
task_id: 9db00803eb852cd978e9fe96577198e33bee96b0134e38ae906f4c0e305ecea9
---

# 当前任务导航：PR #15336 的 A5 Flash MLA 迁移索引

已完成从 MR #10462 到 PR #15336 的 A5 专用迁移并推送 `c643d46bc`；静态检查及多项仓库检查通过，但 pre-commit 状态未完结。后续检索应重点关注 GitHub pre-commit 结果、A5 构建与 NPU 实机验证，以及缺少 `torch` 导致的 pytest 未执行；这些未完成项不能由已通过的静态检查替代。

## Materials

- [将 Flash MLA with KV cache 迁移至 PR #15336 的 A5 专用实现](blocks/f2f3b30c70fd36947a0b8810940750888b72348de72424f3771cad95dc199f43.md): 从 ops-transformer MR #10462（commit `f8018c86`）迁入 FlashMlaWithKvcache、AICPU metadata 算子及必要的 arch35/common 依赖到 PR #15336，提交为 `c643d46bc`，并通过 fast-forward 推送；自定义 OPP、Torch schema 和实现均限制在 `ascend950`/A5，A2/A3 不注册。静态契约、格式、依赖、容量公式、DCO、冲突检查和 PR 元数据检查通过，但 GitHub pre-commit 当时仍在运行；本机因缺少 `torch` 无法加载 pytest，A5 自定义算子编译和 NPU 实机运行尚未执行，不能视为运行验证。原工作区存在未解决冲突但未改动，迁移在独立干净工作树完成。

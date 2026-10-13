---
blocks:
- block_id: d2c794704065312c48546df2fbb70117b52f09297acceb66bb11c191427ccedf
  indexed: true
  sha256: 8b2e6c2abd6e6e697e90a015b08c84ffd7c9f39e25f8fdff6867c79ccf801153
  source_range:
    end: 734453
    part: 0
    start: 0
  summary: '该材料记录了将 Kimi K3 attention-residual Triton 算子接入 `ci-new` 分支的过程，目标限定为单算子和整网 shape 检查，不做四机验证或 `.so` 构建；涉及 `H=7168`、`B=1..8`、BF16、decode token shape 及 `N=129`。助手报告称单卡调用、pytest、路由/shape-anchor 检查均通过，并提交了 `bff11b5a1`，随后创建了 Draft PR #49；这些通过结果和环境清理状态均应视为报告内容，而非独立核验结论。之后材料称最新 `ci-new` 为 `8f192c9f`，功能提交被重放为 `e1f4f30cd`，使用 `--force-with-lease` 更新 PR，保留了 rebase 前备份分支。'
  title: Kimi K3 Triton attention-residual 接入与 PR
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0f8b40415636dee53f42755bf5a42eb96f120bc3747ff718b80d46237d40700f
  kind: experience
  material_digest: 0b3061e9e47387f95e0769430c74facb36b7108b93963cf5498fe9566df60d2c
  revision: 1a6f67aa49b829dc0a7adb6077a627f7c1a33914a94a77188de2d51ab5c958da
  schema: mindie-entry/3
  summary: '核心材料集中于 Kimi K3 attention-residual 的 Triton 实现、shape 覆盖、单卡验证、分支 rebase 和 PR #49；原始内容仍可独立检索。当前记录包含未进行四机验证、未构建 `.so`、GitHub 首次 head 解析失败后重试，以及 rebase 后新提交和备份分支等上下文；所谓测试通过、可合并和 CI 排队状态均是材料中的报告，不能视为已验证事实。'
  title: 当前任务导航：Kimi K3 Triton 算子 PR
navigation: '核心材料集中于 Kimi K3 attention-residual 的 Triton 实现、shape 覆盖、单卡验证、分支 rebase 和 PR #49；原始内容仍可独立检索。当前记录包含未进行四机验证、未构建 `.so`、GitHub 首次 head 解析失败后重试，以及 rebase 后新提交和备份分支等上下文；所谓测试通过、可合并和 CI 排队状态均是材料中的报告，不能视为已验证事实。'
schema: mindie-material-task/1
status: complete
task_id: 0f8b40415636dee53f42755bf5a42eb96f120bc3747ff718b80d46237d40700f
---

# 当前任务导航：Kimi K3 Triton 算子 PR

核心材料集中于 Kimi K3 attention-residual 的 Triton 实现、shape 覆盖、单卡验证、分支 rebase 和 PR #49；原始内容仍可独立检索。当前记录包含未进行四机验证、未构建 `.so`、GitHub 首次 head 解析失败后重试，以及 rebase 后新提交和备份分支等上下文；所谓测试通过、可合并和 CI 排队状态均是材料中的报告，不能视为已验证事实。

## Materials

- [Kimi K3 Triton attention-residual 接入与 PR](blocks/d2c794704065312c48546df2fbb70117b52f09297acceb66bb11c191427ccedf.md): 该材料记录了将 Kimi K3 attention-residual Triton 算子接入 `ci-new` 分支的过程，目标限定为单算子和整网 shape 检查，不做四机验证或 `.so` 构建；涉及 `H=7168`、`B=1..8`、BF16、decode token shape 及 `N=129`。助手报告称单卡调用、pytest、路由/shape-anchor 检查均通过，并提交了 `bff11b5a1`，随后创建了 Draft PR #49；这些通过结果和环境清理状态均应视为报告内容，而非独立核验结论。之后材料称最新 `ci-new` 为 `8f192c9f`，功能提交被重放为 `e1f4f30cd`，使用 `--force-with-lease` 更新 PR，保留了 rebase 前备份分支。

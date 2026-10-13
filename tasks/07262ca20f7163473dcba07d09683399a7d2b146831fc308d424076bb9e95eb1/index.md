---
blocks:
- block_id: 57d5c051c002d71840f706e57c297c8078a3dbde6caf74d23911328fcdc7817a
  indexed: true
  sha256: b794db573dc324a44180c73ad2712165996dc8f13bd139da3a787b1fcd4b9298
  source_range:
    end: 665619
    part: 0
    start: 0
  summary: 静态追踪报告称，Kimi K3 的 Python 路径不直接调用旧算子，但 `chunk_kda_fwd` 的 ACLNN 组合实现中间会调用 `l0op::ChunkGatedDeltaRuleFwdH(..., gk, initial_state, ...)`；据此推断完全删除相关修改可能影响 K3 prefill，但这不是已验证的成功结论。报告还指出，`arch35` 仅在 `__CCE_AICORE__ == 310`（Ascend950/A5）编译，而 A3 的 `ascend910_93` 使用 arch22，因此提出按平台区分现有 arch20/22 修改与新增 arch35/接口扩展；该结论来自只读调用链检查，未包含后续编译或运行验证。
  title: Kimi K3/KDA 算子调用链与架构分支
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 07262ca20f7163473dcba07d09683399a7d2b146831fc308d424076bb9e95eb1
  kind: experience
  material_digest: 5c31426e06c38bcb7698b46193dfb5b2ece5491a5f5f1da092136b0039bb0479
  revision: 524ba300db96b693cbd34e1c3f74086bb29b9b897d05c6acbe4041bb2cfc30b2
  schema: mindie-entry/3
  summary: 本批材料集中记录 `chunk_kda_fwd` 经 ACLNN 调用 `ChunkGatedDeltaRuleFwdH` 的静态关系，以及 Ascend950/A5 的 arch35 与 A3 `ascend910_93` 的 arch22 分支差异。保留的状态是静态分析与范围判断；完全删除会否实际断掉 K3 prefill、以及各架构修改是否可用，材料中均未提供验证结果。
  title: 当前任务导航：Kimi K3/KDA 算子架构影响索引
navigation: 本批材料集中记录 `chunk_kda_fwd` 经 ACLNN 调用 `ChunkGatedDeltaRuleFwdH` 的静态关系，以及 Ascend950/A5 的 arch35 与 A3 `ascend910_93` 的 arch22 分支差异。保留的状态是静态分析与范围判断；完全删除会否实际断掉 K3 prefill、以及各架构修改是否可用，材料中均未提供验证结果。
schema: mindie-material-task/1
status: complete
task_id: 07262ca20f7163473dcba07d09683399a7d2b146831fc308d424076bb9e95eb1
---

# 当前任务导航：Kimi K3/KDA 算子架构影响索引

本批材料集中记录 `chunk_kda_fwd` 经 ACLNN 调用 `ChunkGatedDeltaRuleFwdH` 的静态关系，以及 Ascend950/A5 的 arch35 与 A3 `ascend910_93` 的 arch22 分支差异。保留的状态是静态分析与范围判断；完全删除会否实际断掉 K3 prefill、以及各架构修改是否可用，材料中均未提供验证结果。

## Materials

- [Kimi K3/KDA 算子调用链与架构分支](blocks/57d5c051c002d71840f706e57c297c8078a3dbde6caf74d23911328fcdc7817a.md): 静态追踪报告称，Kimi K3 的 Python 路径不直接调用旧算子，但 `chunk_kda_fwd` 的 ACLNN 组合实现中间会调用 `l0op::ChunkGatedDeltaRuleFwdH(..., gk, initial_state, ...)`；据此推断完全删除相关修改可能影响 K3 prefill，但这不是已验证的成功结论。报告还指出，`arch35` 仅在 `__CCE_AICORE__ == 310`（Ascend950/A5）编译，而 A3 的 `ascend910_93` 使用 arch22，因此提出按平台区分现有 arch20/22 修改与新增 arch35/接口扩展；该结论来自只读调用链检查，未包含后续编译或运行验证。

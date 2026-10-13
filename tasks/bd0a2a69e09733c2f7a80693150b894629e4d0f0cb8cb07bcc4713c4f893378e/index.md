---
blocks:
- block_id: d6c48736d270a33e8b6060abcbfa4a0eac529b006f71678ffc22db1278f904eb
  indexed: true
  sha256: 8e8724541aca601d1dced3527a3993f22b9fe843b5db627bd1bb30f60cbbca14
  source_range:
    end: 5980750
    part: 0
    start: 0
  summary: 记录了对 Kimi K3 项目 407 个底层 session 的统计口径：96 个主 session、311 个子 session，累计约 79,694.618M（79.695B）Token；子 session token 被归回主 session。后续按特性叠加、精度对齐、组网开发、算子开发、性能优化、环境、分支维护和规划/文档重分类，但材料中的精度对齐部分在此块末尾未完成，不能视为完整明细。统计依赖日志中的累计 tokens_used，并提到自动续跑、上下文压缩和子代理归并等不确定性；这些是报告口径，不是经过独立验证的成功结论。
  title: Kimi K3 Session Token 分类统计
- block_id: 19fb2896c0b3e9a36f7797dbbd1a2919cb99f55ed4e57e2d12f74e6c0e34965f
  indexed: true
  sha256: f42ecef31bb95dba806c2181c266bd3ce4ca8a7fc27c4cd92d99c38a48a00f8c
  source_range:
    end: 5980750
    part: 1
    start: 0
  summary: 补充了精度对齐、组网开发、算子开发、性能优化、环境、分支维护和规划/文档的 session 明细，统一以 M 为单位，并将总量换算为约 79.695B Token。随后围绕一张未直接复核的 HEIC 表格，提出组网接入、功能打通、精度对齐、特性叠加和性能攻关中如何使用 Agent 的“优秀实践”与后续改进建议。材料同时指出图片行汇总约 79.85B、早期页首约 81+B，与本地 session 统计 79.695B 不一致；Agent 自动部署、日志分析、性能定位等表述是建议或报告性描述，不能升级为已验证成果。
  title: 分类明细与 Agent 工作实践表述
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bd0a2a69e09733c2f7a80693150b894629e4d0f0cb8cb07bcc4713c4f893378e
  kind: experience
  material_digest: 5bc739b09955d270588d78862c4e6c07e7268f13ae8984bddc4aff4113a96ad2
  revision: ca910a0e78c7162e563e3fb3dd2e69fc4cdb1b92b022963afcf7b40e1d1a0aff
  schema: mindie-entry/3
  summary: 当前材料涵盖 Kimi K3 session 的 token 统计、技术类型重分类、明细与汇报表格措辞。核心参考值为约 79.695B Token，但图片汇总约 79.85B、早期估算约 81+B，存在未消解差异；分类明细和 Agent 工作实践应作为可检索的报告材料，保留未完成、估算及未独立验证的状态。
  title: Kimi K3 Agent 工作与 Token 统计
navigation: 当前材料涵盖 Kimi K3 session 的 token 统计、技术类型重分类、明细与汇报表格措辞。核心参考值为约 79.695B Token，但图片汇总约 79.85B、早期估算约 81+B，存在未消解差异；分类明细和 Agent 工作实践应作为可检索的报告材料，保留未完成、估算及未独立验证的状态。
schema: mindie-material-task/1
status: complete
task_id: bd0a2a69e09733c2f7a80693150b894629e4d0f0cb8cb07bcc4713c4f893378e
---

# Kimi K3 Agent 工作与 Token 统计

当前材料涵盖 Kimi K3 session 的 token 统计、技术类型重分类、明细与汇报表格措辞。核心参考值为约 79.695B Token，但图片汇总约 79.85B、早期估算约 81+B，存在未消解差异；分类明细和 Agent 工作实践应作为可检索的报告材料，保留未完成、估算及未独立验证的状态。

## Materials

- [Kimi K3 Session Token 分类统计](blocks/d6c48736d270a33e8b6060abcbfa4a0eac529b006f71678ffc22db1278f904eb.md): 记录了对 Kimi K3 项目 407 个底层 session 的统计口径：96 个主 session、311 个子 session，累计约 79,694.618M（79.695B）Token；子 session token 被归回主 session。后续按特性叠加、精度对齐、组网开发、算子开发、性能优化、环境、分支维护和规划/文档重分类，但材料中的精度对齐部分在此块末尾未完成，不能视为完整明细。统计依赖日志中的累计 tokens_used，并提到自动续跑、上下文压缩和子代理归并等不确定性；这些是报告口径，不是经过独立验证的成功结论。
- [分类明细与 Agent 工作实践表述](blocks/19fb2896c0b3e9a36f7797dbbd1a2919cb99f55ed4e57e2d12f74e6c0e34965f.md): 补充了精度对齐、组网开发、算子开发、性能优化、环境、分支维护和规划/文档的 session 明细，统一以 M 为单位，并将总量换算为约 79.695B Token。随后围绕一张未直接复核的 HEIC 表格，提出组网接入、功能打通、精度对齐、特性叠加和性能攻关中如何使用 Agent 的“优秀实践”与后续改进建议。材料同时指出图片行汇总约 79.85B、早期页首约 81+B，与本地 session 统计 79.695B 不一致；Agent 自动部署、日志分析、性能定位等表述是建议或报告性描述，不能升级为已验证成果。

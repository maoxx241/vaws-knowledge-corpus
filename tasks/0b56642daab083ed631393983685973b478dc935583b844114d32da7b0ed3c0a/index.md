---
blocks:
- block_id: 4025a7418b245e0a59bdf115bc50b2f2eb97e26071597a18ee17369dd72eed59
  indexed: true
  sha256: c261facca60ed719c8c2fef082a4323c7e3691ce36b84e15b0444866e390f1b8
  source_range:
    end: 853330
    part: 0
    start: 0
  summary: '只读审计认为 PR #9 可叠加到 releases/v0.23.0_ci_new，涉及 Mooncake connector 与 UT，覆盖 P16/D8 等 unequal-TP KDA 状态切分；但尚未完成真实 MultiConnector、DP 端到端和状态精度验证。P 实例失败于 KdaGateCumsum，旧容器的 binary_info_config.json 中该算子及 ChunkKdaFwd 注册数为 0，疑似 launcher 索引/构建环境问题；建议清理旧 build 与 _cann_ops_custom 后完整重编安装。此前的验证方法要求使用同一 X-Request-Id 先请求 P、转发 kv_transfer_params 给 D，并检查 Mooncake transfer、HTTP 200、无 invalid block/recomputed/transfer failure；P TP16 与 D DP2×TP8 不能同时独占同一台 16 卡机器。'
  title: 'PR #9 审计与 KdaGateCumsum 环境问题'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0b56642daab083ed631393983685973b478dc935583b844114d32da7b0ed3c0a
  kind: experience
  material_digest: 3543ee3c1a318dffd19f05d4513a9f0f7a13f838e4669d7488f5741ae904b28a
  revision: f481b200648fc2dc52de1237b9a0c274b48d8e56e75e49298bec338147e4849b
  schema: mindie-entry/3
  summary: '核心线索包括 PR #9 的 P/D unequal-TP KDA 切分逻辑，以及独立的 KdaGateCumsum launcher 注册/旧构建环境失败。后续重点是清理并重编安装后验证 P→D Mooncake 路径；现有结论仍属只读审计和建议，真实端到端验证尚未完成。'
  title: 当前任务导航：Kimi K3 PD 分离验证
navigation: '核心线索包括 PR #9 的 P/D unequal-TP KDA 切分逻辑，以及独立的 KdaGateCumsum launcher 注册/旧构建环境失败。后续重点是清理并重编安装后验证 P→D Mooncake 路径；现有结论仍属只读审计和建议，真实端到端验证尚未完成。'
schema: mindie-material-task/1
status: complete
task_id: 0b56642daab083ed631393983685973b478dc935583b844114d32da7b0ed3c0a
---

# 当前任务导航：Kimi K3 PD 分离验证

核心线索包括 PR #9 的 P/D unequal-TP KDA 切分逻辑，以及独立的 KdaGateCumsum launcher 注册/旧构建环境失败。后续重点是清理并重编安装后验证 P→D Mooncake 路径；现有结论仍属只读审计和建议，真实端到端验证尚未完成。

## Materials

- [PR #9 审计与 KdaGateCumsum 环境问题](blocks/4025a7418b245e0a59bdf115bc50b2f2eb97e26071597a18ee17369dd72eed59.md): 只读审计认为 PR #9 可叠加到 releases/v0.23.0_ci_new，涉及 Mooncake connector 与 UT，覆盖 P16/D8 等 unequal-TP KDA 状态切分；但尚未完成真实 MultiConnector、DP 端到端和状态精度验证。P 实例失败于 KdaGateCumsum，旧容器的 binary_info_config.json 中该算子及 ChunkKdaFwd 注册数为 0，疑似 launcher 索引/构建环境问题；建议清理旧 build 与 _cann_ops_custom 后完整重编安装。此前的验证方法要求使用同一 X-Request-Id 先请求 P、转发 kv_transfer_params 给 D，并检查 Mooncake transfer、HTTP 200、无 invalid block/recomputed/transfer failure；P TP16 与 D DP2×TP8 不能同时独占同一台 16 卡机器。

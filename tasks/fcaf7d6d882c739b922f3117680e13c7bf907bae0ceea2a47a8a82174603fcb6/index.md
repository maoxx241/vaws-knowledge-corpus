---
blocks:
- block_id: 255669d2a92678a8f539da471c19fd6ab6d5b3f201cc08172662c25ab6b9835d
  indexed: true
  sha256: 8834edb62726b0c6d9d1bc0b1c34dc8f1dcf28225e44d7a91551c4314e214bcb
  source_range:
    end: 1165263
    part: 0
    start: 0
  summary: '讨论了 Kimi K3 的 RoPE cos/sin 是否参与计算、SiTU 独立 fallback、packed W4 处理，以及向 vllm-ascend-kimi-k3 的 release/0.23.0 分支提交 AscendC 适配 PR；记录称 Draft PR #4 已提交，但 A5 原生 MXFP4 端到端验证仍待补充，GitHub 检查还受 `Resource not accessible by integration` 和 workflow 启动问题影响，因此不应视为全面验证成功。后续只读审计提到主仓 PR #11345、提交 `9e2b5d34`、KDA-only `SymInt` 修复、`tests/ut/test_meta_registration.py` 定向门禁，以及 `torch_binding.cpp` 无需修改；同时记录了 FULL_DECODE_ONLY 在 Dynamo fullgraph 下的 `Unsupported method call`，并称改用 `object.__setattr__` 与普通属性访问后定向测试通过，但这些仍是该记录中的报告性结果。'
  title: Kimi K3 AscendC 适配、PR 与门禁审计记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: fcaf7d6d882c739b922f3117680e13c7bf907bae0ceea2a47a8a82174603fcb6
  kind: experience
  material_digest: e579a17875f24d5a2c7a0947cc6f26a47e0c5d327e8657e26bc180d50151600a
  revision: 4e191b4657de4802a3676f9b265a01815a3b805b4fdf04710690051cbd5477e5
  schema: mindie-entry/3
  summary: '当前索引涵盖 Kimi K3 的 RoPE/SiTU 设计取舍、AscendC PR #4、A5 MXFP4 未完成验证、GitHub workflow 权限失败，以及 ACLGraph/torch binding 的 meta 门禁审计。后续重点是区分已报告的定向测试结果与尚未完成的原生权重、端到端及 CI 验证；FULL_DECODE_ONLY 的 fullgraph 问题及其修复也应作为独立检索线索保留。'
  title: Kimi K3 Ascend 适配与验证状态
navigation: '当前索引涵盖 Kimi K3 的 RoPE/SiTU 设计取舍、AscendC PR #4、A5 MXFP4 未完成验证、GitHub workflow 权限失败，以及 ACLGraph/torch binding 的 meta 门禁审计。后续重点是区分已报告的定向测试结果与尚未完成的原生权重、端到端及 CI 验证；FULL_DECODE_ONLY 的 fullgraph 问题及其修复也应作为独立检索线索保留。'
schema: mindie-material-task/1
status: complete
task_id: fcaf7d6d882c739b922f3117680e13c7bf907bae0ceea2a47a8a82174603fcb6
---

# Kimi K3 Ascend 适配与验证状态

当前索引涵盖 Kimi K3 的 RoPE/SiTU 设计取舍、AscendC PR #4、A5 MXFP4 未完成验证、GitHub workflow 权限失败，以及 ACLGraph/torch binding 的 meta 门禁审计。后续重点是区分已报告的定向测试结果与尚未完成的原生权重、端到端及 CI 验证；FULL_DECODE_ONLY 的 fullgraph 问题及其修复也应作为独立检索线索保留。

## Materials

- [Kimi K3 AscendC 适配、PR 与门禁审计记录](blocks/255669d2a92678a8f539da471c19fd6ab6d5b3f201cc08172662c25ab6b9835d.md): 讨论了 Kimi K3 的 RoPE cos/sin 是否参与计算、SiTU 独立 fallback、packed W4 处理，以及向 vllm-ascend-kimi-k3 的 release/0.23.0 分支提交 AscendC 适配 PR；记录称 Draft PR #4 已提交，但 A5 原生 MXFP4 端到端验证仍待补充，GitHub 检查还受 `Resource not accessible by integration` 和 workflow 启动问题影响，因此不应视为全面验证成功。后续只读审计提到主仓 PR #11345、提交 `9e2b5d34`、KDA-only `SymInt` 修复、`tests/ut/test_meta_registration.py` 定向门禁，以及 `torch_binding.cpp` 无需修改；同时记录了 FULL_DECODE_ONLY 在 Dynamo fullgraph 下的 `Unsupported method call`，并称改用 `object.__setattr__` 与普通属性访问后定向测试通过，但这些仍是该记录中的报告性结果。

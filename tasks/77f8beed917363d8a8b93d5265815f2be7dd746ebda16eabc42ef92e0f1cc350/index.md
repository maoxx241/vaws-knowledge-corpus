---
blocks:
- block_id: e9b36644e41fd453bb7b447ceec8ab077057edb2137517d78978ba52726b5ac4
  indexed: true
  sha256: 2d761339176657543e9fa86a2b44c675f91a407a6f53f12b5626d1a94ef7edca
  source_range:
    end: 1486515
    part: 0
    start: 0
  summary: 一次只读审计报告指出，K3 的解析器补丁可能遗漏 pinned vLLM 的 `is_harmony` 与 `mm_token_counts` 参数，且非流式 generator 的 token 捕获逻辑可能已被上游原生 `model_output_token_ids` 传递取代；`FusedMoE(activation="situ", situ_beta=..., situ_linear_beta=...)` 也被报告为不符合当前上游 factory 契约。报告建议收敛 serving/parser 的全局 monkey patch、将 SiTU 与 K3 MLA 属性改为显式 typed contract，并修正注册入口、测试引用、meta tensor 设备处理及重复 ABI 常量；同时列出应保留的 schema、checkpoint、padding、capacity、动态 EPLB/offset、layout 和 graph-tracing 防御边界。上述内容是审计观察与建议，未在该材料中证明修复已实施或问题已验证解决。
  title: Kimi K3/vLLM 增量架构审计与兼容性风险
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 77f8beed917363d8a8b93d5265815f2be7dd746ebda16eabc42ef92e0f1cc350
  kind: experience
  material_digest: 675bebf9fb88ab27572d0abe23194e2b2a15c4b5d21e94dc52fc3d2ec14d0238
  revision: d906f2704d19b4bb42089656a8d241f13409b58c45b1ef1c3a2232fc966b5c3c
  schema: mindie-entry/3
  summary: 当前材料聚焦 Kimi K3 与 pinned vLLM 的 API 契约、解析器/renderer monkey patch、SiTU MoE factory、算子注册及 MLA 类型协议风险。可按 `is_harmony`、`mm_token_counts`、`FusedMoE`/`situ`、`KimiK3Renderer`、`REGISTERED_ASCEND_OPS`、meta tensor、OOT registry 和测试契约检索；修复建议与保留的防御边界均仍属于报告内容，未表明已完成或验证成功。
  title: Kimi K3 增量审计索引
navigation: 当前材料聚焦 Kimi K3 与 pinned vLLM 的 API 契约、解析器/renderer monkey patch、SiTU MoE factory、算子注册及 MLA 类型协议风险。可按 `is_harmony`、`mm_token_counts`、`FusedMoE`/`situ`、`KimiK3Renderer`、`REGISTERED_ASCEND_OPS`、meta tensor、OOT registry 和测试契约检索；修复建议与保留的防御边界均仍属于报告内容，未表明已完成或验证成功。
schema: mindie-material-task/1
status: complete
task_id: 77f8beed917363d8a8b93d5265815f2be7dd746ebda16eabc42ef92e0f1cc350
---

# Kimi K3 增量审计索引

当前材料聚焦 Kimi K3 与 pinned vLLM 的 API 契约、解析器/renderer monkey patch、SiTU MoE factory、算子注册及 MLA 类型协议风险。可按 `is_harmony`、`mm_token_counts`、`FusedMoE`/`situ`、`KimiK3Renderer`、`REGISTERED_ASCEND_OPS`、meta tensor、OOT registry 和测试契约检索；修复建议与保留的防御边界均仍属于报告内容，未表明已完成或验证成功。

## Materials

- [Kimi K3/vLLM 增量架构审计与兼容性风险](blocks/e9b36644e41fd453bb7b447ceec8ab077057edb2137517d78978ba52726b5ac4.md): 一次只读审计报告指出，K3 的解析器补丁可能遗漏 pinned vLLM 的 `is_harmony` 与 `mm_token_counts` 参数，且非流式 generator 的 token 捕获逻辑可能已被上游原生 `model_output_token_ids` 传递取代；`FusedMoE(activation="situ", situ_beta=..., situ_linear_beta=...)` 也被报告为不符合当前上游 factory 契约。报告建议收敛 serving/parser 的全局 monkey patch、将 SiTU 与 K3 MLA 属性改为显式 typed contract，并修正注册入口、测试引用、meta tensor 设备处理及重复 ABI 常量；同时列出应保留的 schema、checkpoint、padding、capacity、动态 EPLB/offset、layout 和 graph-tracing 防御边界。上述内容是审计观察与建议，未在该材料中证明修复已实施或问题已验证解决。

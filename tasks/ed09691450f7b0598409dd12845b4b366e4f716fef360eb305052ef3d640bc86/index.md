---
blocks:
- block_id: 64bd0bf2ba4b3d1573c625b4b6ac7da1f0c0d05d7836d9eac86cb40f6f16d05e
  indexed: true
  sha256: 1d3abb1181a502beacda67b8c0f225c0ad29e7b05ab481af94a1a87cdd8e7ffc
  source_range:
    end: 1607520
    part: 0
    start: 0
  summary: 审计未修改文件，也未提出 C++/CMake/manifest 变更；指出 K3 MoE 按 upstream vLLM API 构造会因未知 `situ` activation 或 `situ_beta`/`situ_linear_beta` 参数失败，现有 UT 未覆盖真实 factory 构造。报告还记录了 KDA 通过 `CustomOp.register_oot` 的全局补丁式注册、SiTU 类型向通用 FusedMoE contract 和热路径扩散、重复的 `torch.ops._C_ascend` ABI 调用与 `SITU_MX_DST_TYPE_E4M3FN = 36`，以及 config 注册和 feature flag 的多重 owner；这些是审计结论与建议，不是已验证修复。建议方向包括 K3 专属 runner、直接组网 attention、单一 SiTU/KDA Python ABI adapter、统一 config 注册，并保留未验证的兼容性风险与后续测试需求。
  title: K3 Python 层架构审计与重构建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ed09691450f7b0598409dd12845b4b366e4f716fef360eb305052ef3d640bc86
  kind: experience
  material_digest: 1ee4aea3f0a6b35f156a723be20149a32423b81303eb35ed4154690f046994e1
  revision: 2de83e0dbaae79c1f93b6bb56cea10523ade137f3181c5ac61ee4734c92d9105
  schema: mindie-entry/3
  summary: 当前唯一已索引材料是一份未修改代码的审计：重点检索 K3 MoE factory 契约失败、KDA 全局注册污染、SiTU 通用 contract/热路径扩散、Ascend ABI 适配、config/feature 注册重复等主题。报告中的“能用”、建议和 upstream 对照均不等同于修复成功；真实 factory 构造、ABI schema、config 注册及跨模型影响仍需验证。原始材料保持独立可检索，未提供的更早上下文不作推断。
  title: 当前任务导航：K3 Python 层审计索引
navigation: 当前唯一已索引材料是一份未修改代码的审计：重点检索 K3 MoE factory 契约失败、KDA 全局注册污染、SiTU 通用 contract/热路径扩散、Ascend ABI 适配、config/feature 注册重复等主题。报告中的“能用”、建议和 upstream 对照均不等同于修复成功；真实 factory 构造、ABI schema、config 注册及跨模型影响仍需验证。原始材料保持独立可检索，未提供的更早上下文不作推断。
schema: mindie-material-task/1
status: complete
task_id: ed09691450f7b0598409dd12845b4b366e4f716fef360eb305052ef3d640bc86
---

# 当前任务导航：K3 Python 层审计索引

当前唯一已索引材料是一份未修改代码的审计：重点检索 K3 MoE factory 契约失败、KDA 全局注册污染、SiTU 通用 contract/热路径扩散、Ascend ABI 适配、config/feature 注册重复等主题。报告中的“能用”、建议和 upstream 对照均不等同于修复成功；真实 factory 构造、ABI schema、config 注册及跨模型影响仍需验证。原始材料保持独立可检索，未提供的更早上下文不作推断。

## Materials

- [K3 Python 层架构审计与重构建议](blocks/64bd0bf2ba4b3d1573c625b4b6ac7da1f0c0d05d7836d9eac86cb40f6f16d05e.md): 审计未修改文件，也未提出 C++/CMake/manifest 变更；指出 K3 MoE 按 upstream vLLM API 构造会因未知 `situ` activation 或 `situ_beta`/`situ_linear_beta` 参数失败，现有 UT 未覆盖真实 factory 构造。报告还记录了 KDA 通过 `CustomOp.register_oot` 的全局补丁式注册、SiTU 类型向通用 FusedMoE contract 和热路径扩散、重复的 `torch.ops._C_ascend` ABI 调用与 `SITU_MX_DST_TYPE_E4M3FN = 36`，以及 config 注册和 feature flag 的多重 owner；这些是审计结论与建议，不是已验证修复。建议方向包括 K3 专属 runner、直接组网 attention、单一 SiTU/KDA Python ABI adapter、统一 config 注册，并保留未验证的兼容性风险与后续测试需求。

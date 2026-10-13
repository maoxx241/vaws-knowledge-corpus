---
blocks:
- block_id: 3cb24b15ca5fec7cbbc3eab3f8318c3fbe4aefd2e93f0c9bed767d0881df57f2
  indexed: true
  sha256: 7d4866ab8ee0af83b6620002f5ed3f629b9c4963de2c11986ac1c6cffe9493f1
  source_range:
    end: 967523
    part: 0
    start: 0
  summary: 只读审计认为，上游提交 `7c6729b` 等对 Kimi K3 的支持仍属部分合入，当前 `a0c092e` 缺少 `KimiK3ForConditionalGeneration` 与 `kimi_k3` config 注册，且入口在 Ascend 上可能落入 NVIDIA 分支，因此不能整体替换现有 Ascend modeling。建议优先按文件移植标准 image processor/MM preprocess，并同步 renderer 的 placeholder 协议；保留 DSpark materialized aux capture、KDA state、无 RoPE、W4A8 loader 和 A3 `rot_proj`。需注意 projector 维度冲突（上游可能输出 1024，而语言模型需要 7168）、`mla_use_rope` 缺失导致的 `AttributeError` 风险，以及尚未验证的多图、media ID、placeholder 和输出维度测试。
  title: Kimi K3 上游对齐与 Ascend 保留边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6649a35d3c77134f8ac543ff56ba1ed5647533f0306e6023d6224ac7e61c774c
  kind: experience
  material_digest: 5cc07ef6c74a0bf312fc28ed6e62331eeb7ccbd6a9b938cfeb2aff6dc74798f9
  revision: 0b2e49bbb7798ef39a2522b96917ffd117dc3baf94ba49ab15fd9531af27c6f2
  schema: mindie-entry/3
  summary: 本批包含一项 Kimi K3/Ascend 只读审计索引：记录上游提交边界、processor/MM preprocess 与 renderer 的拟议对齐，以及必须保留的 Ascend 专有逻辑。后续重点是冻结 GQA DSpark 基线、验证 standard-image 协议和相关测试；上游 registry/config 接线、projector 维度与若干兼容性问题仍未确认解决。
  title: 当前任务导航
navigation: 本批包含一项 Kimi K3/Ascend 只读审计索引：记录上游提交边界、processor/MM preprocess 与 renderer 的拟议对齐，以及必须保留的 Ascend 专有逻辑。后续重点是冻结 GQA DSpark 基线、验证 standard-image 协议和相关测试；上游 registry/config 接线、projector 维度与若干兼容性问题仍未确认解决。
schema: mindie-material-task/1
status: complete
task_id: 6649a35d3c77134f8ac543ff56ba1ed5647533f0306e6023d6224ac7e61c774c
---

# 当前任务导航

本批包含一项 Kimi K3/Ascend 只读审计索引：记录上游提交边界、processor/MM preprocess 与 renderer 的拟议对齐，以及必须保留的 Ascend 专有逻辑。后续重点是冻结 GQA DSpark 基线、验证 standard-image 协议和相关测试；上游 registry/config 接线、projector 维度与若干兼容性问题仍未确认解决。

## Materials

- [Kimi K3 上游对齐与 Ascend 保留边界](blocks/3cb24b15ca5fec7cbbc3eab3f8318c3fbe4aefd2e93f0c9bed767d0881df57f2.md): 只读审计认为，上游提交 `7c6729b` 等对 Kimi K3 的支持仍属部分合入，当前 `a0c092e` 缺少 `KimiK3ForConditionalGeneration` 与 `kimi_k3` config 注册，且入口在 Ascend 上可能落入 NVIDIA 分支，因此不能整体替换现有 Ascend modeling。建议优先按文件移植标准 image processor/MM preprocess，并同步 renderer 的 placeholder 协议；保留 DSpark materialized aux capture、KDA state、无 RoPE、W4A8 loader 和 A3 `rot_proj`。需注意 projector 维度冲突（上游可能输出 1024，而语言模型需要 7168）、`mla_use_rope` 缺失导致的 `AttributeError` 风险，以及尚未验证的多图、media ID、placeholder 和输出维度测试。

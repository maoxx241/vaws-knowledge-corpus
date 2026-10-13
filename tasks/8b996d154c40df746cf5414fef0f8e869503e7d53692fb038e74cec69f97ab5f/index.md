---
blocks:
- block_id: e7a0ca2f45d72b8dc47b4524f2979f277a7b18c5a6ce02631bb56cf78f5e0f1e
  indexed: true
  sha256: c26353148e5b4e86938581d08450eae1b24c4c45371920279c96a36817a74cd6
  source_range:
    end: 1829946
    part: 0
    start: 0
  summary: 记录了对 KimiK3MLAAttention、Ascend MLA wrapper 及官方 K3 配置的只读审查，以及 no-RoPE 路径的修正：绕过会重排维度的 Ascend RoPE，q/raw k slice 直通并分开写 cache，同时禁用相关 FA-quant/MLAPO prolog。随后实现了 A3 ModelSlim 量化分解路径，保留 W4A8 routed expert 的 scale_bias，避开 SwiGLU 融合，并为 W8A8 shared expert 的 SiTU 使用线性→SiTU→线性路径；消息末尾报告了多组 CPU/NPU smoke、回归、compileall、ruff 和 diff 检查通过，但这些仍属于来源中的报告，不能视为独立验证。
  title: MLA no-RoPE 与 A3 ModelSlim/W4A8 适配记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8b996d154c40df746cf5414fef0f8e869503e7d53692fb038e74cec69f97ab5f
  kind: experience
  material_digest: 3995df0cfc9b89d3e63ca0d419b2d5f627b829b747a90843076745d6ba4a5fda
  revision: f82d785701d1c2f8e84ac61cd8dc6c6dd944e3e38071f9d9c19b8295cca08f10
  schema: mindie-entry/3
  summary: 本批材料集中于 MLA no-RoPE/cache 行为和 A3 ModelSlim W4A8/W8A8 SiTU 量化路径。后续检索应关注 Ascend RoPE 维度重排、raw q/k slice、paged cache、FA-quant/MLAPO 禁用、scale_bias 保留、SwiGLU 融合规避，以及来源所报告但未独立验证的测试与同步结果。
  title: 当前任务导航：MLA 与 A3 量化适配
navigation: 本批材料集中于 MLA no-RoPE/cache 行为和 A3 ModelSlim W4A8/W8A8 SiTU 量化路径。后续检索应关注 Ascend RoPE 维度重排、raw q/k slice、paged cache、FA-quant/MLAPO 禁用、scale_bias 保留、SwiGLU 融合规避，以及来源所报告但未独立验证的测试与同步结果。
schema: mindie-material-task/1
status: complete
task_id: 8b996d154c40df746cf5414fef0f8e869503e7d53692fb038e74cec69f97ab5f
---

# 当前任务导航：MLA 与 A3 量化适配

本批材料集中于 MLA no-RoPE/cache 行为和 A3 ModelSlim W4A8/W8A8 SiTU 量化路径。后续检索应关注 Ascend RoPE 维度重排、raw q/k slice、paged cache、FA-quant/MLAPO 禁用、scale_bias 保留、SwiGLU 融合规避，以及来源所报告但未独立验证的测试与同步结果。

## Materials

- [MLA no-RoPE 与 A3 ModelSlim/W4A8 适配记录](blocks/e7a0ca2f45d72b8dc47b4524f2979f277a7b18c5a6ce02631bb56cf78f5e0f1e.md): 记录了对 KimiK3MLAAttention、Ascend MLA wrapper 及官方 K3 配置的只读审查，以及 no-RoPE 路径的修正：绕过会重排维度的 Ascend RoPE，q/raw k slice 直通并分开写 cache，同时禁用相关 FA-quant/MLAPO prolog。随后实现了 A3 ModelSlim 量化分解路径，保留 W4A8 routed expert 的 scale_bias，避开 SwiGLU 融合，并为 W8A8 shared expert 的 SiTU 使用线性→SiTU→线性路径；消息末尾报告了多组 CPU/NPU smoke、回归、compileall、ruff 和 diff 检查通过，但这些仍属于来源中的报告，不能视为独立验证。

---
blocks:
- block_id: 0d0faa88e96a93609b0ee4b1c8b00bb9fd722f5a71d23799bb70a98b5508875e
  indexed: true
  sha256: 75c64b300ce441953e5410c5c422fb66f3397f9e795071a1df9da421dec50993
  source_range:
    end: 1214588
    part: 0
    start: 0
  summary: 只读审计相对 `upstream/main` 的完整 diff，核对了 ModelSlim 注册、量化 scheme、层类型调用链及 Kimi 特判；未修改工作树。主要风险是 K3 的 `w1/w3/w2` 映射应由 `AscendKimiK3ForCausalLM.packed_modules_mapping` 提供，且缺少 ModelSlim 权重的 `LinearBase` 不应静默降级为浮点，仅 full-rank gate 的临时 `g_a_proj/g_b_proj` 可做精确能力判断。另涉及 A_log、Step3 `g_proj`、renderer 模型身份判断，以及 custom-op、compressed-tensors scheme 和 architecture/config 注册方向；这些是审计发现与建议，并非已验证成功。
  title: Kimi K3 ModelSlim 集成审计结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 54a5e3783d71210fd1c96a8dc64611d17a502c5b0de3504db571ed639d360a63
  kind: experience
  material_digest: 26253b16c4c5adf20e8b105ba8b0e06672084c767a0efa4b98d86387aa9ff57e
  revision: 2693a53d003139c5b617076d46fe9b7c8d93a841e9f361a908516cb6422c1f9b
  schema: mindie-entry/3
  summary: 已记录一项关于 Kimi K3 ModelSlim 集成的只读审计索引：重点检索 packed module 映射、LinearBase 缺失权重时的 fail-fast 行为、full-rank gate 临时投影能力判断，以及 A_log、Step3 `g_proj`、renderer 身份判断和注册配置。审计未修改工作树，相关方向与风险均不应视为已验证完成。
  title: 当前任务导航：Kimi K3 ModelSlim 集成审计
navigation: 已记录一项关于 Kimi K3 ModelSlim 集成的只读审计索引：重点检索 packed module 映射、LinearBase 缺失权重时的 fail-fast 行为、full-rank gate 临时投影能力判断，以及 A_log、Step3 `g_proj`、renderer 身份判断和注册配置。审计未修改工作树，相关方向与风险均不应视为已验证完成。
schema: mindie-material-task/1
status: complete
task_id: 54a5e3783d71210fd1c96a8dc64611d17a502c5b0de3504db571ed639d360a63
---

# 当前任务导航：Kimi K3 ModelSlim 集成审计

已记录一项关于 Kimi K3 ModelSlim 集成的只读审计索引：重点检索 packed module 映射、LinearBase 缺失权重时的 fail-fast 行为、full-rank gate 临时投影能力判断，以及 A_log、Step3 `g_proj`、renderer 身份判断和注册配置。审计未修改工作树，相关方向与风险均不应视为已验证完成。

## Materials

- [Kimi K3 ModelSlim 集成审计结论](blocks/0d0faa88e96a93609b0ee4b1c8b00bb9fd722f5a71d23799bb70a98b5508875e.md): 只读审计相对 `upstream/main` 的完整 diff，核对了 ModelSlim 注册、量化 scheme、层类型调用链及 Kimi 特判；未修改工作树。主要风险是 K3 的 `w1/w3/w2` 映射应由 `AscendKimiK3ForCausalLM.packed_modules_mapping` 提供，且缺少 ModelSlim 权重的 `LinearBase` 不应静默降级为浮点，仅 full-rank gate 的临时 `g_a_proj/g_b_proj` 可做精确能力判断。另涉及 A_log、Step3 `g_proj`、renderer 模型身份判断，以及 custom-op、compressed-tensors scheme 和 architecture/config 注册方向；这些是审计发现与建议，并非已验证成功。

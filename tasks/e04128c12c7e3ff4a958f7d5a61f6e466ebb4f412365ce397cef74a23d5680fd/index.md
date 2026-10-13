---
blocks:
- block_id: 5a8d16788a2445b2484effcd69190a472bc6bd7dc7b8e78e2bb89cde51760494
  indexed: true
  sha256: bd06ee5e5600d08eed8c308a65fb7ead8d328d25ade0686aadfd4e8720fe8835
  source_range:
    end: 1020398
    part: 0
    start: 0
  summary: 记录称旧 padding 补丁未根治连续 `!`/`None`：r7–r10 的在线结果从 28/32、30/32、31/32 到 mirrored 124/128，且出现 HTTP 500；r11 的 `mamba-consolidate` 实际未部署，因此 121/128 不能归因于该补丁。`d4ed5fe86` 对零长度 row 基本是行为 no-op；当前未验证的定位假设转向首轮 target verify 的真实 row 或 speculative state 生命周期，并建议与 SGLang 对齐检查 persistent state、scratch state、compact scatter/padding 和 accept 后 commit。
  title: SGLang 对齐定位首轮 verify 状态问题
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e04128c12c7e3ff4a958f7d5a61f6e466ebb4f412365ce397cef74a23d5680fd
  kind: experience
  material_digest: fe195cd3d0ee8d73a41f9f6856daf9eee013ac32caf357285275ceb26b077995
  revision: a45d842090443f92067e8addcec74bd9f33ced635a3f553f965683476dc6cdcf
  schema: mindie-entry/3
  summary: 保留旧 padding 补丁及其失败、不完整和部署不确定性记录；当前重点是比较 KDA/conv 状态生命周期与 compact 输出 scatter。已有观察包括 fallback token 0、连续 `!`、`None` 和 HTTP 500，但根因仍未验证，原始材料仍可独立检索。
  title: 当前任务导航：SGLang 对齐首轮 verify 状态
navigation: 保留旧 padding 补丁及其失败、不完整和部署不确定性记录；当前重点是比较 KDA/conv 状态生命周期与 compact 输出 scatter。已有观察包括 fallback token 0、连续 `!`、`None` 和 HTTP 500，但根因仍未验证，原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: e04128c12c7e3ff4a958f7d5a61f6e466ebb4f412365ce397cef74a23d5680fd
---

# 当前任务导航：SGLang 对齐首轮 verify 状态

保留旧 padding 补丁及其失败、不完整和部署不确定性记录；当前重点是比较 KDA/conv 状态生命周期与 compact 输出 scatter。已有观察包括 fallback token 0、连续 `!`、`None` 和 HTTP 500，但根因仍未验证，原始材料仍可独立检索。

## Materials

- [SGLang 对齐定位首轮 verify 状态问题](blocks/5a8d16788a2445b2484effcd69190a472bc6bd7dc7b8e78e2bb89cde51760494.md): 记录称旧 padding 补丁未根治连续 `!`/`None`：r7–r10 的在线结果从 28/32、30/32、31/32 到 mirrored 124/128，且出现 HTTP 500；r11 的 `mamba-consolidate` 实际未部署，因此 121/128 不能归因于该补丁。`d4ed5fe86` 对零长度 row 基本是行为 no-op；当前未验证的定位假设转向首轮 target verify 的真实 row 或 speculative state 生命周期，并建议与 SGLang 对齐检查 persistent state、scratch state、compact scatter/padding 和 accept 后 commit。

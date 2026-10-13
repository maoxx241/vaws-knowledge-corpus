---
blocks:
- block_id: d248ed1a276d2497a87c5f282a3f2679d42142a99876f01c342277239b62b93b
  indexed: true
  sha256: a2a58f05b6d696c4627f77014d00c25a86607d618de4573d3571184654f0bee6
  source_range:
    end: 378518
    part: 0
    start: 0
  summary: 只读审查认为候选兼容补丁方向正确：vLLM main 的 helper 接受 num_heads，并在 heads 无法被 TP 整除时自动进行 DP 回退；v0.25.1 仅支持零参数并读取 mm_encoder_tp_mode，因此 Kimi K3 原有带参调用会触发 TypeError。审查指出不能简单改为零参，否则 12 vision heads / TP16 会丢失自动回退；建议 wrapper 显式保留该语义，并补充 v0.25.1 + 12 heads / TP16 的 UT。此结论是只读参考判断，未编辑文件；不建议捕获 TypeError 后重试，以免掩盖 helper 内部异常。
  title: vLLM 版本兼容与 Kimi K3 helper 调用审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 952cd8dc057068282a85bdb59531260b6a75d4eac809ce001dce8090cda745b1
  kind: experience
  material_digest: f840c90a6d5d40e168888a95d976ebd24803ea92c09e1c8632f6dfeda1ee646d
  revision: 2c590ca37bdccbeca0949e624711983eb807cc0684ee1b9edb747355f379c767
  schema: mindie-entry/3
  summary: 当前材料聚焦 vLLM main 与 v0.25.1 的 vision helper 签名差异、Kimi K3 的 TypeError 风险及 12 heads / TP16 的 DP 回退语义。候选补丁被判断为方向正确但仍建议增加显式 release 回退测试；该判断未验证为最终成功，也未记录文件修改或 PR 操作。原始材料仍需独立检索，且未提供更早上下文。
  title: 任务导航：vLLM helper 兼容性审查
navigation: 当前材料聚焦 vLLM main 与 v0.25.1 的 vision helper 签名差异、Kimi K3 的 TypeError 风险及 12 heads / TP16 的 DP 回退语义。候选补丁被判断为方向正确但仍建议增加显式 release 回退测试；该判断未验证为最终成功，也未记录文件修改或 PR 操作。原始材料仍需独立检索，且未提供更早上下文。
schema: mindie-material-task/1
status: complete
task_id: 952cd8dc057068282a85bdb59531260b6a75d4eac809ce001dce8090cda745b1
---

# 任务导航：vLLM helper 兼容性审查

当前材料聚焦 vLLM main 与 v0.25.1 的 vision helper 签名差异、Kimi K3 的 TypeError 风险及 12 heads / TP16 的 DP 回退语义。候选补丁被判断为方向正确但仍建议增加显式 release 回退测试；该判断未验证为最终成功，也未记录文件修改或 PR 操作。原始材料仍需独立检索，且未提供更早上下文。

## Materials

- [vLLM 版本兼容与 Kimi K3 helper 调用审查](blocks/d248ed1a276d2497a87c5f282a3f2679d42142a99876f01c342277239b62b93b.md): 只读审查认为候选兼容补丁方向正确：vLLM main 的 helper 接受 num_heads，并在 heads 无法被 TP 整除时自动进行 DP 回退；v0.25.1 仅支持零参数并读取 mm_encoder_tp_mode，因此 Kimi K3 原有带参调用会触发 TypeError。审查指出不能简单改为零参，否则 12 vision heads / TP16 会丢失自动回退；建议 wrapper 显式保留该语义，并补充 v0.25.1 + 12 heads / TP16 的 UT。此结论是只读参考判断，未编辑文件；不建议捕获 TypeError 后重试，以免掩盖 helper 内部异常。

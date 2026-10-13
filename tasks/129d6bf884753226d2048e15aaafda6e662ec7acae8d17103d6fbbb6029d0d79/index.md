---
blocks:
- block_id: f428c8185c9bd3e8a3806c7a476515dc6359c4730a1c46ca8f1c2fbe8782cfa2
  indexed: true
  sha256: d889d0ff60605dfeae8956abd1db5cbc662592f5bdc9861dea8c959355cc1859
  source_range:
    end: 808270
    part: 0
    start: 0
  summary: 确认 vLLM 的 `num_accepted_tokens` 只统计通过验证的 draft token，`mean_acceptance_length = 1 + accepted_drafts / verification_rounds` 中的 `+1` 是每轮 target 输出 token，不是 DSpark anchor；模型卡 `5.666` 对应平均接受 4.666 个 draft token，K=7 固定宽度接收率为 66.657%。工程区间 `AL 5.1–6.2` 可换算为 58.57%–74.29%，但因缺少方差或置信区间，不能称为官方正常区间。SpecForge 的 `completion_tokens / spec_verify_ct` 可能受 prefill 首 token、EOS/stop 尾轮及调度、量化、模板、缓存和上下文差异影响，不能直接横比；SSH 公钥 BatchMode 当前不可用，建议手工认证后复用 `/private/tmp/k3-ssh-control-${UID}` ControlMaster socket，且本轮未创建 helper 或操作远端服务。
  title: 接收率公式与 SSH 只读核查结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 129d6bf884753226d2048e15aaafda6e662ec7acae8d17103d6fbbb6029d0d79
  kind: experience
  material_digest: d388b1631d2b435fd2ed65a28767e4d486b6656628844c010c2e62857d5601b6
  revision: f1a1fc2f8eea8a58d8165027d5763ce83840fc86b20c1b3c1ce3feda102b888b
  schema: mindie-entry/3
  summary: 已记录 vLLM 接收率公式、模型卡数值换算及与 SpecForge 指标不可直接横比的条件；工程区间仅作排障参考，未验证为官方范围。SSH 公钥认证不可用，ControlMaster 方案只是建议，远端操作和 helper 创建均未完成；原始材料仍可独立检索。
  title: 当前任务导航：接收率口径与环境核查
navigation: 已记录 vLLM 接收率公式、模型卡数值换算及与 SpecForge 指标不可直接横比的条件；工程区间仅作排障参考，未验证为官方范围。SSH 公钥认证不可用，ControlMaster 方案只是建议，远端操作和 helper 创建均未完成；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 129d6bf884753226d2048e15aaafda6e662ec7acae8d17103d6fbbb6029d0d79
---

# 当前任务导航：接收率口径与环境核查

已记录 vLLM 接收率公式、模型卡数值换算及与 SpecForge 指标不可直接横比的条件；工程区间仅作排障参考，未验证为官方范围。SSH 公钥认证不可用，ControlMaster 方案只是建议，远端操作和 helper 创建均未完成；原始材料仍可独立检索。

## Materials

- [接收率公式与 SSH 只读核查结论](blocks/f428c8185c9bd3e8a3806c7a476515dc6359c4730a1c46ca8f1c2fbe8782cfa2.md): 确认 vLLM 的 `num_accepted_tokens` 只统计通过验证的 draft token，`mean_acceptance_length = 1 + accepted_drafts / verification_rounds` 中的 `+1` 是每轮 target 输出 token，不是 DSpark anchor；模型卡 `5.666` 对应平均接受 4.666 个 draft token，K=7 固定宽度接收率为 66.657%。工程区间 `AL 5.1–6.2` 可换算为 58.57%–74.29%，但因缺少方差或置信区间，不能称为官方正常区间。SpecForge 的 `completion_tokens / spec_verify_ct` 可能受 prefill 首 token、EOS/stop 尾轮及调度、量化、模板、缓存和上下文差异影响，不能直接横比；SSH 公钥 BatchMode 当前不可用，建议手工认证后复用 `/private/tmp/k3-ssh-control-${UID}` ControlMaster socket，且本轮未创建 helper 或操作远端服务。

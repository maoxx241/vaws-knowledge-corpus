---
blocks:
- block_id: 3fe9df993bbb6adb5c5295d708a0589c1b3c73c1f15685bc2eb6cd5479f3d82d
  indexed: true
  sha256: 25e070404850b96ec054d15f9b6448ff69682b795659280f40c9db1d1f12fd69
  source_range:
    end: 325028
    part: 0
    start: 0
  summary: '四台 qjj_k3 的显存 profile 已精确清理：目标 PGID 最终为空、容器保持运行、64/64 张卡释放；其中 153 的进程曾需补发 KILL，且本轮停在权重加载阶段，尚无 peak 结果。随后记录了 FlashComm1/shared-expert-DP draft PR #37 服务、ci-new GPQA 占用与释放 153–156 的状态，并要求在 GPQA 完成前暂停 profile；GPQA 清理据报告已完成，但新一轮只读轮询仍显示四个顶层 PGID 存活、处于 worker 拉起阶段，日志暂无 `Actual usage` 或明确错误，尚未验证成功。'
  title: 四机显存 Profile 清理与 GPQA 后续状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: eeac57e13609454ae12f387511a472644a0548d5fb43bc18759f2b48a6357444
  kind: experience
  material_digest: 4ec34eb439190d5b0519e08eb0fb2facc61b27192be52c57567a27d933624a8d
  revision: b8e43554b73474c2c39e4485e41d2f1531a644180959a661b266a8bb47679ee5
  schema: mindie-entry/3
  summary: '当前重点是四机显存 profile 的恢复与只读观察：ci-new GPQA 据报告已释放 153–156，但后续轮询发现新任务仍在 worker 拉起阶段，尚无 peak 或明确失败。FlashComm1/shared-expert-DP 解耦曾据报告完成完整权重验证并提交 draft PR #37；相关服务与资源清理、端口和 PGID 信息仍需按原始块独立检索，不能将报告性检查或计划视为已验证成功。'
  title: 显存 Profile / GPQA / FlashComm 状态
navigation: '当前重点是四机显存 profile 的恢复与只读观察：ci-new GPQA 据报告已释放 153–156，但后续轮询发现新任务仍在 worker 拉起阶段，尚无 peak 或明确失败。FlashComm1/shared-expert-DP 解耦曾据报告完成完整权重验证并提交 draft PR #37；相关服务与资源清理、端口和 PGID 信息仍需按原始块独立检索，不能将报告性检查或计划视为已验证成功。'
schema: mindie-material-task/1
status: complete
task_id: eeac57e13609454ae12f387511a472644a0548d5fb43bc18759f2b48a6357444
---

# 显存 Profile / GPQA / FlashComm 状态

当前重点是四机显存 profile 的恢复与只读观察：ci-new GPQA 据报告已释放 153–156，但后续轮询发现新任务仍在 worker 拉起阶段，尚无 peak 或明确失败。FlashComm1/shared-expert-DP 解耦曾据报告完成完整权重验证并提交 draft PR #37；相关服务与资源清理、端口和 PGID 信息仍需按原始块独立检索，不能将报告性检查或计划视为已验证成功。

## Materials

- [四机显存 Profile 清理与 GPQA 后续状态](blocks/3fe9df993bbb6adb5c5295d708a0589c1b3c73c1f15685bc2eb6cd5479f3d82d.md): 四台 qjj_k3 的显存 profile 已精确清理：目标 PGID 最终为空、容器保持运行、64/64 张卡释放；其中 153 的进程曾需补发 KILL，且本轮停在权重加载阶段，尚无 peak 结果。随后记录了 FlashComm1/shared-expert-DP draft PR #37 服务、ci-new GPQA 占用与释放 153–156 的状态，并要求在 GPQA 完成前暂停 profile；GPQA 清理据报告已完成，但新一轮只读轮询仍显示四个顶层 PGID 存活、处于 worker 拉起阶段，日志暂无 `Actual usage` 或明确错误，尚未验证成功。

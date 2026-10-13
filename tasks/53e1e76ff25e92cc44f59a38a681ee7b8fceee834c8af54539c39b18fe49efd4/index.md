---
blocks:
- block_id: 55fefacc54ddbce26dd354a051692563f8a8e98461280cebd6480e49c70ddf7e
  indexed: true
  sha256: 0edca564d42184ecac2d23ee9d3e0b417dcec03487801358fcacbe4ab7191937
  source_range:
    end: 744987
    part: 0
    start: 0
  summary: 只读分析记录了 K3 的 Eagle3 五层 aux capture、DSpark 配置与 KDA gamma=7 边界、保守 prefix-cache fixed-point 方案及待验证的 Config/Capture/KDA/slot/cache/TP-DP-EP 测试；这些是分析结论与建议，未验证为成功。后续核对显示现有 Ascend Mooncake patch 仅覆盖 D 侧 hybrid/Mamba per-group 命中，P-side local prefix lookup skip 仍依赖 core `Scheduler.schedule()` 中的 `use_dspark && do_remote_decode is True` 保护；删除该保护可能造成 `dspark_draft_kv_tokens` 虚报，现建议用可恢复的 scheduler/lookup 临时 wrapper，并保留异常恢复、BalanceScheduler 分支及 P/D smoke UT。整个批次未编辑文件、未运行服务，worktree 据报道保持 clean；动态/自定义 scheduler 覆盖、跨模块 token 契约和完整 P/D 行为仍未完成验证。
  title: K3 DSpark 集成与 P-side prefix lookup skip 分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 53e1e76ff25e92cc44f59a38a681ee7b8fceee834c8af54539c39b18fe49efd4
  kind: experience
  material_digest: 26d567615d0bce91db8f2a539573233a36228f6953b627334d2041579cf2d593
  revision: 8ff879b6b0f3a6fbc1e97dd912aa4396727aeac6d37271a79aa07b25ff84e2e8
  schema: mindie-entry/3
  summary: 当前主线是 K3 DSpark 的 Eagle3 aux capture、7-token/KDA 状态边界、prefix cache 保守一致性，以及 P-side local prefix lookup skip 与 D-side remote KV coverage 的契约。已知修正是：Ascend Mooncake patch 不负责 P-side skip，现有实际保护位于 core scheduler；拟议 monkey patch、测试矩阵和 P/D smoke 仍属未验证方案。后续检索应优先关注 `Scheduler.schedule`、KV coordinator/Mooncake lookup、`dspark_draft_kv_tokens`、BalanceScheduler、自定义 scheduler，以及五层 capture 和 KDA 1–8 接受长度测试；原始材料仍可独立检索，不应将报告的检查或计划当作成功。
  title: K3 DSpark 检索导航
navigation: 当前主线是 K3 DSpark 的 Eagle3 aux capture、7-token/KDA 状态边界、prefix cache 保守一致性，以及 P-side local prefix lookup skip 与 D-side remote KV coverage 的契约。已知修正是：Ascend Mooncake patch 不负责 P-side skip，现有实际保护位于 core scheduler；拟议 monkey patch、测试矩阵和 P/D smoke 仍属未验证方案。后续检索应优先关注 `Scheduler.schedule`、KV coordinator/Mooncake lookup、`dspark_draft_kv_tokens`、BalanceScheduler、自定义 scheduler，以及五层 capture 和 KDA 1–8 接受长度测试；原始材料仍可独立检索，不应将报告的检查或计划当作成功。
schema: mindie-material-task/1
status: complete
task_id: 53e1e76ff25e92cc44f59a38a681ee7b8fceee834c8af54539c39b18fe49efd4
---

# K3 DSpark 检索导航

当前主线是 K3 DSpark 的 Eagle3 aux capture、7-token/KDA 状态边界、prefix cache 保守一致性，以及 P-side local prefix lookup skip 与 D-side remote KV coverage 的契约。已知修正是：Ascend Mooncake patch 不负责 P-side skip，现有实际保护位于 core scheduler；拟议 monkey patch、测试矩阵和 P/D smoke 仍属未验证方案。后续检索应优先关注 `Scheduler.schedule`、KV coordinator/Mooncake lookup、`dspark_draft_kv_tokens`、BalanceScheduler、自定义 scheduler，以及五层 capture 和 KDA 1–8 接受长度测试；原始材料仍可独立检索，不应将报告的检查或计划当作成功。

## Materials

- [K3 DSpark 集成与 P-side prefix lookup skip 分析](blocks/55fefacc54ddbce26dd354a051692563f8a8e98461280cebd6480e49c70ddf7e.md): 只读分析记录了 K3 的 Eagle3 五层 aux capture、DSpark 配置与 KDA gamma=7 边界、保守 prefix-cache fixed-point 方案及待验证的 Config/Capture/KDA/slot/cache/TP-DP-EP 测试；这些是分析结论与建议，未验证为成功。后续核对显示现有 Ascend Mooncake patch 仅覆盖 D 侧 hybrid/Mamba per-group 命中，P-side local prefix lookup skip 仍依赖 core `Scheduler.schedule()` 中的 `use_dspark && do_remote_decode is True` 保护；删除该保护可能造成 `dspark_draft_kv_tokens` 虚报，现建议用可恢复的 scheduler/lookup 临时 wrapper，并保留异常恢复、BalanceScheduler 分支及 P/D smoke UT。整个批次未编辑文件、未运行服务，worktree 据报道保持 clean；动态/自定义 scheduler 覆盖、跨模块 token 契约和完整 P/D 行为仍未完成验证。

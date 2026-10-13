---
blocks:
- block_id: 3a50cc3e4a021e03332583e4da011282aa75110682ec50487a54922771361239
  indexed: true
  sha256: cb1ccbdc9a070fb4a839fcb5f7cf766025720641884ca0b384deb5ecbdb888ea
  source_range:
    end: 650501
    part: 0
    start: 0
  summary: '此前 PR #9 的 PD 分离、KDA 重编译、ACLGraph、Mooncake unequal-TP 传输及多组端到端输出被报告为通过，但这些内容属于来源中的验证结论；同时记录了 P 的 `kv_port=20001` 与 ADXL 端口窗随机冲突这一失败尝试。新任务要求基于 `releases/v0.23.0_ci_new@754570e8`，使用 5 层 INT4/W4A8 减层真实权重，在 KDA 上正面复现并修复 FlashComm1 开启时报错，比较开关状态下 token 对齐且无报错；当前仅完成只读盘点，未启动、停止或修改任何内容。候选环境为 154 主机的 `kimi-k3-pool` 容器（nightly v0.23.0-a3，16 个逻辑 NPU 空闲），但 156 仍是旧提交 `543819b5` 加 PR #9，154 工作树有未提交改动，现有材料也没有 FlashComm 报错日志，因此尚未证明修复成功。'
  title: PD 分离已验证，转向 FlashComm1/KDA 定位
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7c21322872b4afb1bac72baa56391d7428ea8d39e294d8ab23426b9895cb5ccf
  kind: experience
  material_digest: a00ac3e2b490412ecf37c241218f0942ca2bbc8da23c5cba8c8194a12065316d
  revision: 4370b173347f5a07be5c1d46cb2f1fef0f5da86d4d5a96696806a35f6412144e
  schema: mindie-entry/3
  summary: '已知背景包括 PR #9 PD 分离相关的报告性成功结论及端口冲突记录；当前未完成事项是基于最新 `v0.23.0_ci_new` 和减层真实权重，对 KDA 的 FlashComm1 on/off 做现场复现、保留失败与不确定结果、定位报错并验证 token 对齐。减层权重配置声明 93 层，启动时需覆盖为 5 层并使用 `--quantization ascend`，不能使用 dummy 权重；原始材料仍可独立检索，未提供的早期细节不应据此推断。'
  title: 当前任务：KDA FlashComm1 复现与修复
navigation: '已知背景包括 PR #9 PD 分离相关的报告性成功结论及端口冲突记录；当前未完成事项是基于最新 `v0.23.0_ci_new` 和减层真实权重，对 KDA 的 FlashComm1 on/off 做现场复现、保留失败与不确定结果、定位报错并验证 token 对齐。减层权重配置声明 93 层，启动时需覆盖为 5 层并使用 `--quantization ascend`，不能使用 dummy 权重；原始材料仍可独立检索，未提供的早期细节不应据此推断。'
schema: mindie-material-task/1
status: complete
task_id: 7c21322872b4afb1bac72baa56391d7428ea8d39e294d8ab23426b9895cb5ccf
---

# 当前任务：KDA FlashComm1 复现与修复

已知背景包括 PR #9 PD 分离相关的报告性成功结论及端口冲突记录；当前未完成事项是基于最新 `v0.23.0_ci_new` 和减层真实权重，对 KDA 的 FlashComm1 on/off 做现场复现、保留失败与不确定结果、定位报错并验证 token 对齐。减层权重配置声明 93 层，启动时需覆盖为 5 层并使用 `--quantization ascend`，不能使用 dummy 权重；原始材料仍可独立检索，未提供的早期细节不应据此推断。

## Materials

- [PD 分离已验证，转向 FlashComm1/KDA 定位](blocks/3a50cc3e4a021e03332583e4da011282aa75110682ec50487a54922771361239.md): 此前 PR #9 的 PD 分离、KDA 重编译、ACLGraph、Mooncake unequal-TP 传输及多组端到端输出被报告为通过，但这些内容属于来源中的验证结论；同时记录了 P 的 `kv_port=20001` 与 ADXL 端口窗随机冲突这一失败尝试。新任务要求基于 `releases/v0.23.0_ci_new@754570e8`，使用 5 层 INT4/W4A8 减层真实权重，在 KDA 上正面复现并修复 FlashComm1 开启时报错，比较开关状态下 token 对齐且无报错；当前仅完成只读盘点，未启动、停止或修改任何内容。候选环境为 154 主机的 `kimi-k3-pool` 容器（nightly v0.23.0-a3，16 个逻辑 NPU 空闲），但 156 仍是旧提交 `543819b5` 加 PR #9，154 工作树有未提交改动，现有材料也没有 FlashComm 报错日志，因此尚未证明修复成功。

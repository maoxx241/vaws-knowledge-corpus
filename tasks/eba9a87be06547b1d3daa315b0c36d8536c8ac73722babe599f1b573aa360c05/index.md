---
blocks:
- block_id: d46f376467117bf60fe069eae4ad8fb552010e562c04924457d4ae4976b4a97e
  indexed: true
  sha256: 2aa726f40f456580c53e35b0ed5ddba2a1be768d79051866c351f2900318019c
  source_range:
    end: 469072
    part: 0
    start: 0
  summary: 记录称 154 上孤立的 DP rank1 服务组 PGID 为 921213，曾仅向该 PGID 发送 TERM，随后报告 153–156 无 NPU 进程且 8077/14569 无监听；同时要求在 OFF/ON 基线 A/B 完成前不要启动 155/156 的 NPU 任务，并提出先用两机、5 层权重做 DSpark 冒烟。后续报告声称 Kimi K3 DSpark aux capture 已实现 SupportsEagle3、默认捕获层为 (8, 24, 52, 68, 84)，并添加接口与语义单测；但仅记录 py_compile、Ruff 和格式检查通过，因缺少 torch_npu/vllm 未运行 pytest，且未提交、未操作远端或 NPU。
  title: 四机 NPU 清理与 Kimi K3 DSpark aux capture
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: eba9a87be06547b1d3daa315b0c36d8536c8ac73722babe599f1b573aa360c05
  kind: experience
  material_digest: e43823759afc5b7e950686e8aa3a9150cc7df0e9c486940f08682aa355228413
  revision: a58613de534eac14f217d04df449a020183fd08d2ae555347ba68fac2743f47a
  schema: mindie-entry/3
  summary: 检索重点包括 PGID 921213、153–156 NPU 清理、端口 8077/14569、OFF/ON 基线与两机 5 层冒烟，以及 SupportsEagle3、捕获层 (8, 24, 52, 68, 84)、aux 返回值和未运行 pytest 的环境限制。四机 A/B 是否最终完成、接收率/精度/特性叠加验证，以及相关实现是否经过实际 NPU 或 pytest 验证，当前材料未证实。
  title: 当前任务导航：四机资源状态与 Kimi K3 DSpark aux capture
navigation: 检索重点包括 PGID 921213、153–156 NPU 清理、端口 8077/14569、OFF/ON 基线与两机 5 层冒烟，以及 SupportsEagle3、捕获层 (8, 24, 52, 68, 84)、aux 返回值和未运行 pytest 的环境限制。四机 A/B 是否最终完成、接收率/精度/特性叠加验证，以及相关实现是否经过实际 NPU 或 pytest 验证，当前材料未证实。
schema: mindie-material-task/1
status: complete
task_id: eba9a87be06547b1d3daa315b0c36d8536c8ac73722babe599f1b573aa360c05
---

# 当前任务导航：四机资源状态与 Kimi K3 DSpark aux capture

检索重点包括 PGID 921213、153–156 NPU 清理、端口 8077/14569、OFF/ON 基线与两机 5 层冒烟，以及 SupportsEagle3、捕获层 (8, 24, 52, 68, 84)、aux 返回值和未运行 pytest 的环境限制。四机 A/B 是否最终完成、接收率/精度/特性叠加验证，以及相关实现是否经过实际 NPU 或 pytest 验证，当前材料未证实。

## Materials

- [四机 NPU 清理与 Kimi K3 DSpark aux capture](blocks/d46f376467117bf60fe069eae4ad8fb552010e562c04924457d4ae4976b4a97e.md): 记录称 154 上孤立的 DP rank1 服务组 PGID 为 921213，曾仅向该 PGID 发送 TERM，随后报告 153–156 无 NPU 进程且 8077/14569 无监听；同时要求在 OFF/ON 基线 A/B 完成前不要启动 155/156 的 NPU 任务，并提出先用两机、5 层权重做 DSpark 冒烟。后续报告声称 Kimi K3 DSpark aux capture 已实现 SupportsEagle3、默认捕获层为 (8, 24, 52, 68, 84)，并添加接口与语义单测；但仅记录 py_compile、Ruff 和格式检查通过，因缺少 torch_npu/vllm 未运行 pytest，且未提交、未操作远端或 NPU。

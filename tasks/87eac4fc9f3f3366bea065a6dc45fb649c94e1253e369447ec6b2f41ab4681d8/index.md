---
blocks:
- block_id: 888ec80e0dd433465ae543bed9c919d5f4be985656a3268188968ea6b89f2783
  indexed: true
  sha256: 7dccae6cceaae97e3cc6d837133a45d1eb2ac2ca635d9f8ba6f0afc6b2776bff
  source_range:
    end: 389857
    part: 0
    start: 0
  summary: 一次定向验收中，候选脚本应用了 CPU float32 参考、FP16 `atol=0.002`、BF16 `atol=0.016`、`rtol=0`及显式误差公式；脚本 SHA-256 为 `52f65b79b8a9a83b39d0481a63e1e79547f350e98e875111496c593203897a8b`。远端检查发现 `/dev/davinci8` 存在但 `torch.npu.device_count()` 为 `0`，触发唯一 NPU 断言，`cases=[]`，因此未执行 RMSNorm 算子，也没有 FP16/BF16 误差或峰值内存结论。未重试、未放宽阈值，容器未停止；该结果仅覆盖隔离算子的输入、参考计算、设备可见性、误差判据和内存约束，不能视为整模或生产级验收通过。
  title: RMSNorm 隔离验收因 NPU 不可见失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 87eac4fc9f3f3366bea065a6dc45fb649c94e1253e369447ec6b2f41ab4681d8
  kind: experience
  material_digest: 255c9c853f15ce4c536f351a3d8b1387be7a1b4cef2441df217576ac596c29cb
  revision: d1f5e8386d9d8a232cf23cb8e2701efcff495da2d6c431c02721b4424fd98a7f
  schema: mindie-entry/3
  summary: 已记录一次真实但失败的隔离验收：知识经验已采用并写入，远端目录和产物已拉取，代码哈希可核对。关键未完成项是 NPU 设备不可见导致未执行任何算子 case；因此不存在误差、内存峰值或通过结论，原始材料仍可独立检索。
  title: 当前任务导航：RMSNorm 隔离验收记录
navigation: 已记录一次真实但失败的隔离验收：知识经验已采用并写入，远端目录和产物已拉取，代码哈希可核对。关键未完成项是 NPU 设备不可见导致未执行任何算子 case；因此不存在误差、内存峰值或通过结论，原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 87eac4fc9f3f3366bea065a6dc45fb649c94e1253e369447ec6b2f41ab4681d8
---

# 当前任务导航：RMSNorm 隔离验收记录

已记录一次真实但失败的隔离验收：知识经验已采用并写入，远端目录和产物已拉取，代码哈希可核对。关键未完成项是 NPU 设备不可见导致未执行任何算子 case；因此不存在误差、内存峰值或通过结论，原始材料仍可独立检索。

## Materials

- [RMSNorm 隔离验收因 NPU 不可见失败](blocks/888ec80e0dd433465ae543bed9c919d5f4be985656a3268188968ea6b89f2783.md): 一次定向验收中，候选脚本应用了 CPU float32 参考、FP16 `atol=0.002`、BF16 `atol=0.016`、`rtol=0`及显式误差公式；脚本 SHA-256 为 `52f65b79b8a9a83b39d0481a63e1e79547f350e98e875111496c593203897a8b`。远端检查发现 `/dev/davinci8` 存在但 `torch.npu.device_count()` 为 `0`，触发唯一 NPU 断言，`cases=[]`，因此未执行 RMSNorm 算子，也没有 FP16/BF16 误差或峰值内存结论。未重试、未放宽阈值，容器未停止；该结果仅覆盖隔离算子的输入、参考计算、设备可见性、误差判据和内存约束，不能视为整模或生产级验收通过。

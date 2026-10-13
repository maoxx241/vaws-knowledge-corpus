---
blocks:
- block_id: 0df75a15880077a25f6c0a80a553492bdf0aa2316f95f39500da8904c35ba1c1
  indexed: true
  sha256: 208f3226e2f30e651c95ca1de5f62dd1e0b8cbc50329f2a07da04c06c9ba9563
  source_range:
    end: 178561
    part: 0
    start: 0
  summary: 在固定容器和远端环境中仅复核已完成的 RMSNorm 小张量实验，未重新运行脚本；float16 最大绝对误差为 0.0019469261169433594（atol/rtol 0.002/0.002），bfloat16 为 0.015557289123535156（atol/rtol 0.016/0.016），两者均 finite 且 allclose。CPU 参考使用转换为对应 dtype 后的舍入输入，而非原始 float32 输入；物理挂载 /dev/davinci8 时，ASCEND_RT_VISIBLE_DEVICES=8 导致运行时不可见，设为 0 后逻辑设备 0 可用。结论仅限单个映射物理 NPU 上的 [5,1536] RMSNorm 正确性，不能提升为模型推理、服务可用性或性能验收。
  title: NPU RMSNorm 只读复核与设备映射边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3ebf4a58555ce3e95136c415cc333149890d350da3947d9e8d9ce2ac3250fb02
  kind: experience
  material_digest: 79aca9034245a0955e5aa832d9ed3f610df9535c26bba604538f632f72a9f567
  revision: 8e0198b8292e1e8a09cdebb30a270f671f7a0eb390be82f8cde405729097592d
  schema: mindie-entry/3
  summary: 已记录一次只读实验复核：误差与容差、舍入输入参考方法，以及物理设备号到逻辑设备索引的适用边界。当前证据不支持模型推理或性能验收结论；原始材料仍可独立检索，未报告其他批次或后续修正。
  title: 当前任务导航：NPU RMSNorm 复核
navigation: 已记录一次只读实验复核：误差与容差、舍入输入参考方法，以及物理设备号到逻辑设备索引的适用边界。当前证据不支持模型推理或性能验收结论；原始材料仍可独立检索，未报告其他批次或后续修正。
schema: mindie-material-task/1
status: complete
task_id: 3ebf4a58555ce3e95136c415cc333149890d350da3947d9e8d9ce2ac3250fb02
---

# 当前任务导航：NPU RMSNorm 复核

已记录一次只读实验复核：误差与容差、舍入输入参考方法，以及物理设备号到逻辑设备索引的适用边界。当前证据不支持模型推理或性能验收结论；原始材料仍可独立检索，未报告其他批次或后续修正。

## Materials

- [NPU RMSNorm 只读复核与设备映射边界](blocks/0df75a15880077a25f6c0a80a553492bdf0aa2316f95f39500da8904c35ba1c1.md): 在固定容器和远端环境中仅复核已完成的 RMSNorm 小张量实验，未重新运行脚本；float16 最大绝对误差为 0.0019469261169433594（atol/rtol 0.002/0.002），bfloat16 为 0.015557289123535156（atol/rtol 0.016/0.016），两者均 finite 且 allclose。CPU 参考使用转换为对应 dtype 后的舍入输入，而非原始 float32 输入；物理挂载 /dev/davinci8 时，ASCEND_RT_VISIBLE_DEVICES=8 导致运行时不可见，设为 0 后逻辑设备 0 可用。结论仅限单个映射物理 NPU 上的 [5,1536] RMSNorm 正确性，不能提升为模型推理、服务可用性或性能验收。

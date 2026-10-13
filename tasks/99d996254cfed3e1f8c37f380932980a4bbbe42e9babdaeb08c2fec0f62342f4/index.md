---
blocks:
- block_id: 7177f5bda48be7952e9b55c2d5bf3b60ad2a1e543fafd4a6e73286d411696d2d
  indexed: true
  sha256: 56ce85639452ac277a2f6fea86bf892976f40880b4c3fdde0a6aeeb99940ae9b
  source_range:
    end: 497693
    part: 0
    start: 0
  summary: 在指定容器和物理 NPU 8 范围内，初始脚本真实运行发现导入 torch/torch_npu 后才处理 ASCEND_RT_VISIBLE_DEVICES=8，以及 CPU 参考未按 FP16/BF16 舍入；固定 0.002 容差导致 BF16 误判，实际误差约 0.0476。脚本随后被修正为导入前归一逻辑设备 0、按 dtype 舍入后用 FP32 计算，并使用各 dtype 4×machine epsilon 容差，同时保留失败结果落盘。修正后的重跑已提交原 job-20260920T115147Z-4476be40，但最后状态仍为运行中，result.json 数值尚未确认；物理 NPU 8 到逻辑 0 的独立拓扑映射也未验证。
  title: MindIE 单卡 NPU RMSNorm 验证未完成
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 99d996254cfed3e1f8c37f380932980a4bbbe42e9babdaeb08c2fec0f62342f4
  kind: experience
  material_digest: 6aaeede45b6f6781b920f41ca8add8f4f6a2f146d71aeb45232001577c12d6f7
  revision: b2f7403ad3ccb0203494d610ddfafcb29552cd1dfd56f5215b78d89a8fc36173
  schema: mindie-entry/3
  summary: 已记录指定容器中 check.py 的真实检查、设备可见性归一、FP16/BF16 CPU 参考修正及 BF16 容差问题；MindIE 已激活且社区采集关闭。当前关键未完成项是原 job-20260920T115147Z-4476be40 的最终状态和 result.json 尚未确认，物理 NPU 8→逻辑 0 映射未独立验证；后续应保留这些不确定性，不将脚本修正或运行中状态表述为验证成功。
  title: 当前任务导航：单卡 NPU RMSNorm 验证
navigation: 已记录指定容器中 check.py 的真实检查、设备可见性归一、FP16/BF16 CPU 参考修正及 BF16 容差问题；MindIE 已激活且社区采集关闭。当前关键未完成项是原 job-20260920T115147Z-4476be40 的最终状态和 result.json 尚未确认，物理 NPU 8→逻辑 0 映射未独立验证；后续应保留这些不确定性，不将脚本修正或运行中状态表述为验证成功。
schema: mindie-material-task/1
status: complete
task_id: 99d996254cfed3e1f8c37f380932980a4bbbe42e9babdaeb08c2fec0f62342f4
---

# 当前任务导航：单卡 NPU RMSNorm 验证

已记录指定容器中 check.py 的真实检查、设备可见性归一、FP16/BF16 CPU 参考修正及 BF16 容差问题；MindIE 已激活且社区采集关闭。当前关键未完成项是原 job-20260920T115147Z-4476be40 的最终状态和 result.json 尚未确认，物理 NPU 8→逻辑 0 映射未独立验证；后续应保留这些不确定性，不将脚本修正或运行中状态表述为验证成功。

## Materials

- [MindIE 单卡 NPU RMSNorm 验证未完成](blocks/7177f5bda48be7952e9b55c2d5bf3b60ad2a1e543fafd4a6e73286d411696d2d.md): 在指定容器和物理 NPU 8 范围内，初始脚本真实运行发现导入 torch/torch_npu 后才处理 ASCEND_RT_VISIBLE_DEVICES=8，以及 CPU 参考未按 FP16/BF16 舍入；固定 0.002 容差导致 BF16 误判，实际误差约 0.0476。脚本随后被修正为导入前归一逻辑设备 0、按 dtype 舍入后用 FP32 计算，并使用各 dtype 4×machine epsilon 容差，同时保留失败结果落盘。修正后的重跑已提交原 job-20260920T115147Z-4476be40，但最后状态仍为运行中，result.json 数值尚未确认；物理 NPU 8 到逻辑 0 的独立拓扑映射也未验证。

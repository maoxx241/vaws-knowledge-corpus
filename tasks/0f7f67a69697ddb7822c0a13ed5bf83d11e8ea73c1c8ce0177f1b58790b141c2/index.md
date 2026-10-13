---
blocks:
- block_id: cab6ee8f4ad2683632e7f0f7d5b132d8400136c7a9ee0ce4d5e0b7e81f40f25a
  indexed: true
  sha256: c046b0840e2795501cc893dc310d9a17e41e8e10ea805e450dc562fc8be764ee
  source_range:
    end: 503160
    part: 0
    start: 0
  summary: 在固定容器和单张 NPU 映射内，检查并修正了 `ASCEND_RT_VISIBLE_DEVICES=8` 导致逻辑设备数为 0 的问题，随后以逻辑设备 0 运行真实 `torch_npu.npu_rms_norm`。修正后的参考计算使用实际送入 NPU 的 FP16/BF16 输入再提升至 FP32；FP16 和 BF16 均报告通过，但物理 NPU 8 的独立映射证据不可用，因此 `physical_mapping_verified=false`。结果写入指定 `result.json`，未验证性能、显存、多卡、模型服务或长时间稳定性；另有非致命内部格式 warning。
  title: 单卡 NPU RMSNorm 验证与可见性修正
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0f7f67a69697ddb7822c0a13ed5bf83d11e8ea73c1c8ce0177f1b58790b141c2
  kind: experience
  material_digest: ac75527151499e1639efd69192aed5d8902f57569fdb183519029a544ec7a599
  revision: 45d7f72607c76837fba84e1a3206b53083430a62147462232be14ebf596ce595
  schema: mindie-entry/3
  summary: 已完成一次真实 NPU 验证：修正设备可见性与参考精度后，FP16/BF16 均报告通过。仍需注意该结论依赖容器内逻辑设备证据，物理 NPU 8 映射未独立验证；性能、显存、多卡、服务及长期稳定性也未覆盖。原始脚本问题、修正方法、结果文件和未验证范围均保留在本块中，后续更正应以独立检索到的原始材料为准。
  title: 当前任务导航：固定容器中的单卡 RMSNorm 数值验证
navigation: 已完成一次真实 NPU 验证：修正设备可见性与参考精度后，FP16/BF16 均报告通过。仍需注意该结论依赖容器内逻辑设备证据，物理 NPU 8 映射未独立验证；性能、显存、多卡、服务及长期稳定性也未覆盖。原始脚本问题、修正方法、结果文件和未验证范围均保留在本块中，后续更正应以独立检索到的原始材料为准。
schema: mindie-material-task/1
status: complete
task_id: 0f7f67a69697ddb7822c0a13ed5bf83d11e8ea73c1c8ce0177f1b58790b141c2
---

# 当前任务导航：固定容器中的单卡 RMSNorm 数值验证

已完成一次真实 NPU 验证：修正设备可见性与参考精度后，FP16/BF16 均报告通过。仍需注意该结论依赖容器内逻辑设备证据，物理 NPU 8 映射未独立验证；性能、显存、多卡、服务及长期稳定性也未覆盖。原始脚本问题、修正方法、结果文件和未验证范围均保留在本块中，后续更正应以独立检索到的原始材料为准。

## Materials

- [单卡 NPU RMSNorm 验证与可见性修正](blocks/cab6ee8f4ad2683632e7f0f7d5b132d8400136c7a9ee0ce4d5e0b7e81f40f25a.md): 在固定容器和单张 NPU 映射内，检查并修正了 `ASCEND_RT_VISIBLE_DEVICES=8` 导致逻辑设备数为 0 的问题，随后以逻辑设备 0 运行真实 `torch_npu.npu_rms_norm`。修正后的参考计算使用实际送入 NPU 的 FP16/BF16 输入再提升至 FP32；FP16 和 BF16 均报告通过，但物理 NPU 8 的独立映射证据不可用，因此 `physical_mapping_verified=false`。结果写入指定 `result.json`，未验证性能、显存、多卡、模型服务或长时间稳定性；另有非致命内部格式 warning。

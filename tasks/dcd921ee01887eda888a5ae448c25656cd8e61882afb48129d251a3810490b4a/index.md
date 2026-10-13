---
blocks:
- block_id: f3f371b674f44a7dfd2daa287f5f550c069a49cede2d7c46edba323261d4a73e
  indexed: true
  sha256: 44b94d290ab48de44cdec8cf3f2bf699bf0af89d148eb42314209b33e60d4adc
  source_range:
    end: 425668
    part: 0
    start: 0
  summary: 在固定容器中只读复核了既有的 `check.py` 与 `result.json`，未重新运行脚本；FP16 最大绝对/相对误差为 0.003387451171875/0.0004873290890827775，BF16 为 0.01551961898803711/0.003857600037008524，均在 `4 * torch.finfo(dtype).eps` 的 atol/rtol 内并通过 `allclose`。CPU 参考使用与 NPU 相同的 dtype-rounded 输入后提升至 FP32 计算；初始 `ASCEND_RT_VISIBLE_DEVICES=8` 修正为逻辑设备 0，实际探测到单个 `Ascend910_9362`。物理设备 8 到容器逻辑设备 0 的映射仍未验证，且该结果仅覆盖 `[7,2048]` 单算子正确性，不代表模型推理、服务吞吐或性能验收；未执行 deactivate，等待清理。
  title: NPU 低精度 RMSNorm 只读复核
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: dcd921ee01887eda888a5ae448c25656cd8e61882afb48129d251a3810490b4a
  kind: experience
  material_digest: 3d71c2afcc7e85e130a776af5ccdff537563f14fe2e8eeeae176d35baf7f9f13
  revision: 220b418f26043c7868cd358c14cc1f461e976738648d8fc164b20a201a05ffd6
  schema: mindie-entry/3
  summary: 已记录固定容器内 FP16/BF16 RMSNorm 的误差、容差、舍入输入方法及单逻辑设备探测结果。物理映射仍是未知，且小张量正确性未升级为模型或性能验证；原始材料可独立检索，未完成的 deactivate 与验收器清理状态仍需保留。
  title: 当前任务导航：低精度 NPU 单算子复核
navigation: 已记录固定容器内 FP16/BF16 RMSNorm 的误差、容差、舍入输入方法及单逻辑设备探测结果。物理映射仍是未知，且小张量正确性未升级为模型或性能验证；原始材料可独立检索，未完成的 deactivate 与验收器清理状态仍需保留。
schema: mindie-material-task/1
status: complete
task_id: dcd921ee01887eda888a5ae448c25656cd8e61882afb48129d251a3810490b4a
---

# 当前任务导航：低精度 NPU 单算子复核

已记录固定容器内 FP16/BF16 RMSNorm 的误差、容差、舍入输入方法及单逻辑设备探测结果。物理映射仍是未知，且小张量正确性未升级为模型或性能验证；原始材料可独立检索，未完成的 deactivate 与验收器清理状态仍需保留。

## Materials

- [NPU 低精度 RMSNorm 只读复核](blocks/f3f371b674f44a7dfd2daa287f5f550c069a49cede2d7c46edba323261d4a73e.md): 在固定容器中只读复核了既有的 `check.py` 与 `result.json`，未重新运行脚本；FP16 最大绝对/相对误差为 0.003387451171875/0.0004873290890827775，BF16 为 0.01551961898803711/0.003857600037008524，均在 `4 * torch.finfo(dtype).eps` 的 atol/rtol 内并通过 `allclose`。CPU 参考使用与 NPU 相同的 dtype-rounded 输入后提升至 FP32 计算；初始 `ASCEND_RT_VISIBLE_DEVICES=8` 修正为逻辑设备 0，实际探测到单个 `Ascend910_9362`。物理设备 8 到容器逻辑设备 0 的映射仍未验证，且该结果仅覆盖 `[7,2048]` 单算子正确性，不代表模型推理、服务吞吐或性能验收；未执行 deactivate，等待清理。

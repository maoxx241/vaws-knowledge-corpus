---
blocks:
- block_id: ce4df7c5752bfb9ee4f69cc79b9effdc6840860235b4ebfb6357c6003d30a828
  indexed: true
  sha256: 212b625b1cb6f6f5e49a2273149dbbc89fa8bb048e12e255875012dd39ff3ec1
  source_range:
    end: 203559
    part: 0
    start: 0
  summary: 在固定容器与 `/mindie` 工作目录中，仅读取已完成的 `rmsnorm_case.py` 和 `general-result.json`，未重新运行脚本。FP16 最大绝对误差为 0.0019469261169433594（atol/rtol 0.002/0.002），BF16 为 0.015557289123535156（0.016/0.016），均 finite 且 `allclose=true`；CPU 参考基于转换并舍入到目标 dtype 后再转回 FP32 的输入。设备映射显示物理 `/dev/davinci8` 在 `ASCEND_RT_VISIBLE_DEVICES=8` 时不可用、设备数为 0，而设为 0 后逻辑设备 0 可用；结论仅限 `[5,1536]` 单 NPU RMSNorm 小张量数值正确性，不代表模型推理、服务、吞吐或性能验收。
  title: MindIE RMSNorm NPU 只读复核
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e369fceaa02de1a0f767fb38476f89857e48d802c688b46c9fb863fb3b248001
  kind: experience
  material_digest: b270008fee0d9c4e3ae3ae0a588b97e68dbf7585bfb607f973f222ed8be10f32
  revision: 2ab03edd8e5d022f1195d9c61e65e2502f79356d7bfd64866cdacadc03319333
  schema: mindie-entry/3
  summary: 已索引 1 个新材料块：固定容器中的 MindIE RMSNorm 只读复核。可检索关键词包括 `rmsnorm_case.py`、`general-result.json`、FP16/BF16 误差与容差、舍入后输入、`ASCEND_RT_VISIBLE_DEVICES`、物理 `/dev/davinci8`、逻辑设备 0、`Ascend910_9362`、小张量数值正确性及性能验收边界；本索引保留设备映射适用范围限制，未将其推广为通用配置。
  title: 当前任务导航：RMSNorm NPU 复核索引
navigation: 已索引 1 个新材料块：固定容器中的 MindIE RMSNorm 只读复核。可检索关键词包括 `rmsnorm_case.py`、`general-result.json`、FP16/BF16 误差与容差、舍入后输入、`ASCEND_RT_VISIBLE_DEVICES`、物理 `/dev/davinci8`、逻辑设备 0、`Ascend910_9362`、小张量数值正确性及性能验收边界；本索引保留设备映射适用范围限制，未将其推广为通用配置。
schema: mindie-material-task/1
status: complete
task_id: e369fceaa02de1a0f767fb38476f89857e48d802c688b46c9fb863fb3b248001
---

# 当前任务导航：RMSNorm NPU 复核索引

已索引 1 个新材料块：固定容器中的 MindIE RMSNorm 只读复核。可检索关键词包括 `rmsnorm_case.py`、`general-result.json`、FP16/BF16 误差与容差、舍入后输入、`ASCEND_RT_VISIBLE_DEVICES`、物理 `/dev/davinci8`、逻辑设备 0、`Ascend910_9362`、小张量数值正确性及性能验收边界；本索引保留设备映射适用范围限制，未将其推广为通用配置。

## Materials

- [MindIE RMSNorm NPU 只读复核](blocks/ce4df7c5752bfb9ee4f69cc79b9effdc6840860235b4ebfb6357c6003d30a828.md): 在固定容器与 `/mindie` 工作目录中，仅读取已完成的 `rmsnorm_case.py` 和 `general-result.json`，未重新运行脚本。FP16 最大绝对误差为 0.0019469261169433594（atol/rtol 0.002/0.002），BF16 为 0.015557289123535156（0.016/0.016），均 finite 且 `allclose=true`；CPU 参考基于转换并舍入到目标 dtype 后再转回 FP32 的输入。设备映射显示物理 `/dev/davinci8` 在 `ASCEND_RT_VISIBLE_DEVICES=8` 时不可用、设备数为 0，而设为 0 后逻辑设备 0 可用；结论仅限 `[5,1536]` 单 NPU RMSNorm 小张量数值正确性，不代表模型推理、服务、吞吐或性能验收。

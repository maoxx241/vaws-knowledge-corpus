---
blocks:
- block_id: 907ff30e9abb2f5434679368c733e6781a59cfa661792c14db27339570a4beb6
  indexed: true
  sha256: e80520b5d1de258acc5336e40cde4301d2f42635cb22d81f58f4295ce9531967
  source_range:
    end: 1832002
    part: 0
    start: 0
  summary: GPQA 曾报告完成 69/198、99/198，后续 FlashComm 开关对比均为 175/198（88.38%），但由于 temperature=1.0 且无固定 seed，仅支持聚合精度未见回退，不能证明逐 token 一致。期间服务多次被要求停止；一次使用 [REDACTED_SECRET] 权重的重启因 155 上的 zrc_kimi 占用 NPU 导致 rank2 显存不足，失败进程已清理且日志保留。随后用户确认 kill 155，最新任务仅计划只读监控完整 GPQA；尚无该新一轮结果。
  title: GPQA 多轮运行、服务清理与重启准备
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2870cee68040b691c9913479a12bb268f5697b28ac3125c90604f622ce4d4d0d
  kind: experience
  material_digest: d269e6e958a5949aa29d8bd34d2bbac3a104b4b52d57c009d4481211b8804f40
  revision: cae47a633d896db5c78a3dae8095268493af4a55fd83494f0c52089252c2fa90
  schema: mindie-entry/3
  summary: 材料涵盖 GPQA 进度预估、准确率与 FlashComm 对比、服务停止，以及使用 [REDACTED_SECRET] 权重重启失败后的资源清理。已知结果包括 175/198 的对比运行，但最新确认的四机完整 GPQA 只处于计划监控阶段，不能视为已完成；相关错误、环境配置和日志仍应独立检索。
  title: GPQA 评测与服务状态导航
navigation: 材料涵盖 GPQA 进度预估、准确率与 FlashComm 对比、服务停止，以及使用 [REDACTED_SECRET] 权重重启失败后的资源清理。已知结果包括 175/198 的对比运行，但最新确认的四机完整 GPQA 只处于计划监控阶段，不能视为已完成；相关错误、环境配置和日志仍应独立检索。
schema: mindie-material-task/1
status: complete
task_id: 2870cee68040b691c9913479a12bb268f5697b28ac3125c90604f622ce4d4d0d
---

# GPQA 评测与服务状态导航

材料涵盖 GPQA 进度预估、准确率与 FlashComm 对比、服务停止，以及使用 [REDACTED_SECRET] 权重重启失败后的资源清理。已知结果包括 175/198 的对比运行，但最新确认的四机完整 GPQA 只处于计划监控阶段，不能视为已完成；相关错误、环境配置和日志仍应独立检索。

## Materials

- [GPQA 多轮运行、服务清理与重启准备](blocks/907ff30e9abb2f5434679368c733e6781a59cfa661792c14db27339570a4beb6.md): GPQA 曾报告完成 69/198、99/198，后续 FlashComm 开关对比均为 175/198（88.38%），但由于 temperature=1.0 且无固定 seed，仅支持聚合精度未见回退，不能证明逐 token 一致。期间服务多次被要求停止；一次使用 [REDACTED_SECRET] 权重的重启因 155 上的 zrc_kimi 占用 NPU 导致 rank2 显存不足，失败进程已清理且日志保留。随后用户确认 kill 155，最新任务仅计划只读监控完整 GPQA；尚无该新一轮结果。

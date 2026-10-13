---
blocks:
- block_id: 2a11b72a8e725794405824e7a20a3fb9ee944925bed660d2b2980e201c576c6f
  indexed: true
  sha256: 6a0f457c0b2a5d9907c6e238b5cc49a4bc50ed94be9713733d9dc85cd1b196eb
  source_range:
    end: 746900
    part: 0
    start: 0
  summary: GPQA 曾报告完成 99/198，随后在 FlashComm 开关对比中均为 175/198、准确率 88.38%，但由于 temperature=1.0 且无固定 seed，仅支持聚合精度未见回退，不能证明逐 token 或数值严格一致；相关服务后来按请求停止，结果和日志据称保留。针对使用 [REDACTED_SECRET] 权重、开启 Prefix Cache 的新测试，只读审计确认 TP16/DP4/EP64、FULL_DECODE_ONLY、language-model-only、无 FlashComm 配置及四机源码一致，但新服务未能启动：153 上已有 kimi-k3-pool 容器中的另一套 TP16 dummy 服务占用显存，触发 “Free memory ... is less than desired GPU memory utilization”，因此尚未进入 health/GPQA 阶段；此前关于已完成测试的报告与本次启动失败应分别保留。
  title: GPQA 多轮运行与 Prefix Cache 配置审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: aa36fee619eeed0f5f3e5267626b015302e4e79e67650ae3a92ce6389261f3dc
  kind: experience
  material_digest: 974ea56e53e83a850de2c163b3432a0c14d60f737bd7092d2c4940729c0b659c
  revision: 5bfe553ef7acd8caab1e936dde5655f869e6df568ccd7720379cd62792e1fac0
  schema: mindie-entry/3
  summary: 已有 GPQA 运行结果包括 175/198 的 FlashComm on/off 对比，但这是无固定 seed 的聚合观察，不是严格一致性证明；服务曾被停止。最新任务对 [REDACTED_SECRET] 权重的 Prefix Cache 配置仅完成只读核查，因 153 显存被另一套 kimi-k3-pool dummy K3 服务占用而启动失败，尚无新的精度结果。
  title: GPQA / K3 服务测试索引
navigation: 已有 GPQA 运行结果包括 175/198 的 FlashComm on/off 对比，但这是无固定 seed 的聚合观察，不是严格一致性证明；服务曾被停止。最新任务对 [REDACTED_SECRET] 权重的 Prefix Cache 配置仅完成只读核查，因 153 显存被另一套 kimi-k3-pool dummy K3 服务占用而启动失败，尚无新的精度结果。
schema: mindie-material-task/1
status: complete
task_id: aa36fee619eeed0f5f3e5267626b015302e4e79e67650ae3a92ce6389261f3dc
---

# GPQA / K3 服务测试索引

已有 GPQA 运行结果包括 175/198 的 FlashComm on/off 对比，但这是无固定 seed 的聚合观察，不是严格一致性证明；服务曾被停止。最新任务对 [REDACTED_SECRET] 权重的 Prefix Cache 配置仅完成只读核查，因 153 显存被另一套 kimi-k3-pool dummy K3 服务占用而启动失败，尚无新的精度结果。

## Materials

- [GPQA 多轮运行与 Prefix Cache 配置审计](blocks/2a11b72a8e725794405824e7a20a3fb9ee944925bed660d2b2980e201c576c6f.md): GPQA 曾报告完成 99/198，随后在 FlashComm 开关对比中均为 175/198、准确率 88.38%，但由于 temperature=1.0 且无固定 seed，仅支持聚合精度未见回退，不能证明逐 token 或数值严格一致；相关服务后来按请求停止，结果和日志据称保留。针对使用 [REDACTED_SECRET] 权重、开启 Prefix Cache 的新测试，只读审计确认 TP16/DP4/EP64、FULL_DECODE_ONLY、language-model-only、无 FlashComm 配置及四机源码一致，但新服务未能启动：153 上已有 kimi-k3-pool 容器中的另一套 TP16 dummy 服务占用显存，触发 “Free memory ... is less than desired GPU memory utilization”，因此尚未进入 health/GPQA 阶段；此前关于已完成测试的报告与本次启动失败应分别保留。

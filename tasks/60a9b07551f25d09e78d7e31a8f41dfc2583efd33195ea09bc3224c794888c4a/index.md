---
blocks:
- block_id: fa03c28ec94b9d989bf57cfd5d0130a3d2672aace913d4742afd2400054c214a
  indexed: true
  sha256: 68c7a2b7cb762322f4727106f65d4a10bc2b164cad88d0f8584995be3a99ef11
  source_range:
    end: 591840
    part: 0
    start: 0
  summary: 记录显示 GPQA 曾从 69/198、99/198 推进，随后按用户要求停止四机服务；相关结果和日志据称保留，但这些状态均应视为当时报告而非独立验证。后续只读比较中，FlashComm-off 与 on 在完整 198 题上均报告为 175/198，McNemar 双侧 p=1.0；由于 temperature=1.0 且无固定 seed，输出与 token 差异存在，不能据此证明严格等价或排除小效应。材料还记录了 id=4 被物理换行拆分、当时拼接解析未改文件，以及早期仅完成 163 题的配对差异：10 个预测变化互相抵消、双方均无请求错误。
  title: GPQA 服务终止与 FlashComm A/B 评测记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 60a9b07551f25d09e78d7e31a8f41dfc2583efd33195ea09bc3224c794888c4a
  kind: experience
  material_digest: df18011a9aa0fb33a4270feb382ce19c7f4a335d6bdd96f1465f9e72718099f1
  revision: ca44963337a75a9bf48abc45cac7d9241d46f90a119337cb0e0b701f1f3d1723
  schema: mindie-entry/3
  summary: 本批材料集中于四机服务生命周期、GPQA 进度与终止、FlashComm 开关的只读 A/B 比较及数据解析异常。可检索关键词包括：HTTP 200/000、K3 serve、FlashComm-off/on、temperature=1.0、无固定 seed、175/198、McNemar p=1.0、id=4 物理换行、请求错误/缺失；结果均为带不确定性的历史报告，不应提升为严格等价或已独立验证的成功结论。
  title: 当前任务导航：GPQA/FlashComm 评测材料索引
navigation: 本批材料集中于四机服务生命周期、GPQA 进度与终止、FlashComm 开关的只读 A/B 比较及数据解析异常。可检索关键词包括：HTTP 200/000、K3 serve、FlashComm-off/on、temperature=1.0、无固定 seed、175/198、McNemar p=1.0、id=4 物理换行、请求错误/缺失；结果均为带不确定性的历史报告，不应提升为严格等价或已独立验证的成功结论。
schema: mindie-material-task/1
status: complete
task_id: 60a9b07551f25d09e78d7e31a8f41dfc2583efd33195ea09bc3224c794888c4a
---

# 当前任务导航：GPQA/FlashComm 评测材料索引

本批材料集中于四机服务生命周期、GPQA 进度与终止、FlashComm 开关的只读 A/B 比较及数据解析异常。可检索关键词包括：HTTP 200/000、K3 serve、FlashComm-off/on、temperature=1.0、无固定 seed、175/198、McNemar p=1.0、id=4 物理换行、请求错误/缺失；结果均为带不确定性的历史报告，不应提升为严格等价或已独立验证的成功结论。

## Materials

- [GPQA 服务终止与 FlashComm A/B 评测记录](blocks/fa03c28ec94b9d989bf57cfd5d0130a3d2672aace913d4742afd2400054c214a.md): 记录显示 GPQA 曾从 69/198、99/198 推进，随后按用户要求停止四机服务；相关结果和日志据称保留，但这些状态均应视为当时报告而非独立验证。后续只读比较中，FlashComm-off 与 on 在完整 198 题上均报告为 175/198，McNemar 双侧 p=1.0；由于 temperature=1.0 且无固定 seed，输出与 token 差异存在，不能据此证明严格等价或排除小效应。材料还记录了 id=4 被物理换行拆分、当时拼接解析未改文件，以及早期仅完成 163 题的配对差异：10 个预测变化互相抵消、双方均无请求错误。

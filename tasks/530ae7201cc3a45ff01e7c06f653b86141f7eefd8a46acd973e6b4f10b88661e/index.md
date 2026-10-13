---
blocks:
- block_id: 4d13a8dc043b4abd356974b45b20684062f1a504ec201002c7fe4b4ec1f899dd
  indexed: true
  sha256: b6fe85f239b58727f5ad5cbc2bd8787ac740da98957eb61c2f5a71edc39dca22
  source_range:
    end: 446658
    part: 0
    start: 0
  summary: 只读检查显示 probe 日志为“前缀文本 + 单行 JSON”，同一 round 同时含 proposal 和 verification，提取 round=0 时需筛选 kind=proposal；记录了字段裁剪、row_stats 顺序及身份门禁条件。材料保留了 SGLang anchor 与旧 vLLM 的 q_fused、q_rope、attn_output 示例，并将 q/k 修正生效、纯 Torch helper 未匹配、attention backend 或后续层分叉列为判读假设，未将任何情况验证为成功。实现位置和远端命令含有已脱敏的路径、地址及容器信息。
  title: Probe 日志提取与 Q/K 数值对齐判读
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 530ae7201cc3a45ff01e7c06f653b86141f7eefd8a46acd973e6b4f10b88661e
  kind: experience
  material_digest: 662db73972e0a65d01834ee41aaffbe03b6694a841b5ae6a9c83e30cd51c0bba
  revision: f3ae1671407198d1484117fee28ffde0578f99aa47ea383eeeda63476f710bb0
  schema: mindie-entry/3
  summary: 本批材料聚焦于不操作远端的 probe 日志读取：从 kind=proposal、round=0 记录中提取压缩字段，并用 anchor_token_id=646、model_positions 与 draft_token_ids 做身份校验。后续可据提取结果区分 Q/K 修正、attention 输出、o_proj/decoder/final 或 fc/hidden_norm 的疑似分叉；现有材料仅提供方法、样例和判读条件，未确认最终对齐或根因。
  title: 当前任务导航：Probe 记录提取与数值定位
navigation: 本批材料聚焦于不操作远端的 probe 日志读取：从 kind=proposal、round=0 记录中提取压缩字段，并用 anchor_token_id=646、model_positions 与 draft_token_ids 做身份校验。后续可据提取结果区分 Q/K 修正、attention 输出、o_proj/decoder/final 或 fc/hidden_norm 的疑似分叉；现有材料仅提供方法、样例和判读条件，未确认最终对齐或根因。
schema: mindie-material-task/1
status: complete
task_id: 530ae7201cc3a45ff01e7c06f653b86141f7eefd8a46acd973e6b4f10b88661e
---

# 当前任务导航：Probe 记录提取与数值定位

本批材料聚焦于不操作远端的 probe 日志读取：从 kind=proposal、round=0 记录中提取压缩字段，并用 anchor_token_id=646、model_positions 与 draft_token_ids 做身份校验。后续可据提取结果区分 Q/K 修正、attention 输出、o_proj/decoder/final 或 fc/hidden_norm 的疑似分叉；现有材料仅提供方法、样例和判读条件，未确认最终对齐或根因。

## Materials

- [Probe 日志提取与 Q/K 数值对齐判读](blocks/4d13a8dc043b4abd356974b45b20684062f1a504ec201002c7fe4b4ec1f899dd.md): 只读检查显示 probe 日志为“前缀文本 + 单行 JSON”，同一 round 同时含 proposal 和 verification，提取 round=0 时需筛选 kind=proposal；记录了字段裁剪、row_stats 顺序及身份门禁条件。材料保留了 SGLang anchor 与旧 vLLM 的 q_fused、q_rope、attn_output 示例，并将 q/k 修正生效、纯 Torch helper 未匹配、attention backend 或后续层分叉列为判读假设，未将任何情况验证为成功。实现位置和远端命令含有已脱敏的路径、地址及容器信息。

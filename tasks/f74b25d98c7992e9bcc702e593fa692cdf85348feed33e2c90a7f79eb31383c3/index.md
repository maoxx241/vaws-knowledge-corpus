---
blocks:
- block_id: d1396becf0364ce8d2877f7521fdbd11c137ca09783816771035f994afd6085b
  indexed: true
  sha256: 5cb3dd768c01862c8e130ee9a64f2718576b5c6d302a2403a04ded392d8032a9
  source_range:
    end: 863148
    part: 0
    start: 0
  summary: 只读核对报告称 vLLM 0.23.0 可通过 `return_token_ids` 获取 `choices[0].token_ids` 与顶层 `prompt_token_ids`，并指出 K3 的 `vision_chunk` 可能需要 `use_unified_vision_chunk=True`；这些是报告中的协议与配置结论，尚未在此材料中验证为成功。材料提出 TP16/EP16、`FULL_DECODE_ONLY`、FlashComm1 off/on 的 text/image 四 case、每 case 重复请求及 token 一致性比较，并列出 HTTP 200、ACLGraph 日志和若干错误字符串作为门禁；实际执行结果、失败尝试和最终状态未提供。
  title: vLLM 多模态分布式推理验证方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f74b25d98c7992e9bcc702e593fa692cdf85348feed33e2c90a7f79eb31383c3
  kind: experience
  material_digest: 8db9346d38cd6b15b5ead3245134a293c74f9fa84d62277866a461662a074af0
  revision: abd5d9c49c10d06a6dc56238057af72b8583a3c2e37f47cef7b63a9bced20932
  schema: mindie-entry/3
  summary: 已索引一份关于 vLLM 0.23.0、K3 `vision_chunk` 配置、TP16/EP16、ACLGraph 与 FlashComm1 对比验证的材料。核心内容是待执行的四 case 验证方案及其判定条件；目前没有材料证明方案已成功执行，也没有提供实际错误、日志结果或后续修正。
  title: 当前任务导航：K3 vLLM 多模态验证
navigation: 已索引一份关于 vLLM 0.23.0、K3 `vision_chunk` 配置、TP16/EP16、ACLGraph 与 FlashComm1 对比验证的材料。核心内容是待执行的四 case 验证方案及其判定条件；目前没有材料证明方案已成功执行，也没有提供实际错误、日志结果或后续修正。
schema: mindie-material-task/1
status: complete
task_id: f74b25d98c7992e9bcc702e593fa692cdf85348feed33e2c90a7f79eb31383c3
---

# 当前任务导航：K3 vLLM 多模态验证

已索引一份关于 vLLM 0.23.0、K3 `vision_chunk` 配置、TP16/EP16、ACLGraph 与 FlashComm1 对比验证的材料。核心内容是待执行的四 case 验证方案及其判定条件；目前没有材料证明方案已成功执行，也没有提供实际错误、日志结果或后续修正。

## Materials

- [vLLM 多模态分布式推理验证方案](blocks/d1396becf0364ce8d2877f7521fdbd11c137ca09783816771035f994afd6085b.md): 只读核对报告称 vLLM 0.23.0 可通过 `return_token_ids` 获取 `choices[0].token_ids` 与顶层 `prompt_token_ids`，并指出 K3 的 `vision_chunk` 可能需要 `use_unified_vision_chunk=True`；这些是报告中的协议与配置结论，尚未在此材料中验证为成功。材料提出 TP16/EP16、`FULL_DECODE_ONLY`、FlashComm1 off/on 的 text/image 四 case、每 case 重复请求及 token 一致性比较，并列出 HTTP 200、ACLGraph 日志和若干错误字符串作为门禁；实际执行结果、失败尝试和最终状态未提供。

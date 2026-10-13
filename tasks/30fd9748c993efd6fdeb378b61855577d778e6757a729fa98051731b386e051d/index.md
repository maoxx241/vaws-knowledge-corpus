---
blocks:
- block_id: a6e7717d44a9402a01fc946186d29cfcf1212495772ebf67324a2ea598bb17be
  indexed: true
  sha256: f74a3a906f2dc345193a5dfe741ecf2a519ed4ab0dc1e8aef3a2b09230ac6bb7
  source_range:
    end: 864785
    part: 0
    start: 0
  summary: 审计比较了 target verify 到下一轮 draft 的 accepted length、slot mapping、KV 写入范围和 seq_len 推进：SGLang 仅写入 accepted prefix，vLLM padded 路径虽先写完整 target verify KV，随后会在 attention 读取前从 accepted 边界起覆盖 rejected suffix；非 padded 路径则已压紧为 accepted prefix。结论认为两边下一轮 seq_len 均为 P+A+1、slot 起点一致，rejected suffix 不会导致下一轮读脏，因此该差异不能解释低接收率或首个 block 的 pos1 分叉；将 rejected context slot 设为 -1 仅是可选的 prefix-cache 语义对齐，未验证为修复方案。
  title: SGLang 与 vLLM 提交链审计结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 30fd9748c993efd6fdeb378b61855577d778e6757a729fa98051731b386e051d
  kind: experience
  material_digest: 0cc39402861026962f34c1b96e89b49ab8c4c16d7d5b8db4333dd354e9043be8
  revision: 5ce8df41154f609407075c8e9e29248e8735c01d6750beaa4d8e553567750661
  schema: mindie-entry/3
  summary: 已记录一项关于 SGLang/vLLM target verify 提交差异的审计结论，涵盖 accepted prefix、rejected suffix、KV 覆盖时序、slot mapping 与 seq_len=P+A+1。当前未确认该差异是低接收率或首 block pos1 分叉的根因；commit mask 或 -1 slot 仅作为后续语义对齐方向，不能视为已验证成功。
  title: 当前任务导航：提交链与 KV 覆盖范围审计
navigation: 已记录一项关于 SGLang/vLLM target verify 提交差异的审计结论，涵盖 accepted prefix、rejected suffix、KV 覆盖时序、slot mapping 与 seq_len=P+A+1。当前未确认该差异是低接收率或首 block pos1 分叉的根因；commit mask 或 -1 slot 仅作为后续语义对齐方向，不能视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 30fd9748c993efd6fdeb378b61855577d778e6757a729fa98051731b386e051d
---

# 当前任务导航：提交链与 KV 覆盖范围审计

已记录一项关于 SGLang/vLLM target verify 提交差异的审计结论，涵盖 accepted prefix、rejected suffix、KV 覆盖时序、slot mapping 与 seq_len=P+A+1。当前未确认该差异是低接收率或首 block pos1 分叉的根因；commit mask 或 -1 slot 仅作为后续语义对齐方向，不能视为已验证成功。

## Materials

- [SGLang 与 vLLM 提交链审计结论](blocks/a6e7717d44a9402a01fc946186d29cfcf1212495772ebf67324a2ea598bb17be.md): 审计比较了 target verify 到下一轮 draft 的 accepted length、slot mapping、KV 写入范围和 seq_len 推进：SGLang 仅写入 accepted prefix，vLLM padded 路径虽先写完整 target verify KV，随后会在 attention 读取前从 accepted 边界起覆盖 rejected suffix；非 padded 路径则已压紧为 accepted prefix。结论认为两边下一轮 seq_len 均为 P+A+1、slot 起点一致，rejected suffix 不会导致下一轮读脏，因此该差异不能解释低接收率或首个 block 的 pos1 分叉；将 rejected context slot 设为 -1 仅是可选的 prefix-cache 语义对齐，未验证为修复方案。

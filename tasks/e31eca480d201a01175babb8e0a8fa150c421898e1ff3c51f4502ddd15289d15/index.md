---
blocks:
- block_id: 6ff1a61cf0021d77f36c1f8d9c476dc05157f5bbae2b8493a06c1060a92589d4
  indexed: true
  sha256: f3e5eab380156d2bd09074dd5f9ac84303d66835a243d34a70d34551778226ca
  source_range:
    end: 1768757
    part: 0
    start: 0
  summary: 只读诊断将根因优先定位为 KDA/GDN FULL graph 的 padding metadata 不一致，而非 DSpark draft graph 或 rejection sampler：`is_linear_attn=True` 未被 `check_gdn_layer()` 识别，使 `_has_gdn=False`，并可能把 dummy 误判为 prefill；相关修复提交为 `0e5adb7b6`。另一个已识别问题是零长度 dummy 的 `num_accepted_tokens` 被填为 1，应改为 0，相关修复为 `d4ed5fe86`；该修复据称已应用，但第一处检查截至最后观察仍未应用，因此组合修复尚未被验证。报告还保留了未决风险：ACLGraph 对嵌套 aux list 的 weak-ref 处理可能不稳，只有在上述修复后 aux 仍异常时才建议进一步参考 `4ae1d9bba`；后续门禁应检查 `_has_gdn=True`、`num_prefills=0`、`num_spec_decodes=1` 及 `[0,8,8]`、`[0,8,0]`、`[real_value,0]` 等 metadata，并比较 graph/eager 的逐位置 token 与接收率。
  title: V1 DSpark FULL graph 中 KDA/GDN padding metadata 诊断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e31eca480d201a01175babb8e0a8fa150c421898e1ff3c51f4502ddd15289d15
  kind: experience
  material_digest: c9d80194800bff2a3a94ca697b84e74cce3e055823e4fd3364fceade93b9099c
  revision: fd6b961f204287fd40d13f062794b373a6d45d86edfc3d8da42a1b7ba4f3783e
  schema: mindie-entry/3
  summary: 当前批次包含一份关于 V1 ACLGraph、DSpark proposer 及 KDA/GDN FULL graph padding metadata 的只读诊断。可检索的关键线索包括 `_has_gdn=False`、`is_linear_attn=True`、`num_accepted_tokens=1` 的 dummy padding、`0e5adb7b6`、`d4ed5fe86`、`4ae1d9bba`、token 0 虚高接收率，以及尚未验证的组合修复和后续 graph/eager 对比门禁；原始材料仍独立可搜索。
  title: 当前任务导航：V1 DSpark FULL graph / KDA-GDN padding 诊断索引
navigation: 当前批次包含一份关于 V1 ACLGraph、DSpark proposer 及 KDA/GDN FULL graph padding metadata 的只读诊断。可检索的关键线索包括 `_has_gdn=False`、`is_linear_attn=True`、`num_accepted_tokens=1` 的 dummy padding、`0e5adb7b6`、`d4ed5fe86`、`4ae1d9bba`、token 0 虚高接收率，以及尚未验证的组合修复和后续 graph/eager 对比门禁；原始材料仍独立可搜索。
schema: mindie-material-task/1
status: complete
task_id: e31eca480d201a01175babb8e0a8fa150c421898e1ff3c51f4502ddd15289d15
---

# 当前任务导航：V1 DSpark FULL graph / KDA-GDN padding 诊断索引

当前批次包含一份关于 V1 ACLGraph、DSpark proposer 及 KDA/GDN FULL graph padding metadata 的只读诊断。可检索的关键线索包括 `_has_gdn=False`、`is_linear_attn=True`、`num_accepted_tokens=1` 的 dummy padding、`0e5adb7b6`、`d4ed5fe86`、`4ae1d9bba`、token 0 虚高接收率，以及尚未验证的组合修复和后续 graph/eager 对比门禁；原始材料仍独立可搜索。

## Materials

- [V1 DSpark FULL graph 中 KDA/GDN padding metadata 诊断](blocks/6ff1a61cf0021d77f36c1f8d9c476dc05157f5bbae2b8493a06c1060a92589d4.md): 只读诊断将根因优先定位为 KDA/GDN FULL graph 的 padding metadata 不一致，而非 DSpark draft graph 或 rejection sampler：`is_linear_attn=True` 未被 `check_gdn_layer()` 识别，使 `_has_gdn=False`，并可能把 dummy 误判为 prefill；相关修复提交为 `0e5adb7b6`。另一个已识别问题是零长度 dummy 的 `num_accepted_tokens` 被填为 1，应改为 0，相关修复为 `d4ed5fe86`；该修复据称已应用，但第一处检查截至最后观察仍未应用，因此组合修复尚未被验证。报告还保留了未决风险：ACLGraph 对嵌套 aux list 的 weak-ref 处理可能不稳，只有在上述修复后 aux 仍异常时才建议进一步参考 `4ae1d9bba`；后续门禁应检查 `_has_gdn=True`、`num_prefills=0`、`num_spec_decodes=1` 及 `[0,8,8]`、`[0,8,0]`、`[real_value,0]` 等 metadata，并比较 graph/eager 的逐位置 token 与接收率。

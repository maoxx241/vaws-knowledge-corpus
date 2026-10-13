---
blocks:
- block_id: d4787d81e87db80444e49de59a6c36a79fe875fb8d22446cea9547118687c743
  indexed: true
  sha256: 35448dd3b513a5cdb4a2b5f5d42f9d772c8e355c65a09409d31dce414a1e72d3
  source_range:
    end: 1307083
    part: 0
    start: 0
  summary: 只读分析显示 hot 分配 26/2/2/2 时 30/32 成功，失败为 rank0 的 job7、job11，而对称的 8/8/8/8 分配 32/32 通过；首个 FULL step2 在进入 ACLGraph 前已出现 `!!!`，因此尚不能将原因限定为 FULL 图尾部 padding。NaN 位置跨请求变化，现象更符合前序并发/NONE 阶段的 KDA 状态偶发污染，但这仍是推测而非已验证根因；dummy reset 未改善数值，且 r9 仍可复现，不能视为修复。记录的环境/方法线索包括 `enable_shared_expert_dp=true`、`multistream=false`、EP64/FlashComm1、无 graph request padding、prefix-cache 0%，后续需补 NONE 路径诊断并比较 target enforce-eager、单 rank 高并发与对称多 rank 场景。
  title: 偶现 NaN 与并发/NONE 阶段状态污染线索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 410258bbd7b5ddc4894e809d731ad10547ddd013c4aa2c5f393f123758694a57
  kind: experience
  material_digest: a072fcff17b4347912d1ec109c0acb79bd860a1dc2de9b4928384dcbbd886d73
  revision: fd6cde1de8bd0267796a3a9b8803df180089a176f024c8013a1bc2befc15875f
  schema: mindie-entry/3
  summary: 当前材料保留了 26/2/2/2 hot 分配下的偶现失败、对称分配通过、FULL 图前已形成 `!!!` 以及 NaN 状态变化等观察；这些结果支持并发或 DP dummy/collective padding 交互的假设，但尚未验证根因。dummy reset 未带来改善，后续诊断仍未完成：需覆盖 NONE 路径的 hidden/KDA state，并复跑 enforce-eager、单 rank 高并发和四 rank 对称高并发对照。原始材料仍应独立检索，不能据此声称问题已修复或已确认。
  title: 当前任务导航：并发/NONE 阶段 NaN 复现分析
navigation: 当前材料保留了 26/2/2/2 hot 分配下的偶现失败、对称分配通过、FULL 图前已形成 `!!!` 以及 NaN 状态变化等观察；这些结果支持并发或 DP dummy/collective padding 交互的假设，但尚未验证根因。dummy reset 未带来改善，后续诊断仍未完成：需覆盖 NONE 路径的 hidden/KDA state，并复跑 enforce-eager、单 rank 高并发和四 rank 对称高并发对照。原始材料仍应独立检索，不能据此声称问题已修复或已确认。
schema: mindie-material-task/1
status: complete
task_id: 410258bbd7b5ddc4894e809d731ad10547ddd013c4aa2c5f393f123758694a57
---

# 当前任务导航：并发/NONE 阶段 NaN 复现分析

当前材料保留了 26/2/2/2 hot 分配下的偶现失败、对称分配通过、FULL 图前已形成 `!!!` 以及 NaN 状态变化等观察；这些结果支持并发或 DP dummy/collective padding 交互的假设，但尚未验证根因。dummy reset 未带来改善，后续诊断仍未完成：需覆盖 NONE 路径的 hidden/KDA state，并复跑 enforce-eager、单 rank 高并发和四 rank 对称高并发对照。原始材料仍应独立检索，不能据此声称问题已修复或已确认。

## Materials

- [偶现 NaN 与并发/NONE 阶段状态污染线索](blocks/d4787d81e87db80444e49de59a6c36a79fe875fb8d22446cea9547118687c743.md): 只读分析显示 hot 分配 26/2/2/2 时 30/32 成功，失败为 rank0 的 job7、job11，而对称的 8/8/8/8 分配 32/32 通过；首个 FULL step2 在进入 ACLGraph 前已出现 `!!!`，因此尚不能将原因限定为 FULL 图尾部 padding。NaN 位置跨请求变化，现象更符合前序并发/NONE 阶段的 KDA 状态偶发污染，但这仍是推测而非已验证根因；dummy reset 未改善数值，且 r9 仍可复现，不能视为修复。记录的环境/方法线索包括 `enable_shared_expert_dp=true`、`multistream=false`、EP64/FlashComm1、无 graph request padding、prefix-cache 0%，后续需补 NONE 路径诊断并比较 target enforce-eager、单 rank 高并发与对称多 rank 场景。

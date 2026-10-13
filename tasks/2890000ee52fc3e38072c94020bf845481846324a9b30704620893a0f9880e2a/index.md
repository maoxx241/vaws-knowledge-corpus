---
blocks:
- block_id: 1d7995147e012ba840c8a8bea86c5454b6760fa2cdf44f7778bafb870baf69dd
  indexed: true
  sha256: 6ccebeae764cbe992672ed1835b52b3e735f7a91f6a84f0f3620fdaae20b7a95
  source_range:
    end: 558184
    part: 0
    start: 0
  summary: 对 vllm_ascend 的 `gdn_attn_builder.py` 中 `num_accepted_tokens[num_spec_decodes:].fill_(0)` 的静态路径分析认为：该写入确实会修改持久 NPU metadata buffer，但对 A3/A5 零长度 dummy row 目前基本是功能性 no-op，不能视为修复 `4!!!`。A3 通用 recurrent KDA 和 A5 arch35 都在读取 `numAcceptedTokens` 前跳过 `seqLen == 0`；causal-conv1d 中 `accepted=1` 与改为 `0` 最终都得到相同的 clamp 后 offset，dummy 状态写回也直接返回。现有 `2 passed` 仅是 A3 环境的 CPU builder UT，A3 四机尚未完成 FULL replay，A5 没有实机 A/B 证据，因此仍缺少 NPU kernel 输出、ACLGraph replay、aux hidden-state 和真实 8-token 输出的验证。
  title: A3/A5 上 fill_(0) 对 dummy metadata 的有效性
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2890000ee52fc3e38072c94020bf845481846324a9b30704620893a0f9880e2a
  kind: experience
  material_digest: 6500778f40aedc3b416b30993cb97288eef6531687dcd396f1f0bdc943d277ca
  revision: 6ab1a280936e6f0ac79a74aeadca6d8f38bd251121c9a8e768b5be0e04398c29
  schema: mindie-entry/3
  summary: 当前证据支持：`fill_(0)` 能更新 metadata buffer，但对 A3/A5 零长度 dummy row 预计不改变实际消费者行为，不能确认其修复乱码或 `4!!!`。已知验证仅覆盖 A3 CPU builder UT；A3 四机 replay 尚未完成，A5 尚无实机 A/B 结果。后续应核查 ACLGraph replay、静态 buffer 更新、aux hidden-state、真实 8-token 输出及首层 KDA 输入，并保留该结论的未验证状态。
  title: A3/A5 metadata 清零有效性索引
navigation: 当前证据支持：`fill_(0)` 能更新 metadata buffer，但对 A3/A5 零长度 dummy row 预计不改变实际消费者行为，不能确认其修复乱码或 `4!!!`。已知验证仅覆盖 A3 CPU builder UT；A3 四机 replay 尚未完成，A5 尚无实机 A/B 结果。后续应核查 ACLGraph replay、静态 buffer 更新、aux hidden-state、真实 8-token 输出及首层 KDA 输入，并保留该结论的未验证状态。
schema: mindie-material-task/1
status: complete
task_id: 2890000ee52fc3e38072c94020bf845481846324a9b30704620893a0f9880e2a
---

# A3/A5 metadata 清零有效性索引

当前证据支持：`fill_(0)` 能更新 metadata buffer，但对 A3/A5 零长度 dummy row 预计不改变实际消费者行为，不能确认其修复乱码或 `4!!!`。已知验证仅覆盖 A3 CPU builder UT；A3 四机 replay 尚未完成，A5 尚无实机 A/B 结果。后续应核查 ACLGraph replay、静态 buffer 更新、aux hidden-state、真实 8-token 输出及首层 KDA 输入，并保留该结论的未验证状态。

## Materials

- [A3/A5 上 fill_(0) 对 dummy metadata 的有效性](blocks/1d7995147e012ba840c8a8bea86c5454b6760fa2cdf44f7778bafb870baf69dd.md): 对 vllm_ascend 的 `gdn_attn_builder.py` 中 `num_accepted_tokens[num_spec_decodes:].fill_(0)` 的静态路径分析认为：该写入确实会修改持久 NPU metadata buffer，但对 A3/A5 零长度 dummy row 目前基本是功能性 no-op，不能视为修复 `4!!!`。A3 通用 recurrent KDA 和 A5 arch35 都在读取 `numAcceptedTokens` 前跳过 `seqLen == 0`；causal-conv1d 中 `accepted=1` 与改为 `0` 最终都得到相同的 clamp 后 offset，dummy 状态写回也直接返回。现有 `2 passed` 仅是 A3 环境的 CPU builder UT，A3 四机尚未完成 FULL replay，A5 没有实机 A/B 证据，因此仍缺少 NPU kernel 输出、ACLGraph replay、aux hidden-state 和真实 8-token 输出的验证。

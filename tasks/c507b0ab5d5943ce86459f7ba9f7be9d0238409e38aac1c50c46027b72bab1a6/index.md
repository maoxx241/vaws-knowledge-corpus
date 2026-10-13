---
blocks:
- block_id: bcfeb520cf530f7cef7be802aa6917cad74190d1f2b54331892b630f519e3d02
  indexed: true
  sha256: 9f5d5a82a0fc25eae64c5cb92bf7a269b9dc1fe1c8bded43677195c8d886d027
  source_range:
    end: 857128
    part: 0
    start: 0
  summary: 只读审查认为首层 VL 判定和 KDA gather-once 设计并非主要问题；较精确的疑似根因是 FlashComm 首层输出维度在 Kimi learned-residual 路径中被 trace 样例固化为 1，进而使 layer 0 MLP 与 layer 1 KDA 的 token 维出现固定的 16 rows，而 profile runtime 的 s59=2048 导致 `[2048,768]` 张量撞上 `[16,6,128]` view。该判断基于失败图和运行时观察，profile 阶段 `attn_metadata=None`，因此尚不能归因于 AscendC/Triton KDA core 或 metadata slicing；`maybe_chunk_residual(prefix_sum, result)` 被建议作为 shape anchor，但仍需 TP16 N=16 trace、N=2048 执行、完整 profile_run、prefill/decode 及四种服务请求回归验证。另有未解决的不确定性：fake `maybe_all_gather_and_maybe_unpad` 未表达非整除或 DP padding 的 unpad 行为，当前 profile 的 pad 为 0。
  title: TP16 动态 token 维与 KDA reshape 失败审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c507b0ab5d5943ce86459f7ba9f7be9d0238409e38aac1c50c46027b72bab1a6
  kind: experience
  material_digest: ac74b692c6b3f65b667ce6f05e4f924783f836c10f614644811ef44c7eadde1b
  revision: 019e01f7aeefde3bc5237b26d2c80fe9b9b89928b31911532a4505de4bb8e1ac
  schema: mindie-entry/3
  summary: 当前线索指向 learned-residual 路径中的 trace-shape 固化，而非首层布局误判或 KDA metadata slicing；尚未验证修复成功。后续应保留失败入口，验证 shape anchor、非零 padding、prefill/decode 以及 FlashComm 开关和文本/图片请求的一致性与 ACLGraph replay 稳定性。
  title: 动态 token 维固化导致的 TP16 KDA reshape 调查
navigation: 当前线索指向 learned-residual 路径中的 trace-shape 固化，而非首层布局误判或 KDA metadata slicing；尚未验证修复成功。后续应保留失败入口，验证 shape anchor、非零 padding、prefill/decode 以及 FlashComm 开关和文本/图片请求的一致性与 ACLGraph replay 稳定性。
schema: mindie-material-task/1
status: complete
task_id: c507b0ab5d5943ce86459f7ba9f7be9d0238409e38aac1c50c46027b72bab1a6
---

# 动态 token 维固化导致的 TP16 KDA reshape 调查

当前线索指向 learned-residual 路径中的 trace-shape 固化，而非首层布局误判或 KDA metadata slicing；尚未验证修复成功。后续应保留失败入口，验证 shape anchor、非零 padding、prefill/decode 以及 FlashComm 开关和文本/图片请求的一致性与 ACLGraph replay 稳定性。

## Materials

- [TP16 动态 token 维与 KDA reshape 失败审查](blocks/bcfeb520cf530f7cef7be802aa6917cad74190d1f2b54331892b630f519e3d02.md): 只读审查认为首层 VL 判定和 KDA gather-once 设计并非主要问题；较精确的疑似根因是 FlashComm 首层输出维度在 Kimi learned-residual 路径中被 trace 样例固化为 1，进而使 layer 0 MLP 与 layer 1 KDA 的 token 维出现固定的 16 rows，而 profile runtime 的 s59=2048 导致 `[2048,768]` 张量撞上 `[16,6,128]` view。该判断基于失败图和运行时观察，profile 阶段 `attn_metadata=None`，因此尚不能归因于 AscendC/Triton KDA core 或 metadata slicing；`maybe_chunk_residual(prefix_sum, result)` 被建议作为 shape anchor，但仍需 TP16 N=16 trace、N=2048 执行、完整 profile_run、prefill/decode 及四种服务请求回归验证。另有未解决的不确定性：fake `maybe_all_gather_and_maybe_unpad` 未表达非整除或 DP padding 的 unpad 行为，当前 profile 的 pad 为 0。

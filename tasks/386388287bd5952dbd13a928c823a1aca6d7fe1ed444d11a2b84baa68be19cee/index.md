---
blocks:
- block_id: 776e1a44e1ed157b94e90f49529da5e260ba5caabf86a8b697e93481cef19ed8
  indexed: true
  sha256: 8314a91ef0dbc8cd2181f73faeb972cab23a86c55375b6c2dd464009b396ad28
  source_range:
    end: 883526
    part: 0
    start: 0
  summary: 审计结论显示，当前 rank0 的 probe8 Qwen3 DSpark（DFlash backbone）设置为 target_mode=eager，并由顶层 --enforce-eager 关闭 torch.compile、CUDAGraph/ACLGraph；日志中的编译、fusion 和 draft ACLGraph marker 均为 0 次。speculative config 的 enforce_eager 主要关闭 draft graph，但 enable_flashcomm1 触发的 eager context 还会使 draft 模型标记为 do_not_compile，DSpark proposer 也无条件禁用 CUDA graph。虽然 fuse_qknorm_rope 默认开启且诊断补丁调用过 split_qkv_rmsnorm_rope，但真实 forward 仍是 separate q_norm → k_norm → rotary_emb → attention；此外 head_dim=64 不符合当前仅支持 head_size==128 的 GraphFusion pattern，因此低接收率不应归因于实际执行了 vLLM GraphFusion。
  title: probe8 实际为 eager 路径
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 386388287bd5952dbd13a928c823a1aca6d7fe1ed444d11a2b84baa68be19cee
  kind: experience
  material_digest: 17ae051648aa348ea2741dd93df94e0905719bdd322432a5eeee65095070b988
  revision: 4dafb2874a0e0f93d50be306089dc78ad465bf35c8f0c8f1ce8d4b4a16fe7c0c
  schema: mindie-entry/3
  summary: 本批唯一材料记录了 probe8 的只读审计结论：实际 forward 未经过 torch.compile、GraphFusionPassManager 或 draft ACLGraph replay，当前对比重点应是 vLLM separate eager 路径与 SGLang fused split_qkv_rmsnorm_rope 的逐层数值差异。保留的不确定性包括诊断旁路产生的 fused 数值不能代表真实 forward，以及低接收率归因尚未被验证；原始材料仍可独立检索。
  title: 当前任务导航：probe8 eager 与 QKNorm+RoPE fusion 审计
navigation: 本批唯一材料记录了 probe8 的只读审计结论：实际 forward 未经过 torch.compile、GraphFusionPassManager 或 draft ACLGraph replay，当前对比重点应是 vLLM separate eager 路径与 SGLang fused split_qkv_rmsnorm_rope 的逐层数值差异。保留的不确定性包括诊断旁路产生的 fused 数值不能代表真实 forward，以及低接收率归因尚未被验证；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 386388287bd5952dbd13a928c823a1aca6d7fe1ed444d11a2b84baa68be19cee
---

# 当前任务导航：probe8 eager 与 QKNorm+RoPE fusion 审计

本批唯一材料记录了 probe8 的只读审计结论：实际 forward 未经过 torch.compile、GraphFusionPassManager 或 draft ACLGraph replay，当前对比重点应是 vLLM separate eager 路径与 SGLang fused split_qkv_rmsnorm_rope 的逐层数值差异。保留的不确定性包括诊断旁路产生的 fused 数值不能代表真实 forward，以及低接收率归因尚未被验证；原始材料仍可独立检索。

## Materials

- [probe8 实际为 eager 路径](blocks/776e1a44e1ed157b94e90f49529da5e260ba5caabf86a8b697e93481cef19ed8.md): 审计结论显示，当前 rank0 的 probe8 Qwen3 DSpark（DFlash backbone）设置为 target_mode=eager，并由顶层 --enforce-eager 关闭 torch.compile、CUDAGraph/ACLGraph；日志中的编译、fusion 和 draft ACLGraph marker 均为 0 次。speculative config 的 enforce_eager 主要关闭 draft graph，但 enable_flashcomm1 触发的 eager context 还会使 draft 模型标记为 do_not_compile，DSpark proposer 也无条件禁用 CUDA graph。虽然 fuse_qknorm_rope 默认开启且诊断补丁调用过 split_qkv_rmsnorm_rope，但真实 forward 仍是 separate q_norm → k_norm → rotary_emb → attention；此外 head_dim=64 不符合当前仅支持 head_size==128 的 GraphFusion pattern，因此低接收率不应归因于实际执行了 vLLM GraphFusion。

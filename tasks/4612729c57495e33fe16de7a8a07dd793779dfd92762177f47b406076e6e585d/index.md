---
blocks:
- block_id: 9e0193351025394f16bf740963b62db7a8dd2cdae10ab171fc0ead8158ccf273
  indexed: true
  sha256: a9e51f6bde6e30c2c8ae8caabbc5a3fd1124e7ae6ef8890d58ceb5a9ffe0e716
  source_range:
    end: 1740288
    part: 0
    start: 0
  summary: 只读审查认为，A3 selector 不会进入存在 latent transform 时序问题的 ALLGATHER 路径；在 MC2/ALLTOALL 下，routed expert 已聚合，FlashComm1 关闭时 shared expert 另行 TP all-reduce，开启时复制权重并直接产生完整 token shard。K3 full-width shared input 已透传，旧的 3584 维 routed latent 误传给 7168 维 shared expert 的风险已修复；但不同 GEMM/通信组合不保证 bitwise 相同，且尚未证明首 token 偏差由 MoE 或 FlashComm1 引起。另记录一个未修复问题：ALLGATHER 且 FlashComm1 关闭时可能计算为 `sum_r Up(RMSNorm(partial_r))`，而正确语义应为 `Up(RMSNorm(sum_r partial_r))`；本次未改文件或操作远端。
  title: K3 A3/EP16 FlashComm1 与 MoE 数据布局审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4612729c57495e33fe16de7a8a07dd793779dfd92762177f47b406076e6e585d
  kind: experience
  material_digest: 7e955e0141d054dd210019ff7f74e9b1fa743a5318faed2f586172aec3eb3d16
  revision: c9166ce8100a051db780059683108f7c889e11e5d1a864e2333e575f50350563
  schema: mindie-entry/3
  summary: 本批材料聚焦 A3 + EP16 下 routed/shared expert 的 global/TP-local 数据布局、FlashComm1 通信路径及首 token 偏差排查。当前结论是不支持将确定的规约错误归因于本次 MoE 路径，但保留 ALLGATHER 场景的 latent 规约顺序问题、不同执行组合导致的非 bitwise 等价，以及尚未完成的首 token 根因判断。
  title: 当前任务导航：K3 MoE/FlashComm1 审查
navigation: 本批材料聚焦 A3 + EP16 下 routed/shared expert 的 global/TP-local 数据布局、FlashComm1 通信路径及首 token 偏差排查。当前结论是不支持将确定的规约错误归因于本次 MoE 路径，但保留 ALLGATHER 场景的 latent 规约顺序问题、不同执行组合导致的非 bitwise 等价，以及尚未完成的首 token 根因判断。
schema: mindie-material-task/1
status: complete
task_id: 4612729c57495e33fe16de7a8a07dd793779dfd92762177f47b406076e6e585d
---

# 当前任务导航：K3 MoE/FlashComm1 审查

本批材料聚焦 A3 + EP16 下 routed/shared expert 的 global/TP-local 数据布局、FlashComm1 通信路径及首 token 偏差排查。当前结论是不支持将确定的规约错误归因于本次 MoE 路径，但保留 ALLGATHER 场景的 latent 规约顺序问题、不同执行组合导致的非 bitwise 等价，以及尚未完成的首 token 根因判断。

## Materials

- [K3 A3/EP16 FlashComm1 与 MoE 数据布局审查](blocks/9e0193351025394f16bf740963b62db7a8dd2cdae10ab171fc0ead8158ccf273.md): 只读审查认为，A3 selector 不会进入存在 latent transform 时序问题的 ALLGATHER 路径；在 MC2/ALLTOALL 下，routed expert 已聚合，FlashComm1 关闭时 shared expert 另行 TP all-reduce，开启时复制权重并直接产生完整 token shard。K3 full-width shared input 已透传，旧的 3584 维 routed latent 误传给 7168 维 shared expert 的风险已修复；但不同 GEMM/通信组合不保证 bitwise 相同，且尚未证明首 token 偏差由 MoE 或 FlashComm1 引起。另记录一个未修复问题：ALLGATHER 且 FlashComm1 关闭时可能计算为 `sum_r Up(RMSNorm(partial_r))`，而正确语义应为 `Up(RMSNorm(sum_r partial_r))`；本次未改文件或操作远端。

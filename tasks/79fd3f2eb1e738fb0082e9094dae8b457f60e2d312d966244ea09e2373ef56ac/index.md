---
blocks:
- block_id: 8a0e38ef05613c4fb6be4d681306ce4b85a9dd44fa91bc76d38a271bb162be72
  indexed: true
  sha256: f97fda75cdc1fdd1ccbdf834bde6e3ddaa68ed8f3dc72575aad6849b57f349be
  source_range:
    end: 670328
    part: 0
    start: 0
  summary: 本批次围绕 ALLGATHER/共享专家归约、TP/DP/SP 契约，以及 W4A16MXFP + SiTU 的激活精度路径进行了只读静态核查；记录称相关代码位置和回归测试已补齐，`git diff --check` 通过。审查报告未发现新的 P0/P1，但明确未执行 pytest 或 NPU runtime 验证，且 `is_sequence_parallel` 与 Ascend 全局 `enable_sp()` 的一致性曾作为需要核实的边界。相关路径包含 `fused_moe.py`、`prepare_finalize.py`、`moe_mlp.py`，其中 W4A16 路径保持 BF16/FP16 的 `GMM1 → SiTU → GMM2`，并先于 W4A8 动态量化分支。
  title: MoE 归约与 W4A16MXFP/SiTU 静态审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 79fd3f2eb1e738fb0082e9094dae8b457f60e2d312d966244ea09e2373ef56ac
  kind: experience
  material_digest: a7c0de3cbc4726ab921f18865834459a397cbb0d8c1d8fae7bcdede7ed9b494b
  revision: 5a5392071b4109fb3c3b012e47669dbfac077b962d0d8bd598b86c4543a9aea4
  schema: mindie-entry/3
  summary: 已完成一批关于 ALLGATHER/共享专家归约、TP/DP/SP 配置契约和 W4A16MXFP + SiTU 精度路径的静态审查索引。当前记录称未发现新的 P0/P1，且 `git diff --check` 通过；但运行依赖不足，未完成 pytest 或 NPU runtime 验证，`is_sequence_parallel` 与全局 `enable_sp()` 的一致性仍应作为边界关注。
  title: 当前任务导航：MoE 高风险契约静态审查
navigation: 已完成一批关于 ALLGATHER/共享专家归约、TP/DP/SP 配置契约和 W4A16MXFP + SiTU 精度路径的静态审查索引。当前记录称未发现新的 P0/P1，且 `git diff --check` 通过；但运行依赖不足，未完成 pytest 或 NPU runtime 验证，`is_sequence_parallel` 与全局 `enable_sp()` 的一致性仍应作为边界关注。
schema: mindie-material-task/1
status: complete
task_id: 79fd3f2eb1e738fb0082e9094dae8b457f60e2d312d966244ea09e2373ef56ac
---

# 当前任务导航：MoE 高风险契约静态审查

已完成一批关于 ALLGATHER/共享专家归约、TP/DP/SP 配置契约和 W4A16MXFP + SiTU 精度路径的静态审查索引。当前记录称未发现新的 P0/P1，且 `git diff --check` 通过；但运行依赖不足，未完成 pytest 或 NPU runtime 验证，`is_sequence_parallel` 与全局 `enable_sp()` 的一致性仍应作为边界关注。

## Materials

- [MoE 归约与 W4A16MXFP/SiTU 静态审查](blocks/8a0e38ef05613c4fb6be4d681306ce4b85a9dd44fa91bc76d38a271bb162be72.md): 本批次围绕 ALLGATHER/共享专家归约、TP/DP/SP 契约，以及 W4A16MXFP + SiTU 的激活精度路径进行了只读静态核查；记录称相关代码位置和回归测试已补齐，`git diff --check` 通过。审查报告未发现新的 P0/P1，但明确未执行 pytest 或 NPU runtime 验证，且 `is_sequence_parallel` 与 Ascend 全局 `enable_sp()` 的一致性曾作为需要核实的边界。相关路径包含 `fused_moe.py`、`prepare_finalize.py`、`moe_mlp.py`，其中 W4A16 路径保持 BF16/FP16 的 `GMM1 → SiTU → GMM2`，并先于 W4A8 动态量化分支。

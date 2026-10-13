---
blocks:
- block_id: a1b44e99d134ac6b6cfc5345eb8a7e20a84c9eb96c05e07cfda2f4cd4e0c0464
  indexed: true
  sha256: f30f4858ae8ab6b5596d7c7cc58033d1f79f0b4adc947f13177c022ac2f83a49
  source_range:
    end: 557512
    part: 0
    start: 0
  summary: '只读审计记录了两组不重叠的提交范围：提交 A 为 PR #141 的 AscendC KDA 算子，提交 B 为 Kimi K3 的模型、KDA、MLA、MoE 与量化支持；当前 tree 与 PR #141 最新 head 一致，并发现 `__pycache__/*.pyc`、`.ruff_cache` 等生成物不应提交。审计称若干 A3 构建、KDA AscendC、Kimi K3 prefill、Triton decode/spec、MLA smoke 与 BF16 TP8 权重加载检查已通过，但 `MoeInitRoutingCustom` 曾因镜像/vendor custom-op 包不匹配首次失败，重装后直接 NPU 调用通过。五层 ModelSlim int4 真实权重结果尚待最终端到端重跑，A5 原生 MXFP4 全权重验证也被延期；FP8 MoE 因 A3 环境不支持未用于验证，且这些结论来自审计草案，不能视为已独立核实的案例报告。'
  title: PR141 与 Kimi K3 提交范围及验证审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5beed3fa79f361ad4939b0c6bc795e04e41feb6c4115e00e51a8bc2c1fa8481e
  kind: experience
  material_digest: fa930d4dca5e0d079d8bc57915e7ad72d71914050d565a5f254867e12242c729
  revision: 26fc56b316ddd78ba3557e8284e45ea003390cf00817fab7083cfaf4c8eeb4ca
  schema: mindie-entry/3
  summary: 当前材料覆盖 PR141 AscendC KDA 算子与 Kimi K3 生产支持的精确文件边界、PR 草案和测试状态。重点检索词包括 `chunk_kda_fwd`、`kda_gate_cumsum`、`MoeInitRoutingCustom`、A3/CANN 9.1、Triton decode/speculative decode、ModelSlim int4、MXFP4、FP8；后续需补充五层 ModelSlim 端到端结果、完成 A5 MXFP4 验证，并重建带 `Signed-off-by` 的提交。
  title: Kimi K3 Ascend 支持提交与验证索引
navigation: 当前材料覆盖 PR141 AscendC KDA 算子与 Kimi K3 生产支持的精确文件边界、PR 草案和测试状态。重点检索词包括 `chunk_kda_fwd`、`kda_gate_cumsum`、`MoeInitRoutingCustom`、A3/CANN 9.1、Triton decode/speculative decode、ModelSlim int4、MXFP4、FP8；后续需补充五层 ModelSlim 端到端结果、完成 A5 MXFP4 验证，并重建带 `Signed-off-by` 的提交。
schema: mindie-material-task/1
status: complete
task_id: 5beed3fa79f361ad4939b0c6bc795e04e41feb6c4115e00e51a8bc2c1fa8481e
---

# Kimi K3 Ascend 支持提交与验证索引

当前材料覆盖 PR141 AscendC KDA 算子与 Kimi K3 生产支持的精确文件边界、PR 草案和测试状态。重点检索词包括 `chunk_kda_fwd`、`kda_gate_cumsum`、`MoeInitRoutingCustom`、A3/CANN 9.1、Triton decode/speculative decode、ModelSlim int4、MXFP4、FP8；后续需补充五层 ModelSlim 端到端结果、完成 A5 MXFP4 验证，并重建带 `Signed-off-by` 的提交。

## Materials

- [PR141 与 Kimi K3 提交范围及验证审计](blocks/a1b44e99d134ac6b6cfc5345eb8a7e20a84c9eb96c05e07cfda2f4cd4e0c0464.md): 只读审计记录了两组不重叠的提交范围：提交 A 为 PR #141 的 AscendC KDA 算子，提交 B 为 Kimi K3 的模型、KDA、MLA、MoE 与量化支持；当前 tree 与 PR #141 最新 head 一致，并发现 `__pycache__/*.pyc`、`.ruff_cache` 等生成物不应提交。审计称若干 A3 构建、KDA AscendC、Kimi K3 prefill、Triton decode/spec、MLA smoke 与 BF16 TP8 权重加载检查已通过，但 `MoeInitRoutingCustom` 曾因镜像/vendor custom-op 包不匹配首次失败，重装后直接 NPU 调用通过。五层 ModelSlim int4 真实权重结果尚待最终端到端重跑，A5 原生 MXFP4 全权重验证也被延期；FP8 MoE 因 A3 环境不支持未用于验证，且这些结论来自审计草案，不能视为已独立核实的案例报告。

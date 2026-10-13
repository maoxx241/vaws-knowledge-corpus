---
blocks:
- block_id: 5579c39b5c30b4d8ac5dc775d3a9c9c64a0171aba4d26f2f140bc527c6bc365e
  indexed: true
  sha256: 9fe4f5421bcbe266e20b809401ffefdddf9344f0fd5a5183abd08ab608677ff3
  source_range:
    end: 142389
    part: 0
    start: 0
  summary: 用户授权对候选 `mindie-agent` 执行一次手动 `activate`，记录称退出码为 0；未设置 `CODEX_THREAD_ID`，也未执行 query、remote、deactivate 或重试，但该激活结果仅为来源中的报告。随后读取的 `CANDIDATE_ACTIVATED_NO_QUERY_DONE` 是 2026-09-18 的历史隔离 RMSNorm 记录：物理设备 8 映射为 `npu:0`，CPU float32 参考，FP16/BF16 阈值分别为 0.002/0.016，Task A、B 报告为零退出。记录同时注明证据不能外推至整模或生产规模，CPU 预算不含驱动/上下文开销，容器内无 `npu-smi`。
  title: MindIE Agent 候选激活与历史 RMSNorm 验收记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: cae91c6b4b0b93afb4bdac60b829bca52145c888be5204bc0c119abe2d9817cb
  kind: experience
  material_digest: 087e342ed9dcc88002bf4902946810ec7904ec0cae013c5e12a2dbe1beb9e4e8
  revision: ad34bd149d8cd90e39f3c3137d64d5f433db9b93c8118a2ae0bb5c4881b2d814
  schema: mindie-entry/3
  summary: 当前仅有一条 MindIE Agent 验收材料：候选激活及历史 RMSNorm 记录。可检索术语包括 `CANDIDATE_ACTIVATED_NO_QUERY_DONE`、`activate`、`npu:0`、RMSNorm、FP16/BF16 阈值、Task A/B、`npu-smi`；激活、零退出与历史测试结果均应视为来源报告，不能提升为独立验证成功。
  title: 当前任务导航
navigation: 当前仅有一条 MindIE Agent 验收材料：候选激活及历史 RMSNorm 记录。可检索术语包括 `CANDIDATE_ACTIVATED_NO_QUERY_DONE`、`activate`、`npu:0`、RMSNorm、FP16/BF16 阈值、Task A/B、`npu-smi`；激活、零退出与历史测试结果均应视为来源报告，不能提升为独立验证成功。
schema: mindie-material-task/1
status: complete
task_id: cae91c6b4b0b93afb4bdac60b829bca52145c888be5204bc0c119abe2d9817cb
---

# 当前任务导航

当前仅有一条 MindIE Agent 验收材料：候选激活及历史 RMSNorm 记录。可检索术语包括 `CANDIDATE_ACTIVATED_NO_QUERY_DONE`、`activate`、`npu:0`、RMSNorm、FP16/BF16 阈值、Task A/B、`npu-smi`；激活、零退出与历史测试结果均应视为来源报告，不能提升为独立验证成功。

## Materials

- [MindIE Agent 候选激活与历史 RMSNorm 验收记录](blocks/5579c39b5c30b4d8ac5dc775d3a9c9c64a0171aba4d26f2f140bc527c6bc365e.md): 用户授权对候选 `mindie-agent` 执行一次手动 `activate`，记录称退出码为 0；未设置 `CODEX_THREAD_ID`，也未执行 query、remote、deactivate 或重试，但该激活结果仅为来源中的报告。随后读取的 `CANDIDATE_ACTIVATED_NO_QUERY_DONE` 是 2026-09-18 的历史隔离 RMSNorm 记录：物理设备 8 映射为 `npu:0`，CPU float32 参考，FP16/BF16 阈值分别为 0.002/0.016，Task A、B 报告为零退出。记录同时注明证据不能外推至整模或生产规模，CPU 预算不含驱动/上下文开销，容器内无 `npu-smi`。

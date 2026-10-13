---
blocks:
- block_id: 3911fbdf4a41e2e46ef2b052d01b59487c1d8f4d3120ac2b79e33006dc957f28
  indexed: true
  sha256: e792a0b65cf8262961c3bf7d4547b77b75360661b553e74e7fc08c88f8879a61
  source_range:
    end: 1067329
    part: 0
    start: 0
  summary: 只读复审记录称，AllGather 与 latent routed transform 场景下无条件 all-reduce shared expert 可能导致启用 shared-expert DP 时重复按 TP 缩放；随后报告称该 MoE reduction contract、KDA metadata tuple contract 及相关清理已同步并复核。最终检查报告称无剩余 P0/P1、未改动 C++/头文件、`git diff --check` 通过且 50 个 Python 文件 AST 解析无错误，但 pytest 未运行，因为本机缺少 `torch`/`torch_npu`；这些结论属于报告的复审结果，并非独立验证。
  title: Python 审计与 MoE/KDA 只读复审
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 909fcb3feb9875d2310d85b90917423ee649e0825328e26836f5f16b563bb2a2
  kind: experience
  material_digest: 7bdfc859c9b4e625845f2011dc0f77b7ae0fe134b981e31a35377ee217ebe809
  revision: beef6fc24d82c5f8bc78938672f5f3174720af29fff12f584e93e26a33026090
  schema: mindie-entry/3
  summary: 本批材料聚焦 Python diff 的最终门禁、MoE reduction（SP、TP shared、shared-DP）与 KDA tuple contract。记录中包含一个先前发现的 shared-expert DP 回归风险及其后续“已修复/已同步”的报告；最终状态仍应保留为未运行 pytest，环境缺少 `torch`/`torch_npu`，且相关成功结论仅是复审报告。
  title: 当前任务导航：Python 重构只读复审
navigation: 本批材料聚焦 Python diff 的最终门禁、MoE reduction（SP、TP shared、shared-DP）与 KDA tuple contract。记录中包含一个先前发现的 shared-expert DP 回归风险及其后续“已修复/已同步”的报告；最终状态仍应保留为未运行 pytest，环境缺少 `torch`/`torch_npu`，且相关成功结论仅是复审报告。
schema: mindie-material-task/1
status: complete
task_id: 909fcb3feb9875d2310d85b90917423ee649e0825328e26836f5f16b563bb2a2
---

# 当前任务导航：Python 重构只读复审

本批材料聚焦 Python diff 的最终门禁、MoE reduction（SP、TP shared、shared-DP）与 KDA tuple contract。记录中包含一个先前发现的 shared-expert DP 回归风险及其后续“已修复/已同步”的报告；最终状态仍应保留为未运行 pytest，环境缺少 `torch`/`torch_npu`，且相关成功结论仅是复审报告。

## Materials

- [Python 审计与 MoE/KDA 只读复审](blocks/3911fbdf4a41e2e46ef2b052d01b59487c1d8f4d3120ac2b79e33006dc957f28.md): 只读复审记录称，AllGather 与 latent routed transform 场景下无条件 all-reduce shared expert 可能导致启用 shared-expert DP 时重复按 TP 缩放；随后报告称该 MoE reduction contract、KDA metadata tuple contract 及相关清理已同步并复核。最终检查报告称无剩余 P0/P1、未改动 C++/头文件、`git diff --check` 通过且 50 个 Python 文件 AST 解析无错误，但 pytest 未运行，因为本机缺少 `torch`/`torch_npu`；这些结论属于报告的复审结果，并非独立验证。

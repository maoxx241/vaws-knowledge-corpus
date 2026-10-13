---
blocks:
- block_id: 16a8a72205adc295a6a3b04322edc57b4c2e7fdbb19d84a4408afd2edb0e4c37
  indexed: true
  sha256: a766d1057647c5b10c082271e7d0e98e38274f02d71875c7757410c69ce6042e
  source_range:
    end: 1223940
    part: 0
    start: 0
  summary: 只读审计将最强候选指向 shared-expert side stream 新增的 TP16 all-gather 与默认流 EP64/MC2 collective 在重叠 ranks 上竞跑；r6 中 64/64 worker 的 Python 主线程均停在该 all-gather 调用，但这可能只是 HCCL 队列反压后的首个 host 阻塞点，不能视为设备首停点，且 Event-query watchdog 会扰动结果。async-off 仅证明异步 batch timing 是必要条件，未单独证明 output copy 是根因；建议 r8 保持 async 开启、仅关闭 multistream overlap，并固定冷启动 1+7 batch、保存 scheduler/DP/collective 证据。后续修复候选是在默认流预先执行 shared TP all-gather，再让 side stream 只做 shared compute，最后 join 后执行 TP reduce-scatter；该方案仍需 host-only 调用序列 UT 和相同条件的正反复现实验，不能把计划或候选修复表述为已验证成功。
  title: shared-expert 多流 TP all-gather 卡死审计与 r8 验证方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d0149808afa0c1d8279f2d3c96c9f28f6bc035151aa9956451daf6f5b6f3e7bc
  kind: experience
  material_digest: 01b5fcf70ee5635a24d092b9c6da1e2a02dcb4578590c27dec446a0951fb25d0
  revision: d6ec1f6cb6936fc61502337e43b5f3c0f7af1de66a2644aa1f5c2d81df256109
  schema: mindie-entry/3
  summary: 当前材料集中于 TP16 shared-expert all-gather、EP64/MC2 重叠 communicator、async scheduling 与 r6/r8 复现判定。已知证据排除了简单的 TP rank Python 分叉和 collective shape 不匹配，但 host 停点、跨流竞跑因果及 pre-gather 修复仍未完成验证；原始材料保持独立可检索，后续应优先核对 r8 条件一致性、scheduler/route/source hash 与三腿结果。
  title: 任务导航：shared-expert 多流 collective 卡死审计
navigation: 当前材料集中于 TP16 shared-expert all-gather、EP64/MC2 重叠 communicator、async scheduling 与 r6/r8 复现判定。已知证据排除了简单的 TP rank Python 分叉和 collective shape 不匹配，但 host 停点、跨流竞跑因果及 pre-gather 修复仍未完成验证；原始材料保持独立可检索，后续应优先核对 r8 条件一致性、scheduler/route/source hash 与三腿结果。
schema: mindie-material-task/1
status: complete
task_id: d0149808afa0c1d8279f2d3c96c9f28f6bc035151aa9956451daf6f5b6f3e7bc
---

# 任务导航：shared-expert 多流 collective 卡死审计

当前材料集中于 TP16 shared-expert all-gather、EP64/MC2 重叠 communicator、async scheduling 与 r6/r8 复现判定。已知证据排除了简单的 TP rank Python 分叉和 collective shape 不匹配，但 host 停点、跨流竞跑因果及 pre-gather 修复仍未完成验证；原始材料保持独立可检索，后续应优先核对 r8 条件一致性、scheduler/route/source hash 与三腿结果。

## Materials

- [shared-expert 多流 TP all-gather 卡死审计与 r8 验证方案](blocks/16a8a72205adc295a6a3b04322edc57b4c2e7fdbb19d84a4408afd2edb0e4c37.md): 只读审计将最强候选指向 shared-expert side stream 新增的 TP16 all-gather 与默认流 EP64/MC2 collective 在重叠 ranks 上竞跑；r6 中 64/64 worker 的 Python 主线程均停在该 all-gather 调用，但这可能只是 HCCL 队列反压后的首个 host 阻塞点，不能视为设备首停点，且 Event-query watchdog 会扰动结果。async-off 仅证明异步 batch timing 是必要条件，未单独证明 output copy 是根因；建议 r8 保持 async 开启、仅关闭 multistream overlap，并固定冷启动 1+7 batch、保存 scheduler/DP/collective 证据。后续修复候选是在默认流预先执行 shared TP all-gather，再让 side stream 只做 shared compute，最后 join 后执行 TP reduce-scatter；该方案仍需 host-only 调用序列 UT 和相同条件的正反复现实验，不能把计划或候选修复表述为已验证成功。

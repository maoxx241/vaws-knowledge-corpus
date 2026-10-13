---
blocks:
- block_id: 7728e3970bb430ae0b43782ba4b256903187209afcdbd2e5800e09f2d6c596fc
  indexed: true
  sha256: bff771a370a23d27b7004ee000871caaaa7836f5b8f6ab5e04e99e1c6e388656
  source_range:
    end: 3040374
    part: 0
    start: 0
  summary: 只读审计认为 r7 的 7/7/7/8 请求均已进入 scheduler，已撤回 exact-route 投递不齐、DP dummy-step 和 async-off output-copy 等解释；当前应聚焦首个 target execute 内的 DP metadata rendezvous、target forward、shared-expert/EP64 collective 及实际 FULL graph 状态。源码调用链显示 DSpark eager 路径主要使用调用方 current stream，没有发现显式 hidden stream 或 target overtaking draft 的 Python 路径，但仍保留 CANN operator 违反 stream 契约、slot/address 语义覆盖和 Mamba block-table 更新竞争等未证实可能性。建议通过四 rank marker、cudagraph_mode 日志、精确 barrier A/B，以及 mock stream/event 单测区分 collective 卡点；所有修复 seam 和测试均为建议或反证手段，不能视为已验证成功。
  title: r7 short-H0：首个 execute_model 卡点与 stream 契约审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f8a92603c9b8558afce721080f236a38d7629bf1168e440e666b154b78c01b31
  kind: experience
  material_digest: 1480def43fbffaf4029f33b9fae63addabb393cdf61e969b15ec09d042266d13
  revision: 14ea9a43b36fe14d9a705c5f19221acbef28cf787c10d0467138ae449b1de0cd
  schema: mindie-entry/3
  summary: 核心结论：已排除若干此前假设，但尚未定位根因。优先检查看首个 target execute 的 DP metadata all-reduce、首个 TP/EP collective、实际 cudagraph_mode 与共享专家 stream；保留未证实的 CANN hidden-stream、cache slot/address 语义覆盖及 Mamba block-table 竞争假设。
  title: 当前任务导航：r7 首个 execute_model 卡点审计
navigation: 核心结论：已排除若干此前假设，但尚未定位根因。优先检查看首个 target execute 的 DP metadata all-reduce、首个 TP/EP collective、实际 cudagraph_mode 与共享专家 stream；保留未证实的 CANN hidden-stream、cache slot/address 语义覆盖及 Mamba block-table 竞争假设。
schema: mindie-material-task/1
status: complete
task_id: f8a92603c9b8558afce721080f236a38d7629bf1168e440e666b154b78c01b31
---

# 当前任务导航：r7 首个 execute_model 卡点审计

核心结论：已排除若干此前假设，但尚未定位根因。优先检查看首个 target execute 的 DP metadata all-reduce、首个 TP/EP collective、实际 cudagraph_mode 与共享专家 stream；保留未证实的 CANN hidden-stream、cache slot/address 语义覆盖及 Mamba block-table 竞争假设。

## Materials

- [r7 short-H0：首个 execute_model 卡点与 stream 契约审计](blocks/7728e3970bb430ae0b43782ba4b256903187209afcdbd2e5800e09f2d6c596fc.md): 只读审计认为 r7 的 7/7/7/8 请求均已进入 scheduler，已撤回 exact-route 投递不齐、DP dummy-step 和 async-off output-copy 等解释；当前应聚焦首个 target execute 内的 DP metadata rendezvous、target forward、shared-expert/EP64 collective 及实际 FULL graph 状态。源码调用链显示 DSpark eager 路径主要使用调用方 current stream，没有发现显式 hidden stream 或 target overtaking draft 的 Python 路径，但仍保留 CANN operator 违反 stream 契约、slot/address 语义覆盖和 Mamba block-table 更新竞争等未证实可能性。建议通过四 rank marker、cudagraph_mode 日志、精确 barrier A/B，以及 mock stream/event 单测区分 collective 卡点；所有修复 seam 和测试均为建议或反证手段，不能视为已验证成功。

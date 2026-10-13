---
blocks:
- block_id: 964f85d3b234f96d166a561582a8cd3932f5bd0e3c3d6aa1b320af2267e5503d
  indexed: true
  sha256: bec10f865d850522eda353c0df887276dc3dce4ced5287d6b17914029cb3f552
  source_range:
    end: 2538359
    part: 0
    start: 0
  summary: 新 head `93436a8f4` 曾因 `pending/jobs=0` 长时间未调度，随后 pre-commit 在 `tests/ut/models/test_kimi_k3.py:180` 的 mypy `[var-annotated]` 错误处失败，CPU 与硬件矩阵被跳过；之后静态 gate 恢复并通过，旧 head 的矩阵也全部测试成功。新 head `38ad149f` 的 pre-commit、CPU、310P、A2/A3 及 upstream 多数 lane 通过，但 v0.25.1 A2 part 3 出现 Eagle3 full-graph 初始化设备内存不足（剩余 `4.76/60.96 GiB`，需求 `56.08 GiB`），被判断为 runner/NPU 残留内存问题；该判断仍是环境归因而非代码根因验证。`ci-gate` 只是汇总失败，main-verified A3-2 part 2 被取消但日志显示 selected targets 通过；监控过程中未完成所有后续状态确认。
  title: CI 监控：从排队阻塞到硬件矩阵单点失败
- block_id: a9f3ac04b4a8bd771b46bd0e6a8dd745f8182f9010f9aa007cbe8a7dba5745b8
  indexed: true
  sha256: 6961ef131741f2214ff9cd79383b138052b0448c659f28d930607f349c46f82b
  source_range:
    end: 2538359
    part: 1
    start: 0
  summary: 在前一轮 E2E 结束后，新 head 的 PR Create 已完成，当前唯一未终态的 E2E run 为 `30558943052`，状态仍是 `pending/jobs=0`。因此尚未产生可诊断的 job、日志或测试结果，不能将排队状态解释为失败，也不能确认后续 gate 是否会启动。记录中明确计划继续识别该 SHA 的 workflow runs，但本块未包含后续收口结果。
  title: 新 head E2E 再次排队，尚无结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b98d11937f0d4ec139bd932f6a5f8347d02b6e5f4347f597efdb351242d35d0b
  kind: experience
  material_digest: b6fae2fb17515aff5074a63ef17f8fec56b6110495c9d3863cfd62a3df2089c2
  revision: 66a44024ce55491a227c853c37e3bff18fb3afd2c7098905b1a0cee8a644428d
  schema: mindie-entry/3
  summary: 已有记录覆盖多次 head 的 CI 调度、pre-commit/mypy 错误、CPU 与硬件矩阵结果，以及一次疑似 runner/NPU 残留内存导致的 A2 part 3 失败；此前部分“全绿”结论仅适用于对应 head 或已完成 lane，不能推广到所有后续 run。最新状态是新 head 的 E2E run `30558943052` 仍处于 `pending/jobs=0`，尚无 job 日志或终态，因此后续检索应优先确认该 run 是否展开、首个真实失败及最终 terminal 结果，同时保留排队、取消、汇总 gate 与环境资源不足之间的区别。
  title: 当前任务导航：持续索引 CI 监控与未完成的新 head 验证
navigation: 已有记录覆盖多次 head 的 CI 调度、pre-commit/mypy 错误、CPU 与硬件矩阵结果，以及一次疑似 runner/NPU 残留内存导致的 A2 part 3 失败；此前部分“全绿”结论仅适用于对应 head 或已完成 lane，不能推广到所有后续 run。最新状态是新 head 的 E2E run `30558943052` 仍处于 `pending/jobs=0`，尚无 job 日志或终态，因此后续检索应优先确认该 run 是否展开、首个真实失败及最终 terminal 结果，同时保留排队、取消、汇总 gate 与环境资源不足之间的区别。
schema: mindie-material-task/1
status: complete
task_id: b98d11937f0d4ec139bd932f6a5f8347d02b6e5f4347f597efdb351242d35d0b
---

# 当前任务导航：持续索引 CI 监控与未完成的新 head 验证

已有记录覆盖多次 head 的 CI 调度、pre-commit/mypy 错误、CPU 与硬件矩阵结果，以及一次疑似 runner/NPU 残留内存导致的 A2 part 3 失败；此前部分“全绿”结论仅适用于对应 head 或已完成 lane，不能推广到所有后续 run。最新状态是新 head 的 E2E run `30558943052` 仍处于 `pending/jobs=0`，尚无 job 日志或终态，因此后续检索应优先确认该 run 是否展开、首个真实失败及最终 terminal 结果，同时保留排队、取消、汇总 gate 与环境资源不足之间的区别。

## Materials

- [CI 监控：从排队阻塞到硬件矩阵单点失败](blocks/964f85d3b234f96d166a561582a8cd3932f5bd0e3c3d6aa1b320af2267e5503d.md): 新 head `93436a8f4` 曾因 `pending/jobs=0` 长时间未调度，随后 pre-commit 在 `tests/ut/models/test_kimi_k3.py:180` 的 mypy `[var-annotated]` 错误处失败，CPU 与硬件矩阵被跳过；之后静态 gate 恢复并通过，旧 head 的矩阵也全部测试成功。新 head `38ad149f` 的 pre-commit、CPU、310P、A2/A3 及 upstream 多数 lane 通过，但 v0.25.1 A2 part 3 出现 Eagle3 full-graph 初始化设备内存不足（剩余 `4.76/60.96 GiB`，需求 `56.08 GiB`），被判断为 runner/NPU 残留内存问题；该判断仍是环境归因而非代码根因验证。`ci-gate` 只是汇总失败，main-verified A3-2 part 2 被取消但日志显示 selected targets 通过；监控过程中未完成所有后续状态确认。
- [新 head E2E 再次排队，尚无结论](blocks/a9f3ac04b4a8bd771b46bd0e6a8dd745f8182f9010f9aa007cbe8a7dba5745b8.md): 在前一轮 E2E 结束后，新 head 的 PR Create 已完成，当前唯一未终态的 E2E run 为 `30558943052`，状态仍是 `pending/jobs=0`。因此尚未产生可诊断的 job、日志或测试结果，不能将排队状态解释为失败，也不能确认后续 gate 是否会启动。记录中明确计划继续识别该 SHA 的 workflow runs，但本块未包含后续收口结果。

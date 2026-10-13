---
blocks:
- block_id: 8c60482d1c6eaf682db06ebacf9722823147d090a1d30fbbf2cbd717036759a0
  indexed: true
  sha256: 326615d78fb62902d2d4f8db840e8929b95aefa5291052c90140601dcab7918a
  source_range:
    end: 546346
    part: 0
    start: 0
  summary: '此前报告称 PR #12950 已更新并完成多项验证，但随后针对 E2E 的只读诊断显示 v0.26 A2 part 4 的根失败为 `TestDCPGraphParams::test_update_mla_dcp_params`，结果为 1 failed、25 passed。原因报告为 MLA split 新增必填 `seq_lens_device` 后，DCP ACLGraph UT 仍用旧位置参数构造元数据，触发缺少 `seq_lens_list` 的初始化错误；生产 builder 使用关键字参数，未被确认受影响。`upload-coverage-to-obs` 被判断为空取消，`ci-gate` 是派生失败；建议修正测试构造，但该材料明确记录全程未编辑、重跑、评论或 push，因此 CI 尚未被修复或验证通过。'
  title: 'PR #12950 验证与 CI 根因诊断'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2923f3ed6f5416e007d6a04c5635be5b24d95fe6ec756bab9808abf45b3d9535
  kind: experience
  material_digest: de80311f89159935b7ca9008c5658e5d0dfc1e48e545946b3fa660c1d9dcb99f
  revision: 2ce327b7785407963ce4654ff819b108c3682ca947ebd1a24f8ee2ffe9fd621c
  schema: mindie-entry/3
  summary: 索引涵盖此前的 PR 验证结果及后续 CI 诊断。此前报告的测试、服务健康和 E2E 排队状态属于当时记录；后续只读检查定位到唯一根失败是 DCP ACLGraph UT 的 MLA 元数据旧位置参数问题，`upload-coverage-to-obs` 与 `ci-gate` 为下游/派生状态。修复建议是使用关键字参数并补充 `seq_lens_device=seq_lens`，但材料未记录已实施或重新验证。
  title: '当前任务：PR #12950 的 CI 失败诊断'
navigation: 索引涵盖此前的 PR 验证结果及后续 CI 诊断。此前报告的测试、服务健康和 E2E 排队状态属于当时记录；后续只读检查定位到唯一根失败是 DCP ACLGraph UT 的 MLA 元数据旧位置参数问题，`upload-coverage-to-obs` 与 `ci-gate` 为下游/派生状态。修复建议是使用关键字参数并补充 `seq_lens_device=seq_lens`，但材料未记录已实施或重新验证。
schema: mindie-material-task/1
status: complete
task_id: 2923f3ed6f5416e007d6a04c5635be5b24d95fe6ec756bab9808abf45b3d9535
---

# 当前任务：PR #12950 的 CI 失败诊断

索引涵盖此前的 PR 验证结果及后续 CI 诊断。此前报告的测试、服务健康和 E2E 排队状态属于当时记录；后续只读检查定位到唯一根失败是 DCP ACLGraph UT 的 MLA 元数据旧位置参数问题，`upload-coverage-to-obs` 与 `ci-gate` 为下游/派生状态。修复建议是使用关键字参数并补充 `seq_lens_device=seq_lens`，但材料未记录已实施或重新验证。

## Materials

- [PR #12950 验证与 CI 根因诊断](blocks/8c60482d1c6eaf682db06ebacf9722823147d090a1d30fbbf2cbd717036759a0.md): 此前报告称 PR #12950 已更新并完成多项验证，但随后针对 E2E 的只读诊断显示 v0.26 A2 part 4 的根失败为 `TestDCPGraphParams::test_update_mla_dcp_params`，结果为 1 failed、25 passed。原因报告为 MLA split 新增必填 `seq_lens_device` 后，DCP ACLGraph UT 仍用旧位置参数构造元数据，触发缺少 `seq_lens_list` 的初始化错误；生产 builder 使用关键字参数，未被确认受影响。`upload-coverage-to-obs` 被判断为空取消，`ci-gate` 是派生失败；建议修正测试构造，但该材料明确记录全程未编辑、重跑、评论或 push，因此 CI 尚未被修复或验证通过。

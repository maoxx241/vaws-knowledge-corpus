---
blocks:
- block_id: 1afec0a7d74107398faeda8266787309a671c232bc7f931bfb53b25946c98a84
  indexed: true
  sha256: 111fef6f361a2b2c27ade6036e403152c5e71d928f8fad7ae676ffa0c6897bda
  source_range:
    end: 5161150
    part: 0
    start: 0
  summary: '审查发现两项 P1：真实 CANN `trace_view.json` 多为顶层数组，原解析器仅支持根对象，导致 `host_events_retained=0` 却仍报告 `status=ok`；另有 `serve_stop` 通过 `pkill` 可能误杀同 session 进程并吞掉失败，但用户明确要求暂不处理后者。随后修复了顶层数组与 Chrome object wrapper 兼容性，并在无 host event 时改为 `missing/partial + limitation`，新增回归测试；定向测试 32/32、完整测试 269/269 通过，修复以 commit `0d7cc6b` 推送至 PR #70，但新一轮 CI 尚未显示。'
  title: 'PR #70：修复 host trace 解析回归'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8a64016dc186fa08cf063702c7a5f5f66944adb8678faebf5382e70d91603784
  kind: experience
  material_digest: 6e93db11942c5d737d228f22e879dca02f52762b7c8252a384034ab763c722a1
  revision: 49b4882dff1991f2d84f1c95158dd22dac69b933a68b640e6142d5724a2a910f
  schema: mindie-entry/3
  summary: '当前材料记录了 PR #70 在 commit `3010484f` 的审查、真实输入格式回归、用户限定的修复范围及后续 commit `0d7cc6b`。可检索关键术语：CANN `trace_view.json`、顶层数组、host event、`summary_manifest.json`、`serve_stop`、`pkill`、`missing/partial`、269 passed；需保留“服务回收问题未处理”及“新一轮 CI 尚未显示”的未完成状态。'
  title: 'PR #70 审查与 host trace 修复'
navigation: '当前材料记录了 PR #70 在 commit `3010484f` 的审查、真实输入格式回归、用户限定的修复范围及后续 commit `0d7cc6b`。可检索关键术语：CANN `trace_view.json`、顶层数组、host event、`summary_manifest.json`、`serve_stop`、`pkill`、`missing/partial`、269 passed；需保留“服务回收问题未处理”及“新一轮 CI 尚未显示”的未完成状态。'
schema: mindie-material-task/1
status: complete
task_id: 8a64016dc186fa08cf063702c7a5f5f66944adb8678faebf5382e70d91603784
---

# PR #70 审查与 host trace 修复

当前材料记录了 PR #70 在 commit `3010484f` 的审查、真实输入格式回归、用户限定的修复范围及后续 commit `0d7cc6b`。可检索关键术语：CANN `trace_view.json`、顶层数组、host event、`summary_manifest.json`、`serve_stop`、`pkill`、`missing/partial`、269 passed；需保留“服务回收问题未处理”及“新一轮 CI 尚未显示”的未完成状态。

## Materials

- [PR #70：修复 host trace 解析回归](blocks/1afec0a7d74107398faeda8266787309a671c232bc7f931bfb53b25946c98a84.md): 审查发现两项 P1：真实 CANN `trace_view.json` 多为顶层数组，原解析器仅支持根对象，导致 `host_events_retained=0` 却仍报告 `status=ok`；另有 `serve_stop` 通过 `pkill` 可能误杀同 session 进程并吞掉失败，但用户明确要求暂不处理后者。随后修复了顶层数组与 Chrome object wrapper 兼容性，并在无 host event 时改为 `missing/partial + limitation`，新增回归测试；定向测试 32/32、完整测试 269/269 通过，修复以 commit `0d7cc6b` 推送至 PR #70，但新一轮 CI 尚未显示。

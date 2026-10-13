---
blocks:
- block_id: bffb45805818ed02b21ad69ab1529a1611389d6bc17366d22a344e99f724ddd2
  indexed: true
  sha256: 5b7a341a72bb6cd0b604e3cacea2441e599c6ffd209d45c1367cc14d5101b47c
  source_range:
    end: 548903
    part: 0
    start: 0
  summary: 审计对照了 r2 与 r8/r9/r11，关注源码同步范围、四机 PGID/日志隔离及短请求门禁脚本的误清理风险，未修改文件或启动服务。确认 r2 及历史 r8/r9/r11 均未显式传入 `--no-async-scheduling`；该版本 vLLM 在未指定时会自动启用，且 DSpark 被列为兼容类型。后续若复用 r2，应在实际 `serve` 参数中显式关闭异步调度，以便区分 padding 修复与异步调度的影响；这只是风险审计结论，不代表修复或验证已成功。
  title: vLLM 异步调度参数审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 10e48bce30708e2ccd810b2b319374983c63631066eeec2c733549428c8c4334
  kind: experience
  material_digest: b6b31fac9581e964cec8be603acb51570360e282d081860beba5dc086633166e
  revision: 5df04de1f75c0550f17e6e85435d9d6a9d94d265f890eed020151f4686675cb9
  schema: mindie-entry/3
  summary: 当前材料记录了对 r2、r8/r9/r11 的只读审计：重点是显式关闭 `--no-async-scheduling`、源码同步、四机 PGID/日志隔离，以及短请求门禁脚本可能误清理其他进程。已确认相关历史配置缺少该参数，并报告 vLLM 默认启用异步调度；尚未进行文件修改、服务启动或修复验证，后续结果仍需独立检索原始块。
  title: 任务导航：vLLM 调度参数与运行隔离审计
navigation: 当前材料记录了对 r2、r8/r9/r11 的只读审计：重点是显式关闭 `--no-async-scheduling`、源码同步、四机 PGID/日志隔离，以及短请求门禁脚本可能误清理其他进程。已确认相关历史配置缺少该参数，并报告 vLLM 默认启用异步调度；尚未进行文件修改、服务启动或修复验证，后续结果仍需独立检索原始块。
schema: mindie-material-task/1
status: complete
task_id: 10e48bce30708e2ccd810b2b319374983c63631066eeec2c733549428c8c4334
---

# 任务导航：vLLM 调度参数与运行隔离审计

当前材料记录了对 r2、r8/r9/r11 的只读审计：重点是显式关闭 `--no-async-scheduling`、源码同步、四机 PGID/日志隔离，以及短请求门禁脚本可能误清理其他进程。已确认相关历史配置缺少该参数，并报告 vLLM 默认启用异步调度；尚未进行文件修改、服务启动或修复验证，后续结果仍需独立检索原始块。

## Materials

- [vLLM 异步调度参数审计](blocks/bffb45805818ed02b21ad69ab1529a1611389d6bc17366d22a344e99f724ddd2.md): 审计对照了 r2 与 r8/r9/r11，关注源码同步范围、四机 PGID/日志隔离及短请求门禁脚本的误清理风险，未修改文件或启动服务。确认 r2 及历史 r8/r9/r11 均未显式传入 `--no-async-scheduling`；该版本 vLLM 在未指定时会自动启用，且 DSpark 被列为兼容类型。后续若复用 r2，应在实际 `serve` 参数中显式关闭异步调度，以便区分 padding 修复与异步调度的影响；这只是风险审计结论，不代表修复或验证已成功。

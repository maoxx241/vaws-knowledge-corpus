---
blocks:
- block_id: 50cb28de4ecd4d283e220f48673769c02173f91ad3384aa95c3ba1ca9210b760
  indexed: true
  sha256: 2621c2891ccd4790b180952fd640483cd6d092425959e0943b8f0fb15dc60a27
  source_range:
    end: 13654774
    part: 0
    start: 0
  summary: 该块记录知识 runtime 的一系列审查与修复：处理数据库/回执表缺失、首轮采集边界、后台错误可见性、升级竞态、快照切换、按文件发布及外部写入后的记账失败，并保留失败语义与原始资料。随后修复了 Windows CRLF marker、SQLite 文件锁、坏父目录、不可访问祖先和失效软链判定，以及最终观测中 stage_timings 丢失；多个提交分别完成了针对性测试，但部分 Windows CI、远端发布和 push 后验证仍明确未完成。安装包与指定 content head 的本地隔离验证已通过，但这些结果不等同于远端发布完成；NPU diagnostics pin 的只读复核也未执行完整安装、HTTP smoke 或其他 pin 检查。
  title: 运行时权威状态、发布与跨平台修复记录
- block_id: d1a340c2e31d638f6d723d732b0678e2c03327acc3a1b32051eb72fb90559dba
  indexed: true
  sha256: 69491464f0bf0ff4fbe59bb14c58e928d0226cd330cd5c24db46ad567e1394ce
  source_range:
    end: 13654774
    part: 1
    start: 0
  summary: 最终 `4dbe838` 的 Package、macOS 和 Ubuntu CI 均报告 868 passed、9 skipped；Windows job 被平台 30 分钟上限取消，已记录 617 passed、27 skipped、0 failures/0 errors，但仍有 233 项未完成。原始 XML 显示 authority、failure visibility、DFX 和 activation authority 的相关直接回归分别通过，覆盖核对记录了 16 项直接回归全通过；这不能替代整套 Windows 验收。针对 Windows-only 60 分钟预算没有原则性反对，粗估约需 40.2 分钟，但测试负载不均，预算仍不是通过保证；未修改 workflow、代码、仓库，也未重跑。
  title: 最终 CI 结果与 Windows 超时未决项
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9958fc6a4b57f90dd587dc07518808ee1895e63fc4ea5e48ef1a26c44d27ba2c
  kind: experience
  material_digest: 53be010d2ffae48bb90fd4e0a8057598ac6b1e523dae2792d2027ba7785b0746
  revision: 84c77971e30288af1aef6ae75b0db0603b5869bb368d93348a9fee4e9c662e6a
  schema: mindie-entry/3
  summary: 检索重点按两条线组织：首先查找权威状态、错误可见性、发布/快照一致性、升级竞态、文件流式处理、Windows 路径与 marker 修复，以及安装包和 content 的本地验证；其次查找最终 CI 的平台结果和测试计数。当前已知 macOS/Ubuntu/Package 通过，Windows 仅有部分进度和针对性回归通过，因平台超时取消而不能宣称整体成功；60 分钟 Windows-only 预算只是待验证方案，远端发布、push 后回读和完整 Windows 验收仍是未完成项。
  title: 当前任务导航：运行时修复已本地验证，Windows 全量 CI 仍未完成
navigation: 检索重点按两条线组织：首先查找权威状态、错误可见性、发布/快照一致性、升级竞态、文件流式处理、Windows 路径与 marker 修复，以及安装包和 content 的本地验证；其次查找最终 CI 的平台结果和测试计数。当前已知 macOS/Ubuntu/Package 通过，Windows 仅有部分进度和针对性回归通过，因平台超时取消而不能宣称整体成功；60 分钟 Windows-only 预算只是待验证方案，远端发布、push 后回读和完整 Windows 验收仍是未完成项。
schema: mindie-material-task/1
status: complete
task_id: 9958fc6a4b57f90dd587dc07518808ee1895e63fc4ea5e48ef1a26c44d27ba2c
---

# 当前任务导航：运行时修复已本地验证，Windows 全量 CI 仍未完成

检索重点按两条线组织：首先查找权威状态、错误可见性、发布/快照一致性、升级竞态、文件流式处理、Windows 路径与 marker 修复，以及安装包和 content 的本地验证；其次查找最终 CI 的平台结果和测试计数。当前已知 macOS/Ubuntu/Package 通过，Windows 仅有部分进度和针对性回归通过，因平台超时取消而不能宣称整体成功；60 分钟 Windows-only 预算只是待验证方案，远端发布、push 后回读和完整 Windows 验收仍是未完成项。

## Materials

- [运行时权威状态、发布与跨平台修复记录](blocks/50cb28de4ecd4d283e220f48673769c02173f91ad3384aa95c3ba1ca9210b760.md): 该块记录知识 runtime 的一系列审查与修复：处理数据库/回执表缺失、首轮采集边界、后台错误可见性、升级竞态、快照切换、按文件发布及外部写入后的记账失败，并保留失败语义与原始资料。随后修复了 Windows CRLF marker、SQLite 文件锁、坏父目录、不可访问祖先和失效软链判定，以及最终观测中 stage_timings 丢失；多个提交分别完成了针对性测试，但部分 Windows CI、远端发布和 push 后验证仍明确未完成。安装包与指定 content head 的本地隔离验证已通过，但这些结果不等同于远端发布完成；NPU diagnostics pin 的只读复核也未执行完整安装、HTTP smoke 或其他 pin 检查。
- [最终 CI 结果与 Windows 超时未决项](blocks/d1a340c2e31d638f6d723d732b0678e2c03327acc3a1b32051eb72fb90559dba.md): 最终 `4dbe838` 的 Package、macOS 和 Ubuntu CI 均报告 868 passed、9 skipped；Windows job 被平台 30 分钟上限取消，已记录 617 passed、27 skipped、0 failures/0 errors，但仍有 233 项未完成。原始 XML 显示 authority、failure visibility、DFX 和 activation authority 的相关直接回归分别通过，覆盖核对记录了 16 项直接回归全通过；这不能替代整套 Windows 验收。针对 Windows-only 60 分钟预算没有原则性反对，粗估约需 40.2 分钟，但测试负载不均，预算仍不是通过保证；未修改 workflow、代码、仓库，也未重跑。

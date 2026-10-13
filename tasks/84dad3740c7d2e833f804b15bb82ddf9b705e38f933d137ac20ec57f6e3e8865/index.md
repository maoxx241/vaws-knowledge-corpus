---
blocks:
- block_id: 9e0c9c10fd40c92b80b6923debdf3f41a83be4ad8a109e70e814b6f343359df4
  indexed: true
  sha256: 46072da5d6706bea65f91e85f2537c014d0edc7de9888f4ab44e07e8ae8de71f
  source_range:
    end: 1995032
    part: 0
    start: 0
  summary: 记录显示 CodexBar 曾出现“进程存活但 UI 失联”、启动前被系统暂停，以及 macOS LaunchServices/CoreServices 代理未放行的问题；相关判断包括系统级故障、自动恢复任务和重启代理，但这些结论来自现场排查叙述，不能视为独立验证的案例报告。后续记录称已从 0.56.2 更新至 0.56.3，并通过官方 SHA-256、开发者签名和 Apple 公证检查；同时发现 `cost-usage-scan`/“Usage & Spend → Track costs”导致单核高 CPU，遂关闭可选索引并保留数据与旧版回退副本。最终叙述报告关闭—重开、CPU 和 OAuth 刷新恢复正常，但此前已有“更新成功后仍短时占满一个核心”的不完整结果，且 Homebrew 元数据落后、可能降级的风险仍需注意。
  title: CodexBar 启动、更新与高 CPU 排查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 84dad3740c7d2e833f804b15bb82ddf9b705e38f933d137ac20ec57f6e3e8865
  kind: experience
  material_digest: 49535cf40fa4be67abb9e796886ee9d954a371783a63aaf56f13f8a0b7fe539d
  revision: 34ae5a89fd62bf916f917b8a49832ade0e5d7c335df87cfe2f8af4fd8d4bb554
  schema: mindie-entry/3
  summary: 可检索主题：LaunchServices 启动前暂停、CoreServices 代理、菜单栏 UI 失联、“cannot open anymore”、0.56.2→0.56.3 官方包校验、Sparkle 更新失败、`cost-usage-scan` 高 CPU、Usage & Spend 成本索引、OAuth `exact` 刷新、自动恢复任务及 Homebrew 降级风险。记录包含多次现场判断、修复尝试和后续修正；不要将报告的恢复状态或根因直接视为已独立验证的成功。
  title: CodexBar 故障与更新索引
navigation: 可检索主题：LaunchServices 启动前暂停、CoreServices 代理、菜单栏 UI 失联、“cannot open anymore”、0.56.2→0.56.3 官方包校验、Sparkle 更新失败、`cost-usage-scan` 高 CPU、Usage & Spend 成本索引、OAuth `exact` 刷新、自动恢复任务及 Homebrew 降级风险。记录包含多次现场判断、修复尝试和后续修正；不要将报告的恢复状态或根因直接视为已独立验证的成功。
schema: mindie-material-task/1
status: complete
task_id: 84dad3740c7d2e833f804b15bb82ddf9b705e38f933d137ac20ec57f6e3e8865
---

# CodexBar 故障与更新索引

可检索主题：LaunchServices 启动前暂停、CoreServices 代理、菜单栏 UI 失联、“cannot open anymore”、0.56.2→0.56.3 官方包校验、Sparkle 更新失败、`cost-usage-scan` 高 CPU、Usage & Spend 成本索引、OAuth `exact` 刷新、自动恢复任务及 Homebrew 降级风险。记录包含多次现场判断、修复尝试和后续修正；不要将报告的恢复状态或根因直接视为已独立验证的成功。

## Materials

- [CodexBar 启动、更新与高 CPU 排查记录](blocks/9e0c9c10fd40c92b80b6923debdf3f41a83be4ad8a109e70e814b6f343359df4.md): 记录显示 CodexBar 曾出现“进程存活但 UI 失联”、启动前被系统暂停，以及 macOS LaunchServices/CoreServices 代理未放行的问题；相关判断包括系统级故障、自动恢复任务和重启代理，但这些结论来自现场排查叙述，不能视为独立验证的案例报告。后续记录称已从 0.56.2 更新至 0.56.3，并通过官方 SHA-256、开发者签名和 Apple 公证检查；同时发现 `cost-usage-scan`/“Usage & Spend → Track costs”导致单核高 CPU，遂关闭可选索引并保留数据与旧版回退副本。最终叙述报告关闭—重开、CPU 和 OAuth 刷新恢复正常，但此前已有“更新成功后仍短时占满一个核心”的不完整结果，且 Homebrew 元数据落后、可能降级的风险仍需注意。

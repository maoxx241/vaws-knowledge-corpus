---
blocks:
- block_id: 21a94d9d5cb2584cff08b65a6fb12fda41b9cc99032e9aed5bc505cde6b56922
  indexed: true
  sha256: 56232f713188a0068d2ec0b742027c7b6ec2c4f0e400fd2d6a1caca321396614
  source_range:
    end: 16777956
    part: 0
    start: 0
  summary: 审查确认旧 updater 使用已删除的 `MaintenanceBudget`/`loop.limits` 接口，导致旧代到新 knowledge 的升级被安全拒绝；同时 `capture-ready` 未区分服务绑定与原生 Hook 投递，历史 generation/cache 的长期回收边界也未闭合。后续方案改为由候选自身 validator 校验精确组件、publication contract、manifest/hash 和运行时身份，updater 仅提供稳定外壳与事务语义；已完成多轮定向验证、错误回执保留和摘要/自动采集边界修复，但最终 immutable 安装、Windows/宿主 Hook 验收及卸载路径保护仍未完成。最新审查还发现卸载可能直接删除仍活跃的旧 MCP 进程或未完成回滚目录，待复用代际清理判据并保持锁至删除结束。
  title: MindIE Codex 设计审查与候选验证收敛
- block_id: b045d69b7bc24ea14de886a73a2ce6e854ad19f2cb855bb5491ac5f69d22d287
  indexed: true
  sha256: e42143c0512db3d3ccb94056f4e18e95285611d169efe8b5031a524431fdc207
  source_range:
    end: 16777956
    part: 1
    start: 0
  summary: '九原则复核未发现剩余原则级阻断；错误可见性、失败阶段/错误码、双失败持久化、并发字段保留、候选协议和摘要取消边界均已有定向验证，PR #19 两平台 CI 也曾报告成功。设计取消摘要固定总时限，区分业务完成与进程清理，未知外部结果不自动重放，并让自动采集校验原生会话身份、项目范围和授权边界；资源缓存中损坏目录和未完成指针问题已修复。报告仍明确最终组合测试、远端 CI、原生安装、Hook 信任、外部 Bot 事件交付及 immutable 安装尚未由组件测试证明，且最新适配器审查发现卸载未接入代际保护，活跃旧进程和未完成事务目录存在误删风险。'
  title: 九原则复核、超时语义与生命周期清理
- block_id: a039bfa177d032833d3fa48443ba65def1a944fa1a196fef2675d036f7fdeb63
  indexed: true
  sha256: 34581a41872cbe101e20a301ddd0e4dd840c0bbfdae53ef2bd358819699efc26
  source_range:
    end: 21179395
    part: 0
    start: 16777956
  summary: Adapter 提交 `9ed188a...` 后，Windows 初轮出现 5 项断言失败和 2 项错误，涉及进程超时预期、换行、只读 Git 对象清理、Stop Hook 及测试服务退出；只读文件清理和测试契约等修复汇入 `9d0d7da`，PowerShell Stop Hook 用例随后通过。最终 Linux CI 为 359 tests、跳过 12，Windows 为 359 tests、跳过 13，均成功；但记录明确要求最终报告区分实际安装版本与新 Core CI head，并未把这些组件证据扩展为真实宿主 Hook、Windows 产品验收或模型端到端验收。
  title: PR19 双平台 CI 修复与最终结果
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 23d47556ae9d8ffde67dd2ba1e169899babd8e5e16166fd62cdbf64f7a3d1a19
  kind: experience
  material_digest: 6318132b76f232e583f677b7780900eb8bbe3bb9acd658a7206c824a22cc18d9
  revision: 31e8e2f03701b5e7cfdd10428e58e6f40815d57f05067878e789297349ab6fb6
  schema: mindie-entry/3
  summary: 当前主线已从卸载路径修复推进到 PR19 的双平台 CI 收敛：`9d0d7da` 的 Linux 与 Windows CI 均成功，Windows 初轮失败的只读 Git 对象清理、旧版本目录回收、PowerShell Stop Hook 和相关测试契约已在后续结果中得到实际验证。仍需在检索时保留早期候选验证协议、错误可见性、无固定超时的摘要生命周期、自动采集身份边界，以及旧升级桥接、缓存长期回收和 `capture-ready` 语义等背景；最终报告必须分别标注实际安装版本与新 Core CI head，且不能把组件测试或预检提升为真实宿主 Hook、原生安装、外部 Bot 事件、immutable 安装或模型端到端验收。
  title: MindIE Codex 当前任务导航
navigation: 当前主线已从卸载路径修复推进到 PR19 的双平台 CI 收敛：`9d0d7da` 的 Linux 与 Windows CI 均成功，Windows 初轮失败的只读 Git 对象清理、旧版本目录回收、PowerShell Stop Hook 和相关测试契约已在后续结果中得到实际验证。仍需在检索时保留早期候选验证协议、错误可见性、无固定超时的摘要生命周期、自动采集身份边界，以及旧升级桥接、缓存长期回收和 `capture-ready` 语义等背景；最终报告必须分别标注实际安装版本与新 Core CI head，且不能把组件测试或预检提升为真实宿主 Hook、原生安装、外部 Bot 事件、immutable 安装或模型端到端验收。
schema: mindie-material-task/1
status: complete
task_id: 23d47556ae9d8ffde67dd2ba1e169899babd8e5e16166fd62cdbf64f7a3d1a19
---

# MindIE Codex 当前任务导航

当前主线已从卸载路径修复推进到 PR19 的双平台 CI 收敛：`9d0d7da` 的 Linux 与 Windows CI 均成功，Windows 初轮失败的只读 Git 对象清理、旧版本目录回收、PowerShell Stop Hook 和相关测试契约已在后续结果中得到实际验证。仍需在检索时保留早期候选验证协议、错误可见性、无固定超时的摘要生命周期、自动采集身份边界，以及旧升级桥接、缓存长期回收和 `capture-ready` 语义等背景；最终报告必须分别标注实际安装版本与新 Core CI head，且不能把组件测试或预检提升为真实宿主 Hook、原生安装、外部 Bot 事件、immutable 安装或模型端到端验收。

## Materials

- [MindIE Codex 设计审查与候选验证收敛](blocks/21a94d9d5cb2584cff08b65a6fb12fda41b9cc99032e9aed5bc505cde6b56922.md): 审查确认旧 updater 使用已删除的 `MaintenanceBudget`/`loop.limits` 接口，导致旧代到新 knowledge 的升级被安全拒绝；同时 `capture-ready` 未区分服务绑定与原生 Hook 投递，历史 generation/cache 的长期回收边界也未闭合。后续方案改为由候选自身 validator 校验精确组件、publication contract、manifest/hash 和运行时身份，updater 仅提供稳定外壳与事务语义；已完成多轮定向验证、错误回执保留和摘要/自动采集边界修复，但最终 immutable 安装、Windows/宿主 Hook 验收及卸载路径保护仍未完成。最新审查还发现卸载可能直接删除仍活跃的旧 MCP 进程或未完成回滚目录，待复用代际清理判据并保持锁至删除结束。
- [九原则复核、超时语义与生命周期清理](blocks/b045d69b7bc24ea14de886a73a2ce6e854ad19f2cb855bb5491ac5f69d22d287.md): 九原则复核未发现剩余原则级阻断；错误可见性、失败阶段/错误码、双失败持久化、并发字段保留、候选协议和摘要取消边界均已有定向验证，PR #19 两平台 CI 也曾报告成功。设计取消摘要固定总时限，区分业务完成与进程清理，未知外部结果不自动重放，并让自动采集校验原生会话身份、项目范围和授权边界；资源缓存中损坏目录和未完成指针问题已修复。报告仍明确最终组合测试、远端 CI、原生安装、Hook 信任、外部 Bot 事件交付及 immutable 安装尚未由组件测试证明，且最新适配器审查发现卸载未接入代际保护，活跃旧进程和未完成事务目录存在误删风险。
- [PR19 双平台 CI 修复与最终结果](blocks/a039bfa177d032833d3fa48443ba65def1a944fa1a196fef2675d036f7fdeb63.md): Adapter 提交 `9ed188a...` 后，Windows 初轮出现 5 项断言失败和 2 项错误，涉及进程超时预期、换行、只读 Git 对象清理、Stop Hook 及测试服务退出；只读文件清理和测试契约等修复汇入 `9d0d7da`，PowerShell Stop Hook 用例随后通过。最终 Linux CI 为 359 tests、跳过 12，Windows 为 359 tests、跳过 13，均成功；但记录明确要求最终报告区分实际安装版本与新 Core CI head，并未把这些组件证据扩展为真实宿主 Hook、Windows 产品验收或模型端到端验收。

---
blocks:
- block_id: 7fdb08d56dd74b10ce83232474bac22676b02b8a99610931157cc4fc65545cb5
  indexed: true
  sha256: 860c9dd0a33e55e0db53d2fda6efab0872a28ca4ff0da64a2855e22c8a53f2de
  source_range:
    end: 16792367
    part: 0
    start: 0
  summary: '审查认为主要宏观风险在知识消费质量、重复引用、摘要滞后和固定传输预算，而非继续增加平台层；建议以来源关系、块级引用、可读性和失败可见性改善检索，同时保留正文、失败、反例与不确定状态。后续设计冻结了引用分组、精确 block ref、续写重基、撤回和损坏错误语义，并报告多轮本地测试通过，但明确未证明真实 GitHub Bot 自动合并、跨仓部署、实机 SSH、原生 Windows、NPU 或完整消费者收益。remote-dev 还经历了默认超时、取消阻塞、负 ACK、清理异常覆盖原错等问题的修正；PR #26 五项 CI 曾对应同一 SHA 全部成功，但 PR 仍为开放草稿，后续 Windows 日志问题仍在核对。'
  title: MindIE Agent 全链路设计审查与知识检索边界
- block_id: 12fcb7dbd458e57f2b71cbaf2bd0ad4c07a2bd1d974bbdd24672d3503b4801c7
  indexed: true
  sha256: 60ed630318ac97876999fac067b0a53151181a7d808d61e848b0b8d26284b793
  source_range:
    end: 16792367
    part: 1
    start: 0
  summary: '九项原则强调单块读取、派生检索索引、Markdown 正文权威、引用仅作导航、按需介入和错误可见；`material_corrupt`、失效引用、withdrawal、continuation 错误及契约不匹配需分别保留。已记录续写竞争问题修复、43 项续写测试、16 项错误可见性测试和 9 项定向测试通过，但终审未重复大测试，仍不等于真实 Bot、跨仓部署或原生 Harness 验收。公开语料为 20 个任务、22 个块，紧凑相关项曾将首屏约 52 KB 降至约 31 KB；这只说明读取量变化，尚未证明消费者任务更快或更准确。Gitleaks 定向测试 20 项通过，PR #26 的 CI 曾全绿，但该 PR 未合并，当前 Windows 错误调查仍可能未完成。'
  title: 九项设计原则、续写语义与评估边界
- block_id: 848518091db59fafd0eb9eb2243f01ae40c8a1558a42c388a400d965b406da80
  indexed: true
  sha256: ee015729301fba02af94f9db0e551181fddb8eef1d103df4f2ef39130e07132b
  source_range:
    end: 18200622
    part: 0
    start: 16792367
  summary: 测试清理已调整为停止测试独占服务、释放 SQLite 所有者锁，并为 PowerShell watchdog 增加阶段记录和整棵子进程清理；定向测试报告 45 passed，最终复测报告 7 passed、6 skipped，但原 PowerShell 超时仍待 Windows CI 验证。只读审查发现 test_material_packages.py 中 13 个 MaterialStore 延迟漏关、3 个 SQLite 连接仅提交未关闭，且 close 后调用 index_status() 可能重开连接；单例实测确认测试返回后 catalog 连接仍开放，但这些潜在问题未被确认是本轮 Windows 失败原因。
  title: Windows 测试清理与 MaterialStore 句柄审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8fb1a7b377badf8719a1c6c38549048da6ba073aceae89d5a7bcc6ad7b0500c2
  kind: experience
  material_digest: aecbe7a1da3c20e62b465649248cec207de89153b5ce31d32c0012b0c1403ae7
  revision: 74893dea84881c56fd441ae3a62bbb9d40368996700fb57e5a30eb5fa7c10805
  schema: mindie-entry/3
  summary: '当前材料覆盖冻结后的知识库检索设计、块级引用与续写/撤回/损坏错误语义，以及 remote-dev 的超时、取消、负 ACK、清理失败和 Gitleaks 扫描边界。新增材料聚焦 Windows 测试清理、PowerShell 15 秒 watchdog、SQLite 所有者锁、服务与子进程回收，以及 MaterialStore/catalog 连接生命周期；已报告的本地测试通过和复测结果仍受环境范围限制，原超时尚待 Windows CI，发现的延迟漏关和未关闭连接也尚未确认为失败根因。消费者实际收益、真实 Bot 自动合并、跨仓部署、实机 SSH、原生 Windows、NPU 与原生 Harness 仍未由这些材料验证；PR #26 仍为开放草稿，Windows 日志问题及后续修复结果仍需核对。'
  title: MindIE Agent 知识检索与远程组件审查导航
navigation: '当前材料覆盖冻结后的知识库检索设计、块级引用与续写/撤回/损坏错误语义，以及 remote-dev 的超时、取消、负 ACK、清理失败和 Gitleaks 扫描边界。新增材料聚焦 Windows 测试清理、PowerShell 15 秒 watchdog、SQLite 所有者锁、服务与子进程回收，以及 MaterialStore/catalog 连接生命周期；已报告的本地测试通过和复测结果仍受环境范围限制，原超时尚待 Windows CI，发现的延迟漏关和未关闭连接也尚未确认为失败根因。消费者实际收益、真实 Bot 自动合并、跨仓部署、实机 SSH、原生 Windows、NPU 与原生 Harness 仍未由这些材料验证；PR #26 仍为开放草稿，Windows 日志问题及后续修复结果仍需核对。'
schema: mindie-material-task/1
status: complete
task_id: 8fb1a7b377badf8719a1c6c38549048da6ba073aceae89d5a7bcc6ad7b0500c2
---

# MindIE Agent 知识检索与远程组件审查导航

当前材料覆盖冻结后的知识库检索设计、块级引用与续写/撤回/损坏错误语义，以及 remote-dev 的超时、取消、负 ACK、清理失败和 Gitleaks 扫描边界。新增材料聚焦 Windows 测试清理、PowerShell 15 秒 watchdog、SQLite 所有者锁、服务与子进程回收，以及 MaterialStore/catalog 连接生命周期；已报告的本地测试通过和复测结果仍受环境范围限制，原超时尚待 Windows CI，发现的延迟漏关和未关闭连接也尚未确认为失败根因。消费者实际收益、真实 Bot 自动合并、跨仓部署、实机 SSH、原生 Windows、NPU 与原生 Harness 仍未由这些材料验证；PR #26 仍为开放草稿，Windows 日志问题及后续修复结果仍需核对。

## Materials

- [MindIE Agent 全链路设计审查与知识检索边界](blocks/7fdb08d56dd74b10ce83232474bac22676b02b8a99610931157cc4fc65545cb5.md): 审查认为主要宏观风险在知识消费质量、重复引用、摘要滞后和固定传输预算，而非继续增加平台层；建议以来源关系、块级引用、可读性和失败可见性改善检索，同时保留正文、失败、反例与不确定状态。后续设计冻结了引用分组、精确 block ref、续写重基、撤回和损坏错误语义，并报告多轮本地测试通过，但明确未证明真实 GitHub Bot 自动合并、跨仓部署、实机 SSH、原生 Windows、NPU 或完整消费者收益。remote-dev 还经历了默认超时、取消阻塞、负 ACK、清理异常覆盖原错等问题的修正；PR #26 五项 CI 曾对应同一 SHA 全部成功，但 PR 仍为开放草稿，后续 Windows 日志问题仍在核对。
- [九项设计原则、续写语义与评估边界](blocks/12fcb7dbd458e57f2b71cbaf2bd0ad4c07a2bd1d974bbdd24672d3503b4801c7.md): 九项原则强调单块读取、派生检索索引、Markdown 正文权威、引用仅作导航、按需介入和错误可见；`material_corrupt`、失效引用、withdrawal、continuation 错误及契约不匹配需分别保留。已记录续写竞争问题修复、43 项续写测试、16 项错误可见性测试和 9 项定向测试通过，但终审未重复大测试，仍不等于真实 Bot、跨仓部署或原生 Harness 验收。公开语料为 20 个任务、22 个块，紧凑相关项曾将首屏约 52 KB 降至约 31 KB；这只说明读取量变化，尚未证明消费者任务更快或更准确。Gitleaks 定向测试 20 项通过，PR #26 的 CI 曾全绿，但该 PR 未合并，当前 Windows 错误调查仍可能未完成。
- [Windows 测试清理与 MaterialStore 句柄审查](blocks/848518091db59fafd0eb9eb2243f01ae40c8a1558a42c388a400d965b406da80.md): 测试清理已调整为停止测试独占服务、释放 SQLite 所有者锁，并为 PowerShell watchdog 增加阶段记录和整棵子进程清理；定向测试报告 45 passed，最终复测报告 7 passed、6 skipped，但原 PowerShell 超时仍待 Windows CI 验证。只读审查发现 test_material_packages.py 中 13 个 MaterialStore 延迟漏关、3 个 SQLite 连接仅提交未关闭，且 close 后调用 index_status() 可能重开连接；单例实测确认测试返回后 catalog 连接仍开放，但这些潜在问题未被确认是本轮 Windows 失败原因。

---
blocks:
- block_id: 60ac968f0c162e769d1d092823395e37e25b4b2fd23c258098cc785cdf1d7d04
  indexed: true
  sha256: 8d8db01ef531976567824b26ecb31f7bceb3c0958033d73d1386648fc9de066e
  source_range:
    end: 4696318
    part: 0
    start: 0
  summary: 多轮只读审查与独立验收围绕以 `public_relpath` 维持文档身份、pending 生命周期、跨 Agent 修订、终态 PR 新分支、脱敏失败恢复及并发覆盖风险展开；早期指出 `submit.py`、`publishing.py`、`capture.py` 和 `gitops.py` 的潜在阻塞点，后续测试报告称 35/45/60 passed，但未进行真实 GitHub、原生 Windows 或生产提交验收。经验反馈后来改用 GitHub Issue 的完整 marker、按账号隔离并以事件 ID 去重；正负反馈独立累计，响应丢失且未携带原事件 ID 时仍可能被视为新反馈。多次复核称 MCP/CLI、owner pin、corpus 52 篇 Markdown 的路径与 Git blob 一致且无新 must-fix，但 consumer 仍待 rebase 新 head 后补核，相关结论均应视为报告的检查结果而非已验证成功。
  title: 稳定文档身份、反馈契约与只读验收记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ff914267ce4e62b1f9bd4208661dc14ff4a383f189fffce1946d3abc8cc14e06
  kind: experience
  material_digest: 56f461a0592a6c62aa2a72dc0ca03927473f9242b47816a6cc131417dc46d330
  revision: 7b6b07cd5dab38508131fa3799b1464d8aebf8de1131aceb39bc73a4934f2100
  schema: mindie-entry/3
  summary: 当前材料集中于稳定 `public_relpath` 与 pending/PR 修订生命周期、经验反馈的授权与幂等去重、MCP/CLI 契约及 owner/corpus pin 复核。可检索的未决或边界包括并发回写覆盖、真实 GitHub/Windows 验收缺失、响应丢失后缺少原事件 ID 的重复反馈风险，以及 consumer rebase 后的补核。
  title: 任务导航：文档身份、反馈与发布复核
navigation: 当前材料集中于稳定 `public_relpath` 与 pending/PR 修订生命周期、经验反馈的授权与幂等去重、MCP/CLI 契约及 owner/corpus pin 复核。可检索的未决或边界包括并发回写覆盖、真实 GitHub/Windows 验收缺失、响应丢失后缺少原事件 ID 的重复反馈风险，以及 consumer rebase 后的补核。
schema: mindie-material-task/1
status: complete
task_id: ff914267ce4e62b1f9bd4208661dc14ff4a383f189fffce1946d3abc8cc14e06
---

# 任务导航：文档身份、反馈与发布复核

当前材料集中于稳定 `public_relpath` 与 pending/PR 修订生命周期、经验反馈的授权与幂等去重、MCP/CLI 契约及 owner/corpus pin 复核。可检索的未决或边界包括并发回写覆盖、真实 GitHub/Windows 验收缺失、响应丢失后缺少原事件 ID 的重复反馈风险，以及 consumer rebase 后的补核。

## Materials

- [稳定文档身份、反馈契约与只读验收记录](blocks/60ac968f0c162e769d1d092823395e37e25b4b2fd23c258098cc785cdf1d7d04.md): 多轮只读审查与独立验收围绕以 `public_relpath` 维持文档身份、pending 生命周期、跨 Agent 修订、终态 PR 新分支、脱敏失败恢复及并发覆盖风险展开；早期指出 `submit.py`、`publishing.py`、`capture.py` 和 `gitops.py` 的潜在阻塞点，后续测试报告称 35/45/60 passed，但未进行真实 GitHub、原生 Windows 或生产提交验收。经验反馈后来改用 GitHub Issue 的完整 marker、按账号隔离并以事件 ID 去重；正负反馈独立累计，响应丢失且未携带原事件 ID 时仍可能被视为新反馈。多次复核称 MCP/CLI、owner pin、corpus 52 篇 Markdown 的路径与 Git blob 一致且无新 must-fix，但 consumer 仍待 rebase 新 head 后补核，相关结论均应视为报告的检查结果而非已验证成功。

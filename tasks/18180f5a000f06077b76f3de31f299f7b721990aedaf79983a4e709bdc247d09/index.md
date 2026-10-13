---
blocks:
- block_id: e004847683cb0d8f889adfaf6d8d482abdb260ce008b1216ce28fdc62ab4b511
  indexed: true
  sha256: 2d9a425b2fa254c03a99426456989af043d705e6d0236ea05258384c12147800
  source_range:
    end: 1689285
    part: 0
    start: 0
  summary: 记录了 LoRA MoE 回退在两个 worktree 中完成，并将 `quant_moe.py` 与对应 UT 恢复为相对最新 `origin/main` 字节一致；focused UT 为 `155 passed, 1 deselected`，但基线同样存在 `N (=4) must be a multiple of 8` 断言。首次启动因 Gloo `device.cc:99 rv == 0, -2 vs 0` 的 hostname/interface 解析失败，在装权重前退出，设置 `GLOO_SOCKET_IFNAME=lo` 后 16/16 ranks、333/333 shards 和图编译完成；随后报告 `/v1/models`、文本、多模态及 16 路请求均返回 200，并观察到 `Replaying aclgraph`，但这些是报告中的验证结果而非独立确认。最终报告称两个 PR 已推送并更新描述，同时指出 CI/E2E 与中文文档构建仍在运行，裁剪权重的输出语义较差，且完整服务日志和失败日志已保留在所述临时路径。
  title: LoRA MoE 回退与 153 真机验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 18180f5a000f06077b76f3de31f299f7b721990aedaf79983a4e709bdc247d09
  kind: experience
  material_digest: ee537bc1f4e3e0e365b1344d414a2f1c0abea7309f5d4dd69c60748f5c5232f6
  revision: 324196e36c3b06256c61fee7e18cba3c0048d1ea053679dfe3be32236a0d988a
  schema: mindie-entry/3
  summary: '核心线索是 #14599/#14454 的 LoRA 回退、Gloo 初始化失败后通过 `GLOO_SOCKET_IFNAME=lo` 重试、TP16/EP 真机请求验证及 PR 发布状态。后续检索应关注提交 head、focused UT 基线差异、服务日志、ACLGraph replay，以及仍未完成或未独立确认的 CI、Docs 构建和输出质量问题。'
  title: 当前任务导航：LoRA MoE 回退与发布验证
navigation: '核心线索是 #14599/#14454 的 LoRA 回退、Gloo 初始化失败后通过 `GLOO_SOCKET_IFNAME=lo` 重试、TP16/EP 真机请求验证及 PR 发布状态。后续检索应关注提交 head、focused UT 基线差异、服务日志、ACLGraph replay，以及仍未完成或未独立确认的 CI、Docs 构建和输出质量问题。'
schema: mindie-material-task/1
status: complete
task_id: 18180f5a000f06077b76f3de31f299f7b721990aedaf79983a4e709bdc247d09
---

# 当前任务导航：LoRA MoE 回退与发布验证

核心线索是 #14599/#14454 的 LoRA 回退、Gloo 初始化失败后通过 `GLOO_SOCKET_IFNAME=lo` 重试、TP16/EP 真机请求验证及 PR 发布状态。后续检索应关注提交 head、focused UT 基线差异、服务日志、ACLGraph replay，以及仍未完成或未独立确认的 CI、Docs 构建和输出质量问题。

## Materials

- [LoRA MoE 回退与 153 真机验证](blocks/e004847683cb0d8f889adfaf6d8d482abdb260ce008b1216ce28fdc62ab4b511.md): 记录了 LoRA MoE 回退在两个 worktree 中完成，并将 `quant_moe.py` 与对应 UT 恢复为相对最新 `origin/main` 字节一致；focused UT 为 `155 passed, 1 deselected`，但基线同样存在 `N (=4) must be a multiple of 8` 断言。首次启动因 Gloo `device.cc:99 rv == 0, -2 vs 0` 的 hostname/interface 解析失败，在装权重前退出，设置 `GLOO_SOCKET_IFNAME=lo` 后 16/16 ranks、333/333 shards 和图编译完成；随后报告 `/v1/models`、文本、多模态及 16 路请求均返回 200，并观察到 `Replaying aclgraph`，但这些是报告中的验证结果而非独立确认。最终报告称两个 PR 已推送并更新描述，同时指出 CI/E2E 与中文文档构建仍在运行，裁剪权重的输出语义较差，且完整服务日志和失败日志已保留在所述临时路径。

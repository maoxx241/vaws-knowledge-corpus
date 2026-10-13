---
blocks:
- block_id: 5cd59e54c29e44933f896573dc558f009fd62864844046e0ac9011d4efb1b4bf
  indexed: true
  sha256: 146c6169d95652fb849cd195f27e648172eb8da94b47c418e6aa10fc21908a55
  source_range:
    end: 456637
    part: 0
    start: 0
  summary: 'H0 客户端已准备并部署到被脱敏主机，尚未向真实服务发送请求；已核对 `X-data-parallel-rank: 0..3`、`POST /reset_prefix_cache` 的精确成功返回校验及按 engine 拆分的 metrics。通过离线假服务自测：32 个 c32 load 加 4 个顺序 tail，共 36 个请求，DP0–3 各路由 9 次；可落盘 `summary.json`、`raw-results.json` 和三份 metrics 快照。此前缺少独立 raw 落盘及负载后的四 DP tail probe，已通过向后兼容参数补齐；SHA256 与路径均已脱敏或记录于源内容中。'
  title: H0 客户端准备与离线自测
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c3aaf814b79b2a4f565ee7ed9f47f2bab3c4c6fecb8a2a437b0c5314d6f32bb4
  kind: experience
  material_digest: e1cdd142116ab8af4cd894d59c004be37dc5ef2c8028560afc79ed4be4809018
  revision: d728c337c14a8ecd5dce7485b0d88166142d006712e6827caffc02e685b249d8
  schema: mindie-entry/3
  summary: H0 客户端已部署但真实服务验证尚未开始；当前可检索参数核对、路由头、reset-prefix-cache API、离线假服务自测、raw 结果及 metrics 落盘信息。离线自测结果不等同于真实服务成功，后续仍需关注真实请求与四 DP tail probe。
  title: 当前任务导航：H0 客户端验证准备
navigation: H0 客户端已部署但真实服务验证尚未开始；当前可检索参数核对、路由头、reset-prefix-cache API、离线假服务自测、raw 结果及 metrics 落盘信息。离线自测结果不等同于真实服务成功，后续仍需关注真实请求与四 DP tail probe。
schema: mindie-material-task/1
status: complete
task_id: c3aaf814b79b2a4f565ee7ed9f47f2bab3c4c6fecb8a2a437b0c5314d6f32bb4
---

# 当前任务导航：H0 客户端验证准备

H0 客户端已部署但真实服务验证尚未开始；当前可检索参数核对、路由头、reset-prefix-cache API、离线假服务自测、raw 结果及 metrics 落盘信息。离线自测结果不等同于真实服务成功，后续仍需关注真实请求与四 DP tail probe。

## Materials

- [H0 客户端准备与离线自测](blocks/5cd59e54c29e44933f896573dc558f009fd62864844046e0ac9011d4efb1b4bf.md): H0 客户端已准备并部署到被脱敏主机，尚未向真实服务发送请求；已核对 `X-data-parallel-rank: 0..3`、`POST /reset_prefix_cache` 的精确成功返回校验及按 engine 拆分的 metrics。通过离线假服务自测：32 个 c32 load 加 4 个顺序 tail，共 36 个请求，DP0–3 各路由 9 次；可落盘 `summary.json`、`raw-results.json` 和三份 metrics 快照。此前缺少独立 raw 落盘及负载后的四 DP tail probe，已通过向后兼容参数补齐；SHA256 与路径均已脱敏或记录于源内容中。

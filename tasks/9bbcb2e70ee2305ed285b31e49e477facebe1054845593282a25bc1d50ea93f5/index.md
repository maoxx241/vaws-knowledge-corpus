---
blocks:
- block_id: 4986a2d64310402ee9ca6176cedf89d42e9540779dd7c1ebf838f1eca6a31b6e
  indexed: true
  sha256: 8ca5811b68e97f8d4e9853c55d4575a71266b3b3e505ecf813a53b3f5e0518d9
  source_range:
    end: 2013068
    part: 0
    start: 0
  summary: 记录了 r7 的 failure-shape 观察、r8 的 async-on/shared-expert multistream off 启动与定向清理计划，以及 r9 对 `TASK_QUEUE_ENABLE=0`、四个 PGID、worker 创建进度和 host-only `py-spy dump` 的只读取证；这些内容包含计划、检查和未完成状态，不能一概视为已验证成功。来源最终自报 r9 H0 为 4/32 成功、四个 DP 均卡在 `wave0/step0`，其余请求在 120 秒超时，随后出现 `sample_tokens` RPC timeout 并自退出；据称已保存日志、scheduler dump、summary、metrics、live-stack 缺口证据及 17 项 manifest，但这是来源陈述而非独立核验。关键术语包括 health=200、EngineCore/RPC、scheduler dump、64 workers、fatal、PGID、py-spy、NPU Event/query/synchronize、SHA256 manifest `c109d0aa61f558cd4ae0c329d407d8504247347287d99243a2c4bf53bdcd8fe5`。
  title: r7–r9 多节点只读监控与 r9 终态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9bbcb2e70ee2305ed285b31e49e477facebe1054845593282a25bc1d50ea93f5
  kind: experience
  material_digest: 4199792ab176745ab66eb3eca7a53555aaa0720d6fe7843bc61b293e2951ffb4
  revision: 8c7f3ae0aba5713f90a105b48105df2497b6acbf4c19f094943e48e5aac48614
  schema: mindie-entry/3
  summary: 当前批次覆盖 r7、r8、r9 的四节点服务观察、变量核对、进程组清理计划和 host-only 栈取证。检索重点是 r9 的 4/32 H0 结果、`wave0/step0` 卡顿、`sample_tokens` RPC timeout、自退出及据称保存的证据清单；r7/r8 的部分内容仍是观察或计划，不能当作已验证成功。
  title: 多节点推理实验监控索引
navigation: 当前批次覆盖 r7、r8、r9 的四节点服务观察、变量核对、进程组清理计划和 host-only 栈取证。检索重点是 r9 的 4/32 H0 结果、`wave0/step0` 卡顿、`sample_tokens` RPC timeout、自退出及据称保存的证据清单；r7/r8 的部分内容仍是观察或计划，不能当作已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 9bbcb2e70ee2305ed285b31e49e477facebe1054845593282a25bc1d50ea93f5
---

# 多节点推理实验监控索引

当前批次覆盖 r7、r8、r9 的四节点服务观察、变量核对、进程组清理计划和 host-only 栈取证。检索重点是 r9 的 4/32 H0 结果、`wave0/step0` 卡顿、`sample_tokens` RPC timeout、自退出及据称保存的证据清单；r7/r8 的部分内容仍是观察或计划，不能当作已验证成功。

## Materials

- [r7–r9 多节点只读监控与 r9 终态](blocks/4986a2d64310402ee9ca6176cedf89d42e9540779dd7c1ebf838f1eca6a31b6e.md): 记录了 r7 的 failure-shape 观察、r8 的 async-on/shared-expert multistream off 启动与定向清理计划，以及 r9 对 `TASK_QUEUE_ENABLE=0`、四个 PGID、worker 创建进度和 host-only `py-spy dump` 的只读取证；这些内容包含计划、检查和未完成状态，不能一概视为已验证成功。来源最终自报 r9 H0 为 4/32 成功、四个 DP 均卡在 `wave0/step0`，其余请求在 120 秒超时，随后出现 `sample_tokens` RPC timeout 并自退出；据称已保存日志、scheduler dump、summary、metrics、live-stack 缺口证据及 17 项 manifest，但这是来源陈述而非独立核验。关键术语包括 health=200、EngineCore/RPC、scheduler dump、64 workers、fatal、PGID、py-spy、NPU Event/query/synchronize、SHA256 manifest `c109d0aa61f558cd4ae0c329d407d8504247347287d99243a2c4bf53bdcd8fe5`。

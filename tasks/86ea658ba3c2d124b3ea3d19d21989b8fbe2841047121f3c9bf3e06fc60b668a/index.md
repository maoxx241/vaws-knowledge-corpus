---
blocks:
- block_id: 208b2050f5ae0aaf7c541a29eb656a3ec5f2acd88e3dc89c541219f1115b4f89
  indexed: true
  sha256: 81161f1837cd39096e5463414120d1058a773e0ba23d00f9922072c056cdb837
  source_range:
    end: 840755
    part: 0
    start: 0
  summary: r18-v2 的只读核验记录称四个 leader、64 个 worker 存活，fatal/RPC timeout/ERROR 为 0，reset 与 completion 请求均返回 HTTP 200，DFX/result 快照停止变化；随后在 exact guard 通过后按 156→155→154→153 发送 TERM，服务进程与端口归零但容器仍运行，产物哈希未变，且未启动 r19。r19 启动期间经历 HCCL distributed-init、worker 创建、351 个权重分片加载、Ascend torch.compile、KV cache profile 与单尺寸图捕获；曾出现 shared-memory 60 秒提示，但记录未见异常堆栈或退出，最终 64/64 workers、API startup complete，静态闭包哈希匹配，四机 fatal/ERROR 为 0，唯一允许的 /health 返回 200。以上是来源中的报告性结果，不等同于独立验证；中间状态包括 worker READY 计数暂为 0、加载进度 216/351，以及日志静默但 CPU ticks 增长的判断。
  title: r18-v2 闭包核验、停服与 r19 READY
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 86ea658ba3c2d124b3ea3d19d21989b8fbe2841047121f3c9bf3e06fc60b668a
  kind: experience
  material_digest: 5aa097149717517f4a388fc9558f4e06ec3a3d1b7080dd1d2178fd1ede36e7a1
  revision: 3d2f9b43b662e8a8bf2551c225910b699a099dec75f09beef3af5ff3f5e69afa
  schema: mindie-entry/3
  summary: 当前可检索上下文涵盖 r18-v2 的只读核验、精确 TERM 停服及产物稳定性，以及 r19 从 distributed-init 到 compile/capture 完成和 /health 200 的启动过程。保留了 shared-memory 提示、无日志初始化、worker 计数与权重加载进度等不确定或中间状态；来源结论均为报告内容，未升级为独立验证。
  title: r18-v2 收尾与 r19 启动状态索引
navigation: 当前可检索上下文涵盖 r18-v2 的只读核验、精确 TERM 停服及产物稳定性，以及 r19 从 distributed-init 到 compile/capture 完成和 /health 200 的启动过程。保留了 shared-memory 提示、无日志初始化、worker 计数与权重加载进度等不确定或中间状态；来源结论均为报告内容，未升级为独立验证。
schema: mindie-material-task/1
status: complete
task_id: 86ea658ba3c2d124b3ea3d19d21989b8fbe2841047121f3c9bf3e06fc60b668a
---

# r18-v2 收尾与 r19 启动状态索引

当前可检索上下文涵盖 r18-v2 的只读核验、精确 TERM 停服及产物稳定性，以及 r19 从 distributed-init 到 compile/capture 完成和 /health 200 的启动过程。保留了 shared-memory 提示、无日志初始化、worker 计数与权重加载进度等不确定或中间状态；来源结论均为报告内容，未升级为独立验证。

## Materials

- [r18-v2 闭包核验、停服与 r19 READY](blocks/208b2050f5ae0aaf7c541a29eb656a3ec5f2acd88e3dc89c541219f1115b4f89.md): r18-v2 的只读核验记录称四个 leader、64 个 worker 存活，fatal/RPC timeout/ERROR 为 0，reset 与 completion 请求均返回 HTTP 200，DFX/result 快照停止变化；随后在 exact guard 通过后按 156→155→154→153 发送 TERM，服务进程与端口归零但容器仍运行，产物哈希未变，且未启动 r19。r19 启动期间经历 HCCL distributed-init、worker 创建、351 个权重分片加载、Ascend torch.compile、KV cache profile 与单尺寸图捕获；曾出现 shared-memory 60 秒提示，但记录未见异常堆栈或退出，最终 64/64 workers、API startup complete，静态闭包哈希匹配，四机 fatal/ERROR 为 0，唯一允许的 /health 返回 200。以上是来源中的报告性结果，不等同于独立验证；中间状态包括 worker READY 计数暂为 0、加载进度 216/351，以及日志静默但 CPU ticks 增长的判断。

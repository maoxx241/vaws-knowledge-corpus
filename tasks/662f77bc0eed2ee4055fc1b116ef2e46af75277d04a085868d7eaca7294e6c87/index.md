---
blocks:
- block_id: 6af1a0a5d58254d80fc4c82fc907264086c34728d47cae70ff9b9ef15bd6344d
  indexed: true
  sha256: d2571c2d8e2c2ebe209cc357aa43c0e493631405f189b2e638f90f0befce8fc5
  source_range:
    end: 1074154
    part: 0
    start: 0
  summary: 基于 r15 `0a356`、唯一变更为 `--no-async-scheduling`，完成四机 staging/preflight，并只读监控至 64 workers、权重加载、compile、graph capture 和 health=200；期间未发送模型请求，fatal/traceback/HCCL/OOM/worker-died 均为 0。随后 B2 的 miss 与两次 warm 完成，48/48 DFX manifest 齐全，但严格门禁判定 FAIL：miss 1024 与 B1 golden 一致，hit 虽完成请求却 1024 个 token 全为 0、文本全为 `!`、hash 不一致，且 TP0–15 出现 non-finite、`required_complete=false`。因此未进入 P1，之前的 READY 只表示服务就绪，不表示 B2 或整体验证成功。
  title: async-off B2 A/B：服务 READY 但严格门禁失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 662f77bc0eed2ee4055fc1b116ef2e46af75277d04a085868d7eaca7294e6c87
  kind: experience
  material_digest: 8460b2f6aef85a8632b9dc08b81b3c47f2789070a673d1a6601d066e8447ab0e
  revision: 67c226f8ea1e76dda019fa9b06547e05f9ad3fbee57972b7a299be21212df652
  schema: mindie-entry/3
  summary: 已完成四机部署准备、服务启动与只读就绪确认；服务 health=200 且基础进程/worker 状态正常，但最终 B2 严格门禁失败，未放行 P1。关键检索词：`--no-async-scheduling`、`PREFLIGHT_ONLY`、64 workers、graph capture、B2 miss/warm/hit、1024 tokens、全 0、全 `!`、non-finite、`required_complete=false`、48 DFX manifests。
  title: 当前任务导航：async-off B2 严格验证
navigation: 已完成四机部署准备、服务启动与只读就绪确认；服务 health=200 且基础进程/worker 状态正常，但最终 B2 严格门禁失败，未放行 P1。关键检索词：`--no-async-scheduling`、`PREFLIGHT_ONLY`、64 workers、graph capture、B2 miss/warm/hit、1024 tokens、全 0、全 `!`、non-finite、`required_complete=false`、48 DFX manifests。
schema: mindie-material-task/1
status: complete
task_id: 662f77bc0eed2ee4055fc1b116ef2e46af75277d04a085868d7eaca7294e6c87
---

# 当前任务导航：async-off B2 严格验证

已完成四机部署准备、服务启动与只读就绪确认；服务 health=200 且基础进程/worker 状态正常，但最终 B2 严格门禁失败，未放行 P1。关键检索词：`--no-async-scheduling`、`PREFLIGHT_ONLY`、64 workers、graph capture、B2 miss/warm/hit、1024 tokens、全 0、全 `!`、non-finite、`required_complete=false`、48 DFX manifests。

## Materials

- [async-off B2 A/B：服务 READY 但严格门禁失败](blocks/6af1a0a5d58254d80fc4c82fc907264086c34728d47cae70ff9b9ef15bd6344d.md): 基于 r15 `0a356`、唯一变更为 `--no-async-scheduling`，完成四机 staging/preflight，并只读监控至 64 workers、权重加载、compile、graph capture 和 health=200；期间未发送模型请求，fatal/traceback/HCCL/OOM/worker-died 均为 0。随后 B2 的 miss 与两次 warm 完成，48/48 DFX manifest 齐全，但严格门禁判定 FAIL：miss 1024 与 B1 golden 一致，hit 虽完成请求却 1024 个 token 全为 0、文本全为 `!`、hash 不一致，且 TP0–15 出现 non-finite、`required_complete=false`。因此未进入 P1，之前的 READY 只表示服务就绪，不表示 B2 或整体验证成功。

---
blocks:
- block_id: 2301ed856eb340c7ed67c0d2af3d3f6dc46cc3e475db840c2e47892a130301b7
  indexed: true
  sha256: ce8d5ed4896c579954fb31097017a07188bdc5272584ffc81032dbce3716ac5d
  source_range:
    end: 1087261
    part: 0
    start: 0
  summary: 离线部署了 P1/P2 成对 runner、严格归并与 verdict，固定模型、Base URL、cache token 校验、逐请求 token/text hash、raw SSE、token IDs 和 metrics；未由该执行者发起推理请求。P1 canonical miss 严格通过（4/4、1024 tokens、prefix queries=3584、hits=0、采信率 0.77920），但 P1 bad 的 hit 虽精确为 384 cache tokens/request，4/4 输出 1024 个 `!`，复现历史坏 hash `f80a534d...`；hit 采信率 1.0 仍不足以证明输出质量。结果将 full Triton KDA 消除算子卡死路径与 prefix-hit 缓存恢复/映射精度问题区分开；P2 及 P1 natural 的最终执行结果在该块中未完成或未报告。
  title: P1/P2 prefix 命中精度工具与 P1 bad 复现
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8fec1a9ab011bd66a901c3460b102887e214f2a4152432492fce3dba22bef2a8
  kind: experience
  material_digest: fab7f47ae8015d9e8b0f603f867c84a3dc90d3d382ada1b93a516af6fa8e0039
  revision: eca47bddd58c65aad73539d103c2648b1c238a76d87cef0b27df65c903e66e4b
  schema: mindie-entry/3
  summary: 已建立并部署 P1/P2 精度检查工具，保留 miss/hit 的精确 cache token、输出与 token hash、raw SSE、metrics 及严格 verdict 证据。已确认 P1 canonical miss 通过，但 P1 bad hit 复现 1024 个 `!` 的坏输出；高采信率不能替代输出质量门禁。full Triton KDA 与 AscendC `fwd_h` 卡死路径已区分，P2 和 P1 natural 的完整结果仍需独立检索确认。
  title: 当前任务导航：prefix-hit 精度与严格验证
navigation: 已建立并部署 P1/P2 精度检查工具，保留 miss/hit 的精确 cache token、输出与 token hash、raw SSE、metrics 及严格 verdict 证据。已确认 P1 canonical miss 通过，但 P1 bad hit 复现 1024 个 `!` 的坏输出；高采信率不能替代输出质量门禁。full Triton KDA 与 AscendC `fwd_h` 卡死路径已区分，P2 和 P1 natural 的完整结果仍需独立检索确认。
schema: mindie-material-task/1
status: complete
task_id: 8fec1a9ab011bd66a901c3460b102887e214f2a4152432492fce3dba22bef2a8
---

# 当前任务导航：prefix-hit 精度与严格验证

已建立并部署 P1/P2 精度检查工具，保留 miss/hit 的精确 cache token、输出与 token hash、raw SSE、metrics 及严格 verdict 证据。已确认 P1 canonical miss 通过，但 P1 bad hit 复现 1024 个 `!` 的坏输出；高采信率不能替代输出质量门禁。full Triton KDA 与 AscendC `fwd_h` 卡死路径已区分，P2 和 P1 natural 的完整结果仍需独立检索确认。

## Materials

- [P1/P2 prefix 命中精度工具与 P1 bad 复现](blocks/2301ed856eb340c7ed67c0d2af3d3f6dc46cc3e475db840c2e47892a130301b7.md): 离线部署了 P1/P2 成对 runner、严格归并与 verdict，固定模型、Base URL、cache token 校验、逐请求 token/text hash、raw SSE、token IDs 和 metrics；未由该执行者发起推理请求。P1 canonical miss 严格通过（4/4、1024 tokens、prefix queries=3584、hits=0、采信率 0.77920），但 P1 bad 的 hit 虽精确为 384 cache tokens/request，4/4 输出 1024 个 `!`，复现历史坏 hash `f80a534d...`；hit 采信率 1.0 仍不足以证明输出质量。结果将 full Triton KDA 消除算子卡死路径与 prefix-hit 缓存恢复/映射精度问题区分开；P2 及 P1 natural 的最终执行结果在该块中未完成或未报告。

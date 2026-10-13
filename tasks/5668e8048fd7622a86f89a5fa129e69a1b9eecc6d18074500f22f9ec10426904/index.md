---
blocks:
- block_id: d10adc73be5540f29505763db462790b0ec0804269a9062effaaa88974302ae2
  indexed: true
  sha256: b94b282f47904ba4fee7b558a06fa26574e0bb5235590d4f10da39ec6c5021ea
  source_range:
    end: 1155257
    part: 0
    start: 0
  summary: 只读整理提出 H0/H1 冷首批与压力卡死、P0–P3 精度/边界/128K 污染、A 最终采信率等阶段；保留历史卡死、`!` 输出、token/hash 异常及未完成验证状态，未启动服务或修改文件。关键方法包括精确 DP 路由、reset/hit/miss 对照、固定 131072-token 请求，以及保存原始响应、metrics delta、路由和全 rank 日志。采信率要求逐格比较 hit/miss 与并发条件，`acceptance=100%` 但输出全 `!` 仍判失败；现有 runner 会 warmup、只能代表 hit，miss 仍需 no-warm 或 paired runner。
  title: 验收矩阵与采信率硬门槛
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5668e8048fd7622a86f89a5fa129e69a1b9eecc6d18074500f22f9ec10426904
  kind: experience
  material_digest: d3a3ad24be66ff1318251868f0d3954b19ed442376973bad36537cf876bc1509
  revision: af84b03017eee06210133ccde4cdb738db37057e3fa9672978797f84278631e3
  schema: mindie-entry/3
  summary: 优先执行 H0/H1 卡死门槛，任一卡死即停止后续精度测试；随后覆盖 P0–P3，并以四 prompt、四 DP 的 hit/miss paired cohort 验证采信率。导航保留历史 c32/c64 卡死、边界场景全 `!`、128K 污染和 acceptance 回归等风险，同时标明现有脚本的 warmup 限制；所有结果须区分观察、假设与 verified success，并保留失败、未完成和后续修正。
  title: 当前任务导航：构建可追溯的 MindIE 验收矩阵
navigation: 优先执行 H0/H1 卡死门槛，任一卡死即停止后续精度测试；随后覆盖 P0–P3，并以四 prompt、四 DP 的 hit/miss paired cohort 验证采信率。导航保留历史 c32/c64 卡死、边界场景全 `!`、128K 污染和 acceptance 回归等风险，同时标明现有脚本的 warmup 限制；所有结果须区分观察、假设与 verified success，并保留失败、未完成和后续修正。
schema: mindie-material-task/1
status: complete
task_id: 5668e8048fd7622a86f89a5fa129e69a1b9eecc6d18074500f22f9ec10426904
---

# 当前任务导航：构建可追溯的 MindIE 验收矩阵

优先执行 H0/H1 卡死门槛，任一卡死即停止后续精度测试；随后覆盖 P0–P3，并以四 prompt、四 DP 的 hit/miss paired cohort 验证采信率。导航保留历史 c32/c64 卡死、边界场景全 `!`、128K 污染和 acceptance 回归等风险，同时标明现有脚本的 warmup 限制；所有结果须区分观察、假设与 verified success，并保留失败、未完成和后续修正。

## Materials

- [验收矩阵与采信率硬门槛](blocks/d10adc73be5540f29505763db462790b0ec0804269a9062effaaa88974302ae2.md): 只读整理提出 H0/H1 冷首批与压力卡死、P0–P3 精度/边界/128K 污染、A 最终采信率等阶段；保留历史卡死、`!` 输出、token/hash 异常及未完成验证状态，未启动服务或修改文件。关键方法包括精确 DP 路由、reset/hit/miss 对照、固定 131072-token 请求，以及保存原始响应、metrics delta、路由和全 rank 日志。采信率要求逐格比较 hit/miss 与并发条件，`acceptance=100%` 但输出全 `!` 仍判失败；现有 runner 会 warmup、只能代表 hit，miss 仍需 no-warm 或 paired runner。

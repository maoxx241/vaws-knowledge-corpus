---
blocks:
- block_id: f78c9121fba65604a02227c23e7b224ff147fa86fdc743ee98ef9cc93901d5b6
  indexed: true
  sha256: 20169a9dc62de87179313840b52f7393fac03278428faf442f5b32bac14d87f1
  source_range:
    end: 994983
    part: 0
    start: 0
  summary: 只读核验显示 r19 的 completion fence 未修复 prefix-hit 输出损坏：B2 的 miss 与 warm 阶段正常，但 hit 阶段虽 HTTP 200、accepted/draft 为 896/896，最终 1024 个 token 全为 0、文本全为 `!`，因此 `experiment_valid=false`、`all_passed=false`。服务、四机进程、源码闭包、路由、cache geometry、preemption 与 HTTP 层均未发现失败证据，日志也无 ERROR/FATAL；四机时钟存在偏移，但不能解释结果。当前问题被约束为 accepted-token completion 到最终 host-visible token buffer 物化之间的 stream/event 或 buffer ownership 依赖不足；这仍是定位方向而非已验证根因，后续应保留未完成状态并继续追踪 hit 路径。
  title: r19 completion-fence 失败证据与后续定位
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 97b67e5432d303f7ec09070c9f5bca72169b1c2ebd9a5f0c2de5845b9addd928
  kind: experience
  material_digest: 8d8ed447798233de2878793de26df100ee466f5bd31d114b220edeb9d0d71fa0
  revision: 7cf044e389fc92e798d1dfba250af86579bbe27a45f7e9eb6b1aa7884081b06f
  schema: mindie-entry/3
  summary: 已确认的失败现象是 r19 B2 cache-hit 在 speculative 全量接受后产生全零/all-`!`输出，completion fence 未能保证最终 host-visible token buffer 正确可见；miss、warm、服务进程与多项环境闭包正常。不要将 stream/event、buffer ownership 或具体生产点假设当作已验证根因；原始证据、哈希、时间线和源码闭包仍应独立检索，后续重点是补齐 accepted-token completion 到输出缓冲区物化之间的设备侧依赖链。
  title: 当前任务导航：r19 cache-hit 输出损坏定位
navigation: 已确认的失败现象是 r19 B2 cache-hit 在 speculative 全量接受后产生全零/all-`!`输出，completion fence 未能保证最终 host-visible token buffer 正确可见；miss、warm、服务进程与多项环境闭包正常。不要将 stream/event、buffer ownership 或具体生产点假设当作已验证根因；原始证据、哈希、时间线和源码闭包仍应独立检索，后续重点是补齐 accepted-token completion 到输出缓冲区物化之间的设备侧依赖链。
schema: mindie-material-task/1
status: complete
task_id: 97b67e5432d303f7ec09070c9f5bca72169b1c2ebd9a5f0c2de5845b9addd928
---

# 当前任务导航：r19 cache-hit 输出损坏定位

已确认的失败现象是 r19 B2 cache-hit 在 speculative 全量接受后产生全零/all-`!`输出，completion fence 未能保证最终 host-visible token buffer 正确可见；miss、warm、服务进程与多项环境闭包正常。不要将 stream/event、buffer ownership 或具体生产点假设当作已验证根因；原始证据、哈希、时间线和源码闭包仍应独立检索，后续重点是补齐 accepted-token completion 到输出缓冲区物化之间的设备侧依赖链。

## Materials

- [r19 completion-fence 失败证据与后续定位](blocks/f78c9121fba65604a02227c23e7b224ff147fa86fdc743ee98ef9cc93901d5b6.md): 只读核验显示 r19 的 completion fence 未修复 prefix-hit 输出损坏：B2 的 miss 与 warm 阶段正常，但 hit 阶段虽 HTTP 200、accepted/draft 为 896/896，最终 1024 个 token 全为 0、文本全为 `!`，因此 `experiment_valid=false`、`all_passed=false`。服务、四机进程、源码闭包、路由、cache geometry、preemption 与 HTTP 层均未发现失败证据，日志也无 ERROR/FATAL；四机时钟存在偏移，但不能解释结果。当前问题被约束为 accepted-token completion 到最终 host-visible token buffer 物化之间的 stream/event 或 buffer ownership 依赖不足；这仍是定位方向而非已验证根因，后续应保留未完成状态并继续追踪 hit 路径。

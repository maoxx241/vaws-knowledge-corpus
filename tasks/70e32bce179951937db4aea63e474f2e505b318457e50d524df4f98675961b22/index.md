---
blocks:
- block_id: fcb8480fc641c4b24c8038497965245210ab25b409a4b79f4d9d3802eb4c8f08
  indexed: true
  sha256: 082db532e4e00288826dfdf196f29254db9b837cfc32e46d63e6e7c62b3f537e
  source_range:
    end: 420634
    part: 0
    start: 0
  summary: 只读排查聚焦 r4 四机的首个真实报错、启动环境和 MC2 profiling 路径，尚未记录文件或进程变更。当前报告的首错发生在 profile dummy run 调用 `aclnnMoeDistributeDispatchV4` 时：`maxBs=16` 下算子计算需要 214 MB 的 EP HCCL window，但当前配置仅 200 MB；该信息尚不足以确认是 DSpark draft profiling 放大了 token capacity，还是完整权重基线配置本身不合法。
  title: r4 四机 MC2 profiling 首错与容量不匹配
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 70e32bce179951937db4aea63e474f2e505b318457e50d524df4f98675961b22
  kind: experience
  material_digest: 47d9a06a01fb9c8afbec048c0b82cd340c821b77d51d0f53a02d7fe761f7d3ac
  revision: c436950e3f496799359502200d255ab27733bb6081c44de565899dd2ab99d0c5
  schema: mindie-entry/3
  summary: 当前索引记录了 `aclnnMoeDistributeDispatchV4`、`maxBs=16`、EP HCCL window 214 MB 对 200 MB 的首错，以及 DSpark draft profiling 与完整权重基线配置之间尚未解决的归因问题。排查声明为只读，未报告成功修复、配置变更或进程操作；后续应继续追溯 `maxBs=16` 的来源并保留任何失败、修正或未完成结果。
  title: r4 四机 MC2 profiling 容量错误排查
navigation: 当前索引记录了 `aclnnMoeDistributeDispatchV4`、`maxBs=16`、EP HCCL window 214 MB 对 200 MB 的首错，以及 DSpark draft profiling 与完整权重基线配置之间尚未解决的归因问题。排查声明为只读，未报告成功修复、配置变更或进程操作；后续应继续追溯 `maxBs=16` 的来源并保留任何失败、修正或未完成结果。
schema: mindie-material-task/1
status: complete
task_id: 70e32bce179951937db4aea63e474f2e505b318457e50d524df4f98675961b22
---

# r4 四机 MC2 profiling 容量错误排查

当前索引记录了 `aclnnMoeDistributeDispatchV4`、`maxBs=16`、EP HCCL window 214 MB 对 200 MB 的首错，以及 DSpark draft profiling 与完整权重基线配置之间尚未解决的归因问题。排查声明为只读，未报告成功修复、配置变更或进程操作；后续应继续追溯 `maxBs=16` 的来源并保留任何失败、修正或未完成结果。

## Materials

- [r4 四机 MC2 profiling 首错与容量不匹配](blocks/fcb8480fc641c4b24c8038497965245210ab25b409a4b79f4d9d3802eb4c8f08.md): 只读排查聚焦 r4 四机的首个真实报错、启动环境和 MC2 profiling 路径，尚未记录文件或进程变更。当前报告的首错发生在 profile dummy run 调用 `aclnnMoeDistributeDispatchV4` 时：`maxBs=16` 下算子计算需要 214 MB 的 EP HCCL window，但当前配置仅 200 MB；该信息尚不足以确认是 DSpark draft profiling 放大了 token capacity，还是完整权重基线配置本身不合法。

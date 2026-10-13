---
blocks:
- block_id: 633c60d8904726ad8071561456da4aafe5990f608c5df32f68c8428b27202c83
  indexed: true
  sha256: 50be386d1f1ba8a27fb3cdb18f8363b61178003d51d32fcbdeb9fa1b663dafc6
  source_range:
    end: 547529
    part: 0
    start: 0
  summary: 记录称 r2 已按 156→155→154 顺序精确停止，并声称四机读回目标 PG 成员、r2 模型进程及 18125/17659 监听均为 0；失败日志、结果和 DFX 证据被保留，但这些检查结果在本索引中不作独立验证。R21 计划保留 exact request ID、role、q/c 匹配、sync/draft-bypass/postprocess-bypass 等最小干预，删除 group/block/snapshot/metadata/JSON 采集及 trace；不匹配时跳过并由客户端判定实验无效。后续消息称现有 hook seam 足够，运行时匹配或 bypass 前置失败必须 fail-open、不得 raise，并计划裁剪为 10-case intervention matrix 等 UT；最小 diff、CPU UT、ruff 和 py_compile 的完成情况未在材料中确认。
  title: R21 intervention-only 方案与 hook 约束
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e2df8212a23b9ac76f81231b419723aaa12a2c027494fcfc49a633d2ca086313
  kind: experience
  material_digest: e8727f99778c1252c24f3da014fb98bf9eddb9c86dbe4b30b13887784d6c331b
  revision: d37ecf4b21320a81596ab8308d4b4e439f323107ae10ca584aa13fc67561ce4c
  schema: mindie-entry/3
  summary: 已索引的材料集中于 r2 停止闭环及 R21 intervention-only 设计。关键检索词包括 PGID 154–156、四机读回、DFX 证据、exact request ID、role、q/c 匹配、sync、draft bypass、postprocess wrapper bypass、fail-open、10-case intervention matrix；R21 的验证命令与最终完成状态仍未确认。
  title: 当前任务导航：R21 最小干预索引
navigation: 已索引的材料集中于 r2 停止闭环及 R21 intervention-only 设计。关键检索词包括 PGID 154–156、四机读回、DFX 证据、exact request ID、role、q/c 匹配、sync、draft bypass、postprocess wrapper bypass、fail-open、10-case intervention matrix；R21 的验证命令与最终完成状态仍未确认。
schema: mindie-material-task/1
status: complete
task_id: e2df8212a23b9ac76f81231b419723aaa12a2c027494fcfc49a633d2ca086313
---

# 当前任务导航：R21 最小干预索引

已索引的材料集中于 r2 停止闭环及 R21 intervention-only 设计。关键检索词包括 PGID 154–156、四机读回、DFX 证据、exact request ID、role、q/c 匹配、sync、draft bypass、postprocess wrapper bypass、fail-open、10-case intervention matrix；R21 的验证命令与最终完成状态仍未确认。

## Materials

- [R21 intervention-only 方案与 hook 约束](blocks/633c60d8904726ad8071561456da4aafe5990f608c5df32f68c8428b27202c83.md): 记录称 r2 已按 156→155→154 顺序精确停止，并声称四机读回目标 PG 成员、r2 模型进程及 18125/17659 监听均为 0；失败日志、结果和 DFX 证据被保留，但这些检查结果在本索引中不作独立验证。R21 计划保留 exact request ID、role、q/c 匹配、sync/draft-bypass/postprocess-bypass 等最小干预，删除 group/block/snapshot/metadata/JSON 采集及 trace；不匹配时跳过并由客户端判定实验无效。后续消息称现有 hook seam 足够，运行时匹配或 bypass 前置失败必须 fail-open、不得 raise，并计划裁剪为 10-case intervention matrix 等 UT；最小 diff、CPU UT、ruff 和 py_compile 的完成情况未在材料中确认。

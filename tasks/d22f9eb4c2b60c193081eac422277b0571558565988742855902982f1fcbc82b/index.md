---
blocks:
- block_id: 00bfd3a49e282b6ef9ac722facac038ef38ccc1da4bf071e05eba996d997ef15
  indexed: true
  sha256: 375e3b1e88c24e6ff13720ece7031ff37029c6015e06a63c6a4de1923668b567
  source_range:
    end: 2746199
    part: 0
    start: 0
  summary: 只读审计将当前最强候选指向 DSpark 多请求 batch reorder 边界移植错误：临时分支把 dspark 的 `reorder_batch_threshold` 设为 `1 + N = 8`，并保留了旧版 backend 阈值相等 override；主线修复 `39d1cf9b3352f3f5b4571894295e64ca515b8ffb` 要求 dflash 使用 `1 + N`、dspark 使用 `N = 7`，runner 删除旧 override。该问题可能使普通 qlen1 decode 与 DSpark qlen8 verifier 进入同一 decode region，造成高并发、请求收缩或 survivor reorder 时的 spec/non-spec 边界失配，但仍属候选原因而非已验证成功的修复。其他检查未确认 causal conv1d 原地写、ACLGraph 写冲突或 prefix-cache 共享块并发写；124→128 padding 被判断为重复 terminal 的零长度 row，sampling 事件时序风险也尚未形成具体写冲突证据。
  title: DSpark GDN reorder 边界审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d22f9eb4c2b60c193081eac422277b0571558565988742855902982f1fcbc82b
  kind: experience
  material_digest: d563871ab9cad806c8f82e9629614073929ca0a3c5d84ba56e9d9232d5a12eb3
  revision: 8c4161c0bc98ea7e6398e61d602f3493a24be6c5edbb37aa747414824a5473d1
  schema: mindie-entry/3
  summary: 重点追踪 DSpark GDN 的 `reorder_batch_threshold` 配置与旧 backend-equal override；主线修复要求 dflash=`1 + N`、dspark=`N = 7`，建议以精确 diff 加三个相关 UT 做最小 A/B，但结果尚未验证。保留其他未决方向：若 reorder A/B 无效，再评估将 `sampling_done_event.record()` 延后至 draft proposal、draft D2H 和 connector finalize 之后；现有审计还记录了 early `num_accepted_tokens_event`、draft/target 独立 KV/state group，以及 124→128 FULL padding 的零长度 terminal row。
  title: 当前任务导航：DSpark reorder 边界候选
navigation: 重点追踪 DSpark GDN 的 `reorder_batch_threshold` 配置与旧 backend-equal override；主线修复要求 dflash=`1 + N`、dspark=`N = 7`，建议以精确 diff 加三个相关 UT 做最小 A/B，但结果尚未验证。保留其他未决方向：若 reorder A/B 无效，再评估将 `sampling_done_event.record()` 延后至 draft proposal、draft D2H 和 connector finalize 之后；现有审计还记录了 early `num_accepted_tokens_event`、draft/target 独立 KV/state group，以及 124→128 FULL padding 的零长度 terminal row。
schema: mindie-material-task/1
status: complete
task_id: d22f9eb4c2b60c193081eac422277b0571558565988742855902982f1fcbc82b
---

# 当前任务导航：DSpark reorder 边界候选

重点追踪 DSpark GDN 的 `reorder_batch_threshold` 配置与旧 backend-equal override；主线修复要求 dflash=`1 + N`、dspark=`N = 7`，建议以精确 diff 加三个相关 UT 做最小 A/B，但结果尚未验证。保留其他未决方向：若 reorder A/B 无效，再评估将 `sampling_done_event.record()` 延后至 draft proposal、draft D2H 和 connector finalize 之后；现有审计还记录了 early `num_accepted_tokens_event`、draft/target 独立 KV/state group，以及 124→128 FULL padding 的零长度 terminal row。

## Materials

- [DSpark GDN reorder 边界审计](blocks/00bfd3a49e282b6ef9ac722facac038ef38ccc1da4bf071e05eba996d997ef15.md): 只读审计将当前最强候选指向 DSpark 多请求 batch reorder 边界移植错误：临时分支把 dspark 的 `reorder_batch_threshold` 设为 `1 + N = 8`，并保留了旧版 backend 阈值相等 override；主线修复 `39d1cf9b3352f3f5b4571894295e64ca515b8ffb` 要求 dflash 使用 `1 + N`、dspark 使用 `N = 7`，runner 删除旧 override。该问题可能使普通 qlen1 decode 与 DSpark qlen8 verifier 进入同一 decode region，造成高并发、请求收缩或 survivor reorder 时的 spec/non-spec 边界失配，但仍属候选原因而非已验证成功的修复。其他检查未确认 causal conv1d 原地写、ACLGraph 写冲突或 prefix-cache 共享块并发写；124→128 padding 被判断为重复 terminal 的零长度 row，sampling 事件时序风险也尚未形成具体写冲突证据。

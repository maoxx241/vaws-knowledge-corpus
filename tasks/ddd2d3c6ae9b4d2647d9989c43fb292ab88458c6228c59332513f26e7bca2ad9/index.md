---
blocks:
- block_id: af24e8e3768036fdbc216dfc8297f837899489b02003ad05d186ece3a66457b8
  indexed: true
  sha256: d2c7931fc1f838a674d0b17cdcd42288e726faa5c8d39b28069c99740c6efec6
  source_range:
    end: 11618116
    part: 0
    start: 0
  summary: '记录了本地知识/经验分目录检索实现、`knowledge_query`/`experience_query` 入口及相关测试结果；同时整理并冻结了公共经验稿，部分来源因私有性被保留本地，且尚未切换环境、发布或合并。后续 owner 更新、经验日期与哈希维护、50 篇保留范围判断，以及 Operators/Kimi B 深度复审均有记录；复核中补回了量化公式、scale/cache 条件、整数排序边界、图回放与新 shape 编译、reload 地址、采样 OOM 和 prefix 命中等细节，但明确未将局部检查提升为整模结论，且未重新运行 NPU。另有 distribution schema `/2`、`content.layout=kinds/v1` 和分类目录查询改动，以及 consumer PR #150（SHA `11ab572…`）的提交记录；这些材料仍包含未 merge、未发布和部分未闭环状态。'
  title: 本地检索分层与经验材料整理复核
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ddd2d3c6ae9b4d2647d9989c43fb292ab88458c6228c59332513f26e7bca2ad9
  kind: experience
  material_digest: 538ddb8043e881c5472499ddc89576bdb6fb6b9ac45af1fe05e1123eca9cd167
  revision: f64d918d2eaaf0df33cb231992daa52a6d8f9ede34f5f5bdb04ca91e71b45fdd
  schema: mindie-entry/3
  summary: 当前材料覆盖本地知识/经验双存储、按分类目录检索、经验稿汇总冻结、owner 更新及 consumer PR 提交；私有来源、未发布/未合并状态和后续 corrections 均需保留。重点可检索项包括 OP/Kimi 经验范围、量化与缓存实现细节、schema `/2` 兼容约束、证据坐标/哈希、独立复审结果，以及尚未完成的发布、环境切换和 NPU 验证。
  title: 任务导航：经验索引、分层发布与证据复核
navigation: 当前材料覆盖本地知识/经验双存储、按分类目录检索、经验稿汇总冻结、owner 更新及 consumer PR 提交；私有来源、未发布/未合并状态和后续 corrections 均需保留。重点可检索项包括 OP/Kimi 经验范围、量化与缓存实现细节、schema `/2` 兼容约束、证据坐标/哈希、独立复审结果，以及尚未完成的发布、环境切换和 NPU 验证。
schema: mindie-material-task/1
status: complete
task_id: ddd2d3c6ae9b4d2647d9989c43fb292ab88458c6228c59332513f26e7bca2ad9
---

# 任务导航：经验索引、分层发布与证据复核

当前材料覆盖本地知识/经验双存储、按分类目录检索、经验稿汇总冻结、owner 更新及 consumer PR 提交；私有来源、未发布/未合并状态和后续 corrections 均需保留。重点可检索项包括 OP/Kimi 经验范围、量化与缓存实现细节、schema `/2` 兼容约束、证据坐标/哈希、独立复审结果，以及尚未完成的发布、环境切换和 NPU 验证。

## Materials

- [本地检索分层与经验材料整理复核](blocks/af24e8e3768036fdbc216dfc8297f837899489b02003ad05d186ece3a66457b8.md): 记录了本地知识/经验分目录检索实现、`knowledge_query`/`experience_query` 入口及相关测试结果；同时整理并冻结了公共经验稿，部分来源因私有性被保留本地，且尚未切换环境、发布或合并。后续 owner 更新、经验日期与哈希维护、50 篇保留范围判断，以及 Operators/Kimi B 深度复审均有记录；复核中补回了量化公式、scale/cache 条件、整数排序边界、图回放与新 shape 编译、reload 地址、采样 OOM 和 prefix 命中等细节，但明确未将局部检查提升为整模结论，且未重新运行 NPU。另有 distribution schema `/2`、`content.layout=kinds/v1` 和分类目录查询改动，以及 consumer PR #150（SHA `11ab572…`）的提交记录；这些材料仍包含未 merge、未发布和部分未闭环状态。

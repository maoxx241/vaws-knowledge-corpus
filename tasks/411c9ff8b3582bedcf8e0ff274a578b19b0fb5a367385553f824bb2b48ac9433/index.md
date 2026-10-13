---
blocks:
- block_id: cd5934dd132455de1335cc105a9db613516cbfa338d8a086986af7402f876f5d
  indexed: true
  sha256: e5450b294f72056d31327210854983dd173f054e9e6ef0f1aa339abe656d970d
  source_range:
    end: 212620
    part: 0
    start: 0
  summary: '记录称已定位 `vllm_ascend/ops/triton/compute_slot_mapping.py`，并将 `_compute_slot_mapping_kernel` 的引入归因于 GitHub 用户 `ckeyever`，commit 签名身份为 `ckey_Dou <<redacted:email-address:dcdeb78aa4f7>>`。该记录引用 PR #13048、提交 `f9c2ecbd039db040e95656dde647a224b8427ef1` 及 2026-07-29/2026-08-04 的时间信息，并声称这是 Ascend 本地优化版算子的首次加入；这些内容属于报告性溯源结果，未在本索引中独立验证。'
  title: compute_slot_mapping_kernel 来源追溯
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 411c9ff8b3582bedcf8e0ff274a578b19b0fb5a367385553f824bb2b48ac9433
  kind: experience
  material_digest: 1f79a5db3276b705487df3a71ff324d1d56934026ddbb2d3995e7e05e6361671
  revision: 5ff059aef82fbdb67045a2eec667d2c77d4e13dfe78cf65cd545d26d2afcebf9
  schema: mindie-entry/3
  summary: '目前仅有一条可检索材料：围绕 `compute_slot_mapping_kernel` 的引入来源、实现文件、PR #13048、提交 `f9c2ecbd` 及相关作者归因。该结论保留为 fallible reference index；原始材料仍应独立检索，且没有更早导航或后续修正可供合并。'
  title: 当前任务导航
navigation: '目前仅有一条可检索材料：围绕 `compute_slot_mapping_kernel` 的引入来源、实现文件、PR #13048、提交 `f9c2ecbd` 及相关作者归因。该结论保留为 fallible reference index；原始材料仍应独立检索，且没有更早导航或后续修正可供合并。'
schema: mindie-material-task/1
status: complete
task_id: 411c9ff8b3582bedcf8e0ff274a578b19b0fb5a367385553f824bb2b48ac9433
---

# 当前任务导航

目前仅有一条可检索材料：围绕 `compute_slot_mapping_kernel` 的引入来源、实现文件、PR #13048、提交 `f9c2ecbd` 及相关作者归因。该结论保留为 fallible reference index；原始材料仍应独立检索，且没有更早导航或后续修正可供合并。

## Materials

- [compute_slot_mapping_kernel 来源追溯](blocks/cd5934dd132455de1335cc105a9db613516cbfa338d8a086986af7402f876f5d.md): 记录称已定位 `vllm_ascend/ops/triton/compute_slot_mapping.py`，并将 `_compute_slot_mapping_kernel` 的引入归因于 GitHub 用户 `ckeyever`，commit 签名身份为 `ckey_Dou <<redacted:email-address:dcdeb78aa4f7>>`。该记录引用 PR #13048、提交 `f9c2ecbd039db040e95656dde647a224b8427ef1` 及 2026-07-29/2026-08-04 的时间信息，并声称这是 Ascend 本地优化版算子的首次加入；这些内容属于报告性溯源结果，未在本索引中独立验证。

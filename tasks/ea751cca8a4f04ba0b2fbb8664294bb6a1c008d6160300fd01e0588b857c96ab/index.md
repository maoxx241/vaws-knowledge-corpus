---
blocks:
- block_id: d4af58d7de906859e83fe6890320812a50c9948d62e1691cfaad3f11ae5b933d
  indexed: true
  sha256: f21a7308efff47487741ae2c63fd955e4030a3dd2713a49a5bef18c6622b7f1b
  source_range:
    end: 2074868
    part: 0
    start: 0
  summary: 核实了 scheduler 的 lookahead 会决定物理 KV page 数，MRV1 的 MultiGroupBlockTable 接收新 block ID；固定 C=720 时 lookahead=7 与 14 落在同一页，但通用请求在 C mod 128 为 115–121 时需要 14 才能预留下一页。block 0 虽是 null/zero page，batch move_row 后未清理的源行可能使 add_row 的未分配尾列残留旧 block ID，因此建议 proposer 将有效长度改为 C+14、对 C+7..C+13 显式写零，并在对应 legacy A/B 开关下将 BalanceScheduler 的 lookahead 从 7 调为 14。建议修改 dspark_proposer.py 与调度器并补充 proposer、schedule 和边界 UT；当前仅完成只读核对，未修改 dirty worktree，通用修复和固定 720-token 单文件实验需区分。
  title: DSpark safe KV lookahead 与尾部清零核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ea751cca8a4f04ba0b2fbb8664294bb6a1c008d6160300fd01e0588b857c96ab
  kind: experience
  material_digest: bb635ce70457d6695b04089f9c6eddbf667269a2e0c49f7fa28ef85fab0b505e
  revision: bf59edb8882c18ca45161da974cfbad2d4572704c15292cf6efa3dfa10b571a1
  schema: mindie-entry/3
  summary: 本批材料记录了 DSpark/MRV1 中 safe KV=734 的分析：保持 7 个 query token，同时让 draft FIA 看到 C+14，并将 C+7..C+13 的 K/V 显式清零。关键未决/风险包括通用请求的 block-table 尾列残留、lookahead=14 的边界必要性，以及 proposer、BalanceScheduler 和相关 UT 的具体改动；结论来自只读核对，尚未验证为已实施或成功。
  title: 当前任务导航：DSpark safe KV lookahead 索引
navigation: 本批材料记录了 DSpark/MRV1 中 safe KV=734 的分析：保持 7 个 query token，同时让 draft FIA 看到 C+14，并将 C+7..C+13 的 K/V 显式清零。关键未决/风险包括通用请求的 block-table 尾列残留、lookahead=14 的边界必要性，以及 proposer、BalanceScheduler 和相关 UT 的具体改动；结论来自只读核对，尚未验证为已实施或成功。
schema: mindie-material-task/1
status: complete
task_id: ea751cca8a4f04ba0b2fbb8664294bb6a1c008d6160300fd01e0588b857c96ab
---

# 当前任务导航：DSpark safe KV lookahead 索引

本批材料记录了 DSpark/MRV1 中 safe KV=734 的分析：保持 7 个 query token，同时让 draft FIA 看到 C+14，并将 C+7..C+13 的 K/V 显式清零。关键未决/风险包括通用请求的 block-table 尾列残留、lookahead=14 的边界必要性，以及 proposer、BalanceScheduler 和相关 UT 的具体改动；结论来自只读核对，尚未验证为已实施或成功。

## Materials

- [DSpark safe KV lookahead 与尾部清零核对](blocks/d4af58d7de906859e83fe6890320812a50c9948d62e1691cfaad3f11ae5b933d.md): 核实了 scheduler 的 lookahead 会决定物理 KV page 数，MRV1 的 MultiGroupBlockTable 接收新 block ID；固定 C=720 时 lookahead=7 与 14 落在同一页，但通用请求在 C mod 128 为 115–121 时需要 14 才能预留下一页。block 0 虽是 null/zero page，batch move_row 后未清理的源行可能使 add_row 的未分配尾列残留旧 block ID，因此建议 proposer 将有效长度改为 C+14、对 C+7..C+13 显式写零，并在对应 legacy A/B 开关下将 BalanceScheduler 的 lookahead 从 7 调为 14。建议修改 dspark_proposer.py 与调度器并补充 proposer、schedule 和边界 UT；当前仅完成只读核对，未修改 dirty worktree，通用修复和固定 720-token 单文件实验需区分。

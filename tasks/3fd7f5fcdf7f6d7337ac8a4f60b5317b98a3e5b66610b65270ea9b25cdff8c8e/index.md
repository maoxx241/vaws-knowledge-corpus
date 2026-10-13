---
blocks:
- block_id: 276f64259d383d62909cd3111214190b142cc8d3375b9a162dd0179f5e7f1bfd
  indexed: true
  sha256: cf8706bb4e467e3539cc6c4da18439dbb9c580a1f0064a3c00da6193e36aaf63
  source_range:
    end: 2806087
    part: 0
    start: 0
  summary: 本块记录一次只读静态追踪：将 scheduler 判断的命中块与各 hybrid group 实际交给 worker 的 block table 对齐，并核对 warm768→hit896 的 hash/cache/free、Mamba 运行几何及新增 MLA scheduler→kernel 映射。当前观察到的最明确契约缺口是最终统一 hit=384 后仅裁剪第一个 FullAttention 组，可能污染 draft GQA 的 cached block1；但该现象是否足以解释 target 首 token 仍需经 r14 logits/cache DFX 分流，尚未验证。
  title: warm768→hit896 缓存块契约静态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3fd7f5fcdf7f6d7337ac8a4f60b5317b98a3e5b66610b65270ea9b25cdff8c8e
  kind: experience
  material_digest: 7344706e30f5e374c89fbdba0a4b927bf25fa07ba59e18d26de993e38d139cb6
  revision: 44f9eb10d54f110e9f4e0288dcba2f2b7dd6b1ec84ce7091aa4bbced77dda4bf
  schema: mindie-entry/3
  summary: 已收录 warm768→hit896 的只读静态审计，重点检索 scheduler 命中长度、hybrid group worker block table、物理块到逻辑页/slot 转换，以及“统一 hit=384 后只裁第一个 FullAttention 组”的不确定契约缺口。该线索仍是未验证的因果假设；target 首 token 与 r14 logits/cache DFX 分流之间的解释尚未完成，新增 MLA 证据仅涉及字段消费和映射核对。
  title: 当前任务导航：缓存恢复块表与 MLA 映射审计
navigation: 已收录 warm768→hit896 的只读静态审计，重点检索 scheduler 命中长度、hybrid group worker block table、物理块到逻辑页/slot 转换，以及“统一 hit=384 后只裁第一个 FullAttention 组”的不确定契约缺口。该线索仍是未验证的因果假设；target 首 token 与 r14 logits/cache DFX 分流之间的解释尚未完成，新增 MLA 证据仅涉及字段消费和映射核对。
schema: mindie-material-task/1
status: complete
task_id: 3fd7f5fcdf7f6d7337ac8a4f60b5317b98a3e5b66610b65270ea9b25cdff8c8e
---

# 当前任务导航：缓存恢复块表与 MLA 映射审计

已收录 warm768→hit896 的只读静态审计，重点检索 scheduler 命中长度、hybrid group worker block table、物理块到逻辑页/slot 转换，以及“统一 hit=384 后只裁第一个 FullAttention 组”的不确定契约缺口。该线索仍是未验证的因果假设；target 首 token 与 r14 logits/cache DFX 分流之间的解释尚未完成，新增 MLA 证据仅涉及字段消费和映射核对。

## Materials

- [warm768→hit896 缓存块契约静态审计](blocks/276f64259d383d62909cd3111214190b142cc8d3375b9a162dd0179f5e7f1bfd.md): 本块记录一次只读静态追踪：将 scheduler 判断的命中块与各 hybrid group 实际交给 worker 的 block table 对齐，并核对 warm768→hit896 的 hash/cache/free、Mamba 运行几何及新增 MLA scheduler→kernel 映射。当前观察到的最明确契约缺口是最终统一 hit=384 后仅裁剪第一个 FullAttention 组，可能污染 draft GQA 的 cached block1；但该现象是否足以解释 target 首 token 仍需经 r14 logits/cache DFX 分流，尚未验证。

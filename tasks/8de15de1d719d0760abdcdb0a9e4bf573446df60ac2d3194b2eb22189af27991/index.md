---
blocks:
- block_id: 16415a9a08bc8d7317fe4aaa05c4967ee08a1c459797b1d787f2623cb1ead346
  indexed: true
  sha256: 767d4a84507ef1bd36163e016edf8076e65aa717ffd8e77c00cded5fdfed3e54
  source_range:
    end: 2370344
    part: 0
    start: 0
  summary: 材料记录了跨 TP `device_index` 语义比较的潜在 fail-closed 风险，以及因果扫描在 `hit896` lookup 处提前停止、漏检后续 `allocate_slots_after` allocation 的确定性 blocker。随后据称仅修改 bundle verifier、反例单测和校验清单，将 hit touch 唯一化并扩大扫描上界；报告称 8 个 standalone 测试通过，新反例命中 `block_hash_owner_changed`，并列出 `py_compile`、`bash -n`、checksum 与 SHA 结果，但这些均保留为来源方报告，未独立核验。
  title: Verifier 因果扫描修复与自报验证结果
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8de15de1d719d0760abdcdb0a9e4bf573446df60ac2d3194b2eb22189af27991
  kind: experience
  material_digest: 4b9516a11e5587785147d1be78ae87c2842e1a19da72f9680dc19235d0220f29
  revision: eed59d28ec283751c709c5e40ec96a947095e227061d26cc26a1c17b99c8edf9
  schema: mindie-entry/3
  summary: 核心线索是 partial-prefill 状态机、warm owner/reuse、publish 后扫描，以及跨 TP `device_index` 比较的潜在误报风险。关键 blocker 是漏检 `allocate_slots_after` 导致破坏可能被错误归因于 device seam；后续材料报告了 verifier-only 修正、反例覆盖和多项校验结果，但仍应将其视为待核验的自报结论。
  title: 当前任务导航：Verifier 因果扫描修复
navigation: 核心线索是 partial-prefill 状态机、warm owner/reuse、publish 后扫描，以及跨 TP `device_index` 比较的潜在误报风险。关键 blocker 是漏检 `allocate_slots_after` 导致破坏可能被错误归因于 device seam；后续材料报告了 verifier-only 修正、反例覆盖和多项校验结果，但仍应将其视为待核验的自报结论。
schema: mindie-material-task/1
status: complete
task_id: 8de15de1d719d0760abdcdb0a9e4bf573446df60ac2d3194b2eb22189af27991
---

# 当前任务导航：Verifier 因果扫描修复

核心线索是 partial-prefill 状态机、warm owner/reuse、publish 后扫描，以及跨 TP `device_index` 比较的潜在误报风险。关键 blocker 是漏检 `allocate_slots_after` 导致破坏可能被错误归因于 device seam；后续材料报告了 verifier-only 修正、反例覆盖和多项校验结果，但仍应将其视为待核验的自报结论。

## Materials

- [Verifier 因果扫描修复与自报验证结果](blocks/16415a9a08bc8d7317fe4aaa05c4967ee08a1c459797b1d787f2623cb1ead346.md): 材料记录了跨 TP `device_index` 语义比较的潜在 fail-closed 风险，以及因果扫描在 `hit896` lookup 处提前停止、漏检后续 `allocate_slots_after` allocation 的确定性 blocker。随后据称仅修改 bundle verifier、反例单测和校验清单，将 hit touch 唯一化并扩大扫描上界；报告称 8 个 standalone 测试通过，新反例命中 `block_hash_owner_changed`，并列出 `py_compile`、`bash -n`、checksum 与 SHA 结果，但这些均保留为来源方报告，未独立核验。

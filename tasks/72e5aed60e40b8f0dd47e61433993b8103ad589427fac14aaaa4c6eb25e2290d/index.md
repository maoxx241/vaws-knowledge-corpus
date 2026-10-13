---
blocks:
- block_id: 0b00b3dd8aac195c759fd0f708f59321868ade3ec4d46bee9e9df79a75848b2d
  indexed: true
  sha256: 8c5f076957e36405388a0d38df4caef19767ca1444c9cfe3c75970d27dd0e035
  source_range:
    end: 679132
    part: 0
    start: 0
  summary: 记录者称已限定修改 3 个 NPU 测试文件，移除若干算子的 `hasattr`/skip 守卫，并保留 Ascend 950 硬件条件 skip；目标硬件上算子未注册据称会直接失败。其报告 `git diff --check`、3 个文件 Python `compile()` 及 `_C_ascend` 守卫搜索通过，但这些结果在本索引中未独立验证。该记录还明确未修改 `test_fused_moe.py`，并保留了平台不匹配、算子未构建或未注册等失败条件。
  title: NPU 测试守卫移除与静态检查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 72e5aed60e40b8f0dd47e61433993b8103ad589427fac14aaaa4c6eb25e2290d
  kind: experience
  material_digest: 372a0641bdd943525aa480d85ff2b6c2ce9ecdaf113c3a7c0c65b72c74721c42
  revision: 8c08b4edc13db0dc54edde4953e6253f2c56c4afacfe49d1e5ffc750b0716b49
  schema: mindie-entry/3
  summary: 当前批次包含 1 个材料块，主题是 3 个 NPU 测试文件中算子注册检查与 skip 守卫的调整，以及静态检查结果。记录声称修改和检查已完成，但仍属于未独立验证的报告；后续检索应关注 Ascend 950 平台条件、算子注册失败、shape/dtype、私有实现耦合，以及未修改的 `test_fused_moe.py`。
  title: 当前任务导航：NPU 测试守卫审查
navigation: 当前批次包含 1 个材料块，主题是 3 个 NPU 测试文件中算子注册检查与 skip 守卫的调整，以及静态检查结果。记录声称修改和检查已完成，但仍属于未独立验证的报告；后续检索应关注 Ascend 950 平台条件、算子注册失败、shape/dtype、私有实现耦合，以及未修改的 `test_fused_moe.py`。
schema: mindie-material-task/1
status: complete
task_id: 72e5aed60e40b8f0dd47e61433993b8103ad589427fac14aaaa4c6eb25e2290d
---

# 当前任务导航：NPU 测试守卫审查

当前批次包含 1 个材料块，主题是 3 个 NPU 测试文件中算子注册检查与 skip 守卫的调整，以及静态检查结果。记录声称修改和检查已完成，但仍属于未独立验证的报告；后续检索应关注 Ascend 950 平台条件、算子注册失败、shape/dtype、私有实现耦合，以及未修改的 `test_fused_moe.py`。

## Materials

- [NPU 测试守卫移除与静态检查记录](blocks/0b00b3dd8aac195c759fd0f708f59321868ade3ec4d46bee9e9df79a75848b2d.md): 记录者称已限定修改 3 个 NPU 测试文件，移除若干算子的 `hasattr`/skip 守卫，并保留 Ascend 950 硬件条件 skip；目标硬件上算子未注册据称会直接失败。其报告 `git diff --check`、3 个文件 Python `compile()` 及 `_C_ascend` 守卫搜索通过，但这些结果在本索引中未独立验证。该记录还明确未修改 `test_fused_moe.py`，并保留了平台不匹配、算子未构建或未注册等失败条件。

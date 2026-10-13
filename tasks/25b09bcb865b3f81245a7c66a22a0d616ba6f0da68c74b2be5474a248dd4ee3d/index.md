---
blocks:
- block_id: c30177d1a93bdb77851132a2169128160668311dddd287775c2bb2bcfa15c8a0
  indexed: true
  sha256: 9bae636f8cc41675c182640efcb7e4c9855b8243fd90518727660683a388c37a
  source_range:
    end: 1243347
    part: 0
    start: 0
  summary: '目标 session 未明确证明最后启动的服务仍在；后续核验发现四节点运行的是端口 18125 的 r21 实例，no-hit GSM8K 并发为 c8 精度 8/8、采信率 65.8922%，c16 精度 15/16、采信率 63.5070%，但 hit 场景输出全部退化为 `!`，实际命中少于预期，不能视为质量通过。另有一次因缺少 `K3_PREFIX_BASE_RUNNER` 环境变量而在请求前失败；随后从 PR #13277 的 `model_runner_v1.py` DSpark accepted-token 分支移除全局 `torch.npu.synchronize()`，完成 AST 和 diff 检查并推送 commit `55c0802da`，但该批材料未提供远端 CI 或功能回归成功证据。'
  title: Prefix Cache 并发验证与 PR 同步移除
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 25b09bcb865b3f81245a7c66a22a0d616ba6f0da68c74b2be5474a248dd4ee3d
  kind: experience
  material_digest: bd5a5710b3c010c54c764055075a11417c68a19db635ce7656f91ed5a9e98ead
  revision: c867b504c19fd7948154eebed2008c888019a22a862610e7c40f8f3ee51008e0
  schema: mindie-entry/3
  summary: '优先检索 Prefix Cache 的 no-hit/hit 并发结果、`prefix_cache_hits_total`、采信率、GSM8K 精度及 `!` 退化输出；命中路径仍应标记为失败或未修复，不能用 100% speculative 采信率替代正确性。另检索 PR #13277 的 `model_runner_v1.py` 同步删除、commit `55c0802da`、本地检查通过与缺少回归验证之间的区别；此前服务身份、命中预期与实际命中存在修正和不确定性，原始块仍需独立搜索。'
  title: '当前任务导航：Prefix Cache 结果与 PR #13277 修改'
navigation: '优先检索 Prefix Cache 的 no-hit/hit 并发结果、`prefix_cache_hits_total`、采信率、GSM8K 精度及 `!` 退化输出；命中路径仍应标记为失败或未修复，不能用 100% speculative 采信率替代正确性。另检索 PR #13277 的 `model_runner_v1.py` 同步删除、commit `55c0802da`、本地检查通过与缺少回归验证之间的区别；此前服务身份、命中预期与实际命中存在修正和不确定性，原始块仍需独立搜索。'
schema: mindie-material-task/1
status: complete
task_id: 25b09bcb865b3f81245a7c66a22a0d616ba6f0da68c74b2be5474a248dd4ee3d
---

# 当前任务导航：Prefix Cache 结果与 PR #13277 修改

优先检索 Prefix Cache 的 no-hit/hit 并发结果、`prefix_cache_hits_total`、采信率、GSM8K 精度及 `!` 退化输出；命中路径仍应标记为失败或未修复，不能用 100% speculative 采信率替代正确性。另检索 PR #13277 的 `model_runner_v1.py` 同步删除、commit `55c0802da`、本地检查通过与缺少回归验证之间的区别；此前服务身份、命中预期与实际命中存在修正和不确定性，原始块仍需独立搜索。

## Materials

- [Prefix Cache 并发验证与 PR 同步移除](blocks/c30177d1a93bdb77851132a2169128160668311dddd287775c2bb2bcfa15c8a0.md): 目标 session 未明确证明最后启动的服务仍在；后续核验发现四节点运行的是端口 18125 的 r21 实例，no-hit GSM8K 并发为 c8 精度 8/8、采信率 65.8922%，c16 精度 15/16、采信率 63.5070%，但 hit 场景输出全部退化为 `!`，实际命中少于预期，不能视为质量通过。另有一次因缺少 `K3_PREFIX_BASE_RUNNER` 环境变量而在请求前失败；随后从 PR #13277 的 `model_runner_v1.py` DSpark accepted-token 分支移除全局 `torch.npu.synchronize()`，完成 AST 和 diff 检查并推送 commit `55c0802da`，但该批材料未提供远端 CI 或功能回归成功证据。

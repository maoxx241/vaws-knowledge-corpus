---
blocks:
- block_id: 2670250141a7c8b686e8a5a8bf900614b0666b085fea00286e715ea190c37da7
  indexed: true
  sha256: 0a1b197ced985307bd7cf7ad6ff8abbeb22b8222e4c0f0c5ecbb958f705fd7b6
  source_range:
    end: 1664647
    part: 0
    start: 0
  summary: '针对 PR #12950 新 head `38ad149f` 的 CI 检查显示，记录中将唯一实质失败归因于 A2 one-card part 3/5：第一轮 Qwen3-8B W8A8/Eagle3 推理后旧 EngineCore 约占 56 GiB，第二轮仅剩 4.76 GiB并触发显存门禁；`ci-gate` 被描述为汇总结果。该记录指出 PR 未修改相关测试或 `VllmRunner` 清理逻辑，且相邻旧矩阵 `437c38cb` 曾通过，但这些观察不足以验证根因，后续提出仅重跑失败 job、暂不改代码；结果未记录。'
  title: 'PR #12950 CI：A2 显存清理失败排查'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: fa398462ec3ffdafa432681a6cd616ace4031da4151303b0fd75d8440a6ff566
  kind: experience
  material_digest: 9f98557685bc49a70907b5b6a0c8e56099cda15dfcd06da63dc890c07620d9b8
  revision: 00f5d3f70b4d9c77baceff1d1f096a70e813c2f8586416cd0f94a6b6a4361614
  schema: mindie-entry/3
  summary: '已索引 PR #12950 新 head `38ad149f` 的 CI 失败记录，重点检索 A2 one-card part 3/5、EngineCore 显存占用、4.76 GiB 显存门禁、Qwen3-8B W8A8/Eagle3、`ci-gate` 与 `VllmRunner` 清理逻辑。记录中的“偶发 runner 问题”及建议重跑属于排查判断/未完成后续，不应视为已验证修复；旧 head `93b073de` 的排队状态和 `437c38cb` 的通过结果仅作为上下文。'
  title: '当前任务导航：PR #12950 CI 排查'
navigation: '已索引 PR #12950 新 head `38ad149f` 的 CI 失败记录，重点检索 A2 one-card part 3/5、EngineCore 显存占用、4.76 GiB 显存门禁、Qwen3-8B W8A8/Eagle3、`ci-gate` 与 `VllmRunner` 清理逻辑。记录中的“偶发 runner 问题”及建议重跑属于排查判断/未完成后续，不应视为已验证修复；旧 head `93b073de` 的排队状态和 `437c38cb` 的通过结果仅作为上下文。'
schema: mindie-material-task/1
status: complete
task_id: fa398462ec3ffdafa432681a6cd616ace4031da4151303b0fd75d8440a6ff566
---

# 当前任务导航：PR #12950 CI 排查

已索引 PR #12950 新 head `38ad149f` 的 CI 失败记录，重点检索 A2 one-card part 3/5、EngineCore 显存占用、4.76 GiB 显存门禁、Qwen3-8B W8A8/Eagle3、`ci-gate` 与 `VllmRunner` 清理逻辑。记录中的“偶发 runner 问题”及建议重跑属于排查判断/未完成后续，不应视为已验证修复；旧 head `93b073de` 的排队状态和 `437c38cb` 的通过结果仅作为上下文。

## Materials

- [PR #12950 CI：A2 显存清理失败排查](blocks/2670250141a7c8b686e8a5a8bf900614b0666b085fea00286e715ea190c37da7.md): 针对 PR #12950 新 head `38ad149f` 的 CI 检查显示，记录中将唯一实质失败归因于 A2 one-card part 3/5：第一轮 Qwen3-8B W8A8/Eagle3 推理后旧 EngineCore 约占 56 GiB，第二轮仅剩 4.76 GiB并触发显存门禁；`ci-gate` 被描述为汇总结果。该记录指出 PR 未修改相关测试或 `VllmRunner` 清理逻辑，且相邻旧矩阵 `437c38cb` 曾通过，但这些观察不足以验证根因，后续提出仅重跑失败 job、暂不改代码；结果未记录。

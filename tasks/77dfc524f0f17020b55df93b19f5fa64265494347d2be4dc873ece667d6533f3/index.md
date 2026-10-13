---
blocks:
- block_id: 9171a45131d8375c9c22a140c6dc460080d9ac154c5905cad2ebe08d708e7446
  indexed: true
  sha256: b21f20b9cfc4a64296fed92177f6a21b80d612e0f2e0943ca95c31bd9d796169
  source_range:
    end: 574021
    part: 0
    start: 0
  summary: 用户质疑 ci-new 分支遇到的问题可能与 DSpark 特性无关，并要求先验证完整权重，再分析 5 层 P/D 分离；此前还提到截断权重目录缺少 `tiktoken.model`，计划通过显式 tokenizer 覆盖避免无关启动失败。记录的 r4 检查显示完整 target 与 DSpark draft 均加载成功，但在 profiling 的 `_dummy_run` 阶段，四机一致触发 MoE dispatch tiling 错误 561002（`Get WinSize failed`）；`/health` 未就绪，154/155/156 根 PGID 已退出，153 PGID `3200653` 仍存活，且未发送推理请求。该结果是已观察到的启动失败记录，不应视为 DSpark 因果关系或完整权重已验证成功；日志据称保留完整。
  title: r4 完整权重启动失败与 ci-new 疑问
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 77dfc524f0f17020b55df93b19f5fa64265494347d2be4dc873ece667d6533f3
  kind: experience
  material_digest: 336a0f30dd59affc567fedaa34dc71361409e5ae066244a5223930e49a0e61a0
  revision: b272449a2effbad6605488c0bda11871d3d995d919131a93ac5a12a0f97a808c
  schema: mindie-entry/3
  summary: 重点检索 ci-new 与 DSpark 关系、完整权重验证状态、5 层 P/D 分离前置条件，以及 r4 的 MoE dispatch tiling 561002（`Get WinSize failed`）失败。已知截断权重存在 `tiktoken.model` 缺失风险；r4 则在 target 与 DSpark draft 加载后于 `_dummy_run` 失败，仍需保留其未就绪、部分 PGID 存活和未发送推理请求等未完成状态。
  title: 当前任务导航：先区分启动前置问题与 r4 profiling 失败
navigation: 重点检索 ci-new 与 DSpark 关系、完整权重验证状态、5 层 P/D 分离前置条件，以及 r4 的 MoE dispatch tiling 561002（`Get WinSize failed`）失败。已知截断权重存在 `tiktoken.model` 缺失风险；r4 则在 target 与 DSpark draft 加载后于 `_dummy_run` 失败，仍需保留其未就绪、部分 PGID 存活和未发送推理请求等未完成状态。
schema: mindie-material-task/1
status: complete
task_id: 77dfc524f0f17020b55df93b19f5fa64265494347d2be4dc873ece667d6533f3
---

# 当前任务导航：先区分启动前置问题与 r4 profiling 失败

重点检索 ci-new 与 DSpark 关系、完整权重验证状态、5 层 P/D 分离前置条件，以及 r4 的 MoE dispatch tiling 561002（`Get WinSize failed`）失败。已知截断权重存在 `tiktoken.model` 缺失风险；r4 则在 target 与 DSpark draft 加载后于 `_dummy_run` 失败，仍需保留其未就绪、部分 PGID 存活和未发送推理请求等未完成状态。

## Materials

- [r4 完整权重启动失败与 ci-new 疑问](blocks/9171a45131d8375c9c22a140c6dc460080d9ac154c5905cad2ebe08d708e7446.md): 用户质疑 ci-new 分支遇到的问题可能与 DSpark 特性无关，并要求先验证完整权重，再分析 5 层 P/D 分离；此前还提到截断权重目录缺少 `tiktoken.model`，计划通过显式 tokenizer 覆盖避免无关启动失败。记录的 r4 检查显示完整 target 与 DSpark draft 均加载成功，但在 profiling 的 `_dummy_run` 阶段，四机一致触发 MoE dispatch tiling 错误 561002（`Get WinSize failed`）；`/health` 未就绪，154/155/156 根 PGID 已退出，153 PGID `3200653` 仍存活，且未发送推理请求。该结果是已观察到的启动失败记录，不应视为 DSpark 因果关系或完整权重已验证成功；日志据称保留完整。

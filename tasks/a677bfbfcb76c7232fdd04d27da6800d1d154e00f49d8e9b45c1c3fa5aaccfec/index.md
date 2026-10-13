---
blocks:
- block_id: bd85c9078a31510babe80945824fb989c75e158a5a4df030305c7418904b4121
  indexed: true
  sha256: 0e52844fd0252706defca09839cf8fc3a1a60bc057849bbfe241da28dda076ad
  source_range:
    end: 10163845
    part: 0
    start: 0
  summary: 围绕 DSV4 的 compressor metadata 调用进行了重构：从每层调用改为按“当前 composite substep × 语义 cache group”惰性复用，移除基于 `id(metadata)` 的 hack，并在 MTP/DSpark 切换 substep 时清理缓存，以保留 ACLGraph capture 边界。#14932 经历了 CI fixture 修复，当前已更新到 signed commit `dcf586494`；本机因缺少 torch 未完成 pytest，NPU 性能和实权重准确性也未验证。此前 CI 还出现过 A2 UT 的 `Forward context is not set`，以及一个尚未取得完整日志的 A3-4 失败。
  title: 'DSV4 compressor metadata 复用与 PR #14932'
- block_id: 79a7013565cd5eb58fef798c70e95cb7ca630263cede1d4298088041157eebb7
  indexed: true
  sha256: a09b78c315de0dc8e14bdca919c7d46aa10b46784c93a20fda7c7e324849890a
  source_range:
    end: 10163845
    part: 1
    start: 0
  summary: '针对 `releases/v0.26.0rc`，由于该分支使用重构前的单体 `dsa_v1.py` 且缺少独立 compressor 基线，已按旧结构手工移植 per-substep cache、语义 group key、substep 清理和回归 UT，创建 backport PR #14994。其 select-tests 曾因新增 A2 UT 未登记 `estimated_times` 而失败，补充 30 秒映射后本地 `coverage.py` 验证 40/40 通过，新的 CI 已启动；`ready-precise` 标签仍因权限未添加。当前新增问题是评估 compressor 是否需要带上 ops-transformer PR #10201 的内容，尚未给出结论。'
  title: 0.26 backport、CI 修复与 ops-transformer 依赖核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a677bfbfcb76c7232fdd04d27da6800d1d154e00f49d8e9b45c1c3fa5aaccfec
  kind: experience
  material_digest: f484a38306151644947777b7955c5223f01ee06a8dc360619379a519b5be313f
  revision: cb39cdd1496333d908c4495ec28643a33671d00752c305caa89873eca1642a02
  schema: mindie-entry/3
  summary: '主线围绕 DSV4 compressor metadata 的跨层复用：最终边界是单个 composite substep 内按语义 cache group 复用，并在 substep 切换时释放，避免 MTP/DSpark 为不同 cos/sin/slot mapping 长期保留多套 tensor。主 PR #14932 已修复由 MagicMock fixture 触发的 `Forward context is not set` CI 问题；0.26 分支因结构差异已手工移植并创建 #14994，select-tests 的估时映射问题已修复，但 CI 仍在运行且部分标签需要 maintainer 权限。待处理事项是判断 compressor 是否必须纳入 ops-transformer PR #10201；此前本机 pytest、NPU 性能、实权重准确性及部分硬件 CI 结果均不能视为已验证成功。'
  title: DSV4 compressor metadata 复用、CI 与 0.26 backport
navigation: '主线围绕 DSV4 compressor metadata 的跨层复用：最终边界是单个 composite substep 内按语义 cache group 复用，并在 substep 切换时释放，避免 MTP/DSpark 为不同 cos/sin/slot mapping 长期保留多套 tensor。主 PR #14932 已修复由 MagicMock fixture 触发的 `Forward context is not set` CI 问题；0.26 分支因结构差异已手工移植并创建 #14994，select-tests 的估时映射问题已修复，但 CI 仍在运行且部分标签需要 maintainer 权限。待处理事项是判断 compressor 是否必须纳入 ops-transformer PR #10201；此前本机 pytest、NPU 性能、实权重准确性及部分硬件 CI 结果均不能视为已验证成功。'
schema: mindie-material-task/1
status: complete
task_id: a677bfbfcb76c7232fdd04d27da6800d1d154e00f49d8e9b45c1c3fa5aaccfec
---

# DSV4 compressor metadata 复用、CI 与 0.26 backport

主线围绕 DSV4 compressor metadata 的跨层复用：最终边界是单个 composite substep 内按语义 cache group 复用，并在 substep 切换时释放，避免 MTP/DSpark 为不同 cos/sin/slot mapping 长期保留多套 tensor。主 PR #14932 已修复由 MagicMock fixture 触发的 `Forward context is not set` CI 问题；0.26 分支因结构差异已手工移植并创建 #14994，select-tests 的估时映射问题已修复，但 CI 仍在运行且部分标签需要 maintainer 权限。待处理事项是判断 compressor 是否必须纳入 ops-transformer PR #10201；此前本机 pytest、NPU 性能、实权重准确性及部分硬件 CI 结果均不能视为已验证成功。

## Materials

- [DSV4 compressor metadata 复用与 PR #14932](blocks/bd85c9078a31510babe80945824fb989c75e158a5a4df030305c7418904b4121.md): 围绕 DSV4 的 compressor metadata 调用进行了重构：从每层调用改为按“当前 composite substep × 语义 cache group”惰性复用，移除基于 `id(metadata)` 的 hack，并在 MTP/DSpark 切换 substep 时清理缓存，以保留 ACLGraph capture 边界。#14932 经历了 CI fixture 修复，当前已更新到 signed commit `dcf586494`；本机因缺少 torch 未完成 pytest，NPU 性能和实权重准确性也未验证。此前 CI 还出现过 A2 UT 的 `Forward context is not set`，以及一个尚未取得完整日志的 A3-4 失败。
- [0.26 backport、CI 修复与 ops-transformer 依赖核查](blocks/79a7013565cd5eb58fef798c70e95cb7ca630263cede1d4298088041157eebb7.md): 针对 `releases/v0.26.0rc`，由于该分支使用重构前的单体 `dsa_v1.py` 且缺少独立 compressor 基线，已按旧结构手工移植 per-substep cache、语义 group key、substep 清理和回归 UT，创建 backport PR #14994。其 select-tests 曾因新增 A2 UT 未登记 `estimated_times` 而失败，补充 30 秒映射后本地 `coverage.py` 验证 40/40 通过，新的 CI 已启动；`ready-precise` 标签仍因权限未添加。当前新增问题是评估 compressor 是否需要带上 ops-transformer PR #10201 的内容，尚未给出结论。

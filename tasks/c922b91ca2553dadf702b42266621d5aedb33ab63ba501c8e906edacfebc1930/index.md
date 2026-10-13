---
blocks:
- block_id: ee3d72b21a4ccd31704a144be4f7d3873e6b77cd83c9c35baf2559f0d260ae8d
  indexed: true
  sha256: 4dd2d06987094a6a12c91609928ea4b119888fada766bf4ad9c3d3b7e3b67f5c
  source_range:
    end: 3419928
    part: 0
    start: 0
  summary: 'CI 失败包含 A2 显存污染级联失败和 A3 DeepSeek-V4 exact-string golden 差异；A3 曾被怀疑与 `enable_shared_expert_dp` 默认值、routed SwiGLU clamp 及 #14397 回归有关。A3 复现过程中出现 GitHub fetch 卡住、源码缺少 `.git`/CATLASS 元数据、CANN 9.0.1/9.1.0 与 nightly `.so` 混用、Gloo 主机名解析 `rv=-2`、`PYTHONPATH` 覆盖导致找不到 `acl` 等环境问题，均不能直接作为 PR 根因。经过对齐 CANN 9.1.0、torch-npu `2.10.0.post4`、Triton Ascend `3.2.2`、模型和精确构建后，`enable_shared_expert_dp=false` 仍产生 CI 中的双引号旧 continuation；初步推翻了“只由该开关导致 golden 变化”的假设。'
  title: 'PR #14513 CI 初查与 A3 复现准备'
- block_id: 738c0b50aa149ff156c76b1d77585e67e06e2a93d161160abd52eb9cbe7aa76d
  indexed: true
  sha256: 55b8ad7091f273c66b7188e976869b96b14d8f12d95405b52ec62d50789f7f7e
  source_range:
    end: 3419928
    part: 1
    start: 0
  summary: '在同一 vLLM、CANN、torch-npu、Triton、模型、TP4/EP 和隔离缓存下，`enable_shared_expert_dp=false/true` 的完整 token 序列完全一致，均为双引号旧串；开关只改变首 token 的 logprob 优势（0.375 对 0.25），未改变 greedy 选择。因此现有证据不支持 #14513 恢复默认值直接破坏 golden，更指向不同物理 A3 runner、未固定的离线 ModelScope 缓存或跨 runner 数值差异，被 exact-string 比较放大。单引号 golden 来自 #13996，并被注释归因于 #14397 的 routed SwiGLU clamp；建议固定模型 revision/权重 checksum、采用更稳定的比较方式或 prompt，避免未经验证地再次翻转 golden；A2 的 EngineCore 残留显存问题仍与该 PR 无关。'
  title: A3 A/B 闭环与 golden 根因收敛
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c922b91ca2553dadf702b42266621d5aedb33ab63ba501c8e906edacfebc1930
  kind: experience
  material_digest: e92885f1ae52360a0d9cd4efd0c605c88b1c7264dd3647baec8d3d3dd6a5f8b8
  revision: 7fd04c25125289b1707f257a766c5316ae003005d8633a42ad78787ff8154699
  schema: mindie-entry/3
  summary: 当前任务已完成 A3 严格 A/B 闭环：shared-expert DP 开关造成轻微 logprob 偏移，但未改变输出，不能作为 golden 差异的直接根因。主要未决风险是跨 runner/模型缓存导致的数值差异与 exact-string golden 脆弱性；应固定模型 revision 和 checksum，并评估更稳定的断言。A2 失败仍记录为前一用例 EngineCore 未清理造成的显存级联失败，需单独 rerun 或修复进程清理。
  title: '#14513 CI 失败：A3 golden 脆弱性与 A2 显存污染'
navigation: 当前任务已完成 A3 严格 A/B 闭环：shared-expert DP 开关造成轻微 logprob 偏移，但未改变输出，不能作为 golden 差异的直接根因。主要未决风险是跨 runner/模型缓存导致的数值差异与 exact-string golden 脆弱性；应固定模型 revision 和 checksum，并评估更稳定的断言。A2 失败仍记录为前一用例 EngineCore 未清理造成的显存级联失败，需单独 rerun 或修复进程清理。
schema: mindie-material-task/1
status: complete
task_id: c922b91ca2553dadf702b42266621d5aedb33ab63ba501c8e906edacfebc1930
---

# #14513 CI 失败：A3 golden 脆弱性与 A2 显存污染

当前任务已完成 A3 严格 A/B 闭环：shared-expert DP 开关造成轻微 logprob 偏移，但未改变输出，不能作为 golden 差异的直接根因。主要未决风险是跨 runner/模型缓存导致的数值差异与 exact-string golden 脆弱性；应固定模型 revision 和 checksum，并评估更稳定的断言。A2 失败仍记录为前一用例 EngineCore 未清理造成的显存级联失败，需单独 rerun 或修复进程清理。

## Materials

- [PR #14513 CI 初查与 A3 复现准备](blocks/ee3d72b21a4ccd31704a144be4f7d3873e6b77cd83c9c35baf2559f0d260ae8d.md): CI 失败包含 A2 显存污染级联失败和 A3 DeepSeek-V4 exact-string golden 差异；A3 曾被怀疑与 `enable_shared_expert_dp` 默认值、routed SwiGLU clamp 及 #14397 回归有关。A3 复现过程中出现 GitHub fetch 卡住、源码缺少 `.git`/CATLASS 元数据、CANN 9.0.1/9.1.0 与 nightly `.so` 混用、Gloo 主机名解析 `rv=-2`、`PYTHONPATH` 覆盖导致找不到 `acl` 等环境问题，均不能直接作为 PR 根因。经过对齐 CANN 9.1.0、torch-npu `2.10.0.post4`、Triton Ascend `3.2.2`、模型和精确构建后，`enable_shared_expert_dp=false` 仍产生 CI 中的双引号旧 continuation；初步推翻了“只由该开关导致 golden 变化”的假设。
- [A3 A/B 闭环与 golden 根因收敛](blocks/738c0b50aa149ff156c76b1d77585e67e06e2a93d161160abd52eb9cbe7aa76d.md): 在同一 vLLM、CANN、torch-npu、Triton、模型、TP4/EP 和隔离缓存下，`enable_shared_expert_dp=false/true` 的完整 token 序列完全一致，均为双引号旧串；开关只改变首 token 的 logprob 优势（0.375 对 0.25），未改变 greedy 选择。因此现有证据不支持 #14513 恢复默认值直接破坏 golden，更指向不同物理 A3 runner、未固定的离线 ModelScope 缓存或跨 runner 数值差异，被 exact-string 比较放大。单引号 golden 来自 #13996，并被注释归因于 #14397 的 routed SwiGLU clamp；建议固定模型 revision/权重 checksum、采用更稳定的比较方式或 prompt，避免未经验证地再次翻转 golden；A2 的 EngineCore 残留显存问题仍与该 PR 无关。

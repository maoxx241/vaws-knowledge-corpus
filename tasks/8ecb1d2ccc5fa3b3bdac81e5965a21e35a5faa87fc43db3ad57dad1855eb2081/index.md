---
blocks:
- block_id: e78c1e5fcf16f65fd672079f3da618ab57cc5add607ce0bc688db8e097e4c466
  indexed: true
  sha256: cce2f247eeb0f26f02053297f4700492802d6d520fabdeeb681e1f01714e4dbc
  source_range:
    end: 1507836
    part: 0
    start: 0
  summary: 在独立 core worktree 中加入 `DSpark + do_remote_decode=True` 的 P 节点保护：Mooncake truncate 前跳过本地 prefix hit，使用空 `KVCacheBlocks`；普通 DSpark、非 remote decode 和非 DSpark 保持原路径，并补充 3 组合 scheduler 单测。Ruff、格式检查、`diff --check` 与 compileall 通过；但 pytest 因本地测试环境缺少依赖停在收集阶段，`uv run` 还受到 macOS 上不存在的 Darwin CPU Torch 变体锁定问题影响，因此不能视为测试验证成功。Mooncake transfer metadata 中已有 D 侧“本地命中数”，但尚未进入 DSpark proposer 持久计数；plugin 未修改，未使用 NPU，且未提交或推送。
  title: DSpark 远程解码的 P 侧 prefix-cache 旁路与验证边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8ecb1d2ccc5fa3b3bdac81e5965a21e35a5faa87fc43db3ad57dad1855eb2081
  kind: experience
  material_digest: 069525aa76ee4726e13a97522037e217b3a771a222e17b6774b311df5c0e66b8
  revision: e28baaa84bb36e185713def7b0ab5d09cd80232cee7f03cbc07ec4db0c5751d4
  schema: mindie-entry/3
  summary: 核心改动位于 core scheduler：仅对 `DSpark + do_remote_decode` 启用 P 侧本地 prefix-cache 旁路，并保留其它路径。静态检查已通过，但 pytest 受依赖环境限制未完成；Mooncake 的 D 侧本地命中数仍停留在 transfer metadata，plugin 传播与四机拓扑相关工作尚未证明完成。
  title: 当前任务导航：DSpark/Mooncake prefix-cache 修复与验证状态
navigation: 核心改动位于 core scheduler：仅对 `DSpark + do_remote_decode` 启用 P 侧本地 prefix-cache 旁路，并保留其它路径。静态检查已通过，但 pytest 受依赖环境限制未完成；Mooncake 的 D 侧本地命中数仍停留在 transfer metadata，plugin 传播与四机拓扑相关工作尚未证明完成。
schema: mindie-material-task/1
status: complete
task_id: 8ecb1d2ccc5fa3b3bdac81e5965a21e35a5faa87fc43db3ad57dad1855eb2081
---

# 当前任务导航：DSpark/Mooncake prefix-cache 修复与验证状态

核心改动位于 core scheduler：仅对 `DSpark + do_remote_decode` 启用 P 侧本地 prefix-cache 旁路，并保留其它路径。静态检查已通过，但 pytest 受依赖环境限制未完成；Mooncake 的 D 侧本地命中数仍停留在 transfer metadata，plugin 传播与四机拓扑相关工作尚未证明完成。

## Materials

- [DSpark 远程解码的 P 侧 prefix-cache 旁路与验证边界](blocks/e78c1e5fcf16f65fd672079f3da618ab57cc5add607ce0bc688db8e097e4c466.md): 在独立 core worktree 中加入 `DSpark + do_remote_decode=True` 的 P 节点保护：Mooncake truncate 前跳过本地 prefix hit，使用空 `KVCacheBlocks`；普通 DSpark、非 remote decode 和非 DSpark 保持原路径，并补充 3 组合 scheduler 单测。Ruff、格式检查、`diff --check` 与 compileall 通过；但 pytest 因本地测试环境缺少依赖停在收集阶段，`uv run` 还受到 macOS 上不存在的 Darwin CPU Torch 变体锁定问题影响，因此不能视为测试验证成功。Mooncake transfer metadata 中已有 D 侧“本地命中数”，但尚未进入 DSpark proposer 持久计数；plugin 未修改，未使用 NPU，且未提交或推送。

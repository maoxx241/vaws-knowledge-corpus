---
blocks:
- block_id: 06458b34bfa08c3d09686688cbb241d70bee8b1640002f76312eb1a1bfafbbd1
  indexed: true
  sha256: f79edc4740371acb3ac24c586903fc545ebf31191d9f4999e27beaee425ea1c7
  source_range:
    end: 7792977
    part: 0
    start: 0
  summary: '基于 0.26 基线创建并持续修订 DeepSeek V4 Full QLI v2 PR #15320，经历双 binding 并存、metadata 的 max_seqlen_k 修正、删除冗余 Optional/assert、README markdownlint 修复及 decode_qli_metadata 类型声明调整；最新 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`，保持单个 Signed-off commit。静态检查、compileall、diff-check 和在忽略缺失外部依赖条件下的 Python 3.10/3.11/3.12 mypy 均通过，但本地缺少完整 pytest、torch_npu、vllm、huggingface_hub，未完成 NPU 编译、硬件 e2e、精度或性能验证；仓库既有 actionlint 全文件基线问题仍存在。尝试添加 `ready` label 时，GraphQL 缺少 `AddLabelsToLabelable` 权限，REST 返回 HTTP 403，PR head 未变，label 仍为 `documentation`、`module:tests`，selected tests 尚未触发且 `ci-gate` 仍失败，需要 upstream Contributor/Maintainer 处理。'
  title: 'Full v2 PR #15320 迭代与权限阻塞'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 40fd4789f8c5706d39fce27e2fe6da9b72e38c3afd28b9adbfe4eb4eee2e8a6c
  kind: experience
  material_digest: 6252e2f0b5717c4ad5bf3effc26342e6e75ecf2bf01dcbac18724a5d6954242a
  revision: 81b95b1456384f04e9184c740b591baeef0bb0d2370ea2e279a6a08042974db1
  schema: mindie-entry/3
  summary: 主线是 v0.26 基线上的 DeepSeek V4 Full QLI v2 PR，当前 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`，代码与静态检查迭代已完成但硬件验证和完整依赖测试未完成。后续关键阻塞是 `ready` label 权限不足（GraphQL 权限错误、REST HTTP 403），因此 selected tests 尚未触发；同时保留 CI pending、`ci-gate` 失败及 actionlint 基线失败等未决状态。
  title: 'Full v2 PR #15320 当前导航'
navigation: 主线是 v0.26 基线上的 DeepSeek V4 Full QLI v2 PR，当前 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`，代码与静态检查迭代已完成但硬件验证和完整依赖测试未完成。后续关键阻塞是 `ready` label 权限不足（GraphQL 权限错误、REST HTTP 403），因此 selected tests 尚未触发；同时保留 CI pending、`ci-gate` 失败及 actionlint 基线失败等未决状态。
schema: mindie-material-task/1
status: complete
task_id: 40fd4789f8c5706d39fce27e2fe6da9b72e38c3afd28b9adbfe4eb4eee2e8a6c
---

# Full v2 PR #15320 当前导航

主线是 v0.26 基线上的 DeepSeek V4 Full QLI v2 PR，当前 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`，代码与静态检查迭代已完成但硬件验证和完整依赖测试未完成。后续关键阻塞是 `ready` label 权限不足（GraphQL 权限错误、REST HTTP 403），因此 selected tests 尚未触发；同时保留 CI pending、`ci-gate` 失败及 actionlint 基线失败等未决状态。

## Materials

- [Full v2 PR #15320 迭代与权限阻塞](blocks/06458b34bfa08c3d09686688cbb241d70bee8b1640002f76312eb1a1bfafbbd1.md): 基于 0.26 基线创建并持续修订 DeepSeek V4 Full QLI v2 PR #15320，经历双 binding 并存、metadata 的 max_seqlen_k 修正、删除冗余 Optional/assert、README markdownlint 修复及 decode_qli_metadata 类型声明调整；最新 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`，保持单个 Signed-off commit。静态检查、compileall、diff-check 和在忽略缺失外部依赖条件下的 Python 3.10/3.11/3.12 mypy 均通过，但本地缺少完整 pytest、torch_npu、vllm、huggingface_hub，未完成 NPU 编译、硬件 e2e、精度或性能验证；仓库既有 actionlint 全文件基线问题仍存在。尝试添加 `ready` label 时，GraphQL 缺少 `AddLabelsToLabelable` 权限，REST 返回 HTTP 403，PR head 未变，label 仍为 `documentation`、`module:tests`，selected tests 尚未触发且 `ci-gate` 仍失败，需要 upstream Contributor/Maintainer 处理。

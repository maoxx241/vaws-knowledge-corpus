---
blocks:
- block_id: 616fbe05c392fae61ed3b55d9cf593ec6d80dc62e822ab821f381c3523939ca0
  indexed: true
  sha256: 55d07bab83b4f54dc192beb30477d4c7fb5b3e829807b52b2ef233aea6a550a9
  source_range:
    end: 5504801
    part: 0
    start: 0
  summary: '已将 fla-npu #390 的 causal conv1d 迁移到独立 main 分支并创建 vllm-ascend PR #15341，保留 device metadata dtype、spec decode 二维 cache stride 等兼容逻辑；本地 format.sh ci、Python 编译检查，以及远端 pre-commit、mypy、DCO、冲突检查通过。迁移尚未合入：NPU/A5 测试虽被选中但全部 skipped，ci-gate 失败，仍需 maintainer 添加 ready-precise 和 ready-a5；此前 `[Feat]` 标题失败已修正为 `[Feature]`，最终 signed commit 为 `de55359d549dfd4c0f9c5a72bf779c520269e04b`。'
  title: '迁移 fla-npu #390 causal conv1d 至 main'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 72b57f3c3237b0ac0b5c2c4fc8336fbf11dc88613fb899d6401cd3da379f1129
  kind: experience
  material_digest: 3a29a671dc76ca2977fd8c22ce9aaea15075a118862cd9363d625963134f9665
  revision: 409f1d452fef9f3dc474010ab2e08ddf907d0fecf134aec9de7e3e801e29f3a4
  schema: mindie-entry/3
  summary: '主线迁移已提交至 vllm-ascend PR #15341，但尚未合入。代码与静态门禁结果较好；NPU/A5 实测不能视为成功，因为测试 job 被 skipped 且 ci-gate 失败，后续需补齐 maintainer 标签权限/门禁后重新确认实际测试结果。'
  title: 当前任务导航：causal conv1d 迁移 PR 待门禁完成
navigation: '主线迁移已提交至 vllm-ascend PR #15341，但尚未合入。代码与静态门禁结果较好；NPU/A5 实测不能视为成功，因为测试 job 被 skipped 且 ci-gate 失败，后续需补齐 maintainer 标签权限/门禁后重新确认实际测试结果。'
schema: mindie-material-task/1
status: complete
task_id: 72b57f3c3237b0ac0b5c2c4fc8336fbf11dc88613fb899d6401cd3da379f1129
---

# 当前任务导航：causal conv1d 迁移 PR 待门禁完成

主线迁移已提交至 vllm-ascend PR #15341，但尚未合入。代码与静态门禁结果较好；NPU/A5 实测不能视为成功，因为测试 job 被 skipped 且 ci-gate 失败，后续需补齐 maintainer 标签权限/门禁后重新确认实际测试结果。

## Materials

- [迁移 fla-npu #390 causal conv1d 至 main](blocks/616fbe05c392fae61ed3b55d9cf593ec6d80dc62e822ab821f381c3523939ca0.md): 已将 fla-npu #390 的 causal conv1d 迁移到独立 main 分支并创建 vllm-ascend PR #15341，保留 device metadata dtype、spec decode 二维 cache stride 等兼容逻辑；本地 format.sh ci、Python 编译检查，以及远端 pre-commit、mypy、DCO、冲突检查通过。迁移尚未合入：NPU/A5 测试虽被选中但全部 skipped，ci-gate 失败，仍需 maintainer 添加 ready-precise 和 ready-a5；此前 `[Feat]` 标题失败已修正为 `[Feature]`，最终 signed commit 为 `de55359d549dfd4c0f9c5a72bf779c520269e04b`。

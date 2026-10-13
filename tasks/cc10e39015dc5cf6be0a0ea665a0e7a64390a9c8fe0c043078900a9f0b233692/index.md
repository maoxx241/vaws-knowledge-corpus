---
blocks:
- block_id: e435af13b5b363d7e1a60c503a992a6a3d0b4c0a37f4eb36f660acc47f23375b
  indexed: true
  sha256: 1a8c6eee17d02bbcffcadfd9ee842e5c1660487fbb64fbfc7e0c84ad9674fc2e
  source_range:
    end: 455331
    part: 0
    start: 0
  summary: 据记录，任务要求算子无需重新编译；助手计划在四机新目录执行同步和只读校验，涉及 Ascend 提交 `93b073de`、core 提交 `d02df748`、生产文件哈希、源文件聚合哈希及只读 manifest。过程中报告 154–156 的 core 因 tar 的 `build` 排除规则过宽而缺少 `requirements/build/` 下 5 个文本清单，随后补入这些文件并重新进行聚合校验；未报告编译、安装、进程或服务操作。最终消息声称四机同步及哈希一致，但这些内容仅是来源中的报告，不能视为已验证成功；目标路径、哈希、manifest 模式 `444` 和若干路径均包含脱敏占位符。
  title: 四机新目录同步与只读校验记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: cc10e39015dc5cf6be0a0ea665a0e7a64390a9c8fe0c043078900a9f0b233692
  kind: experience
  material_digest: 99e05c19ac810b0172eaa53dc00be086591f2a222fce1a8f5fb47bb943a409f9
  revision: 07851b784a95a0506bd447bd997df4cb6cb8679c295bb34ec221131258ef34f5
  schema: mindie-entry/3
  summary: 本批唯一材料涉及不重新编译算子前提下的四机 Ascend/core 源目录同步、哈希比对和只读校验。检索重点包括 `93b073de`、`d02df748`、tar `build` 排除规则、`requirements/build/` 缺失的 5 个文本清单、旧验证目录复用、只读 manifest，以及未编译/未安装/未操作服务等报告；同步完成与一致性结论仍应视为未验证的来源陈述。
  title: 当前任务导航：四机目录同步索引
navigation: 本批唯一材料涉及不重新编译算子前提下的四机 Ascend/core 源目录同步、哈希比对和只读校验。检索重点包括 `93b073de`、`d02df748`、tar `build` 排除规则、`requirements/build/` 缺失的 5 个文本清单、旧验证目录复用、只读 manifest，以及未编译/未安装/未操作服务等报告；同步完成与一致性结论仍应视为未验证的来源陈述。
schema: mindie-material-task/1
status: complete
task_id: cc10e39015dc5cf6be0a0ea665a0e7a64390a9c8fe0c043078900a9f0b233692
---

# 当前任务导航：四机目录同步索引

本批唯一材料涉及不重新编译算子前提下的四机 Ascend/core 源目录同步、哈希比对和只读校验。检索重点包括 `93b073de`、`d02df748`、tar `build` 排除规则、`requirements/build/` 缺失的 5 个文本清单、旧验证目录复用、只读 manifest，以及未编译/未安装/未操作服务等报告；同步完成与一致性结论仍应视为未验证的来源陈述。

## Materials

- [四机新目录同步与只读校验记录](blocks/e435af13b5b363d7e1a60c503a992a6a3d0b4c0a37f4eb36f660acc47f23375b.md): 据记录，任务要求算子无需重新编译；助手计划在四机新目录执行同步和只读校验，涉及 Ascend 提交 `93b073de`、core 提交 `d02df748`、生产文件哈希、源文件聚合哈希及只读 manifest。过程中报告 154–156 的 core 因 tar 的 `build` 排除规则过宽而缺少 `requirements/build/` 下 5 个文本清单，随后补入这些文件并重新进行聚合校验；未报告编译、安装、进程或服务操作。最终消息声称四机同步及哈希一致，但这些内容仅是来源中的报告，不能视为已验证成功；目标路径、哈希、manifest 模式 `444` 和若干路径均包含脱敏占位符。

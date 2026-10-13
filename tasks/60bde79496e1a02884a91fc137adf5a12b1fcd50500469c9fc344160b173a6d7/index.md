---
blocks:
- block_id: 66a325057db00c90e26e016515894ae5740c69c568e508cf36f627e2a7a4291b
  indexed: true
  sha256: bf19f1a657965f30adbca1edeb18077b6b051dc54d6905b0f11e20aa37a2621e
  source_range:
    end: 4251536
    part: 0
    start: 0
  summary: '记录显示，PR #14454 新增的主要是 16 卡 K3 dummy-weight 用例（Block5 MLA DSpark、W4A8 GQA DSpark、MTP+image、MLA P/D），而 8 卡任务实际运行的是既有 GLM-5.2/MiniMax 回归；冷 CI 中 16 卡耗时约 679–688 秒，明显高于原 360 秒估计。提出了 A3 四卡的功能路径映射（TP16→TP4、DP2×TP8→DP2×TP2、P8/D8→P2/D2）及缩短模型长度、减少边界样本、拆分 nodeid 等方法，但这些仍是设计建议，尚无四卡实跑结果，5 分钟仅为待验证目标。GLM/MiniMax 真实权重加载可能使四卡稳定低于 5 分钟不可行，因此建议保留为 optional/nightly，并避免将 dummy smoke 描述为完整权重、精度或接受率等价验证。'
  title: 'PR #14454 的 A3 四卡等价用例分析'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 60bde79496e1a02884a91fc137adf5a12b1fcd50500469c9fc344160b173a6d7
  kind: experience
  material_digest: 17ce9f88e8d5d422703b71fe7593894241157ae5485d4a6ac1ecfaf860870fa0
  revision: 23b25f3538ca512c6f4392566ca01809adf4d0d99e488cd16a4eaff01e2fcb12
  schema: mindie-entry/3
  summary: '重点检索 PR #14454 的 8 卡与 16 卡实际用例、CI 耗时、K3 dummy smoke 覆盖范围，以及 A3×4 的功能路径映射和限时设计。当前结论包含未验证假设：四卡尚未实跑，<300 秒连续三次验收标准、graph replay、DP drafts 与 P/D remote blocks 均待验证；同时保留 8 卡回归与 16 卡新用例职责不同、冷 CI 超过原估时等修正信息。'
  title: '当前任务导航：PR #14454 用例索引与 A3 四卡迁移'
navigation: '重点检索 PR #14454 的 8 卡与 16 卡实际用例、CI 耗时、K3 dummy smoke 覆盖范围，以及 A3×4 的功能路径映射和限时设计。当前结论包含未验证假设：四卡尚未实跑，<300 秒连续三次验收标准、graph replay、DP drafts 与 P/D remote blocks 均待验证；同时保留 8 卡回归与 16 卡新用例职责不同、冷 CI 超过原估时等修正信息。'
schema: mindie-material-task/1
status: complete
task_id: 60bde79496e1a02884a91fc137adf5a12b1fcd50500469c9fc344160b173a6d7
---

# 当前任务导航：PR #14454 用例索引与 A3 四卡迁移

重点检索 PR #14454 的 8 卡与 16 卡实际用例、CI 耗时、K3 dummy smoke 覆盖范围，以及 A3×4 的功能路径映射和限时设计。当前结论包含未验证假设：四卡尚未实跑，<300 秒连续三次验收标准、graph replay、DP drafts 与 P/D remote blocks 均待验证；同时保留 8 卡回归与 16 卡新用例职责不同、冷 CI 超过原估时等修正信息。

## Materials

- [PR #14454 的 A3 四卡等价用例分析](blocks/66a325057db00c90e26e016515894ae5740c69c568e508cf36f627e2a7a4291b.md): 记录显示，PR #14454 新增的主要是 16 卡 K3 dummy-weight 用例（Block5 MLA DSpark、W4A8 GQA DSpark、MTP+image、MLA P/D），而 8 卡任务实际运行的是既有 GLM-5.2/MiniMax 回归；冷 CI 中 16 卡耗时约 679–688 秒，明显高于原 360 秒估计。提出了 A3 四卡的功能路径映射（TP16→TP4、DP2×TP8→DP2×TP2、P8/D8→P2/D2）及缩短模型长度、减少边界样本、拆分 nodeid 等方法，但这些仍是设计建议，尚无四卡实跑结果，5 分钟仅为待验证目标。GLM/MiniMax 真实权重加载可能使四卡稳定低于 5 分钟不可行，因此建议保留为 optional/nightly，并避免将 dummy smoke 描述为完整权重、精度或接受率等价验证。

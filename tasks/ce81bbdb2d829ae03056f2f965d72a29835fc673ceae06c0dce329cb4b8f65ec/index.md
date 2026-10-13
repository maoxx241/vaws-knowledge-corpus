---
blocks:
- block_id: 5f1d48cd47e9327c6c40b6da88950cfd7e19991b5988f0703b4f6287bc38f4b5
  indexed: true
  sha256: 0781d2c0f50608f94fbcdd13ab817acbc64633497e812c8c5c10040989906d24
  source_range:
    end: 1037297
    part: 0
    start: 0
  summary: 讨论确认不做跨 Harness 会话协作；领域内可采用按需发现，但具体能力取决于各 Harness 的调研结果。调研结论倾向于“小领域＋本域目录可见＋正文按需读取”，并记录了 Cursor、Kimi、Codex、Claude、DSH、Grok 在 metadata 筛选、隐藏、刷新或扩展点上的差异，其中部分逐会话能力仍未明确。已明确第三方配置保持原样，插件只影响自身范围内的 Skill；未运行模型任务或 PoC，也未修改其他报告或 issue。
  title: 领域 Skill 按需发现与配置边界结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ce81bbdb2d829ae03056f2f965d72a29835fc673ceae06c0dce329cb4b8f65ec
  kind: experience
  material_digest: 060d4b8cb3668a0a3a443726e47f73161adda99119beb557291726d67a47d267
  revision: bf08f4086956b6e054e7d25ef2da6ae33a02545f6eb639bce8c73862b8b00477
  schema: mindie-entry/3
  summary: 架构层暂不把 GUI、具体 API、进程拆分和仓库目录作为阻塞点；六端先各自提供完整体验，不做跨 Harness 协作。当前已澄清领域内按需发现的方向，以及用户既有全局 Skill/MCP 配置不受插件静默修改的边界；各 Harness 的 metadata 筛选和逐会话能力仍有未明确项，后续实现需按端确认。此前关于领域入口、能力组成、知识检索范围和协作工具的方向仍适用，但本批材料中的调研结论属于可失败参考索引，并非已验证的运行成功。
  title: 当前任务导航：领域架构边界与 Skill 发现调研
navigation: 架构层暂不把 GUI、具体 API、进程拆分和仓库目录作为阻塞点；六端先各自提供完整体验，不做跨 Harness 协作。当前已澄清领域内按需发现的方向，以及用户既有全局 Skill/MCP 配置不受插件静默修改的边界；各 Harness 的 metadata 筛选和逐会话能力仍有未明确项，后续实现需按端确认。此前关于领域入口、能力组成、知识检索范围和协作工具的方向仍适用，但本批材料中的调研结论属于可失败参考索引，并非已验证的运行成功。
schema: mindie-material-task/1
status: complete
task_id: ce81bbdb2d829ae03056f2f965d72a29835fc673ceae06c0dce329cb4b8f65ec
---

# 当前任务导航：领域架构边界与 Skill 发现调研

架构层暂不把 GUI、具体 API、进程拆分和仓库目录作为阻塞点；六端先各自提供完整体验，不做跨 Harness 协作。当前已澄清领域内按需发现的方向，以及用户既有全局 Skill/MCP 配置不受插件静默修改的边界；各 Harness 的 metadata 筛选和逐会话能力仍有未明确项，后续实现需按端确认。此前关于领域入口、能力组成、知识检索范围和协作工具的方向仍适用，但本批材料中的调研结论属于可失败参考索引，并非已验证的运行成功。

## Materials

- [领域 Skill 按需发现与配置边界结论](blocks/5f1d48cd47e9327c6c40b6da88950cfd7e19991b5988f0703b4f6287bc38f4b5.md): 讨论确认不做跨 Harness 会话协作；领域内可采用按需发现，但具体能力取决于各 Harness 的调研结果。调研结论倾向于“小领域＋本域目录可见＋正文按需读取”，并记录了 Cursor、Kimi、Codex、Claude、DSH、Grok 在 metadata 筛选、隐藏、刷新或扩展点上的差异，其中部分逐会话能力仍未明确。已明确第三方配置保持原样，插件只影响自身范围内的 Skill；未运行模型任务或 PoC，也未修改其他报告或 issue。

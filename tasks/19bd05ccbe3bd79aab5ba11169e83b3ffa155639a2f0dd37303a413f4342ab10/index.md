---
blocks:
- block_id: 00b5d13351716e6857b912059a09a5c26aebead3d2ad15a22f5c4832dc2dd28b
  indexed: true
  sha256: 10025ebc390975029eb63499c450beb2f8ec10a214c1c2fd55efe6f24617ecc0
  source_range:
    end: 545982
    part: 0
    start: 0
  summary: 材料提出需区分 ci-new 分支中的 `media_placeholder_token_id` 问题与 DSpark 启动链路中的 EAGLE3 报错；此前回答将前者归因于多模态占位 token 字段兼容缺口，将后者归因于 K3 外层 wrapper 未实现或转发 `SupportsEagle3` 接口。另有未完成的验证要求：应先确认完整权重可正常工作，再判断 5 层 P/D 分离；截断权重目录缺少 `tiktoken.model` 的说法属于已报告的启动风险，并非已验证成功。
  title: ci-new 与 DSpark/K3 兼容性诊断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 19bd05ccbe3bd79aab5ba11169e83b3ffa155639a2f0dd37303a413f4342ab10
  kind: experience
  material_digest: c8ca5929f5f1924f59b3c3a9787116a1bdedf2e2cf3f39adf7b664bb5f175d83
  revision: 4e58390f78fbdcc73cfd1fa44aecf248898a809bc7b662ddfb8296e9a4118e95
  schema: mindie-entry/3
  summary: 重点检索 ci-new 的 `media_placeholder_token_id`、DSpark 初始化、多模态 token 配置、EAGLE3 外层 wrapper 及 aux hidden-state set/get 转发。完整权重优先验证和 5 层 P/D 分离仍是未完成事项；现有结论是诊断性判断，不应视为已验证修复或成功回归。
  title: 当前任务导航：K3/DSpark 问题索引
navigation: 重点检索 ci-new 的 `media_placeholder_token_id`、DSpark 初始化、多模态 token 配置、EAGLE3 外层 wrapper 及 aux hidden-state set/get 转发。完整权重优先验证和 5 层 P/D 分离仍是未完成事项；现有结论是诊断性判断，不应视为已验证修复或成功回归。
schema: mindie-material-task/1
status: complete
task_id: 19bd05ccbe3bd79aab5ba11169e83b3ffa155639a2f0dd37303a413f4342ab10
---

# 当前任务导航：K3/DSpark 问题索引

重点检索 ci-new 的 `media_placeholder_token_id`、DSpark 初始化、多模态 token 配置、EAGLE3 外层 wrapper 及 aux hidden-state set/get 转发。完整权重优先验证和 5 层 P/D 分离仍是未完成事项；现有结论是诊断性判断，不应视为已验证修复或成功回归。

## Materials

- [ci-new 与 DSpark/K3 兼容性诊断](blocks/00b5d13351716e6857b912059a09a5c26aebead3d2ad15a22f5c4832dc2dd28b.md): 材料提出需区分 ci-new 分支中的 `media_placeholder_token_id` 问题与 DSpark 启动链路中的 EAGLE3 报错；此前回答将前者归因于多模态占位 token 字段兼容缺口，将后者归因于 K3 外层 wrapper 未实现或转发 `SupportsEagle3` 接口。另有未完成的验证要求：应先确认完整权重可正常工作，再判断 5 层 P/D 分离；截断权重目录缺少 `tiktoken.model` 的说法属于已报告的启动风险，并非已验证成功。

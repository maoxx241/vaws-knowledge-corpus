---
blocks:
- block_id: f20a9c0849294d5f8fb0b5a164ea99b3aa2bca8edab09149a426f053fe0f4247
  indexed: true
  sha256: 38c64ac23e013735f9350927e114eafb61367c589d0818c15a723990261b3e89
  source_range:
    end: 341859
    part: 0
    start: 0
  summary: '记录称 #13277 后续 QuaRot/128K 排查使用的是 0726 版全量权重，含 363 个顶层文件、346 个 safetensors，以及 `optional/quarot.safetensors` 中形状为 `(7168, 7168)` 的 `global_rotation`；同时保留了 0722、无日期 flex-quarot 等其他候选目录，未完全消除命名歧义。该矩阵据称完成四节点全量加载、4/4 请求和 8192→1024 长度验证，但投机接受率接近 0%（0726 为 `0.2198%`、AL `1.0154`），因此只能表述为加载与请求完成，不能升级为接受效果验证；记录还区分了历史源码 `cfaebdd6` 与当前 PR head `42f80061`，并保留 Chronicle 不可用及路径/版本信息部分被脱敏的限制。'
  title: '#13277 旋转量化全量权重版本与验证边界'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e9df124761b43f306445c3a784c3ed6f7a83262077575d7b567b89884b4168f3
  kind: experience
  material_digest: e4b23c9cadecdde374390e421022e0ad2d7797e4ed9621a56a8fbec2640df72c
  revision: d245513a44a664b73a674c32606e44103735334e4b6e06e935531b51f7ee95ce
  schema: mindie-entry/3
  summary: '当前索引聚焦 PR #13277 的 QuaRot 全量权重版本：记录倾向于 0726 版，但仍保留 0722 和无日期 flex-quarot 等候选。已完成的是全量加载、请求和长度检查；低接受率意味着不能宣称投机效果验证成功，且历史矩阵不可直接视为当前 PR head 的结果。'
  title: 旋转量化权重版本与验证记录
navigation: '当前索引聚焦 PR #13277 的 QuaRot 全量权重版本：记录倾向于 0726 版，但仍保留 0722 和无日期 flex-quarot 等候选。已完成的是全量加载、请求和长度检查；低接受率意味着不能宣称投机效果验证成功，且历史矩阵不可直接视为当前 PR head 的结果。'
schema: mindie-material-task/1
status: complete
task_id: e9df124761b43f306445c3a784c3ed6f7a83262077575d7b567b89884b4168f3
---

# 旋转量化权重版本与验证记录

当前索引聚焦 PR #13277 的 QuaRot 全量权重版本：记录倾向于 0726 版，但仍保留 0722 和无日期 flex-quarot 等候选。已完成的是全量加载、请求和长度检查；低接受率意味着不能宣称投机效果验证成功，且历史矩阵不可直接视为当前 PR head 的结果。

## Materials

- [#13277 旋转量化全量权重版本与验证边界](blocks/f20a9c0849294d5f8fb0b5a164ea99b3aa2bca8edab09149a426f053fe0f4247.md): 记录称 #13277 后续 QuaRot/128K 排查使用的是 0726 版全量权重，含 363 个顶层文件、346 个 safetensors，以及 `optional/quarot.safetensors` 中形状为 `(7168, 7168)` 的 `global_rotation`；同时保留了 0722、无日期 flex-quarot 等其他候选目录，未完全消除命名歧义。该矩阵据称完成四节点全量加载、4/4 请求和 8192→1024 长度验证，但投机接受率接近 0%（0726 为 `0.2198%`、AL `1.0154`），因此只能表述为加载与请求完成，不能升级为接受效果验证；记录还区分了历史源码 `cfaebdd6` 与当前 PR head `42f80061`，并保留 Chronicle 不可用及路径/版本信息部分被脱敏的限制。

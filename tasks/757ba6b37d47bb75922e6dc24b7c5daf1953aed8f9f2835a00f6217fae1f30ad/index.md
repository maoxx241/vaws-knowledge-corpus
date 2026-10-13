---
blocks:
- block_id: e73de8e19e9953baec5275c56f5d9562cf074bce04a9ed80df76c3db9ca58ec7
  indexed: true
  sha256: c558eef2284398dd501ef189230f82eef5351a46bdb500dd2995e228c770d7fe
  source_range:
    end: 865778
    part: 0
    start: 0
  summary: 该块指出 K3 的整除失败据报告来自 AscendMLAAttentionSpec 未兑现 `page_size_padded`：实际页为 884736 B，公共 padded 页为 912384 B，余数正好对应 conv state；建议覆盖 `real_page_size_bytes`、保留 merge 时的 padding，并补充相关单测，但这些仍是分析结论和建议，未在此块中验证修复。用户特别提醒 w4 权重需将两个 int4 打包进一个 int8；同时，K3 的 KDA state 与 Qwen3.5 的 GDN state 形状和 dtype 不同，`num_spec` 被忽略意味着 speculative decode 尚不能据此宣称完整支持。
  title: K3 MLA/KDA KV-cache 页对齐与 W4 适配注意事项
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 757ba6b37d47bb75922e6dc24b7c5daf1953aed8f9f2835a00f6217fae1f30ad
  kind: experience
  material_digest: 4935b5324e044ac3676c7d1cbc58931ac24ea4cf4b5a5dfe1aad1d5fbf7c212e
  revision: c52b640a19f6d1f7d0f87ee05237d75354fc32c9552d500bb836e6e652ce3d7f
  schema: mindie-entry/3
  summary: 已收录本批唯一材料块：重点检索词包括 W4/int4→int8、Qwen3.5、AscendMLAAttentionSpec、`page_size_padded`、`real_page_size_bytes`、KV-cache 页整除失败、KDA/GDN state、speculative decode。材料中的修复方向、单测要求和兼容性判断均保留为未验证或不完整结论；原始块仍可独立检索。
  title: 当前任务：K3/Qwen3.5 适配与检索索引
navigation: 已收录本批唯一材料块：重点检索词包括 W4/int4→int8、Qwen3.5、AscendMLAAttentionSpec、`page_size_padded`、`real_page_size_bytes`、KV-cache 页整除失败、KDA/GDN state、speculative decode。材料中的修复方向、单测要求和兼容性判断均保留为未验证或不完整结论；原始块仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 757ba6b37d47bb75922e6dc24b7c5daf1953aed8f9f2835a00f6217fae1f30ad
---

# 当前任务：K3/Qwen3.5 适配与检索索引

已收录本批唯一材料块：重点检索词包括 W4/int4→int8、Qwen3.5、AscendMLAAttentionSpec、`page_size_padded`、`real_page_size_bytes`、KV-cache 页整除失败、KDA/GDN state、speculative decode。材料中的修复方向、单测要求和兼容性判断均保留为未验证或不完整结论；原始块仍可独立检索。

## Materials

- [K3 MLA/KDA KV-cache 页对齐与 W4 适配注意事项](blocks/e73de8e19e9953baec5275c56f5d9562cf074bce04a9ed80df76c3db9ca58ec7.md): 该块指出 K3 的整除失败据报告来自 AscendMLAAttentionSpec 未兑现 `page_size_padded`：实际页为 884736 B，公共 padded 页为 912384 B，余数正好对应 conv state；建议覆盖 `real_page_size_bytes`、保留 merge 时的 padding，并补充相关单测，但这些仍是分析结论和建议，未在此块中验证修复。用户特别提醒 w4 权重需将两个 int4 打包进一个 int8；同时，K3 的 KDA state 与 Qwen3.5 的 GDN state 形状和 dtype 不同，`num_spec` 被忽略意味着 speculative decode 尚不能据此宣称完整支持。

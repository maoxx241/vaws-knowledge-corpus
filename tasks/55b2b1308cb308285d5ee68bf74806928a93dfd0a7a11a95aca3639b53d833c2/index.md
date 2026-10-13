---
blocks:
- block_id: 1df2025039bdcf62c194c1eb7738158b651682b8e6ee2884871a3effeef88b3c
  indexed: true
  sha256: 49c8f90cfca62c7f60970262bf843102f35896c5dd17b6fa6133314ab5a603e6
  source_range:
    end: 3593274
    part: 0
    start: 0
  summary: 在 vLLM 0.27.1 配套的 vLLM-Ascend MRV2 中，首轴非连续 KV cache 布局已支持，但不会减少按 KV group 调用 `builder.build()` 的次数；K3 GQA DSpark 当前仍为 20 次（MLA 5、Mamba/KDA 14、GQA 1）。vLLM 0.27.1 虽有 `supports_update_block_table`/`update_block_table()` 框架 API，Ascend 的 MRV2 调用链及 MLA/GDN/GQA builder 尚未打通；上游没有承诺迁移 MRV1 缓存方案，且相关 GDN PR 曾关闭未合并。建议性结论是迁移难度中等偏上，GDN 可能先将总次数降至 7，MLA 适配后理论上降至 3，但 Spec Decode、prefix cache、chunked prefill、ACL Graph buffer、stride/state index 刷新等正确性仍需验证，不能视为已完成或已验证成功。
  title: K3 GQA DSpark：MRV2 首轴非连续与 metadata build
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 55b2b1308cb308285d5ee68bf74806928a93dfd0a7a11a95aca3639b53d833c2
  kind: experience
  material_digest: e8888391981c62976e188c0ba1a3f9d2d3956ca156b0533286ec079aac5167f3
  revision: 80e067e6058474b010862b02abd49affbea31d3447d1654d4acb8c3e0e8a2565
  schema: mindie-entry/3
  summary: 当前可检索主线是：vLLM 0.27.1 + vLLM-Ascend MRV2 + 首轴非连续布局下，实际仍为 20 次 metadata build；布局优化与跨 KV group metadata cache/update block table 是相互独立的能力。上游 API 存在但 Ascend MRV2 尚未接入，GDN updater 方案曾有未合并 PR，MLA/GDN/GQA 适配、缓存键正确性及 Spec Decode 等结果仍不确定；此前关于 0.27.0 的判断已被更正为 0.27.1 配套。
  title: K3 GQA DSpark build 次数与 MRV2 优化线索
navigation: 当前可检索主线是：vLLM 0.27.1 + vLLM-Ascend MRV2 + 首轴非连续布局下，实际仍为 20 次 metadata build；布局优化与跨 KV group metadata cache/update block table 是相互独立的能力。上游 API 存在但 Ascend MRV2 尚未接入，GDN updater 方案曾有未合并 PR，MLA/GDN/GQA 适配、缓存键正确性及 Spec Decode 等结果仍不确定；此前关于 0.27.0 的判断已被更正为 0.27.1 配套。
schema: mindie-material-task/1
status: complete
task_id: 55b2b1308cb308285d5ee68bf74806928a93dfd0a7a11a95aca3639b53d833c2
---

# K3 GQA DSpark build 次数与 MRV2 优化线索

当前可检索主线是：vLLM 0.27.1 + vLLM-Ascend MRV2 + 首轴非连续布局下，实际仍为 20 次 metadata build；布局优化与跨 KV group metadata cache/update block table 是相互独立的能力。上游 API 存在但 Ascend MRV2 尚未接入，GDN updater 方案曾有未合并 PR，MLA/GDN/GQA 适配、缓存键正确性及 Spec Decode 等结果仍不确定；此前关于 0.27.0 的判断已被更正为 0.27.1 配套。

## Materials

- [K3 GQA DSpark：MRV2 首轴非连续与 metadata build](blocks/1df2025039bdcf62c194c1eb7738158b651682b8e6ee2884871a3effeef88b3c.md): 在 vLLM 0.27.1 配套的 vLLM-Ascend MRV2 中，首轴非连续 KV cache 布局已支持，但不会减少按 KV group 调用 `builder.build()` 的次数；K3 GQA DSpark 当前仍为 20 次（MLA 5、Mamba/KDA 14、GQA 1）。vLLM 0.27.1 虽有 `supports_update_block_table`/`update_block_table()` 框架 API，Ascend 的 MRV2 调用链及 MLA/GDN/GQA builder 尚未打通；上游没有承诺迁移 MRV1 缓存方案，且相关 GDN PR 曾关闭未合并。建议性结论是迁移难度中等偏上，GDN 可能先将总次数降至 7，MLA 适配后理论上降至 3，但 Spec Decode、prefix cache、chunked prefill、ACL Graph buffer、stride/state index 刷新等正确性仍需验证，不能视为已完成或已验证成功。

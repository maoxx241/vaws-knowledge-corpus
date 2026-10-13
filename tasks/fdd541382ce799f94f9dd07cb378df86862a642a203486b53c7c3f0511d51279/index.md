---
blocks:
- block_id: d3269fea4d492c12a12a4df4e219d609fe06b6037ebb82ffa4b000745f19f6bf
  indexed: true
  sha256: 18f23645f6b7c26d2019f1b45e9726b1a1fe13dd6f57ab64a228c782c9a7b9bb
  source_range:
    end: 1475098
    part: 0
    start: 0
  summary: '只读审计认为“截断所有 FullAttention group”补丁对 K3 基本是 no-op：相关 scheduler block 均为 384，129408=337×384，普通 serving 也不经过 hybrid Mamba 的 longest-hit finder；现有 coordinator 修复和 `curr_hit_length` 单调性加固尚不能解释 c2 全部 `!`。更具体但未验证的缺口位于 pinned vLLM core 的 Mamba align 写路径：state block 可能在非边界或含未提交 draft token 时按边界 hash 写入 prefix cache，上游 PR #50457 的 `_state_write_tokens` 与 mask 逻辑据称可修复该语义错配。建议用跨 384-token 边界的 128K→1024 A/B 验证，但若 c2 前独立清空 cache 且命中完全来自对齐 prefill，则应回查 Mamba state copy/metadata 链路；PCP 结论也被限定为 MRV1/Kimi KDA 不支持，而仓库整体仍保留 MRV2、connector 和 GDN 能力。'
  title: K3 长命中与 Mamba prefix-cache ownership 审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: fdd541382ce799f94f9dd07cb378df86862a642a203486b53c7c3f0511d51279
  kind: experience
  material_digest: 76da8a5955e22f9a48e3a9803242277d776f8b4222cf0e789c42ca69ac90cb1e
  revision: da8b7f780e70dc9ae8e6be2ff2fc2bb482dcfccc3f270ff35dab6ba4e54926b8
  schema: mindie-entry/3
  summary: 已排除或基本降级 FullAttention group 截断、普通 serving 中的 hybrid longest-hit finder，以及将 coordinator 作为唯一根因的解释；这些结论仍是审计判断，不是验证成功。当前未完成项是验证 Mamba align 写路径中的 boundary hash/state 错配，优先执行跨 384-token decode 边界的 128K→1024 A/B，并根据是否复现决定继续检查 state copy/metadata 消费链路。
  title: 当前任务导航：验证 K3 长命中后的 Mamba 状态恢复路径
navigation: 已排除或基本降级 FullAttention group 截断、普通 serving 中的 hybrid longest-hit finder，以及将 coordinator 作为唯一根因的解释；这些结论仍是审计判断，不是验证成功。当前未完成项是验证 Mamba align 写路径中的 boundary hash/state 错配，优先执行跨 384-token decode 边界的 128K→1024 A/B，并根据是否复现决定继续检查 state copy/metadata 消费链路。
schema: mindie-material-task/1
status: complete
task_id: fdd541382ce799f94f9dd07cb378df86862a642a203486b53c7c3f0511d51279
---

# 当前任务导航：验证 K3 长命中后的 Mamba 状态恢复路径

已排除或基本降级 FullAttention group 截断、普通 serving 中的 hybrid longest-hit finder，以及将 coordinator 作为唯一根因的解释；这些结论仍是审计判断，不是验证成功。当前未完成项是验证 Mamba align 写路径中的 boundary hash/state 错配，优先执行跨 384-token decode 边界的 128K→1024 A/B，并根据是否复现决定继续检查 state copy/metadata 消费链路。

## Materials

- [K3 长命中与 Mamba prefix-cache ownership 审计](blocks/d3269fea4d492c12a12a4df4e219d609fe06b6037ebb82ffa4b000745f19f6bf.md): 只读审计认为“截断所有 FullAttention group”补丁对 K3 基本是 no-op：相关 scheduler block 均为 384，129408=337×384，普通 serving 也不经过 hybrid Mamba 的 longest-hit finder；现有 coordinator 修复和 `curr_hit_length` 单调性加固尚不能解释 c2 全部 `!`。更具体但未验证的缺口位于 pinned vLLM core 的 Mamba align 写路径：state block 可能在非边界或含未提交 draft token 时按边界 hash 写入 prefix cache，上游 PR #50457 的 `_state_write_tokens` 与 mask 逻辑据称可修复该语义错配。建议用跨 384-token 边界的 128K→1024 A/B 验证，但若 c2 前独立清空 cache 且命中完全来自对齐 prefill，则应回查 Mamba state copy/metadata 链路；PCP 结论也被限定为 MRV1/Kimi KDA 不支持，而仓库整体仍保留 MRV2、connector 和 GDN 能力。

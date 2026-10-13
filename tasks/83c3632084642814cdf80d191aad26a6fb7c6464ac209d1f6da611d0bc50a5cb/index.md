---
blocks:
- block_id: 9db42be251b42980cb203d502c0471ffa506a879fe18a4b2a7b693377446657b
  indexed: true
  sha256: 6cd7a629cae440ad2043cdd151a15687fb035a0ea0d64ab626a23ac534212484
  source_range:
    end: 2745911
    part: 0
    start: 0
  summary: '讨论认为 Block Table 无需拷贝 KV Cache，可通过固定数量分片、Query 扩展、Local LSE 与 `npu_attention_update` 合并；但 A5 的 FIA v2 在 K3 MLA + PageAttention 组合下能否返回真实 float32 LSE，仍是必须实机验证的前置条件，不能视为已验证成功。ACLGraph 方案要求固定 `num_splits`、静态 shape、预分配 buffer、图缓存键及 replay task update，且需验证 32 路 Reduce、workspace、ACLGraph replay 和有效 batch 放大等约束；此前定位 issue #36 经历了错误仓库、404、禁用 Issues、无关 PR 和浏览器上下文失效等失败尝试，最终确认目标为 `vllm-ascend/vllm-ascend-kimi-k3/issues/36` 并发布评论。'
  title: 'A5 FIA 分片与 ACLGraph 方案及 Issue #36 发布'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 83c3632084642814cdf80d191aad26a6fb7c6464ac209d1f6da611d0bc50a5cb
  kind: experience
  material_digest: 554d0e1fc7ee0dcebba6cd9bcc619a41045d0bf3fdf50ac7b0c8f83d166b7862
  revision: a5c56a38ec7016c22f941b83e1fdf51a042636f25564afea7d90d95388dc06e9
  schema: mindie-entry/3
  summary: '已完成一批关于 A5 FIA 长上下文分片、Local LSE 精确合并和 FULL_DECODE_ONLY ACLGraph 适配的分析，并将详细方案发布到 Issue #36。核心未决项仍是 A5 实机上的 FIA v2 LSE 能力及数值、图捕获/回放、workspace 和性能验证；未修改代码或远端服务。'
  title: '当前任务导航：A5 FIA 分片、ACLGraph 与 Issue #36'
navigation: '已完成一批关于 A5 FIA 长上下文分片、Local LSE 精确合并和 FULL_DECODE_ONLY ACLGraph 适配的分析，并将详细方案发布到 Issue #36。核心未决项仍是 A5 实机上的 FIA v2 LSE 能力及数值、图捕获/回放、workspace 和性能验证；未修改代码或远端服务。'
schema: mindie-material-task/1
status: complete
task_id: 83c3632084642814cdf80d191aad26a6fb7c6464ac209d1f6da611d0bc50a5cb
---

# 当前任务导航：A5 FIA 分片、ACLGraph 与 Issue #36

已完成一批关于 A5 FIA 长上下文分片、Local LSE 精确合并和 FULL_DECODE_ONLY ACLGraph 适配的分析，并将详细方案发布到 Issue #36。核心未决项仍是 A5 实机上的 FIA v2 LSE 能力及数值、图捕获/回放、workspace 和性能验证；未修改代码或远端服务。

## Materials

- [A5 FIA 分片与 ACLGraph 方案及 Issue #36 发布](blocks/9db42be251b42980cb203d502c0471ffa506a879fe18a4b2a7b693377446657b.md): 讨论认为 Block Table 无需拷贝 KV Cache，可通过固定数量分片、Query 扩展、Local LSE 与 `npu_attention_update` 合并；但 A5 的 FIA v2 在 K3 MLA + PageAttention 组合下能否返回真实 float32 LSE，仍是必须实机验证的前置条件，不能视为已验证成功。ACLGraph 方案要求固定 `num_splits`、静态 shape、预分配 buffer、图缓存键及 replay task update，且需验证 32 路 Reduce、workspace、ACLGraph replay 和有效 batch 放大等约束；此前定位 issue #36 经历了错误仓库、404、禁用 Issues、无关 PR 和浏览器上下文失效等失败尝试，最终确认目标为 `vllm-ascend/vllm-ascend-kimi-k3/issues/36` 并发布评论。

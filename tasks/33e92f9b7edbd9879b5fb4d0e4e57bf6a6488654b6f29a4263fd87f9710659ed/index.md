---
blocks:
- block_id: 72bdbc09ad34709962f63e920f6286459ac1c1a4da10a2bc443a88fe2dde7b35
  indexed: true
  sha256: 25eed207945688d3347e6d5b6a105bcdb2f552c66d00bd00005c12ea0d497084
  source_range:
    end: 2478854
    part: 0
    start: 0
  summary: 报告围绕“首 token 正常、后续全为 0 且全部接受”的现象，计划只读反查 target sampler 输出、`next_token_ids`、draft 输入 buffer、ACLGraph replay 的 aux hidden state/logits 以及 rejection sampler，并比较 FULL_DECODE_ONLY 下 eager drafter 与 target graph 的边界更新。消息还声称在本地历史分支发现与 `K=7`、capture size 16 对应的 ACLGraph/KDA 修复痕迹，但尚未确认哪些是根因修复、哪些只是探针；这些内容均不是已验证成功。
  title: ACLGraph 解码链路与历史修复线索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 33e92f9b7edbd9879b5fb4d0e4e57bf6a6488654b6f29a4263fd87f9710659ed
  kind: experience
  material_digest: 5b1bb72637e54591ebdac01a19930f8609b5b52a635b6d46d478d9fd0608c3a5
  revision: 89956183a6055fea9afab8d67c93e69cb3606fb6d9d9866a8464f1e31c13dee5
  schema: mindie-entry/3
  summary: 当前索引聚焦于首 token 正常而后续 token 全为 0、全部接受的 ACLGraph 解码异常，以及围绕 sampler、buffer、hidden state/logits、rejection sampler 和 FULL_DECODE_ONLY 边界更新的只读诊断线索。历史分支中据称存在匹配 `K=7 + capture size 16` 的修复痕迹，但修复性质和有效性仍未确认；后续应保留失败尝试、不确定性及任何更正或未完成结果。
  title: 当前任务导航：ACLGraph/KDA 首 token 异常
navigation: 当前索引聚焦于首 token 正常而后续 token 全为 0、全部接受的 ACLGraph 解码异常，以及围绕 sampler、buffer、hidden state/logits、rejection sampler 和 FULL_DECODE_ONLY 边界更新的只读诊断线索。历史分支中据称存在匹配 `K=7 + capture size 16` 的修复痕迹，但修复性质和有效性仍未确认；后续应保留失败尝试、不确定性及任何更正或未完成结果。
schema: mindie-material-task/1
status: complete
task_id: 33e92f9b7edbd9879b5fb4d0e4e57bf6a6488654b6f29a4263fd87f9710659ed
---

# 当前任务导航：ACLGraph/KDA 首 token 异常

当前索引聚焦于首 token 正常而后续 token 全为 0、全部接受的 ACLGraph 解码异常，以及围绕 sampler、buffer、hidden state/logits、rejection sampler 和 FULL_DECODE_ONLY 边界更新的只读诊断线索。历史分支中据称存在匹配 `K=7 + capture size 16` 的修复痕迹，但修复性质和有效性仍未确认；后续应保留失败尝试、不确定性及任何更正或未完成结果。

## Materials

- [ACLGraph 解码链路与历史修复线索](blocks/72bdbc09ad34709962f63e920f6286459ac1c1a4da10a2bc443a88fe2dde7b35.md): 报告围绕“首 token 正常、后续全为 0 且全部接受”的现象，计划只读反查 target sampler 输出、`next_token_ids`、draft 输入 buffer、ACLGraph replay 的 aux hidden state/logits 以及 rejection sampler，并比较 FULL_DECODE_ONLY 下 eager drafter 与 target graph 的边界更新。消息还声称在本地历史分支发现与 `K=7`、capture size 16 对应的 ACLGraph/KDA 修复痕迹，但尚未确认哪些是根因修复、哪些只是探针；这些内容均不是已验证成功。

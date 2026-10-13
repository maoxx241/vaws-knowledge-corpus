---
blocks:
- block_id: a8e561a8cfce77ab326d2a85a0b45196a8ce034585bd9e40ed927a800e1da000
  indexed: true
  sha256: 49e73a81a38132179e65c7aac734392ca0cf16141439e609f3eee1377f665160
  source_range:
    end: 17227253
    part: 0
    start: 0
  summary: '在 Kimi K3、TP8/EP8、五层 INT4 对照中，AllGather EP 与 AllToAll/MC2 从首 token 分叉，第一处偏差位于第 1 个 MoE 层的专家分片合并之后；routing、expert GMM 和 collective 本身基本一致，根因是 AllGather 在 RMSNorm 前未先归约 routed partial，导致非线性变换顺序错误。最小修复提前归约 routed partial 后，AllToAll、MC2 与 AllGather 输出对齐，layer 1 max diff 从 0.56787 降至 0.00098，并已提交 draft PR #28；验证覆盖的是 FC1 关闭场景，FC1 开启路径未重新测试。后续讨论认为 routed down/up projection 可以采用 ColumnParallel + gather 与 RowParallel + reduce，但若支持 DP4×TP16×EP4，需要将 MoE 改为 EP4 与 expert-TP16 的二维切分，dispatcher、expert GMM、RMSNorm 及通信语义均需相应改造。'
  title: K3 AllGather EP 精度修复与二维 TP/EP 方案讨论
- block_id: 6a1bc7c6e1bf1f641bea1595855ba3b5c8b8c59e69e1e649343e7cf205ab0029
  indexed: true
  sha256: db5b7bb4444e6a66f2ff32306f43b4b8513ef2c43d2ad4e629c7df4b8ca92122
  source_range:
    end: 17227253
    part: 1
    start: 0
  summary: 方案文档的目标拓扑是物理 64 卡上的 DP4×TP16：Dense/Attention 维持 TP16×DP4，MoE expert 沿 DP 轴形成 EP4，并沿 TP 轴形成 expert-TP16，而不是将 DP4×TP16×EP4 误算为 256 卡。用户要求按参考文件 `<redacted:local-path:b8dd889167fe>/refs/kimi-k3-analysis.xlsx` 重做六页：1–3 页为 AllGather EP，分别覆盖现有 TP16/DP4/EP64、router down/up 切 TP 但专家纯 EP64、以及 TP16/DP4/EP4；4–6 页为 AllToAll EP 的对应三种策略。新版应按整个 MoE 的全量算子、shape、通信算子、数据流、计算量和通信量估算，且纳入切 TP 对原有算子和通信的影响；此前 shape 的 `TEXT()` 公式已因 Numbers/Excel 兼容问题改为静态值，文件通过 xlsx 结构校验，但最终版式与内容验收尚未在材料中完成。
  title: 六页 MoE 全量算子与通信量分析文档重构
- block_id: 57e5767c7e4a4cfc713d3fee127e0237fef2cc1cc6ee4a50fd93ffb1ae42c4c6
  indexed: true
  sha256: d3d09ae29f7ea131c48d448ad6285e31aabd08ec6f560152baa758a4dfef2fb7
  source_range:
    end: 31858739
    part: 0
    start: 17227253
  summary: 先前版本据称已生成六页 XLSX，覆盖 AllGather EP 与 AllToAll EP 的三种 TP/DP/EP 场景，并声称完成显存估算、公式扫描和结构/渲染检查；随后发现显存汇总维度不足，且用户要求删除 H/I 通信列、明确 G 列并行类型，并将 MoE 数据流细化为逐算子逐通信组网图。修改过程中曾出现写入宽度不一致和本地 Node 括号语法错误，批次据称未发生半写入；之后报告称六个 tab 已提交，公式回读无 `#REF!`、`#DIV/0!`、`#VALUE!`、`#NAME?`，但实际渲染检查只明确提到部分页面，并发现 A 列过窄后计划统一加宽。相关文件路径、Google Sheets 链接和上传文件名含有 `<redacted:...>` 占位符，不能据此恢复私有信息。
  title: 六页 MoE 表格重构与组网图修改记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b0633ac35bb9d5c82f615aa4c79bd6ef71a64967eda776bbe6032f31d03f2453
  kind: experience
  material_digest: 0503ce8021ef4cb6f5476031e62e64ecdd3cd74144b80ac28e348e8f2db972cd
  revision: 32544085e6d7f73fe8a8b011a64041b079a70b426a6af7be036e58738f66eeb2
  schema: mindie-entry/3
  summary: 当前任务涉及六页 MoE 技术分析：1–3 页 AllGather EP，4–6 页 AllToAll EP，分别对应 TP16/DP4/EP64、仅 routed down/up 切 TP16 且专家 EP64、以及 TP16/DP4/EP4。新记录显示需求已从基础算子、shape、通信、计算/通信量和显存估算，进一步扩展为删除 H/I 列、在 G 列标注 `Replicated`、`ColumnParallel`、`RowParallel`、`ExpertLocal`、`Collective`、`Local`，并按 Routed/Shared/Network lane 细化 dispatch/combine/finalize、DP/EP collective、TP reduction 与 RMSNorm 顺序及 shape/group。此前存在显存汇总不足、批量写入宽度错误和本地脚本语法错误；后续报告称六个 tab 已提交且公式错误扫描通过，但渲染验收并非所有页面都有明确独立证据，A 列过窄的修正结果也未在该材料中确认。
  title: Kimi K3 MoE 六场景 TP/EP 分析文档：表格与组网图修订
navigation: 当前任务涉及六页 MoE 技术分析：1–3 页 AllGather EP，4–6 页 AllToAll EP，分别对应 TP16/DP4/EP64、仅 routed down/up 切 TP16 且专家 EP64、以及 TP16/DP4/EP4。新记录显示需求已从基础算子、shape、通信、计算/通信量和显存估算，进一步扩展为删除 H/I 列、在 G 列标注 `Replicated`、`ColumnParallel`、`RowParallel`、`ExpertLocal`、`Collective`、`Local`，并按 Routed/Shared/Network lane 细化 dispatch/combine/finalize、DP/EP collective、TP reduction 与 RMSNorm 顺序及 shape/group。此前存在显存汇总不足、批量写入宽度错误和本地脚本语法错误；后续报告称六个 tab 已提交且公式错误扫描通过，但渲染验收并非所有页面都有明确独立证据，A 列过窄的修正结果也未在该材料中确认。
schema: mindie-material-task/1
status: complete
task_id: b0633ac35bb9d5c82f615aa4c79bd6ef71a64967eda776bbe6032f31d03f2453
---

# Kimi K3 MoE 六场景 TP/EP 分析文档：表格与组网图修订

当前任务涉及六页 MoE 技术分析：1–3 页 AllGather EP，4–6 页 AllToAll EP，分别对应 TP16/DP4/EP64、仅 routed down/up 切 TP16 且专家 EP64、以及 TP16/DP4/EP4。新记录显示需求已从基础算子、shape、通信、计算/通信量和显存估算，进一步扩展为删除 H/I 列、在 G 列标注 `Replicated`、`ColumnParallel`、`RowParallel`、`ExpertLocal`、`Collective`、`Local`，并按 Routed/Shared/Network lane 细化 dispatch/combine/finalize、DP/EP collective、TP reduction 与 RMSNorm 顺序及 shape/group。此前存在显存汇总不足、批量写入宽度错误和本地脚本语法错误；后续报告称六个 tab 已提交且公式错误扫描通过，但渲染验收并非所有页面都有明确独立证据，A 列过窄的修正结果也未在该材料中确认。

## Materials

- [K3 AllGather EP 精度修复与二维 TP/EP 方案讨论](blocks/a8e561a8cfce77ab326d2a85a0b45196a8ce034585bd9e40ed927a800e1da000.md): 在 Kimi K3、TP8/EP8、五层 INT4 对照中，AllGather EP 与 AllToAll/MC2 从首 token 分叉，第一处偏差位于第 1 个 MoE 层的专家分片合并之后；routing、expert GMM 和 collective 本身基本一致，根因是 AllGather 在 RMSNorm 前未先归约 routed partial，导致非线性变换顺序错误。最小修复提前归约 routed partial 后，AllToAll、MC2 与 AllGather 输出对齐，layer 1 max diff 从 0.56787 降至 0.00098，并已提交 draft PR #28；验证覆盖的是 FC1 关闭场景，FC1 开启路径未重新测试。后续讨论认为 routed down/up projection 可以采用 ColumnParallel + gather 与 RowParallel + reduce，但若支持 DP4×TP16×EP4，需要将 MoE 改为 EP4 与 expert-TP16 的二维切分，dispatcher、expert GMM、RMSNorm 及通信语义均需相应改造。
- [六页 MoE 全量算子与通信量分析文档重构](blocks/6a1bc7c6e1bf1f641bea1595855ba3b5c8b8c59e69e1e649343e7cf205ab0029.md): 方案文档的目标拓扑是物理 64 卡上的 DP4×TP16：Dense/Attention 维持 TP16×DP4，MoE expert 沿 DP 轴形成 EP4，并沿 TP 轴形成 expert-TP16，而不是将 DP4×TP16×EP4 误算为 256 卡。用户要求按参考文件 `<redacted:local-path:b8dd889167fe>/refs/kimi-k3-analysis.xlsx` 重做六页：1–3 页为 AllGather EP，分别覆盖现有 TP16/DP4/EP64、router down/up 切 TP 但专家纯 EP64、以及 TP16/DP4/EP4；4–6 页为 AllToAll EP 的对应三种策略。新版应按整个 MoE 的全量算子、shape、通信算子、数据流、计算量和通信量估算，且纳入切 TP 对原有算子和通信的影响；此前 shape 的 `TEXT()` 公式已因 Numbers/Excel 兼容问题改为静态值，文件通过 xlsx 结构校验，但最终版式与内容验收尚未在材料中完成。
- [六页 MoE 表格重构与组网图修改记录](blocks/57e5767c7e4a4cfc713d3fee127e0237fef2cc1cc6ee4a50fd93ffb1ae42c4c6.md): 先前版本据称已生成六页 XLSX，覆盖 AllGather EP 与 AllToAll EP 的三种 TP/DP/EP 场景，并声称完成显存估算、公式扫描和结构/渲染检查；随后发现显存汇总维度不足，且用户要求删除 H/I 通信列、明确 G 列并行类型，并将 MoE 数据流细化为逐算子逐通信组网图。修改过程中曾出现写入宽度不一致和本地 Node 括号语法错误，批次据称未发生半写入；之后报告称六个 tab 已提交，公式回读无 `#REF!`、`#DIV/0!`、`#VALUE!`、`#NAME?`，但实际渲染检查只明确提到部分页面，并发现 A 列过窄后计划统一加宽。相关文件路径、Google Sheets 链接和上传文件名含有 `<redacted:...>` 占位符，不能据此恢复私有信息。

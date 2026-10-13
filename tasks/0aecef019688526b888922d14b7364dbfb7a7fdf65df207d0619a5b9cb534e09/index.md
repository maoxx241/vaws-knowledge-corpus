---
blocks:
- block_id: debd6e850e363584dfb2964275f49f56349ae2910e6cb4564cdb75b14a81c815
  indexed: true
  sha256: eebdf79f34511e00770bce450d20497c46a41874ba4fb3406b6ea9430089f66f
  source_range:
    end: 1140485
    part: 0
    start: 0
  summary: 对 clientfix2 的分类器逻辑进行只读审计，发现链路若先出现“有限但内容已变”、随后才出现 nonfinite，内置标签可能把后者误判为首坏点；计划用原始 rank JSON 按时间独立重算，但该重算结果尚未提供。首次 warm q384 因 19 vs 20 KV groups fail-close，未产生有效矩阵或 trace；已提出保留完整 20 组及原索引、补充 group 5 的 draft block table/slot mapping、schema 57→59，并指出 client UT 的 range(5,19) 应改为 range(6,20)，但这些修正与后续 raw trace 验证均不可视为已验证成功。
  title: clientfix2 首坏阶段审计与 warm q384 失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0aecef019688526b888922d14b7364dbfb7a7fdf65df207d0619a5b9cb534e09
  kind: experience
  material_digest: 7b2d25959c1abe12bb9de7488522d89140958d16841bc9d27a6860a77bd8074a
  revision: 7ae27cdb5c7733111247692232721807ad5a7e920e73ed402b5824c0a75cafe1
  schema: mindie-entry/3
  summary: 核心线索是 warm q384 在 19 vs 20 KV groups 处失败，未留下有效矩阵或 trace；现有结论还包括分类器可能误归因 finite-wrong，以及待用原始 rank JSON 独立重算首坏阶段。检索关键词：clientfix2、warm q384、KV groups、fail-close、nonfinite、finite-wrong、raw trace、group 5、draft block table、slot mapping、schema 57→59、range(5,19)、range(6,20)。提出的 schema/映射/UT 修正和“无需增加采点”的判断属于报告中的待验证内容，后续 raw trace 结果仍未完成。
  title: 当前任务导航：clientfix2 / warm q384 失败审计
navigation: 核心线索是 warm q384 在 19 vs 20 KV groups 处失败，未留下有效矩阵或 trace；现有结论还包括分类器可能误归因 finite-wrong，以及待用原始 rank JSON 独立重算首坏阶段。检索关键词：clientfix2、warm q384、KV groups、fail-close、nonfinite、finite-wrong、raw trace、group 5、draft block table、slot mapping、schema 57→59、range(5,19)、range(6,20)。提出的 schema/映射/UT 修正和“无需增加采点”的判断属于报告中的待验证内容，后续 raw trace 结果仍未完成。
schema: mindie-material-task/1
status: complete
task_id: 0aecef019688526b888922d14b7364dbfb7a7fdf65df207d0619a5b9cb534e09
---

# 当前任务导航：clientfix2 / warm q384 失败审计

核心线索是 warm q384 在 19 vs 20 KV groups 处失败，未留下有效矩阵或 trace；现有结论还包括分类器可能误归因 finite-wrong，以及待用原始 rank JSON 独立重算首坏阶段。检索关键词：clientfix2、warm q384、KV groups、fail-close、nonfinite、finite-wrong、raw trace、group 5、draft block table、slot mapping、schema 57→59、range(5,19)、range(6,20)。提出的 schema/映射/UT 修正和“无需增加采点”的判断属于报告中的待验证内容，后续 raw trace 结果仍未完成。

## Materials

- [clientfix2 首坏阶段审计与 warm q384 失败](blocks/debd6e850e363584dfb2964275f49f56349ae2910e6cb4564cdb75b14a81c815.md): 对 clientfix2 的分类器逻辑进行只读审计，发现链路若先出现“有限但内容已变”、随后才出现 nonfinite，内置标签可能把后者误判为首坏点；计划用原始 rank JSON 按时间独立重算，但该重算结果尚未提供。首次 warm q384 因 19 vs 20 KV groups fail-close，未产生有效矩阵或 trace；已提出保留完整 20 组及原索引、补充 group 5 的 draft block table/slot mapping、schema 57→59，并指出 client UT 的 range(5,19) 应改为 range(6,20)，但这些修正与后续 raw trace 验证均不可视为已验证成功。

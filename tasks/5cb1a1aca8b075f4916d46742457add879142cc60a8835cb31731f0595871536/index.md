---
blocks:
- block_id: 604cd52e3cae88fda69bed63e9bad8da010b5fae22801e2955b2c210adb1cd18
  indexed: true
  sha256: 21b9a6892cbc93c72875aae995b7255211d065f94c2bd1a62fd73af43f12e98d
  source_range:
    end: 1191107
    part: 0
    start: 0
  summary: 只读观察显示 64 个 worker 已启用 materialized GQA 捕获，但现有日志没有首轮张量或 token 级诊断；`/metrics` 中第 1 位接收率约为 72%–89%（示例报告为 82.1%），第 2 位降至 22%–56%，第 5–7 位几乎为零。该模式不像 aux 完全错位，更可能涉及 7-token 块内 Markov/query 顺序或逐位置 logits 问题，但 KDA 跨步漂移及 verifier 中 draft/target ID 映射仍未排除。建议后续精确重启时仅在 DP0/TP0 用专用环境变量采集前 8 步 aux、draft、target verify 与 KDA state-copy trace；当前服务未改动、未重启、未发请求，也未触发编译。
  title: DSpark 7-token 接收率衰减与只读诊断方案
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5cb1a1aca8b075f4916d46742457add879142cc60a8835cb31731f0595871536
  kind: experience
  material_digest: 039c47ded819ebe8645ee439647c4b485565d85bbe115fced3643cc1de169fe0
  revision: 3035d4270e1acab47e2c5a77f716729222ee53b748e4bd9e1c45de0847639fd1
  schema: mindie-entry/3
  summary: 当前线索集中于 7-token 块内的 query/Markov 顺序或逐位置 logits 问题：首位仍可接收、后续位置快速衰减。KDA 跨步漂移和 verifier 的 draft/target ID 展平映射尚未确认；现有结果是未验证的方向判断，后续应通过 DP0/TP0 的前 8 步 aux、draft、target verify、KDA state-copy trace 继续区分。
  title: DSpark 诊断导航
navigation: 当前线索集中于 7-token 块内的 query/Markov 顺序或逐位置 logits 问题：首位仍可接收、后续位置快速衰减。KDA 跨步漂移和 verifier 的 draft/target ID 展平映射尚未确认；现有结果是未验证的方向判断，后续应通过 DP0/TP0 的前 8 步 aux、draft、target verify、KDA state-copy trace 继续区分。
schema: mindie-material-task/1
status: complete
task_id: 5cb1a1aca8b075f4916d46742457add879142cc60a8835cb31731f0595871536
---

# DSpark 诊断导航

当前线索集中于 7-token 块内的 query/Markov 顺序或逐位置 logits 问题：首位仍可接收、后续位置快速衰减。KDA 跨步漂移和 verifier 的 draft/target ID 展平映射尚未确认；现有结果是未验证的方向判断，后续应通过 DP0/TP0 的前 8 步 aux、draft、target verify、KDA state-copy trace 继续区分。

## Materials

- [DSpark 7-token 接收率衰减与只读诊断方案](blocks/604cd52e3cae88fda69bed63e9bad8da010b5fae22801e2955b2c210adb1cd18.md): 只读观察显示 64 个 worker 已启用 materialized GQA 捕获，但现有日志没有首轮张量或 token 级诊断；`/metrics` 中第 1 位接收率约为 72%–89%（示例报告为 82.1%），第 2 位降至 22%–56%，第 5–7 位几乎为零。该模式不像 aux 完全错位，更可能涉及 7-token 块内 Markov/query 顺序或逐位置 logits 问题，但 KDA 跨步漂移及 verifier 中 draft/target ID 映射仍未排除。建议后续精确重启时仅在 DP0/TP0 用专用环境变量采集前 8 步 aux、draft、target verify 与 KDA state-copy trace；当前服务未改动、未重启、未发请求，也未触发编译。

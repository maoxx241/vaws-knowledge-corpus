---
blocks:
- block_id: da196bf29294aad2db8517f377d6e324768b12b74415472b249a84d2258de889
  indexed: true
  sha256: 8845bee8b1c4ac309ec2212fca6b08372792f02356e073fcd156faf1b91346f7
  source_range:
    end: 898616
    part: 0
    start: 0
  summary: 记录了旧组合的 DSpark K=7 指标（accepted/draft tokens 为 3050/5355，约 56.96%），并强调应使用 workload 前后的 counter delta，排除 `_created` 与错误分母；prefix cache 和共享专家多流当时尚未完成最终组合验证。随后报告最终四 rank 运行中启用了 prefix cache、共享专家多流、FlashComm1、显式 `tokenizer-mode=kimi_k3`，完成 target 352/352 shard 加载、DSpark draft 加载、torch.compile 与 `FULL_DECODE_ONLY` ACLGraph 捕获，health 返回 200；这些均为只读观察报告，仍需保留对 Chat、Tool Call、Completions、实际 cache 命中、delta 接收率及精度门禁的未完成或待确认项。关键环境/方法词包括 DP4/TP16/EP64、engine 0–3、16/16 workers、draft TP16、capture size 16、`vllm:spec_decode_num_accepted_tokens_total`、`vllm:spec_decode_num_draft_tokens_total`、`/metrics`、`/health`、自定义算子 `.so` 与 capture-size 性能告警。
  title: DSpark 四机最终组合启动与验证缺口
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a7e55cb97c988639beec506f1a4133a8e8816d69de4df14ebb90a59bfaab58b8
  kind: experience
  material_digest: f262d95828e5bd2f09d21b0c233e8e7da2a6b7fbdfbe515d6fe6905ebdb05c5e
  revision: 15bf8a4848368bb223db08319aeba4703dab1d3c9a9146a1d6ac15c8e8993e85
  schema: mindie-entry/3
  summary: 当前材料覆盖旧组合只读复核、最终特性组合启动跟踪及其报告的加载/编译/图捕获结果。后续检索应优先区分“旧累计 56.96%”与最终组合的 workload-delta 接收率，并继续追踪 API 功能、prefix-cache 实际命中、确定性精度及仍未完成的验证；已有报告不等同于独立确认的最终成功。
  title: Kimi-K3 DSpark 四机部署索引
navigation: 当前材料覆盖旧组合只读复核、最终特性组合启动跟踪及其报告的加载/编译/图捕获结果。后续检索应优先区分“旧累计 56.96%”与最终组合的 workload-delta 接收率，并继续追踪 API 功能、prefix-cache 实际命中、确定性精度及仍未完成的验证；已有报告不等同于独立确认的最终成功。
schema: mindie-material-task/1
status: complete
task_id: a7e55cb97c988639beec506f1a4133a8e8816d69de4df14ebb90a59bfaab58b8
---

# Kimi-K3 DSpark 四机部署索引

当前材料覆盖旧组合只读复核、最终特性组合启动跟踪及其报告的加载/编译/图捕获结果。后续检索应优先区分“旧累计 56.96%”与最终组合的 workload-delta 接收率，并继续追踪 API 功能、prefix-cache 实际命中、确定性精度及仍未完成的验证；已有报告不等同于独立确认的最终成功。

## Materials

- [DSpark 四机最终组合启动与验证缺口](blocks/da196bf29294aad2db8517f377d6e324768b12b74415472b249a84d2258de889.md): 记录了旧组合的 DSpark K=7 指标（accepted/draft tokens 为 3050/5355，约 56.96%），并强调应使用 workload 前后的 counter delta，排除 `_created` 与错误分母；prefix cache 和共享专家多流当时尚未完成最终组合验证。随后报告最终四 rank 运行中启用了 prefix cache、共享专家多流、FlashComm1、显式 `tokenizer-mode=kimi_k3`，完成 target 352/352 shard 加载、DSpark draft 加载、torch.compile 与 `FULL_DECODE_ONLY` ACLGraph 捕获，health 返回 200；这些均为只读观察报告，仍需保留对 Chat、Tool Call、Completions、实际 cache 命中、delta 接收率及精度门禁的未完成或待确认项。关键环境/方法词包括 DP4/TP16/EP64、engine 0–3、16/16 workers、draft TP16、capture size 16、`vllm:spec_decode_num_accepted_tokens_total`、`vllm:spec_decode_num_draft_tokens_total`、`/metrics`、`/health`、自定义算子 `.so` 与 capture-size 性能告警。

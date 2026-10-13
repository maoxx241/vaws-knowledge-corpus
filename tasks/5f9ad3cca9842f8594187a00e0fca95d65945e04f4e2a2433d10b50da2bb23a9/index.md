---
blocks:
- block_id: 93025195537633c0608d66ab304bd53ff00c681facce81823e8619996cb2395f
  indexed: true
  sha256: 033b6303ed8c8a365d428a39d4c5ba1345c930a95e2c5349e3e491d1059e250e
  source_range:
    end: 950918
    part: 0
    start: 0
  summary: 用户质疑 ci-new 分支遇到的问题可能与 dspark 特性无关，并要求先验证完整权重，再检查 5 层 P/D 分离；上下文提到截断权重目录缺少 tiktoken.model，但完整权重服务不受影响。只读审计报告称 prefix/spec decode 指标、`/reset_prefix_cache`（依赖 `VLLM_SERVER_DEV_MODE=1`）及 full-weight validator 没有必然错误；仍存在未固定 `X-data-parallel-rank` 的低概率路由假阴性风险。GPQA 需按 ID 核对 10 行、`error=null`、A–D 预测、`finish_reason=stop` 并检查末尾答案；Mooncake cold 的 `external_hits == prompt_tokens - 1` 被认为符合 N−1 实现，但这些检查与结论仍应视为待验证参考。
  title: ci-new 问题与完整权重验证审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5f9ad3cca9842f8594187a00e0fca95d65945e04f4e2a2433d10b50da2bb23a9
  kind: experience
  material_digest: 35b0767ccf969c8d84d5af4d1c4f32fdaf3da42ce197ddcbf606f61d979db1d5
  revision: 217edfc18142b995b6c52e6fd8cebe0aa14d49174735054983b572f307389367
  schema: mindie-entry/3
  summary: 本批材料聚焦 ci-new 问题是否与 dspark 相关、完整权重先行验证、5 层 P/D 分离，以及 prefix-cache、Prometheus、GPQA 和 DP 路由检查。已记录 tokenizer 缺失、环境变量依赖、指标/endpoint、N−1 断言和 GPQA 审计条件；报告中的成功或解释均未提升为已验证事实，DP 路由风险及后续核对仍未完成。
  title: 当前任务导航：完整权重与 ci-new 问题审计
navigation: 本批材料聚焦 ci-new 问题是否与 dspark 相关、完整权重先行验证、5 层 P/D 分离，以及 prefix-cache、Prometheus、GPQA 和 DP 路由检查。已记录 tokenizer 缺失、环境变量依赖、指标/endpoint、N−1 断言和 GPQA 审计条件；报告中的成功或解释均未提升为已验证事实，DP 路由风险及后续核对仍未完成。
schema: mindie-material-task/1
status: complete
task_id: 5f9ad3cca9842f8594187a00e0fca95d65945e04f4e2a2433d10b50da2bb23a9
---

# 当前任务导航：完整权重与 ci-new 问题审计

本批材料聚焦 ci-new 问题是否与 dspark 相关、完整权重先行验证、5 层 P/D 分离，以及 prefix-cache、Prometheus、GPQA 和 DP 路由检查。已记录 tokenizer 缺失、环境变量依赖、指标/endpoint、N−1 断言和 GPQA 审计条件；报告中的成功或解释均未提升为已验证事实，DP 路由风险及后续核对仍未完成。

## Materials

- [ci-new 问题与完整权重验证审计](blocks/93025195537633c0608d66ab304bd53ff00c681facce81823e8619996cb2395f.md): 用户质疑 ci-new 分支遇到的问题可能与 dspark 特性无关，并要求先验证完整权重，再检查 5 层 P/D 分离；上下文提到截断权重目录缺少 tiktoken.model，但完整权重服务不受影响。只读审计报告称 prefix/spec decode 指标、`/reset_prefix_cache`（依赖 `VLLM_SERVER_DEV_MODE=1`）及 full-weight validator 没有必然错误；仍存在未固定 `X-data-parallel-rank` 的低概率路由假阴性风险。GPQA 需按 ID 核对 10 行、`error=null`、A–D 预测、`finish_reason=stop` 并检查末尾答案；Mooncake cold 的 `external_hits == prompt_tokens - 1` 被认为符合 N−1 实现，但这些检查与结论仍应视为待验证参考。

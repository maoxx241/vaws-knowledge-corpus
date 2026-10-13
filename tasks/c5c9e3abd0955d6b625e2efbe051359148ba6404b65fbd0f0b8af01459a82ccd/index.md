---
blocks:
- block_id: ab28965bb15b346eacd84f1923aab3bec953080055f820346d7332b774c38df0
  indexed: true
  sha256: 401258e66c72d6b8c19b84fb7c794bdf84784481d22bc907c4011bf6e6b99fb3
  source_range:
    end: 1185834
    part: 0
    start: 0
  summary: 记录了双机 DP2×TP8、ACLGraph FULL_DECODE_ONLY、Prefix Cache align 的验证：首轮 gpu_memory_utilization=0.85 在 156/TP7 graph warmup 因 attention mask OOM，降至 0.75 后离线组合测试报告 hit/cold token、logprob 与 top-20 零差，但未覆盖 piecewise graph。随后服务化命令经历 zsh JSON 引号、缺少 vllm console script、--limit-mm-per-prompt 格式、trust-remote-code、fixture 的 tiktoken.model 及 tiktoken 路径缓存等修正；最终报告 /health=200、/v1/models 成功、completion HTTP 200 且两 rank 出现 Replaying aclgraph，但这些结果仍是来源中的报告，不能视为独立复核的 verified success。相关 PR 末尾据称已追加 154 API/DP rank 0 与 156 headless/DP rank 1 的启动命令，并记录了验证服务停止和 NPU 释放。
  title: 双机 DP2×TP8 ACLGraph 与 Prefix Cache 验证及服务命令
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c5c9e3abd0955d6b625e2efbe051359148ba6404b65fbd0f0b8af01459a82ccd
  kind: experience
  material_digest: 458b385fba2d32d9ba2e476b85c2154c781ce6b52bb2d26cb1615e9d1c752e44
  revision: 5034e2aa4fb8263e91770aa1384e7ff644629060b960e2798cef741e006b0e38
  schema: mindie-entry/3
  summary: '本批唯一材料集中于 154/156 双机 DP2×TP8 组合验证及 PR #4 服务启动命令。检索关键词：FULL_DECODE_ONLY、Replaying aclgraph、Prefix Cache align、1536-token、gpu_memory_utilization=0.85/0.75、TP7 attention mask OOM、HCCL、tiktoken.model、TIKTOKEN_CACHE_DIR、--limit-mm-per-prompt、trust-remote-code、/health、/v1/models、completion。保留的未覆盖项包括 piecewise graph；首轮 OOM、fixture 缺失与缓存问题及其后续修正应与最终报告区分检索。'
  title: 当前任务导航：双机 DP/TP、ACLGraph、Prefix Cache 与服务启动
navigation: '本批唯一材料集中于 154/156 双机 DP2×TP8 组合验证及 PR #4 服务启动命令。检索关键词：FULL_DECODE_ONLY、Replaying aclgraph、Prefix Cache align、1536-token、gpu_memory_utilization=0.85/0.75、TP7 attention mask OOM、HCCL、tiktoken.model、TIKTOKEN_CACHE_DIR、--limit-mm-per-prompt、trust-remote-code、/health、/v1/models、completion。保留的未覆盖项包括 piecewise graph；首轮 OOM、fixture 缺失与缓存问题及其后续修正应与最终报告区分检索。'
schema: mindie-material-task/1
status: complete
task_id: c5c9e3abd0955d6b625e2efbe051359148ba6404b65fbd0f0b8af01459a82ccd
---

# 当前任务导航：双机 DP/TP、ACLGraph、Prefix Cache 与服务启动

本批唯一材料集中于 154/156 双机 DP2×TP8 组合验证及 PR #4 服务启动命令。检索关键词：FULL_DECODE_ONLY、Replaying aclgraph、Prefix Cache align、1536-token、gpu_memory_utilization=0.85/0.75、TP7 attention mask OOM、HCCL、tiktoken.model、TIKTOKEN_CACHE_DIR、--limit-mm-per-prompt、trust-remote-code、/health、/v1/models、completion。保留的未覆盖项包括 piecewise graph；首轮 OOM、fixture 缺失与缓存问题及其后续修正应与最终报告区分检索。

## Materials

- [双机 DP2×TP8 ACLGraph 与 Prefix Cache 验证及服务命令](blocks/ab28965bb15b346eacd84f1923aab3bec953080055f820346d7332b774c38df0.md): 记录了双机 DP2×TP8、ACLGraph FULL_DECODE_ONLY、Prefix Cache align 的验证：首轮 gpu_memory_utilization=0.85 在 156/TP7 graph warmup 因 attention mask OOM，降至 0.75 后离线组合测试报告 hit/cold token、logprob 与 top-20 零差，但未覆盖 piecewise graph。随后服务化命令经历 zsh JSON 引号、缺少 vllm console script、--limit-mm-per-prompt 格式、trust-remote-code、fixture 的 tiktoken.model 及 tiktoken 路径缓存等修正；最终报告 /health=200、/v1/models 成功、completion HTTP 200 且两 rank 出现 Replaying aclgraph，但这些结果仍是来源中的报告，不能视为独立复核的 verified success。相关 PR 末尾据称已追加 154 API/DP rank 0 与 156 headless/DP rank 1 的启动命令，并记录了验证服务停止和 NPU 释放。

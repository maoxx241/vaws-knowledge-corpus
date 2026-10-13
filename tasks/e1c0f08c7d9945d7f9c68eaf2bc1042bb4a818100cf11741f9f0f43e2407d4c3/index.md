---
blocks:
- block_id: f847e1fda7769b7b85c1a1942b9d73d35f46748b9ccbe77ca94637a2763b1fac
  indexed: true
  sha256: eb43d82f962b5cae591381f38b2213a0efb6acb57fce6c2e1b837f39c9190025
  source_range:
    end: 301920
    part: 0
    start: 0
  summary: GPQA 评测曾报告从 69/198、71/198 推进到 99/198，服务 HTTP 200、请求错误和解析失败均为 0；基于生成 token 数和历史速度估计剩余约 2 小时 15 分至 3 小时 10 分，但这是估算而非完成确认。后续对两次 198 题结果的比较显示当前 175/198（88.38%）低于历史 181/198（91.41%），McNemar exact 双侧 p=0.2101，配对差值近似 95% CI 为 [-6.98pp, +0.92pp]。该材料明确认为不足以证明配置导致精度回退，且 temperature=1.0、无固定 seed、权重及 FlashComm、prefix cache、VL 配置存在差异；TextVQA-lite 方案已准备但安装依赖、下载数据和运行状态未确认。
  title: GPQA 进展、耗时预估与两次结果对比
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e1c0f08c7d9945d7f9c68eaf2bc1042bb4a818100cf11741f9f0f43e2407d4c3
  kind: experience
  material_digest: 5668ee98b7ae3a9a8e59e6cd6740607a7074e7cbcb3ffe2d587ffa311badc524
  revision: cfd2ec13ac9b3a9ec33a558f60b3b121b40f183ab4d05d3951d69ce14df48758
  schema: mindie-entry/3
  summary: 检索重点：GPQA 进度/剩余时间估计、HTTP 200、请求错误与解析失败、finish_reason=stop、TextVQA-lite 只读准备及未确认的依赖和数据步骤。保留后续结果对比：当前 175/198 对历史 181/198，p=0.2101 且置信区间跨 0，因此不能将差异归因于单一配置；原始消息仍是独立可检索来源。
  title: 任务导航：GPQA 运行与结果不确定性
navigation: 检索重点：GPQA 进度/剩余时间估计、HTTP 200、请求错误与解析失败、finish_reason=stop、TextVQA-lite 只读准备及未确认的依赖和数据步骤。保留后续结果对比：当前 175/198 对历史 181/198，p=0.2101 且置信区间跨 0，因此不能将差异归因于单一配置；原始消息仍是独立可检索来源。
schema: mindie-material-task/1
status: complete
task_id: e1c0f08c7d9945d7f9c68eaf2bc1042bb4a818100cf11741f9f0f43e2407d4c3
---

# 任务导航：GPQA 运行与结果不确定性

检索重点：GPQA 进度/剩余时间估计、HTTP 200、请求错误与解析失败、finish_reason=stop、TextVQA-lite 只读准备及未确认的依赖和数据步骤。保留后续结果对比：当前 175/198 对历史 181/198，p=0.2101 且置信区间跨 0，因此不能将差异归因于单一配置；原始消息仍是独立可检索来源。

## Materials

- [GPQA 进展、耗时预估与两次结果对比](blocks/f847e1fda7769b7b85c1a1942b9d73d35f46748b9ccbe77ca94637a2763b1fac.md): GPQA 评测曾报告从 69/198、71/198 推进到 99/198，服务 HTTP 200、请求错误和解析失败均为 0；基于生成 token 数和历史速度估计剩余约 2 小时 15 分至 3 小时 10 分，但这是估算而非完成确认。后续对两次 198 题结果的比较显示当前 175/198（88.38%）低于历史 181/198（91.41%），McNemar exact 双侧 p=0.2101，配对差值近似 95% CI 为 [-6.98pp, +0.92pp]。该材料明确认为不足以证明配置导致精度回退，且 temperature=1.0、无固定 seed、权重及 FlashComm、prefix cache、VL 配置存在差异；TextVQA-lite 方案已准备但安装依赖、下载数据和运行状态未确认。

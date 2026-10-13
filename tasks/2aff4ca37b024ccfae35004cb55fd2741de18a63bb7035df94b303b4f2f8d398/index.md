---
blocks:
- block_id: 8786acda059fd9d701ae16175e7fd049263cfe7189fcb9ed0cc5cfe3503b0a81
  indexed: true
  sha256: e7964cdf86c2cc46f5cdea3e27caf40cc86044fcc00e7cc01a87554e23c23819
  source_range:
    end: 2156391
    part: 0
    start: 0
  summary: 该材料报告称，官方 vllm-ascend v0.23.0 分支未见 DSpark 支持，而 main 已有部分实现；将固定长度 DSpark 移植到旧分支并适配 Kimi K3，初步估计约涉及 25–35 个文件、生产代码 1,700–3,000 行及测试代码 600–1,000 行，但这些是分析估算而非已验证结果。核心判断是 draft 侧可复用普通 paged KV cache，target 侧需处理 K3 的 MLA/KDA 状态、五层 hidden capture、draft/target cache group、prefix cache 和 DP/SP；动态 confidence/ragged verify、MRV2 graph 等可暂不纳入。后续修正指出可以固定 speculative 长度，但 K3 DSpark 配置的 block_size 为 7，不能据此安全地把它当作 MTP 任意设置为 1 或 3；建议使用 method=dspark、固定 7，并将“可固定长度”与“可缩短 block size”区分开。
  title: vLLM Ascend DSpark/Kimi K3 移植与固定长度评估
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2aff4ca37b024ccfae35004cb55fd2741de18a63bb7035df94b303b4f2f8d398
  kind: experience
  material_digest: 101fab682deab608646970cc5baa94688eade39876ec4481f81a9fee341ca753
  revision: 79cf212a6db1b7f4baaf2376dd03ca7ada9c1b0881395257bbf262acbff06b59
  schema: mindie-entry/3
  summary: 当前任务聚焦于 vLLM Ascend v0.23.0/main 的 DSpark 支持状态、Kimi K3 适配所需的 KV cache/KDA/hidden capture 修改，以及固定长度运行边界。已有材料保留了未验证的工作量估算和实现假设；最新可检索结论是固定长度方案可缩小移植范围，但 K3 checkpoint 的 block_size=7 使 1/3 token 配置存在语义或正确性风险，不能直接等同于 MTP。
  title: DSpark 移植与固定长度配置
navigation: 当前任务聚焦于 vLLM Ascend v0.23.0/main 的 DSpark 支持状态、Kimi K3 适配所需的 KV cache/KDA/hidden capture 修改，以及固定长度运行边界。已有材料保留了未验证的工作量估算和实现假设；最新可检索结论是固定长度方案可缩小移植范围，但 K3 checkpoint 的 block_size=7 使 1/3 token 配置存在语义或正确性风险，不能直接等同于 MTP。
schema: mindie-material-task/1
status: complete
task_id: 2aff4ca37b024ccfae35004cb55fd2741de18a63bb7035df94b303b4f2f8d398
---

# DSpark 移植与固定长度配置

当前任务聚焦于 vLLM Ascend v0.23.0/main 的 DSpark 支持状态、Kimi K3 适配所需的 KV cache/KDA/hidden capture 修改，以及固定长度运行边界。已有材料保留了未验证的工作量估算和实现假设；最新可检索结论是固定长度方案可缩小移植范围，但 K3 checkpoint 的 block_size=7 使 1/3 token 配置存在语义或正确性风险，不能直接等同于 MTP。

## Materials

- [vLLM Ascend DSpark/Kimi K3 移植与固定长度评估](blocks/8786acda059fd9d701ae16175e7fd049263cfe7189fcb9ed0cc5cfe3503b0a81.md): 该材料报告称，官方 vllm-ascend v0.23.0 分支未见 DSpark 支持，而 main 已有部分实现；将固定长度 DSpark 移植到旧分支并适配 Kimi K3，初步估计约涉及 25–35 个文件、生产代码 1,700–3,000 行及测试代码 600–1,000 行，但这些是分析估算而非已验证结果。核心判断是 draft 侧可复用普通 paged KV cache，target 侧需处理 K3 的 MLA/KDA 状态、五层 hidden capture、draft/target cache group、prefix cache 和 DP/SP；动态 confidence/ragged verify、MRV2 graph 等可暂不纳入。后续修正指出可以固定 speculative 长度，但 K3 DSpark 配置的 block_size 为 7，不能据此安全地把它当作 MTP 任意设置为 1 或 3；建议使用 method=dspark、固定 7，并将“可固定长度”与“可缩短 block size”区分开。

---
blocks:
- block_id: ee7af1272c62a04076724ead3ebacab031a9a170ac383508959aefcd72a2ead6
  indexed: true
  sha256: e2255a97d2e51e723d6ae1ffb1d18a7a85a351d0441db9425f913fb531e59849
  source_range:
    end: 5105029
    part: 0
    start: 0
  summary: 对话记录称，在远端 NFS 挂载上的 351 个 safetensors 分片及索引结构检查通过，包含 992,732 个 tensor；但因缺少官方 SHA256/MD5 清单，不能证明与原始发布源逐字节一致，且 `.154` 与 `.156` 不是独立副本。记录还给出 TP16/DP4/EP64 每卡静态权重约 31.893 GiB、TP16/DP2/EP32 约 52.102 GiB，均不含 KV cache、激活、通信 buffer、ACLGraph 和 workspace；这些是记录中的计算结果，未在本索引中重新验证。最后，记录称 Attention linear 条目均标记为 FLOAT、实际为 BF16（A_log/dt_bias 为 FP32），量化主要集中于 MoE；该结论应保留为对话中报告的检查结果，而非独立验证。
  title: Multicards 权重完整性、显存估算与 Attention 量化核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1db0738f48d257ef5a5fe07133e2b350dff6bbc09195d305af685ffd3a62e31c
  kind: experience
  material_digest: 4c60f684ec8c69a571873bba8fde678ba900605fe40294861224b16c4420630d
  revision: b66581b3540d9b3ac346c32a094cb0802ac599c8f8f23d2516ef48cf35bcdbe6
  schema: mindie-entry/3
  summary: 已索引的材料涵盖三项相关内容：权重分片、tensor 索引和 safetensors 结构的报告性完整性检查；TP16/DP4/EP64 与 TP16/DP2/EP32 的每卡静态权重估算；以及 Attention linear 与 MoE 量化标记和 dtype 的核查。后续使用时应保留两个限制：缺少发布方校验和，无法证明来源级逐字节一致；显存数字和量化结论均来自对话中报告的检查，不能升级为本次索引独立确认的事实。
  title: 当前任务导航：Multicards 权重检查与资源分析
navigation: 已索引的材料涵盖三项相关内容：权重分片、tensor 索引和 safetensors 结构的报告性完整性检查；TP16/DP4/EP64 与 TP16/DP2/EP32 的每卡静态权重估算；以及 Attention linear 与 MoE 量化标记和 dtype 的核查。后续使用时应保留两个限制：缺少发布方校验和，无法证明来源级逐字节一致；显存数字和量化结论均来自对话中报告的检查，不能升级为本次索引独立确认的事实。
schema: mindie-material-task/1
status: complete
task_id: 1db0738f48d257ef5a5fe07133e2b350dff6bbc09195d305af685ffd3a62e31c
---

# 当前任务导航：Multicards 权重检查与资源分析

已索引的材料涵盖三项相关内容：权重分片、tensor 索引和 safetensors 结构的报告性完整性检查；TP16/DP4/EP64 与 TP16/DP2/EP32 的每卡静态权重估算；以及 Attention linear 与 MoE 量化标记和 dtype 的核查。后续使用时应保留两个限制：缺少发布方校验和，无法证明来源级逐字节一致；显存数字和量化结论均来自对话中报告的检查，不能升级为本次索引独立确认的事实。

## Materials

- [Multicards 权重完整性、显存估算与 Attention 量化核查](blocks/ee7af1272c62a04076724ead3ebacab031a9a170ac383508959aefcd72a2ead6.md): 对话记录称，在远端 NFS 挂载上的 351 个 safetensors 分片及索引结构检查通过，包含 992,732 个 tensor；但因缺少官方 SHA256/MD5 清单，不能证明与原始发布源逐字节一致，且 `.154` 与 `.156` 不是独立副本。记录还给出 TP16/DP4/EP64 每卡静态权重约 31.893 GiB、TP16/DP2/EP32 约 52.102 GiB，均不含 KV cache、激活、通信 buffer、ACLGraph 和 workspace；这些是记录中的计算结果，未在本索引中重新验证。最后，记录称 Attention linear 条目均标记为 FLOAT、实际为 BF16（A_log/dt_bias 为 FP32），量化主要集中于 MoE；该结论应保留为对话中报告的检查结果，而非独立验证。

---
blocks:
- block_id: 2bbc3571cd55d301ec8a012f387e54605e6e0268d02c6ba3504d103299d22f2f
  indexed: true
  sha256: 82b02e564d2578f885dc6ca0527d4a67443972dc6f9d26a11faad6fb621288fe
  source_range:
    end: 969549
    part: 0
    start: 0
  summary: 复审称 MRV1、无 offload、prefix-off/eager 默认及 DP4 的 rank/IP、draft TP16、4096 长度、七 token、greedy 配置已确认，`bash -n` 与 `shellcheck` 通过，但未访问四机或修改文件。主要未决风险是 clean exact PR 缺少 GQA 所需的配置归一化和 materialized residual-stream capture，可能导致接收率偏低；exact GQA 身份、继承环境隔离、四机提交与 launcher SHA 仍需逐机核验，相关结论不是已验证成功。
  title: DP4 K3 GQA wrapper 复审与剩余风险
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9e7be95c1492c945835392966ca6e4a589a77913c114b0b8025d2a06b65990c0
  kind: experience
  material_digest: c0c5abfa3c1b002fce22dd1ee6b62dbe51cbc511ecd813524e394ec2d4555c19
  revision: 26d8ffb8ea96ae68a2d4827f5020d08b918a37cabbc673f07eaa001c0ec449e7
  schema: mindie-entry/3
  summary: 当前索引聚焦 DP4 的 MRV1/TP16/4096/7-token、greedy、无 CPU offload 及 rank/IP 配置，并记录 wrapper 对 served-model 名称、空 backend 和环境污染的修订。后续检索应优先关注 clean exact PR 是否缺少 GQA 配置归一化与 materialized capture，以及四机环境、权重身份、manifest 和 launcher SHA 的未完成核验；已有“通过”仅指脚本静态检查，不代表端到端启动或接收率成功。
  title: DP4 K3 GQA 启动配置与 exact PR 风险
navigation: 当前索引聚焦 DP4 的 MRV1/TP16/4096/7-token、greedy、无 CPU offload 及 rank/IP 配置，并记录 wrapper 对 served-model 名称、空 backend 和环境污染的修订。后续检索应优先关注 clean exact PR 是否缺少 GQA 配置归一化与 materialized capture，以及四机环境、权重身份、manifest 和 launcher SHA 的未完成核验；已有“通过”仅指脚本静态检查，不代表端到端启动或接收率成功。
schema: mindie-material-task/1
status: complete
task_id: 9e7be95c1492c945835392966ca6e4a589a77913c114b0b8025d2a06b65990c0
---

# DP4 K3 GQA 启动配置与 exact PR 风险

当前索引聚焦 DP4 的 MRV1/TP16/4096/7-token、greedy、无 CPU offload 及 rank/IP 配置，并记录 wrapper 对 served-model 名称、空 backend 和环境污染的修订。后续检索应优先关注 clean exact PR 是否缺少 GQA 配置归一化与 materialized capture，以及四机环境、权重身份、manifest 和 launcher SHA 的未完成核验；已有“通过”仅指脚本静态检查，不代表端到端启动或接收率成功。

## Materials

- [DP4 K3 GQA wrapper 复审与剩余风险](blocks/2bbc3571cd55d301ec8a012f387e54605e6e0268d02c6ba3504d103299d22f2f.md): 复审称 MRV1、无 offload、prefix-off/eager 默认及 DP4 的 rank/IP、draft TP16、4096 长度、七 token、greedy 配置已确认，`bash -n` 与 `shellcheck` 通过，但未访问四机或修改文件。主要未决风险是 clean exact PR 缺少 GQA 所需的配置归一化和 materialized residual-stream capture，可能导致接收率偏低；exact GQA 身份、继承环境隔离、四机提交与 launcher SHA 仍需逐机核验，相关结论不是已验证成功。

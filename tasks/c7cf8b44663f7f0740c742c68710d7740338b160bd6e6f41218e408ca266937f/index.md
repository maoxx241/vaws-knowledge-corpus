---
blocks:
- block_id: a448b42cf916ede3955d22bfe6ed3f44952eb47ce711d9ae0db161534c77ac11
  indexed: true
  sha256: bb0c5f3cfa983770cd99b0bfe742a209ebcc923878614ae7ddc13d13e6c13aae
  source_range:
    end: 893533
    part: 0
    start: 0
  summary: 只读核对记录显示，PR12950 的 main core pin 为 d02df748…，release core 为 v0.25.1（752a3a5044…）；本地与四机均未发现 exact-d02 core，但部分源码与 v0.25.1 一致。纯 93b073de HEAD 的 Mamba kernel 缺少多个新 ABI 参数和布局标志，可能导致参数错位或 DS conv 布局语义错误；同步版及 PR13071 r1 kernel 被观察为与 d02/v0.25.1 caller 匹配，但这属于核对结论而非已验证成功。建议包括复用 v0.25.1 export 做最快验证，或建立 exact-d02 source bundle；四机缺少 .git 和原始 build log，发布级追溯仍未完成。
  title: PR12950 core pin、vLLM 环境与 Mamba ABI 核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c7cf8b44663f7f0740c742c68710d7740338b160bd6e6f41218e408ca266937f
  kind: experience
  material_digest: 8335fcde1905e55c155eec060c44af4d1f47b4b3e42d5547d5b1fe50cf416e94
  revision: 17a2cd71c1956baeeeb4606c65a9136ed34bda014994ca37e771243bde6468e5
  schema: mindie-entry/3
  summary: 已记录一块只读环境核对材料：涉及 d02df748…、v0.25.1、93b073de、PR13071 r1、四机源码/制品对应关系及 Mamba postprocess ABI 差异。关键未决事项是 exact-d02 core 缺失、r5 launcher 仍指向 fe784ff core，以及建议的复用或 clean rebuild 尚未被报告为成功；后续检索应以原始块为准。
  title: 当前任务导航：PR12950 / vLLM core 与 Mamba ABI 参考索引
navigation: 已记录一块只读环境核对材料：涉及 d02df748…、v0.25.1、93b073de、PR13071 r1、四机源码/制品对应关系及 Mamba postprocess ABI 差异。关键未决事项是 exact-d02 core 缺失、r5 launcher 仍指向 fe784ff core，以及建议的复用或 clean rebuild 尚未被报告为成功；后续检索应以原始块为准。
schema: mindie-material-task/1
status: complete
task_id: c7cf8b44663f7f0740c742c68710d7740338b160bd6e6f41218e408ca266937f
---

# 当前任务导航：PR12950 / vLLM core 与 Mamba ABI 参考索引

已记录一块只读环境核对材料：涉及 d02df748…、v0.25.1、93b073de、PR13071 r1、四机源码/制品对应关系及 Mamba postprocess ABI 差异。关键未决事项是 exact-d02 core 缺失、r5 launcher 仍指向 fe784ff core，以及建议的复用或 clean rebuild 尚未被报告为成功；后续检索应以原始块为准。

## Materials

- [PR12950 core pin、vLLM 环境与 Mamba ABI 核对](blocks/a448b42cf916ede3955d22bfe6ed3f44952eb47ce711d9ae0db161534c77ac11.md): 只读核对记录显示，PR12950 的 main core pin 为 d02df748…，release core 为 v0.25.1（752a3a5044…）；本地与四机均未发现 exact-d02 core，但部分源码与 v0.25.1 一致。纯 93b073de HEAD 的 Mamba kernel 缺少多个新 ABI 参数和布局标志，可能导致参数错位或 DS conv 布局语义错误；同步版及 PR13071 r1 kernel 被观察为与 d02/v0.25.1 caller 匹配，但这属于核对结论而非已验证成功。建议包括复用 v0.25.1 export 做最快验证，或建立 exact-d02 source bundle；四机缺少 .git 和原始 build log，发布级追溯仍未完成。

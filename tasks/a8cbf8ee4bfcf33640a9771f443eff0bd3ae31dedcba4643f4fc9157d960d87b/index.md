---
blocks:
- block_id: e124d61ff6c5260acb8772441c15fa037b75dc127ae8cf643a9071c825abe601
  indexed: true
  sha256: 591199d98439b8c2dc9c5e95adee9126da92870368bbfb48331b54f812bcbfea
  source_range:
    end: 298309
    part: 0
    start: 0
  summary: '记录显示，vllm-ascend 远端 main 已从 vLLM v0.26.0 切换到 v0.27.1 配套；依据包括 PR #14027、.github/vllm-release-tag、硬件 Dockerfile 的 VLLM_TAG，以及 README 主线说明。releases/v0.26.0rc 和少量 0.26 兼容逻辑仍保留，且尚无 releases/v0.27* 分支；结论基于当时拉取的最新 origin/main，本地仍处于功能分支。'
  title: vllm-ascend 主线已切换至 vLLM v0.27.1 配套
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a8cbf8ee4bfcf33640a9771f443eff0bd3ae31dedcba4643f4fc9157d960d87b
  kind: experience
  material_digest: de4a53d26ed31e7f36972488f09c977a6847977b373aef81a713d4606efa4c6e
  revision: d385e01a25b5ae203d7d9b2b30870f4a961700763d441bfc42ca4747f294cec2
  schema: mindie-entry/3
  summary: 当前记录聚焦于 vllm-ascend 主线与 vLLM 版本配套：已报告 main 采用 v0.27.1，但仍保留 v0.26.0rc、兼容逻辑及未出现 releases/v0.27* 分支等上下文。该结论依赖当时的远端状态，原始材料仍应独立检索；未记录进一步验证或后续更正。
  title: vllm-ascend 版本基线核对
navigation: 当前记录聚焦于 vllm-ascend 主线与 vLLM 版本配套：已报告 main 采用 v0.27.1，但仍保留 v0.26.0rc、兼容逻辑及未出现 releases/v0.27* 分支等上下文。该结论依赖当时的远端状态，原始材料仍应独立检索；未记录进一步验证或后续更正。
schema: mindie-material-task/1
status: complete
task_id: a8cbf8ee4bfcf33640a9771f443eff0bd3ae31dedcba4643f4fc9157d960d87b
---

# vllm-ascend 版本基线核对

当前记录聚焦于 vllm-ascend 主线与 vLLM 版本配套：已报告 main 采用 v0.27.1，但仍保留 v0.26.0rc、兼容逻辑及未出现 releases/v0.27* 分支等上下文。该结论依赖当时的远端状态，原始材料仍应独立检索；未记录进一步验证或后续更正。

## Materials

- [vllm-ascend 主线已切换至 vLLM v0.27.1 配套](blocks/e124d61ff6c5260acb8772441c15fa037b75dc127ae8cf643a9071c825abe601.md): 记录显示，vllm-ascend 远端 main 已从 vLLM v0.26.0 切换到 v0.27.1 配套；依据包括 PR #14027、.github/vllm-release-tag、硬件 Dockerfile 的 VLLM_TAG，以及 README 主线说明。releases/v0.26.0rc 和少量 0.26 兼容逻辑仍保留，且尚无 releases/v0.27* 分支；结论基于当时拉取的最新 origin/main，本地仍处于功能分支。

---
blocks:
- block_id: 4dcb2705698514d3c495852738a8039c62182a873343c6c0785a26ae0c8ac9a6
  indexed: true
  sha256: bcc8e5704faf83fc56b1543b9189f3792aae5e5b78ecb8535334895a7ae4a726
  source_range:
    end: 529199
    part: 0
    start: 0
  summary: 材料中的结论称 DSV4 相关实现归于 `ops-transformer` 的 `moe/moe_gating_top_k`，并重点涉及 `MoeGatingTopK`、Hash、V2 与 `normType=2`（sqrtsoftplus）；但同时记录了当前 vllm-ascend 实际调用自带 `aclnnMoeGatingTopKHash`、尚非上游 V2，属于需要保留的路径澄清而非已验证的统一结论。其报告的接口对照认为 `aclnnMoeGatingTopKV2` 可组合 Hash 与 sqrtsoftplus，`aclnnMoeGatingTopK`（V1）支持 sqrtsoftplus 但不支持 Hash；`normType=2` 被材料称为仅支持 Ascend 950PR/950DT（A5），这些均应视为来源中的报告性信息。
  title: DSV4 的 MoeGatingTopK 实现与接口路径
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2e213e16d21dd1a22957e2997ccd04c413e15d10d3f0f2c6319ec6a67905e527
  kind: experience
  material_digest: ef9f89e2f330d71599cbd2d6e29bd1b3e54d60410828708d8bfab59f8bdff0dd
  revision: c7cbb9f24a52a0b9519fee5e147e820d78ebb3389b12e634cd319f88e0d27bf6
  schema: mindie-entry/3
  summary: 当前材料聚焦 DSV4 的 MoeGatingTopK 归属、V1/V2 接口差异及 Hash 与 sqrtsoftplus（`normType=2`）的组合关系。检索时优先使用 `ops-transformer`、`moe_gating_top_k`、`aclnnMoeGatingTopK`、`aclnnMoeGatingTopKV2`、`aclnnMoeGatingTopKHash`、`normType=2` 和 Ascend 950PR/950DT；注意材料后续澄清称 vllm-ascend 当前实际仍走自带 Hash 接口，且未验证为上游 V2。
  title: DSV4 MoeGatingTopK 检索导航
navigation: 当前材料聚焦 DSV4 的 MoeGatingTopK 归属、V1/V2 接口差异及 Hash 与 sqrtsoftplus（`normType=2`）的组合关系。检索时优先使用 `ops-transformer`、`moe_gating_top_k`、`aclnnMoeGatingTopK`、`aclnnMoeGatingTopKV2`、`aclnnMoeGatingTopKHash`、`normType=2` 和 Ascend 950PR/950DT；注意材料后续澄清称 vllm-ascend 当前实际仍走自带 Hash 接口，且未验证为上游 V2。
schema: mindie-material-task/1
status: complete
task_id: 2e213e16d21dd1a22957e2997ccd04c413e15d10d3f0f2c6319ec6a67905e527
---

# DSV4 MoeGatingTopK 检索导航

当前材料聚焦 DSV4 的 MoeGatingTopK 归属、V1/V2 接口差异及 Hash 与 sqrtsoftplus（`normType=2`）的组合关系。检索时优先使用 `ops-transformer`、`moe_gating_top_k`、`aclnnMoeGatingTopK`、`aclnnMoeGatingTopKV2`、`aclnnMoeGatingTopKHash`、`normType=2` 和 Ascend 950PR/950DT；注意材料后续澄清称 vllm-ascend 当前实际仍走自带 Hash 接口，且未验证为上游 V2。

## Materials

- [DSV4 的 MoeGatingTopK 实现与接口路径](blocks/4dcb2705698514d3c495852738a8039c62182a873343c6c0785a26ae0c8ac9a6.md): 材料中的结论称 DSV4 相关实现归于 `ops-transformer` 的 `moe/moe_gating_top_k`，并重点涉及 `MoeGatingTopK`、Hash、V2 与 `normType=2`（sqrtsoftplus）；但同时记录了当前 vllm-ascend 实际调用自带 `aclnnMoeGatingTopKHash`、尚非上游 V2，属于需要保留的路径澄清而非已验证的统一结论。其报告的接口对照认为 `aclnnMoeGatingTopKV2` 可组合 Hash 与 sqrtsoftplus，`aclnnMoeGatingTopK`（V1）支持 sqrtsoftplus 但不支持 Hash；`normType=2` 被材料称为仅支持 Ascend 950PR/950DT（A5），这些均应视为来源中的报告性信息。

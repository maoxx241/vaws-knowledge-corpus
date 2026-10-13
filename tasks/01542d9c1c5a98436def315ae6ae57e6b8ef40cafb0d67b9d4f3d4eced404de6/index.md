---
blocks:
- block_id: 3a53b8e37b812c3becd3cad704429f0473d450195f734fd6daf6b221f6875a5d
  indexed: true
  sha256: 61ea1b755f612900bde0437828282ffed761d1e8492683c80d08ff998c5caca2
  source_range:
    end: 1222710
    part: 0
    start: 0
  summary: '该记录将 GPT-OSS 的 sink 数学语义归因于 OpenAI 参考实现，并报告 vLLM 的 Hopper/FA3 路径来自 vLLM 的 FlashAttention fork（PR #75、commit `b99f8c8217`），Triton 路径则在既有 `triton_unified_attention` 上加入 sink 支持；这些均是记录中的来源判断，未在此索引中验证。还报告 vLLM 通过 PR #22313/#22320 接入，以及 vLLM-Ascend 通过 PR #5901 使用 CANN/torch-npu 的 `npu_fused_infer_attention_score_v2(..., learnable_sink=...)`；链接中含 `[REDACTED_SECRET]`，且不同硬件路径与实现归属存在需继续核对的区分。'
  title: GPT-OSS attention sink 算子来源线索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 01542d9c1c5a98436def315ae6ae57e6b8ef40cafb0d67b9d4f3d4eced404de6
  kind: experience
  material_digest: 5c0593234c5ba8ea1c2069f70131b37ad2e18809d2200ee6ed8caf186301dc9b
  revision: ca6fec69ed56922f118298bfe1cc39d3afccfab86a0231c62e5940a69b2e2b39
  schema: mindie-entry/3
  summary: '当前导航聚焦 GPT-OSS attention sink 的语义来源、vLLM FA3 fork、Triton unified-attention 路径及 vLLM-Ascend/CANN 接入线索。记录保留 PR/commit、API 名称、硬件路径和未验证的归因；后续检索应分别核对 OpenAI 参考实现、vLLM PR #22313/#22320、FlashAttention fork PR #75，以及 vLLM-Ascend PR #5901。'
  title: GPT-OSS attention sink 来源与实现路径
navigation: '当前导航聚焦 GPT-OSS attention sink 的语义来源、vLLM FA3 fork、Triton unified-attention 路径及 vLLM-Ascend/CANN 接入线索。记录保留 PR/commit、API 名称、硬件路径和未验证的归因；后续检索应分别核对 OpenAI 参考实现、vLLM PR #22313/#22320、FlashAttention fork PR #75，以及 vLLM-Ascend PR #5901。'
schema: mindie-material-task/1
status: complete
task_id: 01542d9c1c5a98436def315ae6ae57e6b8ef40cafb0d67b9d4f3d4eced404de6
---

# GPT-OSS attention sink 来源与实现路径

当前导航聚焦 GPT-OSS attention sink 的语义来源、vLLM FA3 fork、Triton unified-attention 路径及 vLLM-Ascend/CANN 接入线索。记录保留 PR/commit、API 名称、硬件路径和未验证的归因；后续检索应分别核对 OpenAI 参考实现、vLLM PR #22313/#22320、FlashAttention fork PR #75，以及 vLLM-Ascend PR #5901。

## Materials

- [GPT-OSS attention sink 算子来源线索](blocks/3a53b8e37b812c3becd3cad704429f0473d450195f734fd6daf6b221f6875a5d.md): 该记录将 GPT-OSS 的 sink 数学语义归因于 OpenAI 参考实现，并报告 vLLM 的 Hopper/FA3 路径来自 vLLM 的 FlashAttention fork（PR #75、commit `b99f8c8217`），Triton 路径则在既有 `triton_unified_attention` 上加入 sink 支持；这些均是记录中的来源判断，未在此索引中验证。还报告 vLLM 通过 PR #22313/#22320 接入，以及 vLLM-Ascend 通过 PR #5901 使用 CANN/torch-npu 的 `npu_fused_infer_attention_score_v2(..., learnable_sink=...)`；链接中含 `[REDACTED_SECRET]`，且不同硬件路径与实现归属存在需继续核对的区分。

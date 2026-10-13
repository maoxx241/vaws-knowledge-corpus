---
blocks:
- block_id: 8430cb5bfd7682ac633b484a4a0fd0390ff1e25de1d572201c7b31ccce15db15
  indexed: true
  sha256: 81743028ede70b8245eac8e4c09ae40bb2c268d301c4e2e0e96cff8998f4651b
  source_range:
    end: 969544
    part: 0
    start: 0
  summary: 只读评估认为 Block5 应使用 `num_speculative_tokens=5`，省略 `speculative_config.max_model_len`，模型是带低秩 KV latent projection 的 full-causal MLA DSpark，而非 GQA；但 vLLM 0.27.1 会因 `hidden size 7168` 与 `96` 个 attention heads 的普通校验失败，正式配置基类适配仍未完成。分析还指出 `full_attention_causal`/`dflash_config.causal`、QuaRot `quant_config=None` 等适配点，以及 Block5 在 TP16+SP 下的通用对齐错误：`uniform_decode_query_len=6` 时应使用 `math.lcm(6,16)=48`，并将 graph gear 从 `[16,32]` 调整为 `[48,96]`；这些结论来自静态路径检查，未进行真实启动或性能验证。
  title: Block5 配置、MLA 适配与 SP 图尺寸分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3f5e526a4b5e312a23ecb0585848ff2fa5794df344a4751b17b7217611c2b9ad
  kind: experience
  material_digest: 15c02826ce5c6b9e6619e76e79a268a2c7ae9d05b65bc1b7cfa6555d343424a8
  revision: 22120b9449094b558f7355130cb0fc0a9975521f5e554997862fc3fcf5d9f132
  schema: mindie-entry/3
  summary: 唯一新增材料涵盖 Block5 的 speculative decoding 配置、MLA DSpark 结构、vLLM 配置解析/causal/QuaRot 适配，以及 TP16+SP 下 `max()` 应改为 LCM 的 cudagraph gear 问题。已报告的修改部分包括共享工作树中的 causal 与 QuaRot 处理，但整体仍是未提交、只读分析；配置基类修复、上游 LCM 修复、真实启动、输出一致性和性能影响均未验证。
  title: 当前任务导航：Block5 vLLM/Ascend 适配与检索索引
navigation: 唯一新增材料涵盖 Block5 的 speculative decoding 配置、MLA DSpark 结构、vLLM 配置解析/causal/QuaRot 适配，以及 TP16+SP 下 `max()` 应改为 LCM 的 cudagraph gear 问题。已报告的修改部分包括共享工作树中的 causal 与 QuaRot 处理，但整体仍是未提交、只读分析；配置基类修复、上游 LCM 修复、真实启动、输出一致性和性能影响均未验证。
schema: mindie-material-task/1
status: complete
task_id: 3f5e526a4b5e312a23ecb0585848ff2fa5794df344a4751b17b7217611c2b9ad
---

# 当前任务导航：Block5 vLLM/Ascend 适配与检索索引

唯一新增材料涵盖 Block5 的 speculative decoding 配置、MLA DSpark 结构、vLLM 配置解析/causal/QuaRot 适配，以及 TP16+SP 下 `max()` 应改为 LCM 的 cudagraph gear 问题。已报告的修改部分包括共享工作树中的 causal 与 QuaRot 处理，但整体仍是未提交、只读分析；配置基类修复、上游 LCM 修复、真实启动、输出一致性和性能影响均未验证。

## Materials

- [Block5 配置、MLA 适配与 SP 图尺寸分析](blocks/8430cb5bfd7682ac633b484a4a0fd0390ff1e25de1d572201c7b31ccce15db15.md): 只读评估认为 Block5 应使用 `num_speculative_tokens=5`，省略 `speculative_config.max_model_len`，模型是带低秩 KV latent projection 的 full-causal MLA DSpark，而非 GQA；但 vLLM 0.27.1 会因 `hidden size 7168` 与 `96` 个 attention heads 的普通校验失败，正式配置基类适配仍未完成。分析还指出 `full_attention_causal`/`dflash_config.causal`、QuaRot `quant_config=None` 等适配点，以及 Block5 在 TP16+SP 下的通用对齐错误：`uniform_decode_query_len=6` 时应使用 `math.lcm(6,16)=48`，并将 graph gear 从 `[16,32]` 调整为 `[48,96]`；这些结论来自静态路径检查，未进行真实启动或性能验证。

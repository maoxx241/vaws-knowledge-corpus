---
blocks:
- block_id: 77842244388cb39bb8c57a54bd08c837b9a64d9baf4a459f0e9d96dd724f2ea9
  indexed: true
  sha256: e2cd7c3933e7f8f0c85ddd4384a27cd10c16eb93243309513368eb3d9eb7acd1
  source_range:
    end: 1266551
    part: 0
    start: 0
  summary: 只读对照报告称，positions、slot mapping、K norm 及 layer/cache 映射未发现确定性静态错误，但这不是已验证的根因结论。重点疑点是 Ascend context RoPE 调用返回式算子却丢弃返回值，并依赖 eager Triton 原地修改，同时 custom op 声明 `mutates_args=[]`；若 `rope_k_delta=0`，该假设更值得核查。报告还记录了 SGLang 仅提交 accepted prefix KV、vLLM 先写 rejected suffix 再覆盖的差异，以及约 50%–56% 条件正确率，建议继续比较 FC、hidden norm、RoPE/cache readback、首层 q/k/v、attention output、draft hidden 和 logits；这些均为后续检查项，未证明成功或失败。
  title: Ascend/vLLM 与 SGLang KV-RoPE 路径静态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: cf4cc27c78f3542781d4b5002ade92ea6d6e6aa00c8875557f525db02220c7d9
  kind: experience
  material_digest: fa554c623119d1147ab7d5de34fc6a2eafb0978c0b77895540f31e15ffe7ca80
  revision: e62499654ea70796c3fc5ae3a074e03586f39602f00ac0f4333c4a7e4bb05b34
  schema: mindie-entry/3
  summary: 当前材料是一份未验证的静态审计：数学链路暂未发现确定性错误，主要保留 RoPE 原地语义、KV cache 覆盖时序及拆分路径与 fused `split_qkv_rmsnorm_rope` 的差异假设。后续应按 FC/hidden norm → rotated K 与 cache readback → 首层 q/k/v 和 attention output → draft hidden/raw logits/top-id 逐点比对，并保留约 54%、56%、50% 条件正确率及未完成验证状态；更早或被省略的上下文仍应独立检索。
  title: 任务导航：vLLM/SGLang Ascend 数值偏差定位
navigation: 当前材料是一份未验证的静态审计：数学链路暂未发现确定性错误，主要保留 RoPE 原地语义、KV cache 覆盖时序及拆分路径与 fused `split_qkv_rmsnorm_rope` 的差异假设。后续应按 FC/hidden norm → rotated K 与 cache readback → 首层 q/k/v 和 attention output → draft hidden/raw logits/top-id 逐点比对，并保留约 54%、56%、50% 条件正确率及未完成验证状态；更早或被省略的上下文仍应独立检索。
schema: mindie-material-task/1
status: complete
task_id: cf4cc27c78f3542781d4b5002ade92ea6d6e6aa00c8875557f525db02220c7d9
---

# 任务导航：vLLM/SGLang Ascend 数值偏差定位

当前材料是一份未验证的静态审计：数学链路暂未发现确定性错误，主要保留 RoPE 原地语义、KV cache 覆盖时序及拆分路径与 fused `split_qkv_rmsnorm_rope` 的差异假设。后续应按 FC/hidden norm → rotated K 与 cache readback → 首层 q/k/v 和 attention output → draft hidden/raw logits/top-id 逐点比对，并保留约 54%、56%、50% 条件正确率及未完成验证状态；更早或被省略的上下文仍应独立检索。

## Materials

- [Ascend/vLLM 与 SGLang KV-RoPE 路径静态审计](blocks/77842244388cb39bb8c57a54bd08c837b9a64d9baf4a459f0e9d96dd724f2ea9.md): 只读对照报告称，positions、slot mapping、K norm 及 layer/cache 映射未发现确定性静态错误，但这不是已验证的根因结论。重点疑点是 Ascend context RoPE 调用返回式算子却丢弃返回值，并依赖 eager Triton 原地修改，同时 custom op 声明 `mutates_args=[]`；若 `rope_k_delta=0`，该假设更值得核查。报告还记录了 SGLang 仅提交 accepted prefix KV、vLLM 先写 rejected suffix 再覆盖的差异，以及约 50%–56% 条件正确率，建议继续比较 FC、hidden norm、RoPE/cache readback、首层 q/k/v、attention output、draft hidden 和 logits；这些均为后续检查项，未证明成功或失败。

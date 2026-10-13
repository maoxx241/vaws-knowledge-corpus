---
blocks:
- block_id: 7da90245210a68854b72a0309f311fc79cd63054907ce5cc85d62bbfb277f0a8
  indexed: true
  sha256: 93f6529513cd2694767a4c10c4d0d75ac24f651bda930bf3c7a385e96cf5fc5d
  source_range:
    end: 506618
    part: 0
    start: 0
  summary: 材料中的分析称，K3 文本模型的 MLA full-attention 层在 prefill 阶段将压缩 latent KV 展开为与 Q 相同数量的 K/V heads，因此按 MHA 形态计算；但 KV cache 仍只保存 1 个压缩 latent KV head，decode 的 kernel 接口表现得类似 MQA。文中以 `num_heads`、`num_key_value_heads`、`kv_b_proj` 和 FIA kernel 参数作为依据，并指出 K3 还包含 KDA 层，所以不能把所有层都概括为 MHA。材料同时解释了 MHA、GQA、MQA 的 Q/KV head 数关系、KV cache 开销及 decode 带宽差异；这些均属于该材料中的判断与示例，未在此索引中提升为独立验证结论。
  title: K3 Prefill 中 MHA、MQA 与 MLA 的区分
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5882b135882652bdb90b042cea36165d4e9a4cb2c64679f04147d21706e9c763
  kind: experience
  material_digest: 4f45c90f985754d72402b4ff7f9bbbb9a11eb15a00512a914b83939e0af0d122
  revision: 86332be50f2947287a1cb087cd3f20731379d56557a67392686c4e0348164f7b
  schema: mindie-entry/3
  summary: 当前材料聚焦于 K3 的 MLA full-attention 在 prefill 中被描述为展开后的 MHA 计算、decode 中的单 latent KV 使用，以及 MHA/MQA/GQA 的区别。需保留 `num_heads`、`num_key_value_heads`、`kv_b_proj`、FIA kernel、KDA 和 TP16 等检索线索，并注意该判断区分了 prefill 计算形态与 KV cache 存储形态，仍应视为可检索的非验证性参考。
  title: K3 注意力形态与缓存结构
navigation: 当前材料聚焦于 K3 的 MLA full-attention 在 prefill 中被描述为展开后的 MHA 计算、decode 中的单 latent KV 使用，以及 MHA/MQA/GQA 的区别。需保留 `num_heads`、`num_key_value_heads`、`kv_b_proj`、FIA kernel、KDA 和 TP16 等检索线索，并注意该判断区分了 prefill 计算形态与 KV cache 存储形态，仍应视为可检索的非验证性参考。
schema: mindie-material-task/1
status: complete
task_id: 5882b135882652bdb90b042cea36165d4e9a4cb2c64679f04147d21706e9c763
---

# K3 注意力形态与缓存结构

当前材料聚焦于 K3 的 MLA full-attention 在 prefill 中被描述为展开后的 MHA 计算、decode 中的单 latent KV 使用，以及 MHA/MQA/GQA 的区别。需保留 `num_heads`、`num_key_value_heads`、`kv_b_proj`、FIA kernel、KDA 和 TP16 等检索线索，并注意该判断区分了 prefill 计算形态与 KV cache 存储形态，仍应视为可检索的非验证性参考。

## Materials

- [K3 Prefill 中 MHA、MQA 与 MLA 的区分](blocks/7da90245210a68854b72a0309f311fc79cd63054907ce5cc85d62bbfb277f0a8.md): 材料中的分析称，K3 文本模型的 MLA full-attention 层在 prefill 阶段将压缩 latent KV 展开为与 Q 相同数量的 K/V heads，因此按 MHA 形态计算；但 KV cache 仍只保存 1 个压缩 latent KV head，decode 的 kernel 接口表现得类似 MQA。文中以 `num_heads`、`num_key_value_heads`、`kv_b_proj` 和 FIA kernel 参数作为依据，并指出 K3 还包含 KDA 层，所以不能把所有层都概括为 MHA。材料同时解释了 MHA、GQA、MQA 的 Q/KV head 数关系、KV cache 开销及 decode 带宽差异；这些均属于该材料中的判断与示例，未在此索引中提升为独立验证结论。

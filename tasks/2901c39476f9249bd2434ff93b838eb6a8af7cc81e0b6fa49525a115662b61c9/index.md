---
blocks:
- block_id: d8f670964c92a9fa644d91bbf68abe9eb64c36d21dd4349a025f533c00099522
  indexed: true
  sha256: ebb6a10ab711b5407cde3f882e4c003541b771a3b57c533c42b8f2026ee06ec2
  source_range:
    end: 663790
    part: 0
    start: 0
  summary: 该材料解释称，MTP=7 表示草拟 7 个 token，而 target 校验输入还包含 1 个起始/anchor token，因此本轮 decode 的 query_len 或 cu_seqlens 增量为 1+7=8。它区分了固定的本轮校验宽度与通常等于已缓存长度加 8 的总 seq_lens，并强调 8 个位置不代表 8 个 token 都已被接受，最终推进量仍取决于验收结果。材料引用了 platform.py 和 model_runner_v1.py 的经脱敏路径与行号；这些解释和代码对应关系仍应视为待核实的参考信息。
  title: MTP=7 时 seqlens 为 8 的原因
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2901c39476f9249bd2434ff93b838eb6a8af7cc81e0b6fa49525a115662b61c9
  kind: experience
  material_digest: 717531c5fa30deb67203deeabaac7cb4af2c545f343cf0530e7e4ec023f35dc4
  revision: a3f47cde54e02d89dbba8090e3067c698afd8adea39f433e7aec5068c6fe470f
  schema: mindie-entry/3
  summary: 当前材料聚焦 MTP=7 场景下 query_len、cu_seqlens 和总 seq_lens 的区别，记录了“anchor + 7 个 draft token”导致本轮宽度为 8 的解释。需保留这一解释的待核实性质，并注意 8 是调度/校验宽度，不等于 8 个 token 均被接受；原始材料可独立检索以核对脱敏代码引用。
  title: MTP speculative decode 与 seqlens
navigation: 当前材料聚焦 MTP=7 场景下 query_len、cu_seqlens 和总 seq_lens 的区别，记录了“anchor + 7 个 draft token”导致本轮宽度为 8 的解释。需保留这一解释的待核实性质，并注意 8 是调度/校验宽度，不等于 8 个 token 均被接受；原始材料可独立检索以核对脱敏代码引用。
schema: mindie-material-task/1
status: complete
task_id: 2901c39476f9249bd2434ff93b838eb6a8af7cc81e0b6fa49525a115662b61c9
---

# MTP speculative decode 与 seqlens

当前材料聚焦 MTP=7 场景下 query_len、cu_seqlens 和总 seq_lens 的区别，记录了“anchor + 7 个 draft token”导致本轮宽度为 8 的解释。需保留这一解释的待核实性质，并注意 8 是调度/校验宽度，不等于 8 个 token 均被接受；原始材料可独立检索以核对脱敏代码引用。

## Materials

- [MTP=7 时 seqlens 为 8 的原因](blocks/d8f670964c92a9fa644d91bbf68abe9eb64c36d21dd4349a025f533c00099522.md): 该材料解释称，MTP=7 表示草拟 7 个 token，而 target 校验输入还包含 1 个起始/anchor token，因此本轮 decode 的 query_len 或 cu_seqlens 增量为 1+7=8。它区分了固定的本轮校验宽度与通常等于已缓存长度加 8 的总 seq_lens，并强调 8 个位置不代表 8 个 token 都已被接受，最终推进量仍取决于验收结果。材料引用了 platform.py 和 model_runner_v1.py 的经脱敏路径与行号；这些解释和代码对应关系仍应视为待核实的参考信息。

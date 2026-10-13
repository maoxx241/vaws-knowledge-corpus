---
blocks:
- block_id: 533510269b838e326888d0f23e0de75ca7b86617b316201aed52414a670a12f8
  indexed: true
  sha256: f485a9f3a2fe15794774daf99acb180f33da0e80710c9a53b9b1cca94771b476
  source_range:
    end: 1218653
    part: 0
    start: 0
  summary: 只读审计记录了 Qwen3.5 的 `in_proj*` 会触发 FlashComm all-gather，而 KDA 的 `q/k/v/b/g` 投影当前未命中该路径；远端引用从 `543819b5` 更新到 `754570e8`，但未修改代码或启动服务。补审指出多模态 Kimi conditional 的首层 `inputs_embeds` 可能是完整 `[N,H]`，layer 0 必须跳过 gather，并将 `block_residual` 与 `o_proj` 输出从 full 对齐到 token shard，否则可能出现 shape mismatch。建议用 5 层权重、`FULL_DECODE_ONLY`、固定 greedy 参数，对 FlashComm1 开关进行逐 token ID 比较；该验证尚未执行。
  title: FlashComm1 与 Kimi KDA 路径审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 29d0f441cdf5f0ecb4456520a88366768aaf592fb8eeda948cf7e26b552a166b
  kind: experience
  material_digest: 32645bd0d9f5bc8e9d6ecaf3ae5feb3634f2625741e2651505730796192caf6c
  revision: 34a1ec422a2ba3b179d57a597f1d5835bf656e0c64e38e09ba7b5c44415760ff
  schema: mindie-entry/3
  summary: 当前材料聚焦 FlashComm1 在 Qwen3.5 与 Kimi KDA 路径中的命中差异，以及多模态首层 full→shard 对齐边界。已知修复关注 layer 0 跳过 gather、`o_proj` 输出缓冲区按 local token 分配和 `block_residual` 同步切分；代码修改、服务启动及最终 token 对齐验证均未完成。
  title: FlashComm1/Kimi KDA 审计导航
navigation: 当前材料聚焦 FlashComm1 在 Qwen3.5 与 Kimi KDA 路径中的命中差异，以及多模态首层 full→shard 对齐边界。已知修复关注 layer 0 跳过 gather、`o_proj` 输出缓冲区按 local token 分配和 `block_residual` 同步切分；代码修改、服务启动及最终 token 对齐验证均未完成。
schema: mindie-material-task/1
status: complete
task_id: 29d0f441cdf5f0ecb4456520a88366768aaf592fb8eeda948cf7e26b552a166b
---

# FlashComm1/Kimi KDA 审计导航

当前材料聚焦 FlashComm1 在 Qwen3.5 与 Kimi KDA 路径中的命中差异，以及多模态首层 full→shard 对齐边界。已知修复关注 layer 0 跳过 gather、`o_proj` 输出缓冲区按 local token 分配和 `block_residual` 同步切分；代码修改、服务启动及最终 token 对齐验证均未完成。

## Materials

- [FlashComm1 与 Kimi KDA 路径审计](blocks/533510269b838e326888d0f23e0de75ca7b86617b316201aed52414a670a12f8.md): 只读审计记录了 Qwen3.5 的 `in_proj*` 会触发 FlashComm all-gather，而 KDA 的 `q/k/v/b/g` 投影当前未命中该路径；远端引用从 `543819b5` 更新到 `754570e8`，但未修改代码或启动服务。补审指出多模态 Kimi conditional 的首层 `inputs_embeds` 可能是完整 `[N,H]`，layer 0 必须跳过 gather，并将 `block_residual` 与 `o_proj` 输出从 full 对齐到 token shard，否则可能出现 shape mismatch。建议用 5 层权重、`FULL_DECODE_ONLY`、固定 greedy 参数，对 FlashComm1 开关进行逐 token ID 比较；该验证尚未执行。

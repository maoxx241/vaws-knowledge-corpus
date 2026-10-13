---
blocks:
- block_id: 317aee83107fd1e061b7b99b1fb9dd3241aafe68ee1e740f6d7220cb3f1aa919
  indexed: true
  sha256: 172094d355bae64df3811e5665841685a67687d188a5248f9cbc9f8199be7490
  source_range:
    end: 875940
    part: 0
    start: 0
  summary: 审计认为问题更可能是 AscendC kernel 在非法 cu_seqlens、slot 或 initial slot 时提前结束，adapter 却用 empty_like 返回有效但未写入的 Tensor；这不会直接产生 Python None，HTTP 的 content=null 可能来自上层 reasoning 未闭合。发现 Triton 将 state_idx<=0 视为无效，而 AscendC 仅拒绝 <0 或 >=capacity，ACLGraph 的 null block、slot 0 和 metadata 错位可能造成语义分歧；现有测试也未覆盖 module-owned self.kv_cache 的 graph replay 状态更新。原始 tuple alias 契约问题据称已在 f9 修复，但仍需以确定性初始化、逐 row 容错、统一 null/slot 语义及真实多 token 回归测试验证；此前四机启动已中止，trace 未完成，编译继续固定 MAX_JOBS=256/-j256。
  title: K3 recurrent_kda 输出 None/未初始化输出白盒审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e9ed92f1236e6d881bd29322b426c5aab0f0c84c38f083bdd71a26eee2fe0349
  kind: experience
  material_digest: ac84c25af600505d5a935f1a7e793a09f69f6c99e49a51911c8e7ad1d89b1cd1
  revision: 93a5f641d6c06f075b92ee8173e897201759a8122d56c2623c4416b2eee876d3
  schema: mindie-entry/3
  summary: 当前索引聚焦 Kimi-K3 单机/5层权重方向、四机启动已中止及 recurrent_kda 的白盒嫌疑：AscendC 可能返回未写入 Tensor，Triton 与 AscendC 对 null slot/slot 0 语义不一致，module-owned cache 的 graph replay 尚未被充分验证。此前审计未修改文件或操作远端服务；trace 请求和最终修复均未完成，建议保留这些不确定性，并继续关注确定性输出初始化、非法 row 隔离和多 token 状态回归。
  title: K3 recurrent_kda 输出异常与 AscendC/Triton 路径审计
navigation: 当前索引聚焦 Kimi-K3 单机/5层权重方向、四机启动已中止及 recurrent_kda 的白盒嫌疑：AscendC 可能返回未写入 Tensor，Triton 与 AscendC 对 null slot/slot 0 语义不一致，module-owned cache 的 graph replay 尚未被充分验证。此前审计未修改文件或操作远端服务；trace 请求和最终修复均未完成，建议保留这些不确定性，并继续关注确定性输出初始化、非法 row 隔离和多 token 状态回归。
schema: mindie-material-task/1
status: complete
task_id: e9ed92f1236e6d881bd29322b426c5aab0f0c84c38f083bdd71a26eee2fe0349
---

# K3 recurrent_kda 输出异常与 AscendC/Triton 路径审计

当前索引聚焦 Kimi-K3 单机/5层权重方向、四机启动已中止及 recurrent_kda 的白盒嫌疑：AscendC 可能返回未写入 Tensor，Triton 与 AscendC 对 null slot/slot 0 语义不一致，module-owned cache 的 graph replay 尚未被充分验证。此前审计未修改文件或操作远端服务；trace 请求和最终修复均未完成，建议保留这些不确定性，并继续关注确定性输出初始化、非法 row 隔离和多 token 状态回归。

## Materials

- [K3 recurrent_kda 输出 None/未初始化输出白盒审计](blocks/317aee83107fd1e061b7b99b1fb9dd3241aafe68ee1e740f6d7220cb3f1aa919.md): 审计认为问题更可能是 AscendC kernel 在非法 cu_seqlens、slot 或 initial slot 时提前结束，adapter 却用 empty_like 返回有效但未写入的 Tensor；这不会直接产生 Python None，HTTP 的 content=null 可能来自上层 reasoning 未闭合。发现 Triton 将 state_idx<=0 视为无效，而 AscendC 仅拒绝 <0 或 >=capacity，ACLGraph 的 null block、slot 0 和 metadata 错位可能造成语义分歧；现有测试也未覆盖 module-owned self.kv_cache 的 graph replay 状态更新。原始 tuple alias 契约问题据称已在 f9 修复，但仍需以确定性初始化、逐 row 容错、统一 null/slot 语义及真实多 token 回归测试验证；此前四机启动已中止，trace 未完成，编译继续固定 MAX_JOBS=256/-j256。

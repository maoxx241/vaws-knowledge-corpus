---
blocks:
- block_id: e77f3c2827ca461f603f1a07a7d281d82d668f8a5f4c0527cbb6419dbdf55d9f
  indexed: true
  sha256: 879aa802d325ee0eab4ebd723755e2052f874fb235a17395ee455711cd79d3f1
  source_range:
    end: 1262883
    part: 0
    start: 0
  summary: 讨论认为当前 K3 FlashComm 修改不改变 fused MoE 的路由、专家计算、EP 通信后端或输出归并；token 维 AllGather 与未切分的 linear/RMSNorm 在数学上可交换，但不同 GEMM 的 M、BF16 reduce-scatter、shared expert 权重形态可能带来微小数值差异并被 recurrent KDA 放大。只读审计未确认 ACLGraph、首层布局、KDA state 或 MoE 存在语义错位；已指出 `maybe_chunk_residual` 对 3D 张量可能错误地 pad dim1、fake shape 不保留 trailing dimensions，但当前 DP1 runner 的 `pad_size == 0`，不能据此解释 eager 精度分叉。后续应分开 enforce eager 与 ACLGraph case，用不同层数、并发请求固定 M、teacher-force 对比 KDA/MLA/残差/state 等中间结果；这些是定位建议，不是已验证修复。
  title: K3 FlashComm 与 fused MoE/首层精度定位
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ad3c127c93d771f8dddc1ba70d4685e97509e9f2ffa4b50f286ad02ab6c0ff60
  kind: experience
  material_digest: 1b993acf447a5f287f68c0c94b859e3979a4c6363d52eb236ee79bc06cee8dd5
  revision: 4270530d36dc0db7f653a0c4ed0a5f949973972ab37386df53a18d012de0b2f3
  schema: mindie-entry/3
  summary: 当前重点是先在 enforce eager 下确认 fc1/FlashComm off-on 精度差异，再单独排查 ACLGraph 首层处理；不要将问题归因于 fused MoE，除非后续证据改变该判断。token 轴通常为 dim0，pad/unpad 可优先复用框架逻辑，但 `maybe_chunk_residual` 的 3D padding 与 fake shape 存在待修正问题，且尚未证明是当前 eager 分叉原因。
  title: K3 FlashComm 精度差异定位索引
navigation: 当前重点是先在 enforce eager 下确认 fc1/FlashComm off-on 精度差异，再单独排查 ACLGraph 首层处理；不要将问题归因于 fused MoE，除非后续证据改变该判断。token 轴通常为 dim0，pad/unpad 可优先复用框架逻辑，但 `maybe_chunk_residual` 的 3D padding 与 fake shape 存在待修正问题，且尚未证明是当前 eager 分叉原因。
schema: mindie-material-task/1
status: complete
task_id: ad3c127c93d771f8dddc1ba70d4685e97509e9f2ffa4b50f286ad02ab6c0ff60
---

# K3 FlashComm 精度差异定位索引

当前重点是先在 enforce eager 下确认 fc1/FlashComm off-on 精度差异，再单独排查 ACLGraph 首层处理；不要将问题归因于 fused MoE，除非后续证据改变该判断。token 轴通常为 dim0，pad/unpad 可优先复用框架逻辑，但 `maybe_chunk_residual` 的 3D padding 与 fake shape 存在待修正问题，且尚未证明是当前 eager 分叉原因。

## Materials

- [K3 FlashComm 与 fused MoE/首层精度定位](blocks/e77f3c2827ca461f603f1a07a7d281d82d668f8a5f4c0527cbb6419dbdf55d9f.md): 讨论认为当前 K3 FlashComm 修改不改变 fused MoE 的路由、专家计算、EP 通信后端或输出归并；token 维 AllGather 与未切分的 linear/RMSNorm 在数学上可交换，但不同 GEMM 的 M、BF16 reduce-scatter、shared expert 权重形态可能带来微小数值差异并被 recurrent KDA 放大。只读审计未确认 ACLGraph、首层布局、KDA state 或 MoE 存在语义错位；已指出 `maybe_chunk_residual` 对 3D 张量可能错误地 pad dim1、fake shape 不保留 trailing dimensions，但当前 DP1 runner 的 `pad_size == 0`，不能据此解释 eager 精度分叉。后续应分开 enforce eager 与 ACLGraph case，用不同层数、并发请求固定 M、teacher-force 对比 KDA/MLA/残差/state 等中间结果；这些是定位建议，不是已验证修复。

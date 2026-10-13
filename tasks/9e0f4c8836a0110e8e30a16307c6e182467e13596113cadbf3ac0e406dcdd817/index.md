---
blocks:
- block_id: 454a3335b70c815416be56593c25baaa431069d63a1886c561e9410aa345b9b6
  indexed: true
  sha256: 55fd0f5e520403b72f673283ad5c5000611dba0346baff324f0d2ffa6b499981
  source_range:
    end: 748927
    part: 0
    start: 0
  summary: 只读诊断将 cache artifact 第 135 行定位为 layer 1 `raw_gate` 的 `aten.reshape`，编译样例固定为 `[16,6,128]`，而 profile 中输入为 2048 tokens、元素数对应 `[2048,6,128]`；该结论被报告为根因，但仍属于未验证的参考判断。对照关闭 FlashComm 的 cache 后，同位置保留动态维，因此报告推测问题来自首层 RS→AG 布局切换导致派生动态维在 ACL/AOT artifact 中静态化，而非权重或 head 配置；artifact 第 315 行还显示 layer 3 MLA output buffer 可能同样固化，所给 KDA reshape 修改与入口按 TP rank 切 local token 的方案均是建议，未报告实施或验证结果。
  title: FlashComm 首层动态维静态化导致 KDA reshape 错配
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9e0f4c8836a0110e8e30a16307c6e182467e13596113cadbf3ac0e406dcdd817
  kind: experience
  material_digest: cb3caaf508c5ffebb6a1d1efbab2565d1db82d330c77f1dd235262c22532cf94
  revision: 6abdaaf0c8002000292859a21dbb1f6122ab4e5f10fab1ec0c28fb84d67fb6c7
  schema: mindie-entry/3
  summary: 当前唯一材料聚焦于 vllm_ascend Kimi KDA 的 `aten.reshape` 维度错误：编译样例 16 tokens 与运行时 2048 tokens 不一致，疑似由首层 FlashComm RS→AG 布局和 ACL/AOT 静态化造成。关闭 FlashComm 的对照现象、layer 3 MLA 的 `[1,7168]` artifact，以及建议中的 reshape 和入口 local-token 改造都应作为未验证线索保留，不能视为修复成功。
  title: 当前任务导航：FlashComm 动态维错配参考索引
navigation: 当前唯一材料聚焦于 vllm_ascend Kimi KDA 的 `aten.reshape` 维度错误：编译样例 16 tokens 与运行时 2048 tokens 不一致，疑似由首层 FlashComm RS→AG 布局和 ACL/AOT 静态化造成。关闭 FlashComm 的对照现象、layer 3 MLA 的 `[1,7168]` artifact，以及建议中的 reshape 和入口 local-token 改造都应作为未验证线索保留，不能视为修复成功。
schema: mindie-material-task/1
status: complete
task_id: 9e0f4c8836a0110e8e30a16307c6e182467e13596113cadbf3ac0e406dcdd817
---

# 当前任务导航：FlashComm 动态维错配参考索引

当前唯一材料聚焦于 vllm_ascend Kimi KDA 的 `aten.reshape` 维度错误：编译样例 16 tokens 与运行时 2048 tokens 不一致，疑似由首层 FlashComm RS→AG 布局和 ACL/AOT 静态化造成。关闭 FlashComm 的对照现象、layer 3 MLA 的 `[1,7168]` artifact，以及建议中的 reshape 和入口 local-token 改造都应作为未验证线索保留，不能视为修复成功。

## Materials

- [FlashComm 首层动态维静态化导致 KDA reshape 错配](blocks/454a3335b70c815416be56593c25baaa431069d63a1886c561e9410aa345b9b6.md): 只读诊断将 cache artifact 第 135 行定位为 layer 1 `raw_gate` 的 `aten.reshape`，编译样例固定为 `[16,6,128]`，而 profile 中输入为 2048 tokens、元素数对应 `[2048,6,128]`；该结论被报告为根因，但仍属于未验证的参考判断。对照关闭 FlashComm 的 cache 后，同位置保留动态维，因此报告推测问题来自首层 RS→AG 布局切换导致派生动态维在 ACL/AOT artifact 中静态化，而非权重或 head 配置；artifact 第 315 行还显示 layer 3 MLA output buffer 可能同样固化，所给 KDA reshape 修改与入口按 TP rank 切 local token 的方案均是建议，未报告实施或验证结果。

---
blocks:
- block_id: 805556d158cd8123de986260d9540c07dfb828e571af4c3a6185236c0a9ccc6c
  indexed: true
  sha256: ae84204bc7d70807dc5d5f42f36d0f2ac5606da534e57bb17a427b5c034bb758
  source_range:
    end: 8757029
    part: 0
    start: 0
  summary: 该批材料记录了对 vLLM Ascend、Kimi K3 相关 session 的全量扫描，形成 30 个知识候选、449 个 thread 分类和专项审阅报告，覆盖模型组网、SiTU/KDA 算子、量化权重、DSpark、缓存、图模式、部署与评测协议；这些内容均为内部待审候选，未入库、未发布，也未执行历史命令。后续基于当前 main 源码复核后，完成 30 条审核（10 rewrite、15 drop、5 defer）及 8 条算子交叉审核；release 中未找到 Post-WU workspace 修复和排序限流实现，MRV2 的 K3 验证也不能由 MRV1 测试替代。保留内容聚焦算子返回值、有效 state 行、SiTU 类型分支、draft 辅助输入和权重重载地址等当前调用契约，并保留历史纠错、弱证据降级及未完成验证状态。
  title: vLLM Ascend/Kimi K3 知识候选审阅与校正
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f4e84031e7b649f282b81d4dc19b4b8f0cdda230b05551ef62b12745bf716404
  kind: experience
  material_digest: ade119aae2341c773a226d164a654b3ee3e5d3493d75d0e9a8276eb605b0555e
  revision: 763abf837822ec226fe77f8f062c9db4f32c873d09203f2c0e3200b0bab3f3ff
  schema: mindie-entry/3
  summary: 当前材料从全量 session 扫描产物推进到基于 main 源码的 K3 候选复核，已记录候选生成、证据范围、纠错链、rewrite/drop/defer 结果及算子交叉审核。可检索重点包括 SiTU/KDA、量化与权重、DSpark、缓存、图模式、部署评测、Post-WU workspace、排序限流、MRV1/MRV2，以及算子返回值、state 行、draft 输入和重载地址契约；release 实现缺失和 MRV2 验证不足仍是未决或否定性结果。
  title: K3 知识候选：扫描、源码复核与未决项
navigation: 当前材料从全量 session 扫描产物推进到基于 main 源码的 K3 候选复核，已记录候选生成、证据范围、纠错链、rewrite/drop/defer 结果及算子交叉审核。可检索重点包括 SiTU/KDA、量化与权重、DSpark、缓存、图模式、部署评测、Post-WU workspace、排序限流、MRV1/MRV2，以及算子返回值、state 行、draft 输入和重载地址契约；release 实现缺失和 MRV2 验证不足仍是未决或否定性结果。
schema: mindie-material-task/1
status: complete
task_id: f4e84031e7b649f282b81d4dc19b4b8f0cdda230b05551ef62b12745bf716404
---

# K3 知识候选：扫描、源码复核与未决项

当前材料从全量 session 扫描产物推进到基于 main 源码的 K3 候选复核，已记录候选生成、证据范围、纠错链、rewrite/drop/defer 结果及算子交叉审核。可检索重点包括 SiTU/KDA、量化与权重、DSpark、缓存、图模式、部署评测、Post-WU workspace、排序限流、MRV1/MRV2，以及算子返回值、state 行、draft 输入和重载地址契约；release 实现缺失和 MRV2 验证不足仍是未决或否定性结果。

## Materials

- [vLLM Ascend/Kimi K3 知识候选审阅与校正](blocks/805556d158cd8123de986260d9540c07dfb828e571af4c3a6185236c0a9ccc6c.md): 该批材料记录了对 vLLM Ascend、Kimi K3 相关 session 的全量扫描，形成 30 个知识候选、449 个 thread 分类和专项审阅报告，覆盖模型组网、SiTU/KDA 算子、量化权重、DSpark、缓存、图模式、部署与评测协议；这些内容均为内部待审候选，未入库、未发布，也未执行历史命令。后续基于当前 main 源码复核后，完成 30 条审核（10 rewrite、15 drop、5 defer）及 8 条算子交叉审核；release 中未找到 Post-WU workspace 修复和排序限流实现，MRV2 的 K3 验证也不能由 MRV1 测试替代。保留内容聚焦算子返回值、有效 state 行、SiTU 类型分支、draft 辅助输入和权重重载地址等当前调用契约，并保留历史纠错、弱证据降级及未完成验证状态。

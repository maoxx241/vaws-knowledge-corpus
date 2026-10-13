---
blocks:
- block_id: b31997d666d1f749951df9cf9979da5049f95bf21735c66bd9685bdfde3aebc4
  indexed: true
  sha256: 27244a493413dc1ceb139644e84925564a99eb99c601f993d767f98cf48e7bb3
  source_range:
    end: 893109
    part: 0
    start: 0
  summary: 只读审计报告称，真实 checkpoint 的 62 个 tensor 中有 60 个精确加载，2 个 confidence head 按临时 MRV1 设计跳过，未观察到 missing、unexpected 或 shape mismatch；该结论来自报告内容，未在此独立验证。报告指出，loader 对除 `lm_head` 外的 key 统一添加 `model.` 前缀，原始 checkpoint 命名是否已含此前缀仍是明确风险点。另报告称 5 层 P/D 配置 `target_layer_ids=[0,1,2,3,4]` 只生成 4 份辅助状态，可能导致 `fc [7168,35840]` 前向维度错误；提出 `[-1,0,1,2,3]` 或在最终层后补采 boundary 5 作为临时方案，结果未验证。
  title: Checkpoint 映射与 P/D 层数审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 311473371b2656767e18093a0f903baf2b47d5d3b22d26335781423263528831
  kind: experience
  material_digest: b868de95ca2a3e54808c8e5566706b6a60c3b7e1894a852dbc0595963268a01b
  revision: 9cce335bda363f52deac95d44550997f9b9ed45f2f06d7fff78d08c170fb28f7
  schema: mindie-entry/3
  summary: 本批材料记录了 checkpoint key/shape、DSpark 权重映射、TP16 分片及 embedding/lm_head 共享的报告性检查，并指出 5 层 P/D 的辅助状态数量与前向维度风险。加载无 blocker、精确加载数量及未出现 mismatch 均属于来源中的审计结论，不应视为独立验证；`model.` 双前缀风险和 P/D 修正方案仍需后续确认。原始材料保留为独立可检索来源。
  title: 当前任务导航：Checkpoint 加载与 P/D 层审计
navigation: 本批材料记录了 checkpoint key/shape、DSpark 权重映射、TP16 分片及 embedding/lm_head 共享的报告性检查，并指出 5 层 P/D 的辅助状态数量与前向维度风险。加载无 blocker、精确加载数量及未出现 mismatch 均属于来源中的审计结论，不应视为独立验证；`model.` 双前缀风险和 P/D 修正方案仍需后续确认。原始材料保留为独立可检索来源。
schema: mindie-material-task/1
status: complete
task_id: 311473371b2656767e18093a0f903baf2b47d5d3b22d26335781423263528831
---

# 当前任务导航：Checkpoint 加载与 P/D 层审计

本批材料记录了 checkpoint key/shape、DSpark 权重映射、TP16 分片及 embedding/lm_head 共享的报告性检查，并指出 5 层 P/D 的辅助状态数量与前向维度风险。加载无 blocker、精确加载数量及未出现 mismatch 均属于来源中的审计结论，不应视为独立验证；`model.` 双前缀风险和 P/D 修正方案仍需后续确认。原始材料保留为独立可检索来源。

## Materials

- [Checkpoint 映射与 P/D 层数审计](blocks/b31997d666d1f749951df9cf9979da5049f95bf21735c66bd9685bdfde3aebc4.md): 只读审计报告称，真实 checkpoint 的 62 个 tensor 中有 60 个精确加载，2 个 confidence head 按临时 MRV1 设计跳过，未观察到 missing、unexpected 或 shape mismatch；该结论来自报告内容，未在此独立验证。报告指出，loader 对除 `lm_head` 外的 key 统一添加 `model.` 前缀，原始 checkpoint 命名是否已含此前缀仍是明确风险点。另报告称 5 层 P/D 配置 `target_layer_ids=[0,1,2,3,4]` 只生成 4 份辅助状态，可能导致 `fc [7168,35840]` 前向维度错误；提出 `[-1,0,1,2,3]` 或在最终层后补采 boundary 5 作为临时方案，结果未验证。

---
blocks:
- block_id: dc2ff60c71e37c18e52b8b2091349bd33f630e9d9911823ea58da40950a74b7e
  indexed: true
  sha256: 49f3bbce9e6057a151577edc7d94a54c318b73f0344d98a654d7b9a9931e14eb
  source_range:
    end: 1029144
    part: 0
    start: 0
  summary: 讨论认为当前 K3 修改据报告未涉及 fused MoE 的路由、专家计算、EP 后端或输出归并；可能存在的仅是 GEMM shape 导致的微小浮点舍入差异，不能据此视为已验证。后续应分开检查 enforce-eager 下 fc1 精度、ACLGraph 首层处理，以及 pad/unpad 是否可复用首轴 token 逻辑；还需关注 K3 的 `[N, block, hidden]`、DeepSeek V4/HC pre 的非标准布局、`maybe_chunk_residual` 的 fake/meta `empty_like(x)` 问题和 W4A8 GEMM 的 M=1/M=16 差异。已报告诊断方案与 canonical probe 对齐规则，但本批次未修改文件或操作远端，问题结论仍需通过实际探针和 case 对比确认。
  title: K3 FlashComm 与首层 token 布局诊断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: de154f93c0ad6c537990d037d3a8c4689743801d9288a22bdb1ef598a59cb1c5
  kind: experience
  material_digest: 78f6f75992134a3ac9b87a1d0e7ba77a519f984f526e20858a701aba1f4994ea
  revision: 57d1ba934a8cd63454e80734d46695d1fc66895cd61ba617337fb8afb8627575
  schema: mindie-entry/3
  summary: 当前索引聚焦于 fused MoE 影响边界、enforce-eager fc1 精度、ACLGraph 首层路径，以及 token 轴上的 pad/unpad 复用。保留未完成事项：验证 `[N, block, hidden]` 与 HC pre 布局在框架 gather/chunk 中的处理、修正或确认 `maybe_chunk_residual` meta 行为，并区分 collective、padding 与 GEMM shape 对精度的影响；现有内容是可检索的推断和诊断记录，不代表已验证成功。
  title: K3 FlashComm 首层精度与布局排查
navigation: 当前索引聚焦于 fused MoE 影响边界、enforce-eager fc1 精度、ACLGraph 首层路径，以及 token 轴上的 pad/unpad 复用。保留未完成事项：验证 `[N, block, hidden]` 与 HC pre 布局在框架 gather/chunk 中的处理、修正或确认 `maybe_chunk_residual` meta 行为，并区分 collective、padding 与 GEMM shape 对精度的影响；现有内容是可检索的推断和诊断记录，不代表已验证成功。
schema: mindie-material-task/1
status: complete
task_id: de154f93c0ad6c537990d037d3a8c4689743801d9288a22bdb1ef598a59cb1c5
---

# K3 FlashComm 首层精度与布局排查

当前索引聚焦于 fused MoE 影响边界、enforce-eager fc1 精度、ACLGraph 首层路径，以及 token 轴上的 pad/unpad 复用。保留未完成事项：验证 `[N, block, hidden]` 与 HC pre 布局在框架 gather/chunk 中的处理、修正或确认 `maybe_chunk_residual` meta 行为，并区分 collective、padding 与 GEMM shape 对精度的影响；现有内容是可检索的推断和诊断记录，不代表已验证成功。

## Materials

- [K3 FlashComm 与首层 token 布局诊断](blocks/dc2ff60c71e37c18e52b8b2091349bd33f630e9d9911823ea58da40950a74b7e.md): 讨论认为当前 K3 修改据报告未涉及 fused MoE 的路由、专家计算、EP 后端或输出归并；可能存在的仅是 GEMM shape 导致的微小浮点舍入差异，不能据此视为已验证。后续应分开检查 enforce-eager 下 fc1 精度、ACLGraph 首层处理，以及 pad/unpad 是否可复用首轴 token 逻辑；还需关注 K3 的 `[N, block, hidden]`、DeepSeek V4/HC pre 的非标准布局、`maybe_chunk_residual` 的 fake/meta `empty_like(x)` 问题和 W4A8 GEMM 的 M=1/M=16 差异。已报告诊断方案与 canonical probe 对齐规则，但本批次未修改文件或操作远端，问题结论仍需通过实际探针和 case 对比确认。

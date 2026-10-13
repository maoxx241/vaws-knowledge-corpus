---
blocks:
- block_id: f67edf329fdbca557375870849aa181b8cb48874434c090c4f7ad0da486f9770
  indexed: true
  sha256: 9af46d988f9c0e35c91c639fa86864373011f76135a9ad0a257a792db5c55424
  source_range:
    end: 740230
    part: 0
    start: 0
  summary: 审查结论是不建议提交：当 checkpoint reload 仅更新部分 q/k/v 权重或存在缺失权重时，`_pack_conv_weights()` 可能因兄弟模块仍为 meta tensor 提前返回，继而将新 materialize 但未初始化的 `packed_conv_weights` 复制回 ACLGraph storage，造成静默精度错误。完整 reload、kernel-format direct reload、`prefer_copy=True` 的 data_ptr 稳定性、v0.25.1 API 兼容及热路径设计被认为成立，但新增 persistent Parameter 会使缺少该 key 的旧 `sharded_state` 报 missing key；需补 partial/missing reload 的回退语义与 UT。
  title: checkpoint reload 部分权重回载阻断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 982552075e7aed3f6b5fc4902e89ada26f34afde085f3fd12dd9f554744af087
  kind: experience
  material_digest: 440bec12d65a3f71317f238ce6b814994a70e8ed406a10b91b9dcd600b17b305
  revision: eca34f62b58578c561e4685bbc87f695b10bce88ec0c3103ee8d2bc292cbd40b
  schema: mindie-entry/3
  summary: 当前重点是 checkpoint reload 的部分权重与缺失权重处理，尤其关注 meta tensor、packed_conv_weights 初始化和 ACLGraph storage 回写导致的静默精度风险。已有结论保留：完整 reload、kernel-format direct reload、data_ptr 稳定性、API 兼容及热路径布局转换移除未发现阻断；旧 sharded checkpoint 的新增 Parameter 兼容边界仍需明确。提交前需补齐 partial/missing reload 的正确性与测试。
  title: 当前任务导航：reload 正确性审查
navigation: 当前重点是 checkpoint reload 的部分权重与缺失权重处理，尤其关注 meta tensor、packed_conv_weights 初始化和 ACLGraph storage 回写导致的静默精度风险。已有结论保留：完整 reload、kernel-format direct reload、data_ptr 稳定性、API 兼容及热路径布局转换移除未发现阻断；旧 sharded checkpoint 的新增 Parameter 兼容边界仍需明确。提交前需补齐 partial/missing reload 的正确性与测试。
schema: mindie-material-task/1
status: complete
task_id: 982552075e7aed3f6b5fc4902e89ada26f34afde085f3fd12dd9f554744af087
---

# 当前任务导航：reload 正确性审查

当前重点是 checkpoint reload 的部分权重与缺失权重处理，尤其关注 meta tensor、packed_conv_weights 初始化和 ACLGraph storage 回写导致的静默精度风险。已有结论保留：完整 reload、kernel-format direct reload、data_ptr 稳定性、API 兼容及热路径布局转换移除未发现阻断；旧 sharded checkpoint 的新增 Parameter 兼容边界仍需明确。提交前需补齐 partial/missing reload 的正确性与测试。

## Materials

- [checkpoint reload 部分权重回载阻断](blocks/f67edf329fdbca557375870849aa181b8cb48874434c090c4f7ad0da486f9770.md): 审查结论是不建议提交：当 checkpoint reload 仅更新部分 q/k/v 权重或存在缺失权重时，`_pack_conv_weights()` 可能因兄弟模块仍为 meta tensor 提前返回，继而将新 materialize 但未初始化的 `packed_conv_weights` 复制回 ACLGraph storage，造成静默精度错误。完整 reload、kernel-format direct reload、`prefer_copy=True` 的 data_ptr 稳定性、v0.25.1 API 兼容及热路径设计被认为成立，但新增 persistent Parameter 会使缺少该 key 的旧 `sharded_state` 报 missing key；需补 partial/missing reload 的回退语义与 UT。

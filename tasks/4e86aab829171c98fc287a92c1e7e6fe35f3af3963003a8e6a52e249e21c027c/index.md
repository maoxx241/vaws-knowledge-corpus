---
blocks:
- block_id: dacbecaa8f09f3c90ccc9a9846a709c251a916138a35cd1e9f2bbd4ce3b66a48
  indexed: true
  sha256: e6a10e47c10f6bc5184e36a9d56b64742cb4144ee933cdc27ca3ebaba4b19df2
  source_range:
    end: 704279
    part: 0
    start: 0
  summary: 只读核查报告称，Kimi K3 实际注册类型正确，且不启用 processor cache 时 dummy processing 正常；启用 MultiModalProcessorOnlyCache 后，MM-only 快路径仅识别 images/videos/audio，将 vision_chunks 丢弃，随后因存在 hash 但缺少 item 触发 _merge_mm_kwargs IndexError。临时加入 Kimi K2.5 风格的 _call_hf_processor override 后，据报告同一测试返回 1 个 vision_chunk item，但该结论属于远端实测与分析记录，仍应通过回归测试确认。建议补充覆盖 cache miss/hit、joint text/MM 分支及 pixel_values、grid_thws 保留情况的测试；use_unified_vision_chunk=True 被认为是必要配置，但与本次 dummy 崩溃并非同一问题。
  title: Kimi K3 多模态缓存快路径导致 dummy processing 崩溃
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4e86aab829171c98fc287a92c1e7e6fe35f3af3963003a8e6a52e249e21c027c
  kind: experience
  material_digest: 5a3eb0826f4475a80dbd26f2a6176298224f7f013b12461fd0658a77877be7bf
  revision: 6c392a0935e4bd35c5fc71546f27353f47f90e8dab17e89e2410e5acd708b105
  schema: mindie-entry/3
  summary: 当前材料聚焦于 Kimi K3 的 vision_chunks 在 vLLM 多模态 processor cache 的 MM-only 快路径中被忽略，导致空 MM kwargs 后出现 IndexError。已记录的临时修复是增加与 Kimi K2.5 相同的 _call_hf_processor override；修复效果和 cache miss/hit 回归覆盖仍待确认，相关材料应按原 block 独立检索。
  title: 当前任务导航：Kimi K3 processor cache 路径问题
navigation: 当前材料聚焦于 Kimi K3 的 vision_chunks 在 vLLM 多模态 processor cache 的 MM-only 快路径中被忽略，导致空 MM kwargs 后出现 IndexError。已记录的临时修复是增加与 Kimi K2.5 相同的 _call_hf_processor override；修复效果和 cache miss/hit 回归覆盖仍待确认，相关材料应按原 block 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 4e86aab829171c98fc287a92c1e7e6fe35f3af3963003a8e6a52e249e21c027c
---

# 当前任务导航：Kimi K3 processor cache 路径问题

当前材料聚焦于 Kimi K3 的 vision_chunks 在 vLLM 多模态 processor cache 的 MM-only 快路径中被忽略，导致空 MM kwargs 后出现 IndexError。已记录的临时修复是增加与 Kimi K2.5 相同的 _call_hf_processor override；修复效果和 cache miss/hit 回归覆盖仍待确认，相关材料应按原 block 独立检索。

## Materials

- [Kimi K3 多模态缓存快路径导致 dummy processing 崩溃](blocks/dacbecaa8f09f3c90ccc9a9846a709c251a916138a35cd1e9f2bbd4ce3b66a48.md): 只读核查报告称，Kimi K3 实际注册类型正确，且不启用 processor cache 时 dummy processing 正常；启用 MultiModalProcessorOnlyCache 后，MM-only 快路径仅识别 images/videos/audio，将 vision_chunks 丢弃，随后因存在 hash 但缺少 item 触发 _merge_mm_kwargs IndexError。临时加入 Kimi K2.5 风格的 _call_hf_processor override 后，据报告同一测试返回 1 个 vision_chunk item，但该结论属于远端实测与分析记录，仍应通过回归测试确认。建议补充覆盖 cache miss/hit、joint text/MM 分支及 pixel_values、grid_thws 保留情况的测试；use_unified_vision_chunk=True 被认为是必要配置，但与本次 dummy 崩溃并非同一问题。

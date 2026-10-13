---
blocks:
- block_id: 11f88baf5bba239a6752748f2de926e14fe584f0d834c5992ac86be5e0c28720
  indexed: true
  sha256: bf1466a71ff3136d98a7f391e12f6aa6be6b21ebd83199df59d5827c1d7af9c8
  source_range:
    end: 574386
    part: 0
    start: 0
  summary: '只读分析认为，vLLM 0.23 的 `MultiModalBudget` cache-miss 路径将 `vision_chunk` 转为 `vision_chunks`，但通用 MM-only helper 仅支持 `images`、`videos`、`audio`，导致返回空 `BatchFeature`，后续索引空的 `missing_kwargs` 时触发 `IndexError`；实际加载的 `KimiK3MultiModalProcessor` 与 `KimiK3Processor` 类型被确认正确。分析提出在 `KimiK3MultiModalProcessor` 中透传覆写 `_call_hf_processor`，以强制 joint text/MM 路径，并报告等价运行时 override 下 `MultiModalBudget` 成功、得到 `budget_ok {''vision_chunk'': 1}`；但材料同时称未修改文件，又称 worktree 中存在并发修复及回归测试，因此修复是否已落地、测试是否实际完成仍需区分验证。'
  title: K3 多模态 processor 的 MM-only 分支诊断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e876b4a8217e9c6bb5f920243df240974100ccfc9ff3cb5561ed39e57a939cf4
  kind: experience
  material_digest: be0b13778c6a9bff5748025b5dbef20ae63748f600042d2c8f3fef963e9f9a28
  revision: 18587c126998bcf130d23230bbbeffa93cf1d3554c5ef66e558c184c399a876d
  schema: mindie-entry/3
  summary: 已索引 K3 多模态启动失败的诊断材料：核心线索是 `vision_chunks` 被 MM-only helper 忽略并最终触发 `IndexError`，而非 FlashComm/KDA 或错误 checkpoint processor。建议后续检索 `_call_hf_processor`、`MultiModalBudget`、`vision_chunk(s)`、`missing_kwargs`、cache-miss 回归测试；透传 override 及其成功结果属于报告的验证线索，但材料对文件是否已修改和测试是否完成存在不一致，不能视为已确认落地。
  title: 当前任务导航
navigation: 已索引 K3 多模态启动失败的诊断材料：核心线索是 `vision_chunks` 被 MM-only helper 忽略并最终触发 `IndexError`，而非 FlashComm/KDA 或错误 checkpoint processor。建议后续检索 `_call_hf_processor`、`MultiModalBudget`、`vision_chunk(s)`、`missing_kwargs`、cache-miss 回归测试；透传 override 及其成功结果属于报告的验证线索，但材料对文件是否已修改和测试是否完成存在不一致，不能视为已确认落地。
schema: mindie-material-task/1
status: complete
task_id: e876b4a8217e9c6bb5f920243df240974100ccfc9ff3cb5561ed39e57a939cf4
---

# 当前任务导航

已索引 K3 多模态启动失败的诊断材料：核心线索是 `vision_chunks` 被 MM-only helper 忽略并最终触发 `IndexError`，而非 FlashComm/KDA 或错误 checkpoint processor。建议后续检索 `_call_hf_processor`、`MultiModalBudget`、`vision_chunk(s)`、`missing_kwargs`、cache-miss 回归测试；透传 override 及其成功结果属于报告的验证线索，但材料对文件是否已修改和测试是否完成存在不一致，不能视为已确认落地。

## Materials

- [K3 多模态 processor 的 MM-only 分支诊断](blocks/11f88baf5bba239a6752748f2de926e14fe584f0d834c5992ac86be5e0c28720.md): 只读分析认为，vLLM 0.23 的 `MultiModalBudget` cache-miss 路径将 `vision_chunk` 转为 `vision_chunks`，但通用 MM-only helper 仅支持 `images`、`videos`、`audio`，导致返回空 `BatchFeature`，后续索引空的 `missing_kwargs` 时触发 `IndexError`；实际加载的 `KimiK3MultiModalProcessor` 与 `KimiK3Processor` 类型被确认正确。分析提出在 `KimiK3MultiModalProcessor` 中透传覆写 `_call_hf_processor`，以强制 joint text/MM 路径，并报告等价运行时 override 下 `MultiModalBudget` 成功、得到 `budget_ok {'vision_chunk': 1}`；但材料同时称未修改文件，又称 worktree 中存在并发修复及回归测试，因此修复是否已落地、测试是否实际完成仍需区分验证。

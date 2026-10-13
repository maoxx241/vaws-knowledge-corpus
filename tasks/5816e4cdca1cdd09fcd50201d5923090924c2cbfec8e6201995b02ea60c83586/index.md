---
blocks:
- block_id: 9d41a05fbcb2566ada0f80e82a330560b4c2b472754ff0e03147928df6249f64
  indexed: true
  sha256: b38ca24a3bdd2097c4e4983e7001135e4531f2ec119dc62eda6f2ff31f6df4df
  source_range:
    end: 503374
    part: 0
    start: 0
  summary: '只读审计认为，`vision_chunk` 被规范化为 `vision_chunks` 本身并非根因；K3 未覆写 `_call_hf_processor`，使预算 profiling 的 MM-only 路径误用仅支持 `images`、`videos`、`audio` 的通用 helper，导致 `vision_chunks` 被忽略并最终触发 `IndexError`。已确认 vLLM 与 HF processor 类型及继承关系；通过独立进程中的等价运行时 override 重跑 `MultiModalBudget` 得到 `budget_ok {''vision_chunk'': 1}`，但这不是代码修复后的验证结果。建议增加透传 override 和回归测试，当前仍属于修复建议，未报告实际源码修改或完整测试结果。'
  title: K3 vision_chunks MM-only 分支审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5816e4cdca1cdd09fcd50201d5923090924c2cbfec8e6201995b02ea60c83586
  kind: experience
  material_digest: 63c33b6e3149a02b6294c41f38a56ffdbbfbb68a61198aae67d99b97457a13a4
  revision: 57ddf0c134cd023241ec862121e45eab2a5e3abc128db4116561192e74a183f4
  schema: mindie-entry/3
  summary: 当前材料集中于 K3 的 `vision_chunk`/`vision_chunks` 参数转换、`MultiModalBudget` 启动阶段 MM-only 路径、`KimiK3Processor` 未被调用及其 `IndexError` 机制。已有等价 override 的单独运行观察结果，但不能视为正式修复成功；建议的 `_call_hf_processor` override 与回归测试尚未在材料中确认实施。原始块仍可独立检索，后续应保留任何失败尝试、未完成测试和修正结论。
  title: K3 多模态 processor 调用链与预算 profiling
navigation: 当前材料集中于 K3 的 `vision_chunk`/`vision_chunks` 参数转换、`MultiModalBudget` 启动阶段 MM-only 路径、`KimiK3Processor` 未被调用及其 `IndexError` 机制。已有等价 override 的单独运行观察结果，但不能视为正式修复成功；建议的 `_call_hf_processor` override 与回归测试尚未在材料中确认实施。原始块仍可独立检索，后续应保留任何失败尝试、未完成测试和修正结论。
schema: mindie-material-task/1
status: complete
task_id: 5816e4cdca1cdd09fcd50201d5923090924c2cbfec8e6201995b02ea60c83586
---

# K3 多模态 processor 调用链与预算 profiling

当前材料集中于 K3 的 `vision_chunk`/`vision_chunks` 参数转换、`MultiModalBudget` 启动阶段 MM-only 路径、`KimiK3Processor` 未被调用及其 `IndexError` 机制。已有等价 override 的单独运行观察结果，但不能视为正式修复成功；建议的 `_call_hf_processor` override 与回归测试尚未在材料中确认实施。原始块仍可独立检索，后续应保留任何失败尝试、未完成测试和修正结论。

## Materials

- [K3 vision_chunks MM-only 分支审计](blocks/9d41a05fbcb2566ada0f80e82a330560b4c2b472754ff0e03147928df6249f64.md): 只读审计认为，`vision_chunk` 被规范化为 `vision_chunks` 本身并非根因；K3 未覆写 `_call_hf_processor`，使预算 profiling 的 MM-only 路径误用仅支持 `images`、`videos`、`audio` 的通用 helper，导致 `vision_chunks` 被忽略并最终触发 `IndexError`。已确认 vLLM 与 HF processor 类型及继承关系；通过独立进程中的等价运行时 override 重跑 `MultiModalBudget` 得到 `budget_ok {'vision_chunk': 1}`，但这不是代码修复后的验证结果。建议增加透传 override 和回归测试，当前仍属于修复建议，未报告实际源码修改或完整测试结果。

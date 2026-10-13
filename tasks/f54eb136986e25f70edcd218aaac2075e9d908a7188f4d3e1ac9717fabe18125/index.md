---
blocks:
- block_id: b3c9c1b6fd10aaee25fc84f3ae15f42f1b6e214937b82ce4e2abbeef0c57f8de
  indexed: true
  sha256: 52dc7797381e0c5283441a17c0d712e6758e09398ae9f86a2b577fe4c0e79382
  source_range:
    end: 1418027
    part: 0
    start: 0
  summary: 审计认为当前分支暂不适合直接发 PR：多个全局 monkeypatch 已偏离目标 vLLM main API，可能因缺少 `mm_token_counts` 或 `is_harmony=False` 触发 `TypeError`，且本次仅审计、未修改文件。建议移除服务层和 renderer 的猴子补丁、静默 fail-soft、过度 `getattr/hasattr` 与跳过即通过测试，同时将 K3/MLA/XTML 等隐式通道重构为 typed contract；但 checkpoint ABI、输入解析、multimodal 形状、DMA 对齐等真实边界校验必须保留。
  title: 防御性编程与过度兼容审计结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f54eb136986e25f70edcd218aaac2075e9d908a7188f4d3e1ac9717fabe18125
  kind: experience
  material_digest: 8af58639352bae019f72b35c68e8242514b4d2f92037dc6547f892ff1ae51858
  revision: e71073dc308137c9f80cf9d1d4d4801ecaacd084c395107c7e136e82343c15dd
  schema: mindie-entry/3
  summary: 已记录一轮针对 vLLM API 漂移、全局 monkeypatch、duck typing、静默降级和热路径校验的审计结果；结论是存在 P0 级兼容风险，尚未实施修复。后续应优先处理缺少 `mm_token_counts`/`is_harmony` 的接口问题及服务层补丁，再区分可删除的伪兼容与必须保留的 ABI、输入和配置边界校验；测试中的 schema 缺失不应继续被无条件 skip。
  title: 当前任务导航：防御性兼容审计
navigation: 已记录一轮针对 vLLM API 漂移、全局 monkeypatch、duck typing、静默降级和热路径校验的审计结果；结论是存在 P0 级兼容风险，尚未实施修复。后续应优先处理缺少 `mm_token_counts`/`is_harmony` 的接口问题及服务层补丁，再区分可删除的伪兼容与必须保留的 ABI、输入和配置边界校验；测试中的 schema 缺失不应继续被无条件 skip。
schema: mindie-material-task/1
status: complete
task_id: f54eb136986e25f70edcd218aaac2075e9d908a7188f4d3e1ac9717fabe18125
---

# 当前任务导航：防御性兼容审计

已记录一轮针对 vLLM API 漂移、全局 monkeypatch、duck typing、静默降级和热路径校验的审计结果；结论是存在 P0 级兼容风险，尚未实施修复。后续应优先处理缺少 `mm_token_counts`/`is_harmony` 的接口问题及服务层补丁，再区分可删除的伪兼容与必须保留的 ABI、输入和配置边界校验；测试中的 schema 缺失不应继续被无条件 skip。

## Materials

- [防御性编程与过度兼容审计结论](blocks/b3c9c1b6fd10aaee25fc84f3ae15f42f1b6e214937b82ce4e2abbeef0c57f8de.md): 审计认为当前分支暂不适合直接发 PR：多个全局 monkeypatch 已偏离目标 vLLM main API，可能因缺少 `mm_token_counts` 或 `is_harmony=False` 触发 `TypeError`，且本次仅审计、未修改文件。建议移除服务层和 renderer 的猴子补丁、静默 fail-soft、过度 `getattr/hasattr` 与跳过即通过测试，同时将 K3/MLA/XTML 等隐式通道重构为 typed contract；但 checkpoint ABI、输入解析、multimodal 形状、DMA 对齐等真实边界校验必须保留。

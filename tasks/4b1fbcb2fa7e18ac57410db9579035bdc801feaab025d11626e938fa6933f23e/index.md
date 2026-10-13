---
blocks:
- block_id: fd7e867cfb970528bc7247e20f93f98f116d67763241d2a72b28f97a102827f7
  indexed: true
  sha256: 9126e161d9cd671f7b269e07e70f227387d6dcbda7e5cf32cb2a080bba10182d
  source_range:
    end: 1278339
    part: 0
    start: 0
  summary: 已完成本地实验性修改，涉及 RoPE/KV cache 探针、逐层 KV 处理、FP32 非因果 GQA attention 参考、context KV/cache trace、FC feature slice 及序列长度元数据；未部署或操作远端。仅确认通过 `python3 -m py_compile` 和 `git diff --check`，尚无 NPU 运行验证，且 `_seq_lens_cpu` 是否导致 builder 使用 720 而非 727 仍未修正。结果存在明确适用范围和风险：全局挂载尚未收窄至 legacy Kimi K3 DSpark，直接切片 `qkv_proj.weight` 不应泛化至量化 DFlash，cache 布局识别和 FP32 attention reference 也仅覆盖特定场景。
  title: 本地 DFlash/Qwen3 实验改动与验证状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4b1fbcb2fa7e18ac57410db9579035bdc801feaab025d11626e938fa6933f23e
  kind: experience
  material_digest: de959b3633a282beea39f0b7c81119836a0db406db5065fbbf8c3fa7f495c4e2
  revision: dfffeb71989f69282aafe052d3cd76a728ea2d5aa6d20ecb7d7b2c53496d5545
  schema: mindie-entry/3
  summary: 唯一新增材料记录了一组已完成但仍属实验性的本地 patch，及其静态检查结果、未完成的 NPU 验证和若干适用性风险；后续检索应优先关注全局 patch 范围、量化权重兼容性、cache 布局，以及 720/727 序列长度问题。
  title: 当前任务导航：DFlash/Qwen3 本地实验索引
navigation: 唯一新增材料记录了一组已完成但仍属实验性的本地 patch，及其静态检查结果、未完成的 NPU 验证和若干适用性风险；后续检索应优先关注全局 patch 范围、量化权重兼容性、cache 布局，以及 720/727 序列长度问题。
schema: mindie-material-task/1
status: complete
task_id: 4b1fbcb2fa7e18ac57410db9579035bdc801feaab025d11626e938fa6933f23e
---

# 当前任务导航：DFlash/Qwen3 本地实验索引

唯一新增材料记录了一组已完成但仍属实验性的本地 patch，及其静态检查结果、未完成的 NPU 验证和若干适用性风险；后续检索应优先关注全局 patch 范围、量化权重兼容性、cache 布局，以及 720/727 序列长度问题。

## Materials

- [本地 DFlash/Qwen3 实验改动与验证状态](blocks/fd7e867cfb970528bc7247e20f93f98f116d67763241d2a72b28f97a102827f7.md): 已完成本地实验性修改，涉及 RoPE/KV cache 探针、逐层 KV 处理、FP32 非因果 GQA attention 参考、context KV/cache trace、FC feature slice 及序列长度元数据；未部署或操作远端。仅确认通过 `python3 -m py_compile` 和 `git diff --check`，尚无 NPU 运行验证，且 `_seq_lens_cpu` 是否导致 builder 使用 720 而非 727 仍未修正。结果存在明确适用范围和风险：全局挂载尚未收窄至 legacy Kimi K3 DSpark，直接切片 `qkv_proj.weight` 不应泛化至量化 DFlash，cache 布局识别和 FP32 attention reference 也仅覆盖特定场景。

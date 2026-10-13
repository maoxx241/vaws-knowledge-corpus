---
blocks:
- block_id: 75d4730a54d4d4118897c2c65bab3d4064bb21390c23829348856dcb21afcf32
  indexed: true
  sha256: 5dc3c1a066a3a32223f5349d78b698ace2e7b94b32db0f4a1af72ab7b3915aa1
  source_range:
    end: 1998494
    part: 0
    start: 0
  summary: 记录了在固定 core 与当前 vLLM 上游对比中确认的接口兼容性，以及对真实最大图像 profiling 低估、renderer 与 request shim 冲突等边界的待核查项；后续新增 standard-image 对齐测试与 renderer 断言，但未修改实现。默认 Python 因缺少 `torch/_build_info` 在 conftest 前退出，未完成原生 pytest；隔离环境中 renderer 测试报告 23 passed、多模态测试报告 4 passed，另有 `py_compile` 与 `git diff --check` 通过。结果属于测试与静态检查记录，不应推广为完整验证成功；保留了环境缺失、profiling 未充分确认及兼容层问题的不确定性。
  title: PR12950 Kimi K3 多模态与 renderer 单测索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2f40ec9a69b7ae38f8631bcfd0b9c37779acaf93c520725e0657ca87b5d498cf
  kind: experience
  material_digest: f2be958c2da355c885a63df0891513ff6be89a7f15d486821c65b51b735118a7
  revision: 4f330b3e06b414bcc0d4df868e9cd9c359a62f0e7b7389b684cff9255d762c7c
  schema: mindie-entry/3
  summary: 本批材料集中于固定 core 上的 Kimi K3 多模态主链、standard-image 包装、renderer 行为及测试环境限制。可检索关键词：`standard-image`、`_hf_processor_applies_updates=False`、image modality、prompt update、3000×3000 dummy、`image_prompts`、tool-result 重排、developer→system、`torch/_build_info`、23 passed、4 passed、原生 pytest 未通过；隔离测试结果与完整环境验证需区分，未决的真实最大图像 profiling 和 renderer/request shim 冲突仍应单独核查。
  title: 当前任务导航：PR12950 Kimi K3 多模态测试与兼容性记录
navigation: 本批材料集中于固定 core 上的 Kimi K3 多模态主链、standard-image 包装、renderer 行为及测试环境限制。可检索关键词：`standard-image`、`_hf_processor_applies_updates=False`、image modality、prompt update、3000×3000 dummy、`image_prompts`、tool-result 重排、developer→system、`torch/_build_info`、23 passed、4 passed、原生 pytest 未通过；隔离测试结果与完整环境验证需区分，未决的真实最大图像 profiling 和 renderer/request shim 冲突仍应单独核查。
schema: mindie-material-task/1
status: complete
task_id: 2f40ec9a69b7ae38f8631bcfd0b9c37779acaf93c520725e0657ca87b5d498cf
---

# 当前任务导航：PR12950 Kimi K3 多模态测试与兼容性记录

本批材料集中于固定 core 上的 Kimi K3 多模态主链、standard-image 包装、renderer 行为及测试环境限制。可检索关键词：`standard-image`、`_hf_processor_applies_updates=False`、image modality、prompt update、3000×3000 dummy、`image_prompts`、tool-result 重排、developer→system、`torch/_build_info`、23 passed、4 passed、原生 pytest 未通过；隔离测试结果与完整环境验证需区分，未决的真实最大图像 profiling 和 renderer/request shim 冲突仍应单独核查。

## Materials

- [PR12950 Kimi K3 多模态与 renderer 单测索引](blocks/75d4730a54d4d4118897c2c65bab3d4064bb21390c23829348856dcb21afcf32.md): 记录了在固定 core 与当前 vLLM 上游对比中确认的接口兼容性，以及对真实最大图像 profiling 低估、renderer 与 request shim 冲突等边界的待核查项；后续新增 standard-image 对齐测试与 renderer 断言，但未修改实现。默认 Python 因缺少 `torch/_build_info` 在 conftest 前退出，未完成原生 pytest；隔离环境中 renderer 测试报告 23 passed、多模态测试报告 4 passed，另有 `py_compile` 与 `git diff --check` 通过。结果属于测试与静态检查记录，不应推广为完整验证成功；保留了环境缺失、profiling 未充分确认及兼容层问题的不确定性。

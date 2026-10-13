---
blocks:
- block_id: 46a3a72bae87477bdd3c866ce971ce147ae2784376e58527bd1e05b7c1fb5853
  indexed: true
  sha256: 5b1bb4ffc06bcd4e48926cdda615b3d9be7c7e32f31ce505525dff915d4c62ed
  source_range:
    end: 389690
    part: 0
    start: 0
  summary: 检查结果称，vLLM Ascend nightly 中确认有两类按数据集计算精度的多模态用例：通过 lm-eval 执行 MMMU validation，以及通过 AISBench 执行 TextVQA/textvqa_gen_base64，并列出了部分模型、部署配置、基线和容差。结论区分了性能压测 `vision-arena-bench` 与精度评测，也指出 Qwen2-Audio 当前运行文本 GSM8K，不应算作多模态数据集精度用例；相关仓库路径、链接和一个秘密字段存在 redaction 占位符。该内容是一次检查报告，保留了“初步不能直接算作精度用例”到后续确认的修正，但未独立验证所有配置或基线。
  title: vLLM Ascend nightly 多模态精度用例
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6dddacca401e7161c40e74de222d7d8664d70c51527de03cce3b93d1a4a5d76d
  kind: experience
  material_digest: 85fb333277f7fdda96d272d9e50cf06d6aa1d330e80e391ffd6566b803d39467
  revision: b310a111106f9a5608729f75a6264968bd067dcc595dbb89c4df0857bff8ef04
  schema: mindie-entry/3
  summary: 现有材料聚焦于确认 nightly 是否包含多模态数据集精度用例。当前索引记录了 MMMU 与 TextVQA 两条候选评测链路、对应工具和部分模型/部署基线，同时保留性能压测与精度评测的区分、Qwen2-Audio 的排除判断，以及报告中的路径和秘密字段 redaction；这些仍属于可检索的检查结果，不应视为已验证的案例报告。
  title: 当前任务导航：vLLM Ascend nightly 多模态精度索引
navigation: 现有材料聚焦于确认 nightly 是否包含多模态数据集精度用例。当前索引记录了 MMMU 与 TextVQA 两条候选评测链路、对应工具和部分模型/部署基线，同时保留性能压测与精度评测的区分、Qwen2-Audio 的排除判断，以及报告中的路径和秘密字段 redaction；这些仍属于可检索的检查结果，不应视为已验证的案例报告。
schema: mindie-material-task/1
status: complete
task_id: 6dddacca401e7161c40e74de222d7d8664d70c51527de03cce3b93d1a4a5d76d
---

# 当前任务导航：vLLM Ascend nightly 多模态精度索引

现有材料聚焦于确认 nightly 是否包含多模态数据集精度用例。当前索引记录了 MMMU 与 TextVQA 两条候选评测链路、对应工具和部分模型/部署基线，同时保留性能压测与精度评测的区分、Qwen2-Audio 的排除判断，以及报告中的路径和秘密字段 redaction；这些仍属于可检索的检查结果，不应视为已验证的案例报告。

## Materials

- [vLLM Ascend nightly 多模态精度用例](blocks/46a3a72bae87477bdd3c866ce971ce147ae2784376e58527bd1e05b7c1fb5853.md): 检查结果称，vLLM Ascend nightly 中确认有两类按数据集计算精度的多模态用例：通过 lm-eval 执行 MMMU validation，以及通过 AISBench 执行 TextVQA/textvqa_gen_base64，并列出了部分模型、部署配置、基线和容差。结论区分了性能压测 `vision-arena-bench` 与精度评测，也指出 Qwen2-Audio 当前运行文本 GSM8K，不应算作多模态数据集精度用例；相关仓库路径、链接和一个秘密字段存在 redaction 占位符。该内容是一次检查报告，保留了“初步不能直接算作精度用例”到后续确认的修正，但未独立验证所有配置或基线。

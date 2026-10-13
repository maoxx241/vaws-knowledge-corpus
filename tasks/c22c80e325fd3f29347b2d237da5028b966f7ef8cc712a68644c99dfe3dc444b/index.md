---
blocks:
- block_id: 71c7ed4199e3c5c7a6e9173d6f9c2e8a495977d01a64e286d740409d7753e049
  indexed: true
  sha256: 7f52c1e5308e0488fbf81d8b9f32a4a504a760897b6b0f1a06203a8e27a3e71f
  source_range:
    end: 967376
    part: 0
    start: 0
  summary: 只读审计发现一组全模型 70K、8192 tokens/4 seqs 的新目录，初始化期间 64 个 worker 持续高 CPU，日志暂时静默，尚未确认 profile peak；此前矩阵还区分了 language-only/VIT-off 的 0.32 GiB 与 full-MM/VIT-on 的 1.89/2.25 GiB，避免将模式差异误判为 max_model_len 效应。现有历史数据包含 70K、130K、327680 等配置，但 130K 改变了 batch-token 和 vision items，320K/tokens24576 未产生完成的 Actual usage，不能作为严格对照。五层 A/B 结果报告称在固定 max_num_batched_tokens=8192 时 peak activation 与 max_model_len 无关；版本、权重目录及部分同源关系仍有日志自证与目录推断之分，不能将该报告升级为已验证的普遍结论。
  title: 固定 batch-token 下的激活峰值对照审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c22c80e325fd3f29347b2d237da5028b966f7ef8cc712a68644c99dfe3dc444b
  kind: experience
  material_digest: e35e8d66b3da54fe4efe3bb83dba424f1b151f1cc1a9f8a8299cb69417ef49a9
  revision: e95b37dea8d8a1735e6a0dae9e3780aa82a655c9a2ac5dd8b67a3b52b16e1016
  schema: mindie-entry/3
  summary: 本批唯一材料围绕 Ascend/多节点全模型 profiling 的只读审计：检索 70K 第二数据点，并比较 70K、130K、327680 的 max_model_len、batch tokens、seqs、vision 模式、EP/FlashComm、权重与 commit。关键检索词包括 language_model_only、skip_mm_profiling、vision_chunk、Actual usage、profile peak、TP rank、max_num_batched_tokens、tokens=8192、seqs=4，以及 0.32/1.89/2.25/2.35/2.47/0.83/1.97 GiB。后续应保留 70K 新目录的未完成状态，核对其是否成功到达 profile peak及是否与 327680 同源同参；同时区分日志确认、目录标签和推断，避免把失败尝试、静默初始化或历史报告当作验证成功。
  title: 当前任务导航：激活峰值与 max_model_len 对照索引
navigation: 本批唯一材料围绕 Ascend/多节点全模型 profiling 的只读审计：检索 70K 第二数据点，并比较 70K、130K、327680 的 max_model_len、batch tokens、seqs、vision 模式、EP/FlashComm、权重与 commit。关键检索词包括 language_model_only、skip_mm_profiling、vision_chunk、Actual usage、profile peak、TP rank、max_num_batched_tokens、tokens=8192、seqs=4，以及 0.32/1.89/2.25/2.35/2.47/0.83/1.97 GiB。后续应保留 70K 新目录的未完成状态，核对其是否成功到达 profile peak及是否与 327680 同源同参；同时区分日志确认、目录标签和推断，避免把失败尝试、静默初始化或历史报告当作验证成功。
schema: mindie-material-task/1
status: complete
task_id: c22c80e325fd3f29347b2d237da5028b966f7ef8cc712a68644c99dfe3dc444b
---

# 当前任务导航：激活峰值与 max_model_len 对照索引

本批唯一材料围绕 Ascend/多节点全模型 profiling 的只读审计：检索 70K 第二数据点，并比较 70K、130K、327680 的 max_model_len、batch tokens、seqs、vision 模式、EP/FlashComm、权重与 commit。关键检索词包括 language_model_only、skip_mm_profiling、vision_chunk、Actual usage、profile peak、TP rank、max_num_batched_tokens、tokens=8192、seqs=4，以及 0.32/1.89/2.25/2.35/2.47/0.83/1.97 GiB。后续应保留 70K 新目录的未完成状态，核对其是否成功到达 profile peak及是否与 327680 同源同参；同时区分日志确认、目录标签和推断，避免把失败尝试、静默初始化或历史报告当作验证成功。

## Materials

- [固定 batch-token 下的激活峰值对照审计](blocks/71c7ed4199e3c5c7a6e9173d6f9c2e8a495977d01a64e286d740409d7753e049.md): 只读审计发现一组全模型 70K、8192 tokens/4 seqs 的新目录，初始化期间 64 个 worker 持续高 CPU，日志暂时静默，尚未确认 profile peak；此前矩阵还区分了 language-only/VIT-off 的 0.32 GiB 与 full-MM/VIT-on 的 1.89/2.25 GiB，避免将模式差异误判为 max_model_len 效应。现有历史数据包含 70K、130K、327680 等配置，但 130K 改变了 batch-token 和 vision items，320K/tokens24576 未产生完成的 Actual usage，不能作为严格对照。五层 A/B 结果报告称在固定 max_num_batched_tokens=8192 时 peak activation 与 max_model_len 无关；版本、权重目录及部分同源关系仍有日志自证与目录推断之分，不能将该报告升级为已验证的普遍结论。

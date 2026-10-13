---
blocks:
- block_id: 36fee16e83591321a4db56530298bf7a05fe7bdae7a989587001ef113ed848c7
  indexed: true
  sha256: 6f0fcc5841afeec6c409f915063d00ad1039fc68187c38dda6cdb9a04a8bc5e8
  source_range:
    end: 687392
    part: 0
    start: 0
  summary: 只读设计建议在 `Qwen3DSparkModel` 构造完成后，依据 target 身份、draft 架构及完整 legacy GQA 配置，为 5 个 attention 实例设置私有标记；全局 monkeypatch 仅对标记实例启用现有 FP32 Q/K RMSNorm 与 RoPE，其余调用保存的 upstream `forward`。不应依据运行时 token 数等于 7 判断，因为多请求批次可能是 `7 × batch_size`。建议用 CPU tensor、stub 和 `SimpleNamespace` 覆盖配置判定、实例标记、原路径回退、FP32 路径及数学参考；诊断 payload、环境变量及 runner/rejection sampler/proposer probe 应移除，但保留生产 DSpark/Markov/aux-stream 逻辑、无 probe 的 context-KV patch 和 checkpoint mask embedding 兼容逻辑。该方案是设计结论，并非已验证实现或测试成功。
  title: Legacy K3 DSpark FP32 gating 设计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8f699cf767040d7757ac1211c320f3f60f823dbde1d7655a2ac44ec272c4b4d7
  kind: experience
  material_digest: e4c77e6787b867bb3cc04f4e7a672e3fd96cc02977a14b9bec47a9d252a2f061
  revision: 446944e5d7407c9f10f1a2cbcbac97b2b5f891268879744f9b2c9017bb1b610f
  schema: mindie-entry/3
  summary: 本批材料给出只读设计结论：通过模型构造后的精确 target/draft/legacy GQA 配置判定，限制 monkeypatch 仅命中 legacy K3 DSpark attention，并让其他实例无条件回退 upstream。UT 计划覆盖 CPU 可测的配置、标记、回退、FP32 数学路径；尚未证明实现、测试或清理结果已完成。
  title: 当前任务导航：Legacy K3 DSpark gating
navigation: 本批材料给出只读设计结论：通过模型构造后的精确 target/draft/legacy GQA 配置判定，限制 monkeypatch 仅命中 legacy K3 DSpark attention，并让其他实例无条件回退 upstream。UT 计划覆盖 CPU 可测的配置、标记、回退、FP32 数学路径；尚未证明实现、测试或清理结果已完成。
schema: mindie-material-task/1
status: complete
task_id: 8f699cf767040d7757ac1211c320f3f60f823dbde1d7655a2ac44ec272c4b4d7
---

# 当前任务导航：Legacy K3 DSpark gating

本批材料给出只读设计结论：通过模型构造后的精确 target/draft/legacy GQA 配置判定，限制 monkeypatch 仅命中 legacy K3 DSpark attention，并让其他实例无条件回退 upstream。UT 计划覆盖 CPU 可测的配置、标记、回退、FP32 数学路径；尚未证明实现、测试或清理结果已完成。

## Materials

- [Legacy K3 DSpark FP32 gating 设计](blocks/36fee16e83591321a4db56530298bf7a05fe7bdae7a989587001ef113ed848c7.md): 只读设计建议在 `Qwen3DSparkModel` 构造完成后，依据 target 身份、draft 架构及完整 legacy GQA 配置，为 5 个 attention 实例设置私有标记；全局 monkeypatch 仅对标记实例启用现有 FP32 Q/K RMSNorm 与 RoPE，其余调用保存的 upstream `forward`。不应依据运行时 token 数等于 7 判断，因为多请求批次可能是 `7 × batch_size`。建议用 CPU tensor、stub 和 `SimpleNamespace` 覆盖配置判定、实例标记、原路径回退、FP32 路径及数学参考；诊断 payload、环境变量及 runner/rejection sampler/proposer probe 应移除，但保留生产 DSpark/Markov/aux-stream 逻辑、无 probe 的 context-KV patch 和 checkpoint mask embedding 兼容逻辑。该方案是设计结论，并非已验证实现或测试成功。

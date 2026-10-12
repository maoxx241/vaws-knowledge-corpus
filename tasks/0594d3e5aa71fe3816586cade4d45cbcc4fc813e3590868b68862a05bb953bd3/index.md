---
blocks:
- block_id: 4583dd0c67d59f3bd361bcbf12af09808019b3648b4c44d7cffc9e77a6a71e1b
  indexed: true
  sha256: 56e6605398df6d2a89831615bd36a721d76b4136534161923fe140d6ec7322de
  source_range:
    end: 2404169
    part: 0
    start: 0
  summary: '讨论将 DSV41 的 compressor/indexer 等 RMSNorm 直接替换为 `torch_npu.npu_rms_norm`，并曾报告在 153 上完成 104 项单算子数值与图重放检查；这些结果属于消息中的报告，未独立核验，且整模型服务验收未完成。随后按要求移除 CPU fallback、norm 模块传参并更新 PR #14，但最新请求要求先回退 mHC 和 Engram 的无权重 RMSNorm 改动，原因是使用 `torch.ones` 看起来不合理；该回退尚未报告完成。'
  title: DSV41 RMSNorm 融合接口替换与回退请求
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0594d3e5aa71fe3816586cade4d45cbcc4fc813e3590868b68862a05bb953bd3
  kind: experience
  material_digest: 8756b81446881231cd1e3cfe55c71e0c4d1f2ad101c8702f9927ee85608fb0eb
  revision: f4277c813ae99f032140eb79f90fd49b3369be3743e96d4e2066d6fd88f7eb02
  schema: mindie-entry/3
  summary: '当前重点是处理 `DeepseekV41RMSNorm` 及相关 RMSNorm 的 `torch_npu.npu_rms_norm` 直接调用方案。此前消息报告带权路径以及 mHC/Engram 无权重路径已修改并通过 153 上 104 项检查、PR #14 已更新，但这些均为未核验的报告；最新未完成事项是先回退 mHC 和 Engram 改动，保留其余替换范围，并注意 CI、全仓 lint 和整模型验收状态仍不确定。'
  title: DSV41 RMSNorm 替换任务导航
navigation: '当前重点是处理 `DeepseekV41RMSNorm` 及相关 RMSNorm 的 `torch_npu.npu_rms_norm` 直接调用方案。此前消息报告带权路径以及 mHC/Engram 无权重路径已修改并通过 153 上 104 项检查、PR #14 已更新，但这些均为未核验的报告；最新未完成事项是先回退 mHC 和 Engram 改动，保留其余替换范围，并注意 CI、全仓 lint 和整模型验收状态仍不确定。'
schema: mindie-material-task/1
status: complete
task_id: 0594d3e5aa71fe3816586cade4d45cbcc4fc813e3590868b68862a05bb953bd3
---

# DSV41 RMSNorm 替换任务导航

当前重点是处理 `DeepseekV41RMSNorm` 及相关 RMSNorm 的 `torch_npu.npu_rms_norm` 直接调用方案。此前消息报告带权路径以及 mHC/Engram 无权重路径已修改并通过 153 上 104 项检查、PR #14 已更新，但这些均为未核验的报告；最新未完成事项是先回退 mHC 和 Engram 改动，保留其余替换范围，并注意 CI、全仓 lint 和整模型验收状态仍不确定。

## Materials

- [DSV41 RMSNorm 融合接口替换与回退请求](blocks/4583dd0c67d59f3bd361bcbf12af09808019b3648b4c44d7cffc9e77a6a71e1b.md): 讨论将 DSV41 的 compressor/indexer 等 RMSNorm 直接替换为 `torch_npu.npu_rms_norm`，并曾报告在 153 上完成 104 项单算子数值与图重放检查；这些结果属于消息中的报告，未独立核验，且整模型服务验收未完成。随后按要求移除 CPU fallback、norm 模块传参并更新 PR #14，但最新请求要求先回退 mHC 和 Engram 的无权重 RMSNorm 改动，原因是使用 `torch.ones` 看起来不合理；该回退尚未报告完成。

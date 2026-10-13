---
blocks:
- block_id: 59d78629d2ad2bb105184636dbdb78c01922dbf22e54b3fb7bd4fd0034caad37
  indexed: true
  sha256: 47c29e81678e41a763ead18eae46bc8cb769aa1941687be7709dcf0e53a89ae7
  source_range:
    end: 8011098
    part: 0
    start: 0
  summary: 分析围绕 DSV4 的 WOB MatMul 后、HcPost 前出现 TensorMove 的具体链路展开；较明确的候选来源是 `output[...] = self.wo_b(...)` 对预分配输出执行的 `copy_`，而非 KV cache 或 npugraph_ex 泛化问题。直接把 `output` 重绑定为 MatMul 返回值可能破坏 mutation ABI、ACLGraph replay 的稳定地址或导致调用者继续读取未写入的 `torch.empty`，因此不能视为安全优化。当前仍未完成实测验证；可行方向包括让量化 MatMul 支持写入既有 output 的 out-variant，或完整改造 functional-return ABI，并需确认实际 `wo_b.quant_method`。
  title: DSV4 MatMul→TensorMove→HcPost 链路分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: df9e6abc50f83d6415c34e7eb853e3552a0594438adfda37e6160fbeee45162f
  kind: experience
  material_digest: cdb0dc79791d814ea37baa9ddfb7013fef1987932ecc041e582340d084f76bf3
  revision: 791f9a8ba2d9395788fe6d13877d705034940108a8555e5902521c5a08ff1579
  schema: mindie-entry/3
  summary: 重点检索 DSV4 中 `wo_b MatMul → TensorMove(copy_) → HcPost`、`output` mutation ABI、ACLGraph replay 以及量化 MatMul binding。此前关于 KV cache、TorchAir/npugraph_ex 和 KDA reinplace 的泛化判断已被用户纠正为不适用的主要方向；保留的当前假设仍未通过 NPU Profiling 或代码改动验证。优先确认 `wo_b` 的实际量化方法及是否存在安全的 out-variant，同时保留精度风险与未完成状态。
  title: 当前任务导航：定位 DSV4 WOB TensorMove
navigation: 重点检索 DSV4 中 `wo_b MatMul → TensorMove(copy_) → HcPost`、`output` mutation ABI、ACLGraph replay 以及量化 MatMul binding。此前关于 KV cache、TorchAir/npugraph_ex 和 KDA reinplace 的泛化判断已被用户纠正为不适用的主要方向；保留的当前假设仍未通过 NPU Profiling 或代码改动验证。优先确认 `wo_b` 的实际量化方法及是否存在安全的 out-variant，同时保留精度风险与未完成状态。
schema: mindie-material-task/1
status: complete
task_id: df9e6abc50f83d6415c34e7eb853e3552a0594438adfda37e6160fbeee45162f
---

# 当前任务导航：定位 DSV4 WOB TensorMove

重点检索 DSV4 中 `wo_b MatMul → TensorMove(copy_) → HcPost`、`output` mutation ABI、ACLGraph replay 以及量化 MatMul binding。此前关于 KV cache、TorchAir/npugraph_ex 和 KDA reinplace 的泛化判断已被用户纠正为不适用的主要方向；保留的当前假设仍未通过 NPU Profiling 或代码改动验证。优先确认 `wo_b` 的实际量化方法及是否存在安全的 out-variant，同时保留精度风险与未完成状态。

## Materials

- [DSV4 MatMul→TensorMove→HcPost 链路分析](blocks/59d78629d2ad2bb105184636dbdb78c01922dbf22e54b3fb7bd4fd0034caad37.md): 分析围绕 DSV4 的 WOB MatMul 后、HcPost 前出现 TensorMove 的具体链路展开；较明确的候选来源是 `output[...] = self.wo_b(...)` 对预分配输出执行的 `copy_`，而非 KV cache 或 npugraph_ex 泛化问题。直接把 `output` 重绑定为 MatMul 返回值可能破坏 mutation ABI、ACLGraph replay 的稳定地址或导致调用者继续读取未写入的 `torch.empty`，因此不能视为安全优化。当前仍未完成实测验证；可行方向包括让量化 MatMul 支持写入既有 output 的 out-variant，或完整改造 functional-return ABI，并需确认实际 `wo_b.quant_method`。

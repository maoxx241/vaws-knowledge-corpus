---
blocks:
- block_id: 4d817dcd4360bfcf0f1d4532228929f72671c2329a1256ab1ab8aa3102c6d989
  indexed: true
  sha256: 4f1b081c1829e0958b979e810cc78da909bda24fe295abf523e4677b8178a9cc
  source_range:
    end: 761521
    part: 0
    start: 0
  summary: 在 `kimi-k3-pool`、CANN 9.0.1、torch-npu 2.10.0.post2 环境中，P 实例于首个 1-token prefill 调用 `kda_gate_cumsum` 时失败，ACL 报 `ParseDynamicKernelConfig`、`Failed to ParseDynamicKernels` 和 `Op KdaGateCumsum does not has any binary`；D 实例的 decode-only 成功不能证明 P 端正常。取证将问题归因于自定义算子安装包的聚合清单缺少 `KdaGateCumsum`、`ChunkKdaFwd`、`KdaLayoutSwap12`，而非 TP16 shape 或 KDA 实现；不完整包的 H/T 测试均失败，旧完整包均通过，两个包的 KDA `.o` SHA256 一致。当前源码为 `543819b5b`（`releases/v0.23.0_ci_new`），PR9 head 为 `07e2ce8d` 且尚未合入；建议重编译安装后检查清单，再执行 AscendC 单卡用例和完整 PD 验证，结果仍属于待验证事项。
  title: P 实例 KDA 算子加载失败与 PD 验证取证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6d546acc72b6b873ea12f68a2990a443cead823c389bc256c0d9e6704abfbc33
  kind: experience
  material_digest: 86ec345378c0a608beb0646887bc1a391806f6aaf60d53f4ea5a4f4476945d12
  revision: c73906152fd2ae96221e1e741c2f7fe647b4ea358c5ad5096fed6dab6f4a4803
  schema: mindie-entry/3
  summary: 当前线索集中在 P 端 `KdaGateCumsum` 动态 kernel 配置/安装清单缺失，尚未证明 PD 分离功能已验证通过。D 端 decode-only 请求曾成功，但不能覆盖 P 端 prefill；后续应保留 PR9 合入状态、重编译安装、清单检查及单卡和完整 PD 测试的未完成状态，并记录任何后续修正。
  title: KDA 算子安装清单问题与 PD 分离验证
navigation: 当前线索集中在 P 端 `KdaGateCumsum` 动态 kernel 配置/安装清单缺失，尚未证明 PD 分离功能已验证通过。D 端 decode-only 请求曾成功，但不能覆盖 P 端 prefill；后续应保留 PR9 合入状态、重编译安装、清单检查及单卡和完整 PD 测试的未完成状态，并记录任何后续修正。
schema: mindie-material-task/1
status: complete
task_id: 6d546acc72b6b873ea12f68a2990a443cead823c389bc256c0d9e6704abfbc33
---

# KDA 算子安装清单问题与 PD 分离验证

当前线索集中在 P 端 `KdaGateCumsum` 动态 kernel 配置/安装清单缺失，尚未证明 PD 分离功能已验证通过。D 端 decode-only 请求曾成功，但不能覆盖 P 端 prefill；后续应保留 PR9 合入状态、重编译安装、清单检查及单卡和完整 PD 测试的未完成状态，并记录任何后续修正。

## Materials

- [P 实例 KDA 算子加载失败与 PD 验证取证](blocks/4d817dcd4360bfcf0f1d4532228929f72671c2329a1256ab1ab8aa3102c6d989.md): 在 `kimi-k3-pool`、CANN 9.0.1、torch-npu 2.10.0.post2 环境中，P 实例于首个 1-token prefill 调用 `kda_gate_cumsum` 时失败，ACL 报 `ParseDynamicKernelConfig`、`Failed to ParseDynamicKernels` 和 `Op KdaGateCumsum does not has any binary`；D 实例的 decode-only 成功不能证明 P 端正常。取证将问题归因于自定义算子安装包的聚合清单缺少 `KdaGateCumsum`、`ChunkKdaFwd`、`KdaLayoutSwap12`，而非 TP16 shape 或 KDA 实现；不完整包的 H/T 测试均失败，旧完整包均通过，两个包的 KDA `.o` SHA256 一致。当前源码为 `543819b5b`（`releases/v0.23.0_ci_new`），PR9 head 为 `07e2ce8d` 且尚未合入；建议重编译安装后检查清单，再执行 AscendC 单卡用例和完整 PD 验证，结果仍属于待验证事项。

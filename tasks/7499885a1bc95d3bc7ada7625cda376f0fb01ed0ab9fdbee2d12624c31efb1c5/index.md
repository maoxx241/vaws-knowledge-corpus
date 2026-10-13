---
blocks:
- block_id: bebb82c28f815f7ec5cdc80351eced7f02fef950e40f777fc0ad1a2e4e6fc90f
  indexed: true
  sha256: 5af882403269f991570005284d7ff556704d8ff50a6163aee9db49e753e86d70
  source_range:
    end: 1468399
    part: 0
    start: 0
  summary: 当前只读核对聚焦真实模型形状、七个 singleton 的 task 映射，以及 stateful 1-token 改路由到 `recurrent_kda` 后的 tiling、task 和同步。已观察到 TP16 下每 rank 有 6 个 KDA head：7×1-token 产生 42 个 `(chunk, head)` task，A3 的 24 个 AIC 使 0–17 号物理核执行第二个 task；1×1-token 产生 6 个 task。完整 prefill 子链已厘清，但反向握手是否在每个 task 后归零仍在核对，不能据此确认 bug。
  title: TP16 KDA singleton task 映射与同步核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7499885a1bc95d3bc7ada7625cda376f0fb01ed0ab9fdbee2d12624c31efb1c5
  kind: experience
  material_digest: e2ffd269b333a3be2c7c6c54e545f05f2ba64a3a64584f62aa8d74640d5dcded
  revision: 6a42726f0be3aa2d23fa1aaf4773456dc747cc712f1348c8a2ff5bf24c0ba0ac
  schema: mindie-entry/3
  summary: 已建立 TP16 下 1-token/7-token 的 task 数量与 AIC 分配上下文，并区分了已明确的静态映射和未完成的反向握手验证。后续重点是精确算子签名、H 子算子输入布局，以及 `recurrent_kda` 路径的 tiling/task/同步；不要将跨过 24 核阈值或当前检查计划视为已证实故障。
  title: 当前任务导航：KDA singleton 映射与同步验证
navigation: 已建立 TP16 下 1-token/7-token 的 task 数量与 AIC 分配上下文，并区分了已明确的静态映射和未完成的反向握手验证。后续重点是精确算子签名、H 子算子输入布局，以及 `recurrent_kda` 路径的 tiling/task/同步；不要将跨过 24 核阈值或当前检查计划视为已证实故障。
schema: mindie-material-task/1
status: complete
task_id: 7499885a1bc95d3bc7ada7625cda376f0fb01ed0ab9fdbee2d12624c31efb1c5
---

# 当前任务导航：KDA singleton 映射与同步验证

已建立 TP16 下 1-token/7-token 的 task 数量与 AIC 分配上下文，并区分了已明确的静态映射和未完成的反向握手验证。后续重点是精确算子签名、H 子算子输入布局，以及 `recurrent_kda` 路径的 tiling/task/同步；不要将跨过 24 核阈值或当前检查计划视为已证实故障。

## Materials

- [TP16 KDA singleton task 映射与同步核对](blocks/bebb82c28f815f7ec5cdc80351eced7f02fef950e40f777fc0ad1a2e4e6fc90f.md): 当前只读核对聚焦真实模型形状、七个 singleton 的 task 映射，以及 stateful 1-token 改路由到 `recurrent_kda` 后的 tiling、task 和同步。已观察到 TP16 下每 rank 有 6 个 KDA head：7×1-token 产生 42 个 `(chunk, head)` task，A3 的 24 个 AIC 使 0–17 号物理核执行第二个 task；1×1-token 产生 6 个 task。完整 prefill 子链已厘清，但反向握手是否在每个 task 后归零仍在核对，不能据此确认 bug。

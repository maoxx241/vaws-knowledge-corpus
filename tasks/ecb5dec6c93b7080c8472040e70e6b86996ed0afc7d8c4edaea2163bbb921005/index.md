---
blocks:
- block_id: c97869c368bafdd3646b15e5b866030026586725fecbd5b3ba1f6f45f5535aac
  indexed: true
  sha256: 340d6641d218fd689cc3041ada54adc51b41b2f0d428ad8064265c9d9c119b1e
  source_range:
    end: 858715
    part: 0
    start: 0
  summary: 对生产热路径中的 probe 进行了逐文件静态审查，并核对 target verify→rejection sampler→下一轮 draft 的张量生命周期及 TP rank 同步要求。确认在 `enable_reduce_sample=false`、`lmhead_tensor_parallel_size=0` 下，Ascend `LogitsProcessor` 会先 `_gather_logits`，proposal top-k/argmax 和 verification 的 `tuple[0].argmax` 均为全局 token ID；不能仅在 TP0 调用带内部 TP all-gather 的 `greedy_sample`，否则可能死锁。当前 greedy、非 reduce-sample、LM-head TP=0、单短请求配置未发现必然 TP16 死锁或 token ID 误读，但仍存在长 prompt 的 FP32/RoPE 显存峰值、pending 覆盖、空 tensor `amax()` 及异常后 payload 未清理等诊断健壮性风险。
  title: Ascend TP16 probe 只读审查与风险
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ecb5dec6c93b7080c8472040e70e6b86996ed0afc7d8c4edaea2163bbb921005
  kind: experience
  material_digest: 1e86b53dfc128f3c47400818c386e727c7ce23316ce5c7a33508a39fb9230eb3
  revision: 15b3d77c811c5fec5d390ac9518988067f5db355d79fd6e611ff1a0cdf438c69
  schema: mindie-entry/3
  summary: 已完成一批针对生产热路径 probe 的静态与调用时序核对：重点结论是当前配置下全词表 logits 语义成立，未发现必然 TP16 死锁或 token ID 误读；但这些结论属于只读审查，不能视为运行验证成功。后续检索应关注启动期 dummy/profile/capture 触发、显存峰值，以及 pending 覆盖、空 tensor `amax()` 和异常清理等未决风险；原始材料仍可独立检索。
  title: 当前任务导航：Ascend probe 只读审查
navigation: 已完成一批针对生产热路径 probe 的静态与调用时序核对：重点结论是当前配置下全词表 logits 语义成立，未发现必然 TP16 死锁或 token ID 误读；但这些结论属于只读审查，不能视为运行验证成功。后续检索应关注启动期 dummy/profile/capture 触发、显存峰值，以及 pending 覆盖、空 tensor `amax()` 和异常清理等未决风险；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: ecb5dec6c93b7080c8472040e70e6b86996ed0afc7d8c4edaea2163bbb921005
---

# 当前任务导航：Ascend probe 只读审查

已完成一批针对生产热路径 probe 的静态与调用时序核对：重点结论是当前配置下全词表 logits 语义成立，未发现必然 TP16 死锁或 token ID 误读；但这些结论属于只读审查，不能视为运行验证成功。后续检索应关注启动期 dummy/profile/capture 触发、显存峰值，以及 pending 覆盖、空 tensor `amax()` 和异常清理等未决风险；原始材料仍可独立检索。

## Materials

- [Ascend TP16 probe 只读审查与风险](blocks/c97869c368bafdd3646b15e5b866030026586725fecbd5b3ba1f6f45f5535aac.md): 对生产热路径中的 probe 进行了逐文件静态审查，并核对 target verify→rejection sampler→下一轮 draft 的张量生命周期及 TP rank 同步要求。确认在 `enable_reduce_sample=false`、`lmhead_tensor_parallel_size=0` 下，Ascend `LogitsProcessor` 会先 `_gather_logits`，proposal top-k/argmax 和 verification 的 `tuple[0].argmax` 均为全局 token ID；不能仅在 TP0 调用带内部 TP all-gather 的 `greedy_sample`，否则可能死锁。当前 greedy、非 reduce-sample、LM-head TP=0、单短请求配置未发现必然 TP16 死锁或 token ID 误读，但仍存在长 prompt 的 FP32/RoPE 显存峰值、pending 覆盖、空 tensor `amax()` 及异常后 payload 未清理等诊断健壮性风险。

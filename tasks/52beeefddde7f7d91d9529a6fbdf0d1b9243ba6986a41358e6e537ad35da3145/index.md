---
blocks:
- block_id: 7ce4a7a8cdb43284230cd4a8d95a1923958d320ecf7c90e1f91ae8c24d926616
  indexed: true
  sha256: bd0a06e50fdf9e51bb5a692ad7db25c6ace3af83c03591522a8f829c5b7d1054
  source_range:
    end: 4820394
    part: 0
    start: 0
  summary: 审查 PR 15589 时确认 live head 为 `91665f45`，新代码直接对 `in_proj_qkvz.weight` 使用 `F.linear`，绕过了 Qwen3.5/ModelSlim 的量化 `quant_method.apply`，在 W8A8 下可能因权重布局、scale 和 `npu_quant_matmul` 处理缺失而报错或产生错误结果；bias 指摘经构造约束核实为不成立。随后已提交并回读确认绑定该 head 的 `CHANGES_REQUESTED`，inline comment 位于 `gdn.py:205`。对 PR 15590 的审查发现缓存的 packed `conv1d.weight` 不会随运行时 `model.load_weights()` 或原地 `copy_` 更新，新增 UT 反而将旧缓存行为固定为预期；相关 reviewer comment 尚未修复，也未形成 change-request，且 NPU selected tests 因缺少 `ready-precise/ready-all` 被 skipped，性能收益同样缺少 profile/benchmark 证据。
  title: PR 15589 量化路径审查与 PR 15590 缓存失效风险
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 52beeefddde7f7d91d9529a6fbdf0d1b9243ba6986a41358e6e537ad35da3145
  kind: experience
  material_digest: 8a4e3b25a2bb40274c7ee0c89b11f3e596d252378aee4f34f715ed7bc246a54b
  revision: 9846e98ca2b18f892709d0b923e63826b813358d8221bfa33e94946a913bceab
  schema: mindie-entry/3
  summary: 已确认 PR 15589 存在量化 linear 路径回归，并已提交绑定 `91665f45` 的 `CHANGES_REQUESTED`；bias 风险不成立。PR 15590 仍有未解决的运行时 weight-transfer/缓存失效 P1 风险，现有测试和 CI 未验证该路径，性能提升也尚无充分证据；后续应关注修复、reload regression、真实 NPU 测试及性能数据。
  title: PR 15589/15590 审查索引
navigation: 已确认 PR 15589 存在量化 linear 路径回归，并已提交绑定 `91665f45` 的 `CHANGES_REQUESTED`；bias 风险不成立。PR 15590 仍有未解决的运行时 weight-transfer/缓存失效 P1 风险，现有测试和 CI 未验证该路径，性能提升也尚无充分证据；后续应关注修复、reload regression、真实 NPU 测试及性能数据。
schema: mindie-material-task/1
status: complete
task_id: 52beeefddde7f7d91d9529a6fbdf0d1b9243ba6986a41358e6e537ad35da3145
---

# PR 15589/15590 审查索引

已确认 PR 15589 存在量化 linear 路径回归，并已提交绑定 `91665f45` 的 `CHANGES_REQUESTED`；bias 风险不成立。PR 15590 仍有未解决的运行时 weight-transfer/缓存失效 P1 风险，现有测试和 CI 未验证该路径，性能提升也尚无充分证据；后续应关注修复、reload regression、真实 NPU 测试及性能数据。

## Materials

- [PR 15589 量化路径审查与 PR 15590 缓存失效风险](blocks/7ce4a7a8cdb43284230cd4a8d95a1923958d320ecf7c90e1f91ae8c24d926616.md): 审查 PR 15589 时确认 live head 为 `91665f45`，新代码直接对 `in_proj_qkvz.weight` 使用 `F.linear`，绕过了 Qwen3.5/ModelSlim 的量化 `quant_method.apply`，在 W8A8 下可能因权重布局、scale 和 `npu_quant_matmul` 处理缺失而报错或产生错误结果；bias 指摘经构造约束核实为不成立。随后已提交并回读确认绑定该 head 的 `CHANGES_REQUESTED`，inline comment 位于 `gdn.py:205`。对 PR 15590 的审查发现缓存的 packed `conv1d.weight` 不会随运行时 `model.load_weights()` 或原地 `copy_` 更新，新增 UT 反而将旧缓存行为固定为预期；相关 reviewer comment 尚未修复，也未形成 change-request，且 NPU selected tests 因缺少 `ready-precise/ready-all` 被 skipped，性能收益同样缺少 profile/benchmark 证据。

---
blocks:
- block_id: 82a6482b2e8cb4da42b38c4909df01656f8767f6df032c6f8e88641d3876dcf4
  indexed: true
  sha256: 3a7d8e90be3bb027c741eb110e47f22ec71355d91004b3343e5f461b02ce58ca
  source_range:
    end: 1798996
    part: 0
    start: 0
  summary: 只读审查聚焦真实权重加载、首个 prefill/decode、TP/EP 参数形状，以及多模态注册时序和 checkpoint 中 vision/projector 权重名；未修改共享工作树。报告称两个阻断点已有修复：ModelSlim 按具体 text config 识别，`A_log` 在 TP 分片前裁掉 128→96 的 checkpoint padding；CANN 9.1 容器中 62 个针对性测试通过，但这不等同于端到端成功。源码未废除 `npu_moe_init_routing_custom`，首 forward 故障被归因于 vendor ACLNN 包/符号缺失，建议完整重编以同步 binding/schema/vendor kernel；仍有 NPU 端到端验证缺口，完整多模态模式依赖 `trust_remote_code=True`。
  title: vLLM-Ascend KDA/MoE 加载与运行时核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 351a2a934c53d6ab0003c177f1973f13d1b17f942ea9e1d8dc9acd499df0a465
  kind: experience
  material_digest: f8a2428b52d137b915bbefb9d8193e61e7d69c858aeca72e8d6676d58160d2ef
  revision: 062f6d955a1eaca3879681ba999dd93e1daa206bf689832f0b5b1a2d38b3b11d
  schema: mindie-entry/3
  summary: 当前材料记录了一次只读代码与环境核查：重点包括真实 checkpoint 权重名、ModelSlim/KDA 的 TP 分片、`A_log` padding、`npu_moe_init_routing_custom` 的 ACLNN binding/schema/vendor 符号，以及多模态注册告警。已报告的 62 个回归测试通过和“未发现新增 P0”属于该次检查结果；vendor 包缺失的根因判断仍需通过完整重编与 NPU 端到端运行确认。后续应保留 language-only 与完整多模态模式的差异、`trust_remote_code=True` 依赖及未完成验证，不将计划或静态核对提升为已验证运行成功。
  title: 当前任务导航：Ascend 权重加载与 MoE/多模态运行时核查
navigation: 当前材料记录了一次只读代码与环境核查：重点包括真实 checkpoint 权重名、ModelSlim/KDA 的 TP 分片、`A_log` padding、`npu_moe_init_routing_custom` 的 ACLNN binding/schema/vendor 符号，以及多模态注册告警。已报告的 62 个回归测试通过和“未发现新增 P0”属于该次检查结果；vendor 包缺失的根因判断仍需通过完整重编与 NPU 端到端运行确认。后续应保留 language-only 与完整多模态模式的差异、`trust_remote_code=True` 依赖及未完成验证，不将计划或静态核对提升为已验证运行成功。
schema: mindie-material-task/1
status: complete
task_id: 351a2a934c53d6ab0003c177f1973f13d1b17f942ea9e1d8dc9acd499df0a465
---

# 当前任务导航：Ascend 权重加载与 MoE/多模态运行时核查

当前材料记录了一次只读代码与环境核查：重点包括真实 checkpoint 权重名、ModelSlim/KDA 的 TP 分片、`A_log` padding、`npu_moe_init_routing_custom` 的 ACLNN binding/schema/vendor 符号，以及多模态注册告警。已报告的 62 个回归测试通过和“未发现新增 P0”属于该次检查结果；vendor 包缺失的根因判断仍需通过完整重编与 NPU 端到端运行确认。后续应保留 language-only 与完整多模态模式的差异、`trust_remote_code=True` 依赖及未完成验证，不将计划或静态核对提升为已验证运行成功。

## Materials

- [vLLM-Ascend KDA/MoE 加载与运行时核查](blocks/82a6482b2e8cb4da42b38c4909df01656f8767f6df032c6f8e88641d3876dcf4.md): 只读审查聚焦真实权重加载、首个 prefill/decode、TP/EP 参数形状，以及多模态注册时序和 checkpoint 中 vision/projector 权重名；未修改共享工作树。报告称两个阻断点已有修复：ModelSlim 按具体 text config 识别，`A_log` 在 TP 分片前裁掉 128→96 的 checkpoint padding；CANN 9.1 容器中 62 个针对性测试通过，但这不等同于端到端成功。源码未废除 `npu_moe_init_routing_custom`，首 forward 故障被归因于 vendor ACLNN 包/符号缺失，建议完整重编以同步 binding/schema/vendor kernel；仍有 NPU 端到端验证缺口，完整多模态模式依赖 `trust_remote_code=True`。

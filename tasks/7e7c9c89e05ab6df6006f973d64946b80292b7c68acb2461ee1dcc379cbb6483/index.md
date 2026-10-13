---
blocks:
- block_id: fda0de6e3d3a734728511fcae5170b33766dbae12efa7a9d1427b80b6a663f6d
  indexed: true
  sha256: 87e4bb97826befcb3cd14f4d8ba289e38f05f222724f31be9bd934fec24af640
  source_range:
    end: 5219700
    part: 0
    start: 0
  summary: '针对 PR #13277、0.26.0、scheduler-off 的 D 节点卡死问题，代码分析指向 KV 不足后将 num_computed_tokens 清零并本地重算，可能形成自抢占活锁；上游 #48245 的 P/D preemption race 修复在定向回归中通过，AsyncRecomputeScheduler 压力模型也通过，但尚未完成双机硬件验证。此前因 kimi_c8/QuaRot 服务占用四台机器而暂停，获授权后已精确释放并隔离 153/154 与 155/156；scheduler-on session 仅完成 smoke，尚未形成 KV 打满结论。scheduler-off 基线当前受独立的 P 侧 aclnnFusedInferAttentionScoreV3、key format=RESERVED、EZ0017 错误阻断，短 prompt、eager 和多种输入仍未使请求到达 D；最新方案是使用合法极短 token-id prompt 配合 2048 强制输出，以继续验证 D 的 KV 抢占/重算路径，故问题尚未被硬件复现或确认修复。'
  title: 双机 P/D 重计算卡死定位与 KV 压测进展
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7e7c9c89e05ab6df6006f973d64946b80292b7c68acb2461ee1dcc379cbb6483
  kind: experience
  material_digest: b6c2930e9f9dfd12b96b8fc257b5514bf1914e4623095223fb86e459e6e7d7fe
  revision: 1480294a2a2a962f59cecbd88d566c80f3b598f4e5eb221396c6db3e31527cef
  schema: mindie-entry/3
  summary: '当前主线是 153/154 上用 PR #13277 的 scheduler-off 配置完成 D 侧 KV 超限复现，并与 #48245 修复版做单变量对照；现阶段仍被 P 侧 FIA key format=RESERVED/EZ0017 错误阻断，不能据此判断 D 调度故障。155/156 的 scheduler-on 对照已验证基础 Mooncake、ACLGraph replay 和 speculative decode 链路，但只有 smoke 结果，尚无 KV 压力结论。代码回归支持 #48245 方向，但尚未等同于双机问题已解决。'
  title: 任务导航：P/D 重计算卡死与 scheduler 对照
navigation: '当前主线是 153/154 上用 PR #13277 的 scheduler-off 配置完成 D 侧 KV 超限复现，并与 #48245 修复版做单变量对照；现阶段仍被 P 侧 FIA key format=RESERVED/EZ0017 错误阻断，不能据此判断 D 调度故障。155/156 的 scheduler-on 对照已验证基础 Mooncake、ACLGraph replay 和 speculative decode 链路，但只有 smoke 结果，尚无 KV 压力结论。代码回归支持 #48245 方向，但尚未等同于双机问题已解决。'
schema: mindie-material-task/1
status: complete
task_id: 7e7c9c89e05ab6df6006f973d64946b80292b7c68acb2461ee1dcc379cbb6483
---

# 任务导航：P/D 重计算卡死与 scheduler 对照

当前主线是 153/154 上用 PR #13277 的 scheduler-off 配置完成 D 侧 KV 超限复现，并与 #48245 修复版做单变量对照；现阶段仍被 P 侧 FIA key format=RESERVED/EZ0017 错误阻断，不能据此判断 D 调度故障。155/156 的 scheduler-on 对照已验证基础 Mooncake、ACLGraph replay 和 speculative decode 链路，但只有 smoke 结果，尚无 KV 压力结论。代码回归支持 #48245 方向，但尚未等同于双机问题已解决。

## Materials

- [双机 P/D 重计算卡死定位与 KV 压测进展](blocks/fda0de6e3d3a734728511fcae5170b33766dbae12efa7a9d1427b80b6a663f6d.md): 针对 PR #13277、0.26.0、scheduler-off 的 D 节点卡死问题，代码分析指向 KV 不足后将 num_computed_tokens 清零并本地重算，可能形成自抢占活锁；上游 #48245 的 P/D preemption race 修复在定向回归中通过，AsyncRecomputeScheduler 压力模型也通过，但尚未完成双机硬件验证。此前因 kimi_c8/QuaRot 服务占用四台机器而暂停，获授权后已精确释放并隔离 153/154 与 155/156；scheduler-on session 仅完成 smoke，尚未形成 KV 打满结论。scheduler-off 基线当前受独立的 P 侧 aclnnFusedInferAttentionScoreV3、key format=RESERVED、EZ0017 错误阻断，短 prompt、eager 和多种输入仍未使请求到达 D；最新方案是使用合法极短 token-id prompt 配合 2048 强制输出，以继续验证 D 的 KV 抢占/重算路径，故问题尚未被硬件复现或确认修复。

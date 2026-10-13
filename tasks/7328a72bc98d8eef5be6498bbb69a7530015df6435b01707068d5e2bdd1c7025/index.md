---
blocks:
- block_id: f010bfdc96c361fa7cc0e3573220a39136c35d5e7c31c41595893e9c3b788f29
  indexed: true
  sha256: 573685ab479f85dcc29a4b760f17e23cadcd4c03daf39ac7dbbe5237ae21802e
  source_range:
    end: 1121104
    part: 0
    start: 0
  summary: 'D实例请求正常，但P实例服务请求触发 RuntimeError：Worker failed with error，底层为 aclnnKdaGateCumsum、AclNN_Parameter_Error(EZ1001) 及 ADD_TO_LAUNCHER_LIST_AICORE；日志位于 p.log。只读审计认为更可能是增量构建产生陈旧或不完整的 binary_info_config.json：KDA二进制文件存在但未被聚合配置收录，且 aggregate 依赖与缓存清理不足；尚未验证 PR #9 或确认 PD 分离功能成功。建议后续保留对实际 op inventory、.done 文件、聚合路径及三个 KDA op 的完整性检查，并注意当前P容器仍为 base 543819b5。'
  title: P实例 KdaGateCumsum 失败与增量构建审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7328a72bc98d8eef5be6498bbb69a7530015df6435b01707068d5e2bdd1c7025
  kind: experience
  material_digest: 7d9e484f791d61df387a3f5126b60b1dc437d313699e3dfd8c078fc5dc2764c7
  revision: 1c52032bd35a709fb8ad7067ba027051c4699d5d716d97197e38767a95a7b13e
  schema: mindie-entry/3
  summary: '已记录一个批次：P实例在 AscendC prefill 路径触发 KdaGateCumsum 运行时错误，D实例正常；初步判断指向增量构建导致的聚合配置缺失或陈旧缓存，而非已证实的TP16输入规格问题。PR #9 仅涉及 Mooncake unequal-TP 状态传输地址计算，尚未应用或验证，PD分离功能仍属未完成验证事项；后续应独立检查源码版本、op inventory、binary manifest、缓存清理和KDA包完整性。'
  title: 当前任务导航：P/D分离故障与KDA包完整性
navigation: '已记录一个批次：P实例在 AscendC prefill 路径触发 KdaGateCumsum 运行时错误，D实例正常；初步判断指向增量构建导致的聚合配置缺失或陈旧缓存，而非已证实的TP16输入规格问题。PR #9 仅涉及 Mooncake unequal-TP 状态传输地址计算，尚未应用或验证，PD分离功能仍属未完成验证事项；后续应独立检查源码版本、op inventory、binary manifest、缓存清理和KDA包完整性。'
schema: mindie-material-task/1
status: complete
task_id: 7328a72bc98d8eef5be6498bbb69a7530015df6435b01707068d5e2bdd1c7025
---

# 当前任务导航：P/D分离故障与KDA包完整性

已记录一个批次：P实例在 AscendC prefill 路径触发 KdaGateCumsum 运行时错误，D实例正常；初步判断指向增量构建导致的聚合配置缺失或陈旧缓存，而非已证实的TP16输入规格问题。PR #9 仅涉及 Mooncake unequal-TP 状态传输地址计算，尚未应用或验证，PD分离功能仍属未完成验证事项；后续应独立检查源码版本、op inventory、binary manifest、缓存清理和KDA包完整性。

## Materials

- [P实例 KdaGateCumsum 失败与增量构建审计](blocks/f010bfdc96c361fa7cc0e3573220a39136c35d5e7c31c41595893e9c3b788f29.md): D实例请求正常，但P实例服务请求触发 RuntimeError：Worker failed with error，底层为 aclnnKdaGateCumsum、AclNN_Parameter_Error(EZ1001) 及 ADD_TO_LAUNCHER_LIST_AICORE；日志位于 p.log。只读审计认为更可能是增量构建产生陈旧或不完整的 binary_info_config.json：KDA二进制文件存在但未被聚合配置收录，且 aggregate 依赖与缓存清理不足；尚未验证 PR #9 或确认 PD 分离功能成功。建议后续保留对实际 op inventory、.done 文件、聚合路径及三个 KDA op 的完整性检查，并注意当前P容器仍为 base 543819b5。

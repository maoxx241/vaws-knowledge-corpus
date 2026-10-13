---
blocks:
- block_id: 54f833f041961f0512be7854d72bf430e10fa49fed2b4a8900d3564681fa6734
  indexed: true
  sha256: 3cd9f88832a84d32df2a21b4a662aea6013a0a853ab3fe985a4fc7a9de798bbe
  source_range:
    end: 1479718
    part: 0
    start: 0
  summary: '该批材料记录了将仓库切换至 vllm-ascend/vllm-ascend-kimi-k3 的 release/0.23.0 分支并提交 Draft PR #4，涉及 AscendC KDA 算子、SiTU W4A8 fallback、packed W4、64D positional slice 及 identity cos/sin metadata；材料称通用 _restore_per_channel_scale_group_axis 已删除，但 A5 原生 MXFP4 端到端验证仍待补充。后续报告称 ACLGraph 在真实 5 层 INT4、TP8、enforce_eager=False、FULL_DECODE_ONLY 下通过定向 fullgraph 回归，期间曾暴露并修复 _KimiRoutedOutputTransform 的 object.__getattribute__ Dynamo trace 问题；不过最新同步后仅重跑了模型单测（20 passed, 14 warnings），未重跑 ACLGraph 或 PIECEWISE。精度 A/B 报告称 eager 与 FULL decode 的 tokens、chosen/top-20 logprob 完全一致且 status 均为 0，但未比较全量 logits；GitHub main 检查此前出现 Resource not accessible by integration，E2E workflow 也曾因未创建 job 而失败。'
  title: Kimi K3 AscendC 适配、ACLGraph 与精度 A/B 验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5bc1fdd46312b59f2c487017932758f24ba2e9c7da7df8d146461fb83975b709
  kind: experience
  material_digest: 2a36a850603268b16d49e9f13a76b249c9e6068b41f86591cefb049461926bed
  revision: 8b5d81ad19b88494f8e08242d843e57f2354c117c5ee18303a0f1801d5bd2d9a
  schema: mindie-entry/3
  summary: 索引覆盖仓库/分支与 Draft PR、RoPE 与 SiTU 分支处理、ACLGraph fullgraph 回归、模型单测以及 eager/FULL decode 精度 A/B。当前可检索的未完成或未确认事项包括 A5 原生 MXFP4 端到端验证、最新文件同步后的 ACLGraph/PIECEWISE 重跑，以及曾出现但未归因于代码测试失败的 GitHub workflow 权限与 E2E job 启动问题；材料中的验证结果均保留为报告内容，不提升为独立确认事实。
  title: 当前任务导航：Kimi K3 Ascend 适配与验证索引
navigation: 索引覆盖仓库/分支与 Draft PR、RoPE 与 SiTU 分支处理、ACLGraph fullgraph 回归、模型单测以及 eager/FULL decode 精度 A/B。当前可检索的未完成或未确认事项包括 A5 原生 MXFP4 端到端验证、最新文件同步后的 ACLGraph/PIECEWISE 重跑，以及曾出现但未归因于代码测试失败的 GitHub workflow 权限与 E2E job 启动问题；材料中的验证结果均保留为报告内容，不提升为独立确认事实。
schema: mindie-material-task/1
status: complete
task_id: 5bc1fdd46312b59f2c487017932758f24ba2e9c7da7df8d146461fb83975b709
---

# 当前任务导航：Kimi K3 Ascend 适配与验证索引

索引覆盖仓库/分支与 Draft PR、RoPE 与 SiTU 分支处理、ACLGraph fullgraph 回归、模型单测以及 eager/FULL decode 精度 A/B。当前可检索的未完成或未确认事项包括 A5 原生 MXFP4 端到端验证、最新文件同步后的 ACLGraph/PIECEWISE 重跑，以及曾出现但未归因于代码测试失败的 GitHub workflow 权限与 E2E job 启动问题；材料中的验证结果均保留为报告内容，不提升为独立确认事实。

## Materials

- [Kimi K3 AscendC 适配、ACLGraph 与精度 A/B 验证](blocks/54f833f041961f0512be7854d72bf430e10fa49fed2b4a8900d3564681fa6734.md): 该批材料记录了将仓库切换至 vllm-ascend/vllm-ascend-kimi-k3 的 release/0.23.0 分支并提交 Draft PR #4，涉及 AscendC KDA 算子、SiTU W4A8 fallback、packed W4、64D positional slice 及 identity cos/sin metadata；材料称通用 _restore_per_channel_scale_group_axis 已删除，但 A5 原生 MXFP4 端到端验证仍待补充。后续报告称 ACLGraph 在真实 5 层 INT4、TP8、enforce_eager=False、FULL_DECODE_ONLY 下通过定向 fullgraph 回归，期间曾暴露并修复 _KimiRoutedOutputTransform 的 object.__getattribute__ Dynamo trace 问题；不过最新同步后仅重跑了模型单测（20 passed, 14 warnings），未重跑 ACLGraph 或 PIECEWISE。精度 A/B 报告称 eager 与 FULL decode 的 tokens、chosen/top-20 logprob 完全一致且 status 均为 0，但未比较全量 logits；GitHub main 检查此前出现 Resource not accessible by integration，E2E workflow 也曾因未创建 job 而失败。

---
blocks:
- block_id: 76ea2bb7a046cce4e380b628ae5534fc46f996fdac0415bddfa335b3b3c78aea
  indexed: true
  sha256: 35d87ad53e6b72be3b6c96c5f52a52c2d66994c72087a7feb041239a9e418f9d
  source_range:
    end: 1054199
    part: 0
    start: 0
  summary: 记录了 GPQA 评测从后台运行、进度与预计耗时，到按用户要求停止四机服务，再到关闭 FlashComm、开启 prefix cache 和 FULL_DECODE_ONLY 后的全量测试；部分结果报告为 175/198、准确率 88.38%，但属于当时报告的观测，不能视为严格复现或数值一致性的证明。此前服务曾被停止并保留结果与日志，随后使用被编辑的权重标识重新启动四机服务，报告称 TP16 × DP4 × EP64、prefix cache 开启、352/352 分片、ACLGraph 7/7 capture 和 health=200；该批材料没有提供新一轮 GPQA 精度结果，后续状态仍需独立确认。
  title: GPQA 多轮服务与评测状态记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a5fdde9b364c3d088bf8e64254c096cc84ffcc01ce95214544b1b41373d64e59
  kind: experience
  material_digest: b2ab40489626f0f2b49f650323ea3729a7344adfb6d7b07595ed38cf1c1c2ee8
  revision: e102ab881432351d3b6b08c3179330efc23fa5f8ff04c032dd36134f3e5aa71e
  schema: mindie-entry/3
  summary: 可检索内容包括 GPQA 进度/耗时估计、服务停止与重启、FlashComm on/off 对比、prefix cache、FULL_DECODE_ONLY、TP16 × DP4 × EP64、ACLGraph capture、健康检查及保留的结果日志。已有报告中的成功状态和精度数字均为非验证性记录；最新材料只报告服务启动与配置就绪，尚未报告新权重下的 GPQA 全量测试结果。
  title: GPQA 服务、配置与评测进展
navigation: 可检索内容包括 GPQA 进度/耗时估计、服务停止与重启、FlashComm on/off 对比、prefix cache、FULL_DECODE_ONLY、TP16 × DP4 × EP64、ACLGraph capture、健康检查及保留的结果日志。已有报告中的成功状态和精度数字均为非验证性记录；最新材料只报告服务启动与配置就绪，尚未报告新权重下的 GPQA 全量测试结果。
schema: mindie-material-task/1
status: complete
task_id: a5fdde9b364c3d088bf8e64254c096cc84ffcc01ce95214544b1b41373d64e59
---

# GPQA 服务、配置与评测进展

可检索内容包括 GPQA 进度/耗时估计、服务停止与重启、FlashComm on/off 对比、prefix cache、FULL_DECODE_ONLY、TP16 × DP4 × EP64、ACLGraph capture、健康检查及保留的结果日志。已有报告中的成功状态和精度数字均为非验证性记录；最新材料只报告服务启动与配置就绪，尚未报告新权重下的 GPQA 全量测试结果。

## Materials

- [GPQA 多轮服务与评测状态记录](blocks/76ea2bb7a046cce4e380b628ae5534fc46f996fdac0415bddfa335b3b3c78aea.md): 记录了 GPQA 评测从后台运行、进度与预计耗时，到按用户要求停止四机服务，再到关闭 FlashComm、开启 prefix cache 和 FULL_DECODE_ONLY 后的全量测试；部分结果报告为 175/198、准确率 88.38%，但属于当时报告的观测，不能视为严格复现或数值一致性的证明。此前服务曾被停止并保留结果与日志，随后使用被编辑的权重标识重新启动四机服务，报告称 TP16 × DP4 × EP64、prefix cache 开启、352/352 分片、ACLGraph 7/7 capture 和 health=200；该批材料没有提供新一轮 GPQA 精度结果，后续状态仍需独立确认。

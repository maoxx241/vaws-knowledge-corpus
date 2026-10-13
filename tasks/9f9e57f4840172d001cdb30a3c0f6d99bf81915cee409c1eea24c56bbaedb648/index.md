---
blocks:
- block_id: d938533423dbf4fab86c094ab1bf7c8a9864625bd0280b31d51f1c2bebbaae3d
  indexed: true
  sha256: caa9779e2ef6f479bfb97d2f37318a9da5e2cbff74a372512c9c110d3ac0456d
  source_range:
    end: 853758
    part: 0
    start: 0
  summary: 显存 profile 曾在 153–156 精确清理并释放 64 张卡，随后因 GPQA 与 FlashComm/shared-expert-DP 服务安排保持暂停；GPQA 清理结果报告为 runner/服务 PGID 均已 KILL、设备无 holder，但恢复前仍要求自行复核。只读源码审计认为最新提交相对对照主要新增 shared-expert-DP 解耦和 A5 MLA FIA block-table 拆分：前者可能改变峰值绝对值但不引入 max_model_len 依赖，后者在 A5 显式开启且真实 decode/graph capture 时存在条件式长上下文风险，但 A3 profile 分支会被空 metadata 短路。结论仍属分析判断而非验证成功；建议在同一提交、固定 FC1/shared-DP/token 配置下仅改变 maxlen，并继续区分运行环境、启动参数和 profile 外 lazy allocation 等未完成排查项。
  title: 四机资源释放与 K3 提交差异审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9f9e57f4840172d001cdb30a3c0f6d99bf81915cee409c1eea24c56bbaedb648
  kind: experience
  material_digest: 17ce574d6a91ef0690092e970fd20b137a6ba701a1c69ee4f11db68215db5bc5
  revision: a6624564948f7f0552975be872f3a22e752c78b904534f9154c24d8decfe39af
  schema: mindie-entry/3
  summary: 此前四机 profile、FlashComm 服务和 ci-new GPQA 之间发生资源让渡，profile 曾暂停；GPQA 释放资源的结果已被报告，但恢复前需复核。当前源码审计未证明最新提交会让 A3 纯语言 profile 的峰值随 max_model_len 增长：shared-expert-DP 解耦可能改变绝对峰值，A5 FIA 拆分风险仅在特定运行路径出现。后续重点是完成同提交、固定 token/并行配置的 maxlen A/B，并保留对环境、参数及未完成 profile 外分配的核查。
  title: 当前任务导航：显存 profile 资源状态与 K3 maxlen 峰值审计
navigation: 此前四机 profile、FlashComm 服务和 ci-new GPQA 之间发生资源让渡，profile 曾暂停；GPQA 释放资源的结果已被报告，但恢复前需复核。当前源码审计未证明最新提交会让 A3 纯语言 profile 的峰值随 max_model_len 增长：shared-expert-DP 解耦可能改变绝对峰值，A5 FIA 拆分风险仅在特定运行路径出现。后续重点是完成同提交、固定 token/并行配置的 maxlen A/B，并保留对环境、参数及未完成 profile 外分配的核查。
schema: mindie-material-task/1
status: complete
task_id: 9f9e57f4840172d001cdb30a3c0f6d99bf81915cee409c1eea24c56bbaedb648
---

# 当前任务导航：显存 profile 资源状态与 K3 maxlen 峰值审计

此前四机 profile、FlashComm 服务和 ci-new GPQA 之间发生资源让渡，profile 曾暂停；GPQA 释放资源的结果已被报告，但恢复前需复核。当前源码审计未证明最新提交会让 A3 纯语言 profile 的峰值随 max_model_len 增长：shared-expert-DP 解耦可能改变绝对峰值，A5 FIA 拆分风险仅在特定运行路径出现。后续重点是完成同提交、固定 token/并行配置的 maxlen A/B，并保留对环境、参数及未完成 profile 外分配的核查。

## Materials

- [四机资源释放与 K3 提交差异审计](blocks/d938533423dbf4fab86c094ab1bf7c8a9864625bd0280b31d51f1c2bebbaae3d.md): 显存 profile 曾在 153–156 精确清理并释放 64 张卡，随后因 GPQA 与 FlashComm/shared-expert-DP 服务安排保持暂停；GPQA 清理结果报告为 runner/服务 PGID 均已 KILL、设备无 holder，但恢复前仍要求自行复核。只读源码审计认为最新提交相对对照主要新增 shared-expert-DP 解耦和 A5 MLA FIA block-table 拆分：前者可能改变峰值绝对值但不引入 max_model_len 依赖，后者在 A5 显式开启且真实 decode/graph capture 时存在条件式长上下文风险，但 A3 profile 分支会被空 metadata 短路。结论仍属分析判断而非验证成功；建议在同一提交、固定 FC1/shared-DP/token 配置下仅改变 maxlen，并继续区分运行环境、启动参数和 profile 外 lazy allocation 等未完成排查项。

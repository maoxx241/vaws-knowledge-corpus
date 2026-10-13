---
blocks:
- block_id: 5628001dd0120bbdb9d95ddf49341d531704a9f6a356b68f8ae97e30d3d7345f
  indexed: true
  sha256: 3614196f580c1024a60d05a12dbf5a06c35586e05a41f41aec3b01c201efc13a
  source_range:
    end: 401723
    part: 0
    start: 0
  summary: 来源报告称，154 的 D 服务配置为 DP2×TP8、FULL_DECODE_ONLY，16 个 rank 已初始化，ACLGraph decode FULL capture 报告为 1/1 成功，两个 API server 也报告完成启动；`GET /health` 返回 200，`/v1/models` 报告正常，模型为 `kimi-k3`。这些是未独立验证的报告结果，且尚未收到正式推理请求，因此 ACLGraph replay 日志仍待在后续 PD 请求后确认。服务地址包含已脱敏的 IPv4 占位符。
  title: 154 D 服务启动状态与待验证项
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6151242c2514c5916a28dab9c902ecd6bfe660f507bd991408eb6c674bbb4151
  kind: experience
  material_digest: 9ecdffcefb24210853c72e5a81445111fcdd4f5f5fc7097f7918192d54ac7561
  revision: 71d029311cb7f1ee7f115b8181f174af9f776ed980f8ae2643d8d453ccdf5746
  schema: mindie-entry/3
  summary: 新材料聚焦于 154 D 服务的启动与健康检查报告：包括 DP/TP 配置、rank 初始化、ACLGraph decode capture、API server、health 和 models API。正式推理请求尚未发生，ACLGraph replay 结果仍未确认；原始材料可按其 block_id 独立检索。
  title: 当前任务导航
navigation: 新材料聚焦于 154 D 服务的启动与健康检查报告：包括 DP/TP 配置、rank 初始化、ACLGraph decode capture、API server、health 和 models API。正式推理请求尚未发生，ACLGraph replay 结果仍未确认；原始材料可按其 block_id 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 6151242c2514c5916a28dab9c902ecd6bfe660f507bd991408eb6c674bbb4151
---

# 当前任务导航

新材料聚焦于 154 D 服务的启动与健康检查报告：包括 DP/TP 配置、rank 初始化、ACLGraph decode capture、API server、health 和 models API。正式推理请求尚未发生，ACLGraph replay 结果仍未确认；原始材料可按其 block_id 独立检索。

## Materials

- [154 D 服务启动状态与待验证项](blocks/5628001dd0120bbdb9d95ddf49341d531704a9f6a356b68f8ae97e30d3d7345f.md): 来源报告称，154 的 D 服务配置为 DP2×TP8、FULL_DECODE_ONLY，16 个 rank 已初始化，ACLGraph decode FULL capture 报告为 1/1 成功，两个 API server 也报告完成启动；`GET /health` 返回 200，`/v1/models` 报告正常，模型为 `kimi-k3`。这些是未独立验证的报告结果，且尚未收到正式推理请求，因此 ACLGraph replay 日志仍待在后续 PD 请求后确认。服务地址包含已脱敏的 IPv4 占位符。

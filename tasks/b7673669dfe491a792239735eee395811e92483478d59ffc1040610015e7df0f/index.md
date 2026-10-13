---
blocks:
- block_id: c5a7decb01140b210b2cd1454bd83e718d67d99902559cf22414c9d04a93d67c
  indexed: true
  sha256: 394390dd50d4c2da54d1ba7b43559fdc140eec0be632f6dcae968f4f52c01a0e
  source_range:
    end: 483201
    part: 0
    start: 0
  summary: 消息称已部署客户端侧脚本，尚未发送真实服务请求，也未修改生产源码或 NPU；方案包含 H1a 20/20、H1b 68/68 主体及各自 4 个独立尾探针，并要求四 DP 精确覆盖。校验器据称会重算 token、文本和完整响应 SHA256，检查 HTTP 200、reset、preemption、`None`、替换字符及 P0 答案一致性；相关“py_compile”“bash -n”等检查仅为报告结果，不能视为端到端验证成功。结果目录据称使用 `exist_ok=False` 拒绝覆盖，执行命令、路径、主机和容器信息含有已脱敏占位符。
  title: H1a/H1b/P0 客户端压测准备与严格校验
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b7673669dfe491a792239735eee395811e92483478d59ffc1040610015e7df0f
  kind: experience
  material_digest: d241eb7be7490f7fd699cae8a21beda9d6f41c25b8874a432289698f1d9a876f
  revision: a3273eddeceb43d467d0fa2fd8180e18b76a3a78e1242d0d14c97dfc40966f53
  schema: mindie-entry/3
  summary: 当前材料聚焦 H1a/H1b/P0 的客户端脚本、请求计数、四 DP 尾探针、严格 JSON 判定及 SHA256 校验；消息报告尚未发送真实请求，因此尚无压测结果或验证成功结论。后续应保留独立尾探针、68/68 计数语义、脱敏路径和未完成的端到端验证状态。
  title: 当前任务导航：客户端压测索引准备
navigation: 当前材料聚焦 H1a/H1b/P0 的客户端脚本、请求计数、四 DP 尾探针、严格 JSON 判定及 SHA256 校验；消息报告尚未发送真实请求，因此尚无压测结果或验证成功结论。后续应保留独立尾探针、68/68 计数语义、脱敏路径和未完成的端到端验证状态。
schema: mindie-material-task/1
status: complete
task_id: b7673669dfe491a792239735eee395811e92483478d59ffc1040610015e7df0f
---

# 当前任务导航：客户端压测索引准备

当前材料聚焦 H1a/H1b/P0 的客户端脚本、请求计数、四 DP 尾探针、严格 JSON 判定及 SHA256 校验；消息报告尚未发送真实请求，因此尚无压测结果或验证成功结论。后续应保留独立尾探针、68/68 计数语义、脱敏路径和未完成的端到端验证状态。

## Materials

- [H1a/H1b/P0 客户端压测准备与严格校验](blocks/c5a7decb01140b210b2cd1454bd83e718d67d99902559cf22414c9d04a93d67c.md): 消息称已部署客户端侧脚本，尚未发送真实服务请求，也未修改生产源码或 NPU；方案包含 H1a 20/20、H1b 68/68 主体及各自 4 个独立尾探针，并要求四 DP 精确覆盖。校验器据称会重算 token、文本和完整响应 SHA256，检查 HTTP 200、reset、preemption、`None`、替换字符及 P0 答案一致性；相关“py_compile”“bash -n”等检查仅为报告结果，不能视为端到端验证成功。结果目录据称使用 `exist_ok=False` 拒绝覆盖，执行命令、路径、主机和容器信息含有已脱敏占位符。

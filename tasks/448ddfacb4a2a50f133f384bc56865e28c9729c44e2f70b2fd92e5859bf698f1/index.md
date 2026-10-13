---
blocks:
- block_id: 78efcbded10f4f6aa1be556a9065b308fd3c4c5af469a109c0e061091340de15
  indexed: true
  sha256: 4d2a67ffb204d6eaafcf2ea2c300e1c6b8d4518e3a85ec9ded4cd8f11b9df2e8
  source_range:
    end: 1154614
    part: 0
    start: 0
  summary: 围绕 0726 版本 ModelSlim 权重，记录了从 896 个路由专家裁剪为每层 0–15 专家的普通量化版，以及保留 `optional/quarot.safetensors` 和 `global_rotation` 的 QuaRot 16 专家版；过程中出现过量化元数据字段计数过严、gate 集合误当字典等校验失败，均在正式发布前修正。随后又构造了 0726 QuaRot 32 专家版，保留专家 0–31、`num_experts=32`、`num_experts_per_token=16`；记录声称分片、safe_open、四节点可见性及旋转文件哈希复核通过，但这些仍属于报告中的结果而非独立验证。源权重据称未修改、服务未启动，路径和敏感信息按原文以 `[REDACTED_SECRET]` 保留。
  title: 构造 16/32 专家及 QuaRot 权重
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 448ddfacb4a2a50f133f384bc56865e28c9729c44e2f70b2fd92e5859bf698f1
  kind: experience
  material_digest: f87e718013d7150029cf7ba6ca0398b10499ca274dbd26886ec29ee00436b97a
  revision: 966f00b308bbf61521cf06686cee354b13301442db8fb24ae3e8614c6c1b7c4f
  schema: mindie-entry/3
  summary: 当前材料集中记录 0726 源权重的专家裁剪任务：普通量化版 16 专家、QuaRot 16 专家，以及 QuaRot 32 专家。可检索的关键术语包括 93 层、896→16/32 专家、gate/bias 同步裁剪、`num_experts_per_token=16`、safetensors 分片、`optional/quarot.safetensors`、`global_rotation`、NFS、safe_open，以及两次发布前校验脚本错误。相关完成状态、哈希/节点复核和源目录未修改等内容均是原对话中的报告，不能视为本索引独立确认的成功；具体路径已脱敏。
  title: 权重专家裁剪与 QuaRot 版本索引
navigation: 当前材料集中记录 0726 源权重的专家裁剪任务：普通量化版 16 专家、QuaRot 16 专家，以及 QuaRot 32 专家。可检索的关键术语包括 93 层、896→16/32 专家、gate/bias 同步裁剪、`num_experts_per_token=16`、safetensors 分片、`optional/quarot.safetensors`、`global_rotation`、NFS、safe_open，以及两次发布前校验脚本错误。相关完成状态、哈希/节点复核和源目录未修改等内容均是原对话中的报告，不能视为本索引独立确认的成功；具体路径已脱敏。
schema: mindie-material-task/1
status: complete
task_id: 448ddfacb4a2a50f133f384bc56865e28c9729c44e2f70b2fd92e5859bf698f1
---

# 权重专家裁剪与 QuaRot 版本索引

当前材料集中记录 0726 源权重的专家裁剪任务：普通量化版 16 专家、QuaRot 16 专家，以及 QuaRot 32 专家。可检索的关键术语包括 93 层、896→16/32 专家、gate/bias 同步裁剪、`num_experts_per_token=16`、safetensors 分片、`optional/quarot.safetensors`、`global_rotation`、NFS、safe_open，以及两次发布前校验脚本错误。相关完成状态、哈希/节点复核和源目录未修改等内容均是原对话中的报告，不能视为本索引独立确认的成功；具体路径已脱敏。

## Materials

- [构造 16/32 专家及 QuaRot 权重](blocks/78efcbded10f4f6aa1be556a9065b308fd3c4c5af469a109c0e061091340de15.md): 围绕 0726 版本 ModelSlim 权重，记录了从 896 个路由专家裁剪为每层 0–15 专家的普通量化版，以及保留 `optional/quarot.safetensors` 和 `global_rotation` 的 QuaRot 16 专家版；过程中出现过量化元数据字段计数过严、gate 集合误当字典等校验失败，均在正式发布前修正。随后又构造了 0726 QuaRot 32 专家版，保留专家 0–31、`num_experts=32`、`num_experts_per_token=16`；记录声称分片、safe_open、四节点可见性及旋转文件哈希复核通过，但这些仍属于报告中的结果而非独立验证。源权重据称未修改、服务未启动，路径和敏感信息按原文以 `[REDACTED_SECRET]` 保留。

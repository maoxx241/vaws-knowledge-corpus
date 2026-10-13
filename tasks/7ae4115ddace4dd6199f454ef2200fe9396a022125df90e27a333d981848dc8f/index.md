---
blocks:
- block_id: 9cd3971473b9a06074757ec1c1d58edcf30aa9cbdc4c36c668a41b4308e560fd
  indexed: true
  sha256: 934ca95f6217b148aa1ec8e4c4a3d17f6ceaebcc29a04b97769880043a1738bc
  source_range:
    end: 2328969
    part: 0
    start: 0
  summary: '只读审计聚焦 Mamba/KDA prefix-cache 生命周期、长序列 c2 的 block/slot ownership、eviction、异步 copy 边界及 0.23.0 满 KV 修复；all-FullAttention 截断补丁在 exact 128K c2 上结果不变，因此被保留为独立真实 bug，不能作为当前根因。已确认 PR #13277 远端与本地均为 e560c8031、工作区干净，并提出按功能拆分提交的整理边界。当前 Kimi K3 + ModelRunner V1 路径已禁用 PCP，但通用 PCP 代码仍存在：platform.py 和 kimi_kda.py 拒绝 PCP，patch_kv_cache_coordinator.py 固定 pcp_world_size=1；本次未修改或推送。'
  title: Kimi K3 Mamba/KDA PCP 与提交状态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7ae4115ddace4dd6199f454ef2200fe9396a022125df90e27a333d981848dc8f
  kind: experience
  material_digest: b7bd432b66829619689ceb734cd7d3bcec700b5a82ab3fc02545c4ec7c3de0f4
  revision: 64715d7662b9cf87e5bd00f62cf80a685ae1760f5881e77b318bde00fe9e660f
  schema: mindie-entry/3
  summary: '重点追踪 c2/full-KV 的生产—消费链路、cache-group 拓扑、ownership/eviction/异步 copy，并区分独立截断 bug 与当前根因。已知代码状态是 MRV1/Kimi 路径 PCP 不可达但通用分支未全删；PR #13277 状态为本地与远端同为 e560c8031、工作区干净。提交整理候选包括 Kimi DSpark、CI fixture、0.23.0 GDN 净修复、QuaRot/共享权重和 Mamba metadata 时序修复；精度修复仍需独立确认，未将假设或未验证结果升级为成功。'
  title: 当前任务导航：Kimi K3 cache/PCP 审计与提交整理
navigation: '重点追踪 c2/full-KV 的生产—消费链路、cache-group 拓扑、ownership/eviction/异步 copy，并区分独立截断 bug 与当前根因。已知代码状态是 MRV1/Kimi 路径 PCP 不可达但通用分支未全删；PR #13277 状态为本地与远端同为 e560c8031、工作区干净。提交整理候选包括 Kimi DSpark、CI fixture、0.23.0 GDN 净修复、QuaRot/共享权重和 Mamba metadata 时序修复；精度修复仍需独立确认，未将假设或未验证结果升级为成功。'
schema: mindie-material-task/1
status: complete
task_id: 7ae4115ddace4dd6199f454ef2200fe9396a022125df90e27a333d981848dc8f
---

# 当前任务导航：Kimi K3 cache/PCP 审计与提交整理

重点追踪 c2/full-KV 的生产—消费链路、cache-group 拓扑、ownership/eviction/异步 copy，并区分独立截断 bug 与当前根因。已知代码状态是 MRV1/Kimi 路径 PCP 不可达但通用分支未全删；PR #13277 状态为本地与远端同为 e560c8031、工作区干净。提交整理候选包括 Kimi DSpark、CI fixture、0.23.0 GDN 净修复、QuaRot/共享权重和 Mamba metadata 时序修复；精度修复仍需独立确认，未将假设或未验证结果升级为成功。

## Materials

- [Kimi K3 Mamba/KDA PCP 与提交状态审计](blocks/9cd3971473b9a06074757ec1c1d58edcf30aa9cbdc4c36c668a41b4308e560fd.md): 只读审计聚焦 Mamba/KDA prefix-cache 生命周期、长序列 c2 的 block/slot ownership、eviction、异步 copy 边界及 0.23.0 满 KV 修复；all-FullAttention 截断补丁在 exact 128K c2 上结果不变，因此被保留为独立真实 bug，不能作为当前根因。已确认 PR #13277 远端与本地均为 e560c8031、工作区干净，并提出按功能拆分提交的整理边界。当前 Kimi K3 + ModelRunner V1 路径已禁用 PCP，但通用 PCP 代码仍存在：platform.py 和 kimi_kda.py 拒绝 PCP，patch_kv_cache_coordinator.py 固定 pcp_world_size=1；本次未修改或推送。

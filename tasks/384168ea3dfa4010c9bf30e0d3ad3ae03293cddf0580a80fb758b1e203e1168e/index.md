---
blocks:
- block_id: 511bcf02b2df1585e9e7fbddd6f8295db9392379765e2cf564f906f4018abe2a
  indexed: true
  sha256: e10a8c6f570b1e526867ca3ddd71ba93716552c64611f2a053ceae48c1d5dfc8
  source_range:
    end: 1186991
    part: 0
    start: 0
  summary: 只读审计将 PR12950 提交 93b073de 作为基线，认为不应整包移植 PR13071；建议聚焦 6 个文件，涉及 legacy DSpark 配置归一化、GQA materialized residual stream、MRV1 架构识别、proposer 的 greedy/reduce-sample/LM-head TP fail-fast、Kimi 多模态 placeholder fallback，以及 KDA dim-first 状态复制 ABI。审计指出 PR12950 已覆盖 Qwen3 DSpark、V1 proposer、draft KV 分组、aux layer ID 等内容，且当前两个 linear 文件与其一致；PR13071 的 MLA、MRV2、shared-expert 等改动不应带入。prefix cache 的 off、首次 miss、二次 hit 和逐请求 accepted-token/GPQA 检查仍是建议的后续门禁，尚未被该材料证明为已验证成功。
  title: PR12950 基线下的 GQA/Kimi K3 最小移植审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 384168ea3dfa4010c9bf30e0d3ad3ae03293cddf0580a80fb758b1e203e1168e
  kind: experience
  material_digest: 8ae71244e89d95182a943e8d9387a424a0fe65880c7c6939fb70b52048fae8cf
  revision: 8c1886da7207c1ccc21fb57ecf8767bb6c4f47cc6b864ec5f2ca32c00cc272eb
  schema: mindie-entry/3
  summary: 已有一份只读审计索引：以 PR12950 为基线，区分 PR13071 中应移植的最小改动与不应带入的内容，并记录了配置、materialized aux stream、proposer 限制、媒体 token fallback、KDA 状态复制和 prefix cache 门禁等关键词。测试建议仍属于未完成验证的后续工作；原始材料应独立检索，不能将审计判断或建议测试视为已确认成功。
  title: 当前任务导航：PR12950 基线下的 GQA/Kimi K3 差异审计
navigation: 已有一份只读审计索引：以 PR12950 为基线，区分 PR13071 中应移植的最小改动与不应带入的内容，并记录了配置、materialized aux stream、proposer 限制、媒体 token fallback、KDA 状态复制和 prefix cache 门禁等关键词。测试建议仍属于未完成验证的后续工作；原始材料应独立检索，不能将审计判断或建议测试视为已确认成功。
schema: mindie-material-task/1
status: complete
task_id: 384168ea3dfa4010c9bf30e0d3ad3ae03293cddf0580a80fb758b1e203e1168e
---

# 当前任务导航：PR12950 基线下的 GQA/Kimi K3 差异审计

已有一份只读审计索引：以 PR12950 为基线，区分 PR13071 中应移植的最小改动与不应带入的内容，并记录了配置、materialized aux stream、proposer 限制、媒体 token fallback、KDA 状态复制和 prefix cache 门禁等关键词。测试建议仍属于未完成验证的后续工作；原始材料应独立检索，不能将审计判断或建议测试视为已确认成功。

## Materials

- [PR12950 基线下的 GQA/Kimi K3 最小移植审计](blocks/511bcf02b2df1585e9e7fbddd6f8295db9392379765e2cf564f906f4018abe2a.md): 只读审计将 PR12950 提交 93b073de 作为基线，认为不应整包移植 PR13071；建议聚焦 6 个文件，涉及 legacy DSpark 配置归一化、GQA materialized residual stream、MRV1 架构识别、proposer 的 greedy/reduce-sample/LM-head TP fail-fast、Kimi 多模态 placeholder fallback，以及 KDA dim-first 状态复制 ABI。审计指出 PR12950 已覆盖 Qwen3 DSpark、V1 proposer、draft KV 分组、aux layer ID 等内容，且当前两个 linear 文件与其一致；PR13071 的 MLA、MRV2、shared-expert 等改动不应带入。prefix cache 的 off、首次 miss、二次 hit 和逐请求 accepted-token/GPQA 检查仍是建议的后续门禁，尚未被该材料证明为已验证成功。

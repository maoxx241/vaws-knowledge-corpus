---
blocks:
- block_id: e8842cde8238f68d4d2e10bfe876b7802010236161b4fbbc46a7dac7aff34a20
  indexed: true
  sha256: 91a54a5c0c598cedccd5762c66af32b97031309fac045a2f5aa2f4ef49d7d4ec
  source_range:
    end: 6384308
    part: 0
    start: 0
  summary: 该批次围绕 vLLM Ascend、Kimi K3 的问题定位、模型开发、算子接入和服务部署会话进行全量扫描，产出 operator-review.md、operator-candidates.json 与 operator-triage.json；共审阅 19 个 lane 文件及 6 个跨领域会话，提炼 22 个簇并验证 96 个原始路径/行号。后续固定主线源码复核显示 MLA 已转向 Prolog V3，历史 FlashMLA 包装路径不再存在；一次 cast、融合 RMSNorm、路由 ID 去重和动态长度 JIT 修复已在私有主线，相关旧优化建议被删除。最终 OP01–OP22 中 8 条重写、13 条删除、1 条暂缓，另有 Kimi 交叉复核 6 条通过、4 条需修改；未重跑 NPU，材料未入库或发布，历史性能数字和硬件通过次数被移除。
  title: vLLM Ascend 与 Kimi K3 知识库贡献审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 66a99e798d60949030de19818fa28f01e012e1b8529199d99dd27b5bb9f458fa
  kind: experience
  material_digest: 5d5b7a54587f1cdbb5b7930ed6afe70db6115b770342cad8f821baaaf9174955
  revision: 04da5b07dc0d942ba981d41cf721cea8b2652a182add7ccbfbf35abe38df288b
  schema: mindie-entry/3
  summary: 当前索引覆盖一批关于问题定位、模型开发、算子接入和服务部署的会话审查结果，以及基于固定主线源码的后续修订。可重点检索 operator-review.md、operator-candidates.json、operator-triage.json、main-review/operator-review.json、operator-review.md、cross-kimi.json 和 proposed/；这些材料保留了已验证路径、助手结论级内容、撤回/删除、暂缓项和待修改项的区分。结论受限于未重跑 NPU、私有内容范围和未发布状态，不能将审查结果视为已验证案例或线上成功。
  title: 当前任务导航：vLLM Ascend / Kimi K3 会话知识库索引
navigation: 当前索引覆盖一批关于问题定位、模型开发、算子接入和服务部署的会话审查结果，以及基于固定主线源码的后续修订。可重点检索 operator-review.md、operator-candidates.json、operator-triage.json、main-review/operator-review.json、operator-review.md、cross-kimi.json 和 proposed/；这些材料保留了已验证路径、助手结论级内容、撤回/删除、暂缓项和待修改项的区分。结论受限于未重跑 NPU、私有内容范围和未发布状态，不能将审查结果视为已验证案例或线上成功。
schema: mindie-material-task/1
status: complete
task_id: 66a99e798d60949030de19818fa28f01e012e1b8529199d99dd27b5bb9f458fa
---

# 当前任务导航：vLLM Ascend / Kimi K3 会话知识库索引

当前索引覆盖一批关于问题定位、模型开发、算子接入和服务部署的会话审查结果，以及基于固定主线源码的后续修订。可重点检索 operator-review.md、operator-candidates.json、operator-triage.json、main-review/operator-review.json、operator-review.md、cross-kimi.json 和 proposed/；这些材料保留了已验证路径、助手结论级内容、撤回/删除、暂缓项和待修改项的区分。结论受限于未重跑 NPU、私有内容范围和未发布状态，不能将审查结果视为已验证案例或线上成功。

## Materials

- [vLLM Ascend 与 Kimi K3 知识库贡献审查](blocks/e8842cde8238f68d4d2e10bfe876b7802010236161b4fbbc46a7dac7aff34a20.md): 该批次围绕 vLLM Ascend、Kimi K3 的问题定位、模型开发、算子接入和服务部署会话进行全量扫描，产出 operator-review.md、operator-candidates.json 与 operator-triage.json；共审阅 19 个 lane 文件及 6 个跨领域会话，提炼 22 个簇并验证 96 个原始路径/行号。后续固定主线源码复核显示 MLA 已转向 Prolog V3，历史 FlashMLA 包装路径不再存在；一次 cast、融合 RMSNorm、路由 ID 去重和动态长度 JIT 修复已在私有主线，相关旧优化建议被删除。最终 OP01–OP22 中 8 条重写、13 条删除、1 条暂缓，另有 Kimi 交叉复核 6 条通过、4 条需修改；未重跑 NPU，材料未入库或发布，历史性能数字和硬件通过次数被移除。

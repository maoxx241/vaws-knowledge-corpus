---
blocks:
- block_id: b1b7412cec9d27a358d157c0bb7560c9901e51d724cd83f004a7faaa726680f6
  indexed: true
  sha256: ae42bb1a087bf3cef8e7de6b911c872a2ffaba354d34e30edde1e72d15940a85
  source_range:
    end: 1141441
    part: 0
    start: 0
  summary: 只读审计将 `global_bs=0 + x_active_mask` 视为当前 7/7/7/8 路径中的待验证合法用法，并提出用 runtime `global_bs=64`、保持 mask、强制 ALLGATHER 的对照实验；这些结果尚未实际完成，不能据此确认 CANN、MC2 或 KDA/MLA 的根因。记录了 dispatch/combine 传递 seam、A3 fallback 条件、[8,8,8,8] 与 [7,7,7,8] 对照，以及现有测试不足（仅检查 kwargs、含不满足前缀约束的示例 mask）。未修改文件或操作远端服务；文中链接和路径含有保留的 redaction 占位符。
  title: MC2 global_bs、mask 与 ALLGATHER 排查索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7fc2e32de9fc2195d05d875d87c51ad69158c658a9afc2fabbf445792386e90f
  kind: experience
  material_digest: 80362e47654d5a0b3ee29161b3f418226cd417b1f1d7908590841ff68e4954ad
  revision: b4e9231e810e495ca29d53a5a079addb949ed559286fee7616ebd019f8718802
  schema: mindie-entry/3
  summary: 本批唯一材料聚焦 Ascend/MindIE MoE 路径中 `global_bs`、`x_active_mask`、ALLGATHER 与 MC2 dispatch/combine 的只读契约审计。后续检索应区分已观察到的代码 seam、建议的实验与尚未验证的判断；特别保留 7/7/7/8 与 [8,8,8,8] 对照、64-rank/TP16 条件、A3 fallback、测试缺口及未完成的根因定位。原始材料仍独立可搜索，不据此声称实验成功或记住未提供的更早细节。
  title: 当前任务导航：MC2 unequal-DP batch 的检索索引
navigation: 本批唯一材料聚焦 Ascend/MindIE MoE 路径中 `global_bs`、`x_active_mask`、ALLGATHER 与 MC2 dispatch/combine 的只读契约审计。后续检索应区分已观察到的代码 seam、建议的实验与尚未验证的判断；特别保留 7/7/7/8 与 [8,8,8,8] 对照、64-rank/TP16 条件、A3 fallback、测试缺口及未完成的根因定位。原始材料仍独立可搜索，不据此声称实验成功或记住未提供的更早细节。
schema: mindie-material-task/1
status: complete
task_id: 7fc2e32de9fc2195d05d875d87c51ad69158c658a9afc2fabbf445792386e90f
---

# 当前任务导航：MC2 unequal-DP batch 的检索索引

本批唯一材料聚焦 Ascend/MindIE MoE 路径中 `global_bs`、`x_active_mask`、ALLGATHER 与 MC2 dispatch/combine 的只读契约审计。后续检索应区分已观察到的代码 seam、建议的实验与尚未验证的判断；特别保留 7/7/7/8 与 [8,8,8,8] 对照、64-rank/TP16 条件、A3 fallback、测试缺口及未完成的根因定位。原始材料仍独立可搜索，不据此声称实验成功或记住未提供的更早细节。

## Materials

- [MC2 global_bs、mask 与 ALLGATHER 排查索引](blocks/b1b7412cec9d27a358d157c0bb7560c9901e51d724cd83f004a7faaa726680f6.md): 只读审计将 `global_bs=0 + x_active_mask` 视为当前 7/7/7/8 路径中的待验证合法用法，并提出用 runtime `global_bs=64`、保持 mask、强制 ALLGATHER 的对照实验；这些结果尚未实际完成，不能据此确认 CANN、MC2 或 KDA/MLA 的根因。记录了 dispatch/combine 传递 seam、A3 fallback 条件、[8,8,8,8] 与 [7,7,7,8] 对照，以及现有测试不足（仅检查 kwargs、含不满足前缀约束的示例 mask）。未修改文件或操作远端服务；文中链接和路径含有保留的 redaction 占位符。

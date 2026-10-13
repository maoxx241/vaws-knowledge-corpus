---
blocks:
- block_id: 282d3da046fb054cab386ce454869626885cfc64f7df727512b475df17d2db00
  indexed: true
  sha256: 4433dafbbac3f159705d963d293554dcfacaf99e64b9ccd67e1270321f8be0cc
  source_range:
    end: 1289788
    part: 0
    start: 0
  summary: '记录称已整理并发布 PR #13277 的 8 条逐 commit 评论，涵盖 GDN replay、QuaRot 坐标对齐、四平面 Cache 所有权、FULL replay stale state、FullAttention cache group 裁剪、Mamba producer hit 边界及 idle DP dummy 状态更新；同时删除了两条旧的本人评论，并保留权重回复、机器人和其他人员评论。算子提交 `9998aa44e` 的归档复现记录报告：`chunk_gated_delta_rule_fwd_h` 修复前 41 tasks 稳定 60 秒超时、40 tasks 可完成，修复后 41/42/80/96 tasks 均完成，42-task native 与 7 次 isolated native 对比为 `max_abs=0`；报告将原因归于空 `Vnew` AIV 子块提前返回时未正确消费/恢复 `HardEvent`。这些结论据称来自 GitHub 评论、归档脚本和日志的只读核对，未启动四机服务；代码、提交和推送均未修改，相关成功与根因仍应视为该记录中的报告而非独立验证。'
  title: 'PR #13277 精度修复与 KDA 单算子复现记录'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0bd214497709c117bc0306d184cdb5a5494ced6172366cb79010afee0469754f
  kind: experience
  material_digest: e09cfa6835c8284e2baaecdb2c88fc80e29b985f230908a37851d4e907334f79
  revision: 53dc31d05d6a7ec6e455aa6529ba6d8b6f6de6c6d5baa74bbc987ae13f26c268
  schema: mindie-entry/3
  summary: '当前材料集中于 PR #13277 的精度修复 commit 说明、评论清理结果，以及 `9998aa44e` KDA 单算子 hang 的稳定复现边界和归档日志核对。后续检索应区分已发布的评论内容、据称的回读校验与尚未独立验证的修复/根因结论，并保留 8 个评论链接、任务阈值和相关 commit 映射。'
  title: 'PR #13277 精度与算子复现索引'
navigation: '当前材料集中于 PR #13277 的精度修复 commit 说明、评论清理结果，以及 `9998aa44e` KDA 单算子 hang 的稳定复现边界和归档日志核对。后续检索应区分已发布的评论内容、据称的回读校验与尚未独立验证的修复/根因结论，并保留 8 个评论链接、任务阈值和相关 commit 映射。'
schema: mindie-material-task/1
status: complete
task_id: 0bd214497709c117bc0306d184cdb5a5494ced6172366cb79010afee0469754f
---

# PR #13277 精度与算子复现索引

当前材料集中于 PR #13277 的精度修复 commit 说明、评论清理结果，以及 `9998aa44e` KDA 单算子 hang 的稳定复现边界和归档日志核对。后续检索应区分已发布的评论内容、据称的回读校验与尚未独立验证的修复/根因结论，并保留 8 个评论链接、任务阈值和相关 commit 映射。

## Materials

- [PR #13277 精度修复与 KDA 单算子复现记录](blocks/282d3da046fb054cab386ce454869626885cfc64f7df727512b475df17d2db00.md): 记录称已整理并发布 PR #13277 的 8 条逐 commit 评论，涵盖 GDN replay、QuaRot 坐标对齐、四平面 Cache 所有权、FULL replay stale state、FullAttention cache group 裁剪、Mamba producer hit 边界及 idle DP dummy 状态更新；同时删除了两条旧的本人评论，并保留权重回复、机器人和其他人员评论。算子提交 `9998aa44e` 的归档复现记录报告：`chunk_gated_delta_rule_fwd_h` 修复前 41 tasks 稳定 60 秒超时、40 tasks 可完成，修复后 41/42/80/96 tasks 均完成，42-task native 与 7 次 isolated native 对比为 `max_abs=0`；报告将原因归于空 `Vnew` AIV 子块提前返回时未正确消费/恢复 `HardEvent`。这些结论据称来自 GitHub 评论、归档脚本和日志的只读核对，未启动四机服务；代码、提交和推送均未修改，相关成功与根因仍应视为该记录中的报告而非独立验证。

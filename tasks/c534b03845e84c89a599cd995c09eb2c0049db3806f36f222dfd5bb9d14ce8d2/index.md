---
blocks:
- block_id: af4616d8cdead973a0944b4a60a6a7096217075558f91f0a625ced6c04ef4e49
  indexed: true
  sha256: 61bbf1865affdc87861e0d92f44dc517d09b2417b8757ff630254b94c999bd7e
  source_range:
    end: 6954620
    part: 0
    start: 0
  summary: '记录将 AttnResFwd、FLA fused_norm_gate 和主线 mla_preprocess 接入 Kimi K3/PR #13277 的过程，包含 DCO、默认 custom-op 打包、tiling UB 溢出、include 缺失及 harness 导入等失败与修复。来源称 a3840725 上 10/10 单卡算子、4/4 数学题及 GQA/MLA 的 8K、128K 性能波次通过，但 CI 尚有排队项，结论属于任务记录而非独立核验报告。性能数据包括 MLA 8K/off AL 约 3.875，以及 prefix hit 98.7305%。'
  title: 三算子接入与首轮性能验证
- block_id: 21e8f53baa60c3b28890e083512c912d88b498375875a2a4745891c3e861c925
  indexed: true
  sha256: f04d5ee1db562870ee5255be248d987776907630e6df919b8b3290a5c2be937b
  source_range:
    end: 6954620
    part: 1
    start: 0
  summary: 针对 MLA 8K/1K、并发 1 的单算子消融记录显示，全算子 AL 从旧基线约 5.2347 降至约 3.8755；禁用 AttnResFwd/res_attn 后 accepted/drafts 恢复为 830/196，输出 token hash 也与旧基线一致。禁用 fused_norm_gate 或 mla_preprocess 未恢复该指标，因此来源将主要问题归因于 AttnResFwd，并生成删除该算子的提交 cfaebdd6；本机 pytest 因缺 torch、gitleaks/wget 缺失，部分验证仍受环境限制。
  title: AttnResFwd 导致接收长度回退
- block_id: 02f48c6db9b35d9aae39c39ba9f2fa42b0ae79ad214ceff66d2ae9f087bbab77
  indexed: true
  sha256: ed8d439bd5ce7af4e63dcad3eba9674575e375d2f3d3156057b263a8b95eac2e
  source_range:
    end: 6954620
    part: 2
    start: 0
  summary: '来源记录在剔除 AttnResFwd、保留 fused_norm_gate 和 mla_preprocess 的 cfaebdd6 上重跑 MLA/GQA 的 8K/off 与 128K/on 矩阵，报告 1024 输出、质量门通过及 98.7305% prefix hit；随后以最终主 PR head 9d442fb1 更新材料。另创建 Draft PR #13315（head 963ca32b），组合 DSpark 与三个算子，用于单独排查 AttnRes 精度/采信率；其旧 pre-commit 失败被归因于标题前缀，后续是否完整重跑仍需以最新 CI 为准。'
  title: 两算子性能重测与三算子调试 PR
- block_id: e467a11ee5fa69f34f7c4766e3d7eb746bbe6f5b475beb20a015efc8056691ca
  indexed: true
  sha256: b26bfd1d42f6187d967a178d9197f817eb4859ca0934cb12b55f9582d403eac6
  source_range:
    end: 6954620
    part: 3
    start: 0
  summary: '来源称主 PR #13277 当前保留 fused_norm_gate 与 mla_preprocess、移除 AttnResFwd，head 为 9d442fb1；旧性能/验证评论 5141512104 已删除，当前性能评论 5144927690 保留并与最新版本内容校验一致。其摘要还记录 Draft #13315 仍用于三算子加 DSpark 的 AttnRes 调试，以及最终 GQA prefix-on 服务在 18077 保活；这些运行状态、CI 全通过和报告验证均应视为来源声称，非本轮独立核实。'
  title: 清理过期评论并保留最新 PR 结果
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c534b03845e84c89a599cd995c09eb2c0049db3806f36f222dfd5bb9d14ce8d2
  kind: experience
  material_digest: d395ccaf3e64afa06edfa5c35f6735c5726c02d9c6015b9dbe4383b73517dca4
  revision: ba1eb42de746ffc0088dc91742f888528a8326d11bebabc5b346359d4590a6fb
  schema: mindie-entry/3
  summary: '主线已从三算子版本收敛为移除 AttnResFwd、保留 fused_norm_gate 与 mla_preprocess 的 PR #13277，当前记录 head 为 9d442fb1；关键诊断是 AttnResFwd 消融恢复旧 AL，而另外两个算子未恢复。性能记录覆盖 MLA/GQA 的 8K/off 与 128K/on，prefix hit 报告为 98.7305%，但部分 CI、测试环境和运行态结论应按来源声明理解。三算子加 DSpark 的 Draft PR #13315 保留给其他人继续检查 AttnRes 精度/采信率；过期评论已删除，最新性能评论保留。'
  title: '当前任务导航：PR #13277 两算子版本与 AttnRes 调试'
navigation: '主线已从三算子版本收敛为移除 AttnResFwd、保留 fused_norm_gate 与 mla_preprocess 的 PR #13277，当前记录 head 为 9d442fb1；关键诊断是 AttnResFwd 消融恢复旧 AL，而另外两个算子未恢复。性能记录覆盖 MLA/GQA 的 8K/off 与 128K/on，prefix hit 报告为 98.7305%，但部分 CI、测试环境和运行态结论应按来源声明理解。三算子加 DSpark 的 Draft PR #13315 保留给其他人继续检查 AttnRes 精度/采信率；过期评论已删除，最新性能评论保留。'
schema: mindie-material-task/1
status: complete
task_id: c534b03845e84c89a599cd995c09eb2c0049db3806f36f222dfd5bb9d14ce8d2
---

# 当前任务导航：PR #13277 两算子版本与 AttnRes 调试

主线已从三算子版本收敛为移除 AttnResFwd、保留 fused_norm_gate 与 mla_preprocess 的 PR #13277，当前记录 head 为 9d442fb1；关键诊断是 AttnResFwd 消融恢复旧 AL，而另外两个算子未恢复。性能记录覆盖 MLA/GQA 的 8K/off 与 128K/on，prefix hit 报告为 98.7305%，但部分 CI、测试环境和运行态结论应按来源声明理解。三算子加 DSpark 的 Draft PR #13315 保留给其他人继续检查 AttnRes 精度/采信率；过期评论已删除，最新性能评论保留。

## Materials

- [三算子接入与首轮性能验证](blocks/af4616d8cdead973a0944b4a60a6a7096217075558f91f0a625ced6c04ef4e49.md): 记录将 AttnResFwd、FLA fused_norm_gate 和主线 mla_preprocess 接入 Kimi K3/PR #13277 的过程，包含 DCO、默认 custom-op 打包、tiling UB 溢出、include 缺失及 harness 导入等失败与修复。来源称 a3840725 上 10/10 单卡算子、4/4 数学题及 GQA/MLA 的 8K、128K 性能波次通过，但 CI 尚有排队项，结论属于任务记录而非独立核验报告。性能数据包括 MLA 8K/off AL 约 3.875，以及 prefix hit 98.7305%。
- [AttnResFwd 导致接收长度回退](blocks/21e8f53baa60c3b28890e083512c912d88b498375875a2a4745891c3e861c925.md): 针对 MLA 8K/1K、并发 1 的单算子消融记录显示，全算子 AL 从旧基线约 5.2347 降至约 3.8755；禁用 AttnResFwd/res_attn 后 accepted/drafts 恢复为 830/196，输出 token hash 也与旧基线一致。禁用 fused_norm_gate 或 mla_preprocess 未恢复该指标，因此来源将主要问题归因于 AttnResFwd，并生成删除该算子的提交 cfaebdd6；本机 pytest 因缺 torch、gitleaks/wget 缺失，部分验证仍受环境限制。
- [两算子性能重测与三算子调试 PR](blocks/02f48c6db9b35d9aae39c39ba9f2fa42b0ae79ad214ceff66d2ae9f087bbab77.md): 来源记录在剔除 AttnResFwd、保留 fused_norm_gate 和 mla_preprocess 的 cfaebdd6 上重跑 MLA/GQA 的 8K/off 与 128K/on 矩阵，报告 1024 输出、质量门通过及 98.7305% prefix hit；随后以最终主 PR head 9d442fb1 更新材料。另创建 Draft PR #13315（head 963ca32b），组合 DSpark 与三个算子，用于单独排查 AttnRes 精度/采信率；其旧 pre-commit 失败被归因于标题前缀，后续是否完整重跑仍需以最新 CI 为准。
- [清理过期评论并保留最新 PR 结果](blocks/e467a11ee5fa69f34f7c4766e3d7eb746bbe6f5b475beb20a015efc8056691ca.md): 来源称主 PR #13277 当前保留 fused_norm_gate 与 mla_preprocess、移除 AttnResFwd，head 为 9d442fb1；旧性能/验证评论 5141512104 已删除，当前性能评论 5144927690 保留并与最新版本内容校验一致。其摘要还记录 Draft #13315 仍用于三算子加 DSpark 的 AttnRes 调试，以及最终 GQA prefix-on 服务在 18077 保活；这些运行状态、CI 全通过和报告验证均应视为来源声称，非本轮独立核实。

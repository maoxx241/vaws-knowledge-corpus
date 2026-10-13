---
blocks:
- block_id: 3ab9e08f7281683b06d94fb8df39f1db6c1c8e23f4233d64037436897aa4dffd
  indexed: true
  sha256: 39e98db4cfd02b1f453f441414d7f2163ad5d52a725b8eecebf4c29369f2b2ba
  source_range:
    end: 542295
    part: 0
    start: 0
  summary: '来源先报告 PR #12950 已更新、检视意见已处理、UT 与多项四机验证通过，并记录命名 tool choice 的 `finish_reason="stop"` 先前失败源于验证脚本断言错误；这些内容属于来源陈述，非本索引独立核验。随后用户要求修 CI，针对同一 head 的两个 A2 job 进行只读诊断：`91018490777` 在 `test_mla_cp.py:116` 缺少必填 `seq_lens_device`，`91018490780` 在 `test_acl_graph.py:909` 因旧位置参数错位并最终缺少 `seq_lens_list`；推断根因是 MLA split commit `302dbc342` 为 `AscendMLADecodeMetadata` 新增必填字段而 DCP UT fixture 未同步，建议显式传参并改用关键字参数，但未修改或执行外部操作。'
  title: 'PR #12950 验证与 CI 失败诊断'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: db273384933a1b960e3758607755a52ecc428472f970f139a3262da065571ca4
  kind: experience
  material_digest: 768ef543e0a74276a786dc00fdb3830a1024f094769d1de2b7cc022b44403059
  revision: f43b900a8e5faa882fcd9e55e2722cb858a380b2f4fd47f504f68273390b4948
  schema: mindie-entry/3
  summary: '材料涵盖 PR #12950 的来源验证结果、未完成的排队 E2E，以及后续两个 A2 CI job 的只读失败诊断。优先检索 `seq_lens_device`、`seq_lens_list`、`AscendMLADecodeMetadata`、`test_mla_cp.py:116`、`test_acl_graph.py:909` 和 commit `302dbc342`；CI 根因与修复建议仍是未验证的诊断结论，原始验证成功声明也应视为来源陈述而非已独立确认事实。'
  title: 当前任务导航：PR 验证记录与 CI 修复线索
navigation: '材料涵盖 PR #12950 的来源验证结果、未完成的排队 E2E，以及后续两个 A2 CI job 的只读失败诊断。优先检索 `seq_lens_device`、`seq_lens_list`、`AscendMLADecodeMetadata`、`test_mla_cp.py:116`、`test_acl_graph.py:909` 和 commit `302dbc342`；CI 根因与修复建议仍是未验证的诊断结论，原始验证成功声明也应视为来源陈述而非已独立确认事实。'
schema: mindie-material-task/1
status: complete
task_id: db273384933a1b960e3758607755a52ecc428472f970f139a3262da065571ca4
---

# 当前任务导航：PR 验证记录与 CI 修复线索

材料涵盖 PR #12950 的来源验证结果、未完成的排队 E2E，以及后续两个 A2 CI job 的只读失败诊断。优先检索 `seq_lens_device`、`seq_lens_list`、`AscendMLADecodeMetadata`、`test_mla_cp.py:116`、`test_acl_graph.py:909` 和 commit `302dbc342`；CI 根因与修复建议仍是未验证的诊断结论，原始验证成功声明也应视为来源陈述而非已独立确认事实。

## Materials

- [PR #12950 验证与 CI 失败诊断](blocks/3ab9e08f7281683b06d94fb8df39f1db6c1c8e23f4233d64037436897aa4dffd.md): 来源先报告 PR #12950 已更新、检视意见已处理、UT 与多项四机验证通过，并记录命名 tool choice 的 `finish_reason="stop"` 先前失败源于验证脚本断言错误；这些内容属于来源陈述，非本索引独立核验。随后用户要求修 CI，针对同一 head 的两个 A2 job 进行只读诊断：`91018490777` 在 `test_mla_cp.py:116` 缺少必填 `seq_lens_device`，`91018490780` 在 `test_acl_graph.py:909` 因旧位置参数错位并最终缺少 `seq_lens_list`；推断根因是 MLA split commit `302dbc342` 为 `AscendMLADecodeMetadata` 新增必填字段而 DCP UT fixture 未同步，建议显式传参并改用关键字参数，但未修改或执行外部操作。

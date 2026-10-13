---
blocks:
- block_id: 574e277a4494360b661e27165c7c03907cd3214588ecc55b284274030accca27
  indexed: true
  sha256: d8a000b178e66a23e5b087531a19970fa45a8fca4ad89591b293ee0012c6799c
  source_range:
    end: 684466
    part: 0
    start: 0
  summary: '此前记录称 PR #12950 已更新至 `6ca068805`，检视意见已处理，UT 194/194、四机重编、ViT/chat/tool call、GSM8K 等验证均报告成功；但这些内容属于脱敏验证记录，且新 E2E 当时仍在排队。随后针对用户“修下ci”的任务仅完成只读诊断，定位到 `AscendMLADCPDecodeMetadata` 新增必填 `seq_lens_device` 后，`test_mla_cp.py` 与 `test_acl_graph.py` 的显式 fixture 未同步，另一个 job 的 artifact 为 `190 passed / 29 skipped`，外层因 custom container 中断而取消；未编辑、重跑、评论或 push，因此 CI 修复仍未完成。'
  title: 'PR #12950 验证与 v0.26 CI 诊断'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 67aecb522ccecea2786194297aacc3e17055328596e0e59c5b722a690d20f89e
  kind: experience
  material_digest: 122e837846b829238378f928cea6c2eede5760c1d44844ae9a804486ce50beb3
  revision: 97f44dd69eb2975122fe6144e813e32a288442f105ad0e2b0198be5d5a4f9a6b
  schema: mindie-entry/3
  summary: 可检索此前的 PR 验证结果、失败断言更正及当前 CI 诊断。当前未完成事项是为两处 DCP metadata 构造补充 `seq_lens_device=seq_lens`（ACLGraph 同时改用关键字参数），并单独重跑疑似 runner/container 中断的 job；原始材料仍应独立检索，不将报告的验证或修复计划视为已验证成功。
  title: 'PR #12950：验证记录与未完成的 CI 修复'
navigation: 可检索此前的 PR 验证结果、失败断言更正及当前 CI 诊断。当前未完成事项是为两处 DCP metadata 构造补充 `seq_lens_device=seq_lens`（ACLGraph 同时改用关键字参数），并单独重跑疑似 runner/container 中断的 job；原始材料仍应独立检索，不将报告的验证或修复计划视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 67aecb522ccecea2786194297aacc3e17055328596e0e59c5b722a690d20f89e
---

# PR #12950：验证记录与未完成的 CI 修复

可检索此前的 PR 验证结果、失败断言更正及当前 CI 诊断。当前未完成事项是为两处 DCP metadata 构造补充 `seq_lens_device=seq_lens`（ACLGraph 同时改用关键字参数），并单独重跑疑似 runner/container 中断的 job；原始材料仍应独立检索，不将报告的验证或修复计划视为已验证成功。

## Materials

- [PR #12950 验证与 v0.26 CI 诊断](blocks/574e277a4494360b661e27165c7c03907cd3214588ecc55b284274030accca27.md): 此前记录称 PR #12950 已更新至 `6ca068805`，检视意见已处理，UT 194/194、四机重编、ViT/chat/tool call、GSM8K 等验证均报告成功；但这些内容属于脱敏验证记录，且新 E2E 当时仍在排队。随后针对用户“修下ci”的任务仅完成只读诊断，定位到 `AscendMLADCPDecodeMetadata` 新增必填 `seq_lens_device` 后，`test_mla_cp.py` 与 `test_acl_graph.py` 的显式 fixture 未同步，另一个 job 的 artifact 为 `190 passed / 29 skipped`，外层因 custom container 中断而取消；未编辑、重跑、评论或 push，因此 CI 修复仍未完成。

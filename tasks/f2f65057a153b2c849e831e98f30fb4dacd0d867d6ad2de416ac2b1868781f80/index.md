---
blocks:
- block_id: 15f50798a490b96d7fdd9ceac2069335d2ccf6f021ab67b4c157ea7e40f87b65
  indexed: true
  sha256: c115fd3583a85ee5ebc1435b8d0747ed93e131e58998b55e4b0db3a5d2c6ab32
  source_range:
    end: 473813
    part: 0
    start: 0
  summary: 来源报告认为问题更可能出在验证脚本门禁：请求使用 named tool choice，却强制要求 `finish_reason == "tool_calls"`；其引用的 vLLM v0.26 与 main 实现将 named choice 映射为 `stop`，而 `auto`/`required` 映射为 `tool_calls`。报告还提到 Kimi K3 parser、兼容层 `patch_kimi_k3_parsers.py` 及相关提交，并建议 named case 精确校验 `stop`；但该结论属于待检验的参考判断，不应视为已验证成功。
  title: named tool choice 的 finish_reason 校验争议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f2f65057a153b2c849e831e98f30fb4dacd0d867d6ad2de416ac2b1868781f80
  kind: experience
  material_digest: e949a98c3f43d3adcc6eddadda7184934ebd0e3bfbadd0be9a050123dcde1e44
  revision: a7116d3d4c0db49703033c3095c6ca4506e49c0e78cccbf8cb41537ab1304255
  schema: mindie-entry/3
  summary: 本批次包含一个关于验证脚本、vLLM serving 契约、Kimi K3 parser 兼容层及 `finish_reason`（`stop`/`tool_calls`）判断的参考索引。保留来源中的文件/版本/提交线索、诊断结论与修改建议；该结论仍是可检索的非验证性记录，未包含后续修正或完成状态。
  title: 当前任务导航：vLLM/Kimi K3 tool choice finish_reason
navigation: 本批次包含一个关于验证脚本、vLLM serving 契约、Kimi K3 parser 兼容层及 `finish_reason`（`stop`/`tool_calls`）判断的参考索引。保留来源中的文件/版本/提交线索、诊断结论与修改建议；该结论仍是可检索的非验证性记录，未包含后续修正或完成状态。
schema: mindie-material-task/1
status: complete
task_id: f2f65057a153b2c849e831e98f30fb4dacd0d867d6ad2de416ac2b1868781f80
---

# 当前任务导航：vLLM/Kimi K3 tool choice finish_reason

本批次包含一个关于验证脚本、vLLM serving 契约、Kimi K3 parser 兼容层及 `finish_reason`（`stop`/`tool_calls`）判断的参考索引。保留来源中的文件/版本/提交线索、诊断结论与修改建议；该结论仍是可检索的非验证性记录，未包含后续修正或完成状态。

## Materials

- [named tool choice 的 finish_reason 校验争议](blocks/15f50798a490b96d7fdd9ceac2069335d2ccf6f021ab67b4c157ea7e40f87b65.md): 来源报告认为问题更可能出在验证脚本门禁：请求使用 named tool choice，却强制要求 `finish_reason == "tool_calls"`；其引用的 vLLM v0.26 与 main 实现将 named choice 映射为 `stop`，而 `auto`/`required` 映射为 `tool_calls`。报告还提到 Kimi K3 parser、兼容层 `patch_kimi_k3_parsers.py` 及相关提交，并建议 named case 精确校验 `stop`；但该结论属于待检验的参考判断，不应视为已验证成功。

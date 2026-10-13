---
blocks:
- block_id: 26aedfdcea8155405d519cb3976bc5118a6fc550ff044edb377b58fd2a5addd0
  indexed: true
  sha256: 4fa2367e8d320d666153f0c7001ddf91895cd190bace626ab65f9a5aa510e0b2
  source_range:
    end: 306960
    part: 0
    start: 0
  summary: 先前助手报告四台节点（153–156）的只读监控结果均为 exit code 0，日志末行是 `full_source_retry_finish_rc=0`，并称未见 error；该内容是报告性结果，并非独立核验。报告列出生成的 `.so`、custom-op 库及 Ascend 910_93 配置文件，称每机均包含 `dequant_situ_quant.json`、`recurrent_kda.json` 等文件，且六个关键产物的 SHA256 在四机一致。报告还称全程未修改远端状态、未操作进程、未访问 NPU；未提供失败尝试或后续更正。
  title: 四机编译与产物一致性检查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1024de48c80a7238f98e630e6daf3542f5dd995e25b5b339378def29fccf0a6c
  kind: experience
  material_digest: afd108f8cd0b86384fb977438162c018c137bd1b74daf09292557f55a73b2af2
  revision: 780abbccd8392f46e009bdd18f15dea1ff29a4d9bc264d71469cbe0b969d439e
  schema: mindie-entry/3
  summary: 本批唯一材料是关于四台节点编译、custom-op 文件、配置登记和 SHA256 一致性的只读报告；其中成功与一致性结论均应视为来源方陈述，尚未独立验证。原文仍可按节点编号、`full_source_retry_finish_rc=0`、产物文件名、`dequant_situ_quant.json`、`recurrent_kda.json` 和 SHA256 关键词检索。
  title: 当前任务导航：四机编译产物报告
navigation: 本批唯一材料是关于四台节点编译、custom-op 文件、配置登记和 SHA256 一致性的只读报告；其中成功与一致性结论均应视为来源方陈述，尚未独立验证。原文仍可按节点编号、`full_source_retry_finish_rc=0`、产物文件名、`dequant_situ_quant.json`、`recurrent_kda.json` 和 SHA256 关键词检索。
schema: mindie-material-task/1
status: complete
task_id: 1024de48c80a7238f98e630e6daf3542f5dd995e25b5b339378def29fccf0a6c
---

# 当前任务导航：四机编译产物报告

本批唯一材料是关于四台节点编译、custom-op 文件、配置登记和 SHA256 一致性的只读报告；其中成功与一致性结论均应视为来源方陈述，尚未独立验证。原文仍可按节点编号、`full_source_retry_finish_rc=0`、产物文件名、`dequant_situ_quant.json`、`recurrent_kda.json` 和 SHA256 关键词检索。

## Materials

- [四机编译与产物一致性检查记录](blocks/26aedfdcea8155405d519cb3976bc5118a6fc550ff044edb377b58fd2a5addd0.md): 先前助手报告四台节点（153–156）的只读监控结果均为 exit code 0，日志末行是 `full_source_retry_finish_rc=0`，并称未见 error；该内容是报告性结果，并非独立核验。报告列出生成的 `.so`、custom-op 库及 Ascend 910_93 配置文件，称每机均包含 `dequant_situ_quant.json`、`recurrent_kda.json` 等文件，且六个关键产物的 SHA256 在四机一致。报告还称全程未修改远端状态、未操作进程、未访问 NPU；未提供失败尝试或后续更正。

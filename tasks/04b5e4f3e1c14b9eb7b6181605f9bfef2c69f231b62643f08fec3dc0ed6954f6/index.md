---
blocks:
- block_id: e4d6728b9b8c56e99a1d96d96bd73548343b98d9588db514fef086f429cc3cb7
  indexed: true
  sha256: aece92e30f0ab3b3e975cb6501daadff7ca31d3c7d53a9883978a9390e72ce1f
  source_range:
    end: 379825
    part: 0
    start: 0
  summary: 本块记录在本地 `remote-dev/flashcomm1-validation` 中新增 `compare_column_inputs.py` 和 `probe_case.py` 的结果，未连接或操作远端。比较工具会拼接 on 侧 TP rank、裁剪尾部 padding，并报告 shape、dtype、`torch.equal`、`allclose`、误差、cosine、padding 行数及首个不一致；on 侧按 off 的 active token 行数裁剪，trailing dimensions 不一致会判错。
  title: 新增输入比较与探测工具
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 04b5e4f3e1c14b9eb7b6181605f9bfef2c69f231b62643f08fec3dc0ed6954f6
  kind: experience
  material_digest: 4ac306554d482b31cdaf9440450303017c70169bbfa769b2521e9bcdc67b89bc
  revision: 64225f223d58515e91a690654346e6259faad834bc047adbc7a2512104964311
  schema: mindie-entry/3
  summary: 当前批次包含一个本地验证工具块：新增请求探测与列输入比较脚本，支持 text/image endpoint 探测及 OFF/ON 张量比较。需保留其未操作远端的限定，以及 padding 裁剪、shape 校验和数值差异报告等方法细节；本块未宣称远端验证成功。
  title: 当前任务导航
navigation: 当前批次包含一个本地验证工具块：新增请求探测与列输入比较脚本，支持 text/image endpoint 探测及 OFF/ON 张量比较。需保留其未操作远端的限定，以及 padding 裁剪、shape 校验和数值差异报告等方法细节；本块未宣称远端验证成功。
schema: mindie-material-task/1
status: complete
task_id: 04b5e4f3e1c14b9eb7b6181605f9bfef2c69f231b62643f08fec3dc0ed6954f6
---

# 当前任务导航

当前批次包含一个本地验证工具块：新增请求探测与列输入比较脚本，支持 text/image endpoint 探测及 OFF/ON 张量比较。需保留其未操作远端的限定，以及 padding 裁剪、shape 校验和数值差异报告等方法细节；本块未宣称远端验证成功。

## Materials

- [新增输入比较与探测工具](blocks/e4d6728b9b8c56e99a1d96d96bd73548343b98d9588db514fef086f429cc3cb7.md): 本块记录在本地 `remote-dev/flashcomm1-validation` 中新增 `compare_column_inputs.py` 和 `probe_case.py` 的结果，未连接或操作远端。比较工具会拼接 on 侧 TP rank、裁剪尾部 padding，并报告 shape、dtype、`torch.equal`、`allclose`、误差、cosine、padding 行数及首个不一致；on 侧按 off 的 active token 行数裁剪，trailing dimensions 不一致会判错。

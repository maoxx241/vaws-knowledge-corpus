---
blocks:
- block_id: ee0223c00a9781e5afb7ef06d2871aef098a8541c014f19596992522f488bac7
  indexed: true
  sha256: b1d09b154ebee8fb8e18b8079972da9481dbf476491b9622dc51467349f9b10b
  source_range:
    end: 741017
    part: 0
    start: 0
  summary: 只读诊断显示，FlashComm on/off 的 text（299 tokens）与 image（262 tokens）prompt IDs 完全一致，各自重复运行也确定性一致，但两类输入均从生成 token 0 开始分歧：off 为 `139512 (_syn)`，on 为 `12826 (oga)`。首步 top-logprobs 中 off 首 token 在 on 结果中均排第二，差值分别为 text `0.5625`、image `0.0625`；由于首 token 来自 prefill，当前证据不支持将原因归于 `FULL_DECODE_ONLY` ACLGraph decode replay。采集期间 health 为 200、ACLGraph capture 为 1/1，日志未见 Worker failed、RuntimeError 或 AclNN 报错；编译图提示差异位于 FlashComm RS/AG 语义链路（off 为 GEMM+all-reduce，on 为 `matmul_and_reduce` 后保持 token shard），MLA gate gather 顺序修复及重测仍未完成。
  title: FlashComm on/off 首 token 分歧诊断
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e302ccab14bc778f2b296b39c566cfc2e42752161fb4fba434742582641531f0
  kind: experience
  material_digest: 2ca8500c49329c67363c75b72c651f4da9d9357cfcaf2ae34ebdba7d4d0dd0eb
  revision: c948a017a020413f3f563e8904a9f97c1ea9e44b6f65b1bacc0dbf0c035e96ea
  schema: mindie-entry/3
  summary: 已定位到 FlashComm on/off 在 prefill 首个生成 token 的确定性差异，prompt IDs 与重复运行结果一致；现有检查未发现服务或 ACLGraph 错误，且尚不能证明 decode replay 是原因。重点线索是 RS/AG 语义链路及 MLA gate gather 顺序，修复后的重测仍待完成；原始块可独立检索，以上诊断仍属 fallible reference index，不能视为已验证修复成功。
  title: 当前任务导航：FlashComm 首步输出差异
navigation: 已定位到 FlashComm on/off 在 prefill 首个生成 token 的确定性差异，prompt IDs 与重复运行结果一致；现有检查未发现服务或 ACLGraph 错误，且尚不能证明 decode replay 是原因。重点线索是 RS/AG 语义链路及 MLA gate gather 顺序，修复后的重测仍待完成；原始块可独立检索，以上诊断仍属 fallible reference index，不能视为已验证修复成功。
schema: mindie-material-task/1
status: complete
task_id: e302ccab14bc778f2b296b39c566cfc2e42752161fb4fba434742582641531f0
---

# 当前任务导航：FlashComm 首步输出差异

已定位到 FlashComm on/off 在 prefill 首个生成 token 的确定性差异，prompt IDs 与重复运行结果一致；现有检查未发现服务或 ACLGraph 错误，且尚不能证明 decode replay 是原因。重点线索是 RS/AG 语义链路及 MLA gate gather 顺序，修复后的重测仍待完成；原始块可独立检索，以上诊断仍属 fallible reference index，不能视为已验证修复成功。

## Materials

- [FlashComm on/off 首 token 分歧诊断](blocks/ee0223c00a9781e5afb7ef06d2871aef098a8541c014f19596992522f488bac7.md): 只读诊断显示，FlashComm on/off 的 text（299 tokens）与 image（262 tokens）prompt IDs 完全一致，各自重复运行也确定性一致，但两类输入均从生成 token 0 开始分歧：off 为 `139512 (_syn)`，on 为 `12826 (oga)`。首步 top-logprobs 中 off 首 token 在 on 结果中均排第二，差值分别为 text `0.5625`、image `0.0625`；由于首 token 来自 prefill，当前证据不支持将原因归于 `FULL_DECODE_ONLY` ACLGraph decode replay。采集期间 health 为 200、ACLGraph capture 为 1/1，日志未见 Worker failed、RuntimeError 或 AclNN 报错；编译图提示差异位于 FlashComm RS/AG 语义链路（off 为 GEMM+all-reduce，on 为 `matmul_and_reduce` 后保持 token shard），MLA gate gather 顺序修复及重测仍未完成。

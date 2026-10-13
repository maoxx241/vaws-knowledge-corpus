---
blocks:
- block_id: d94a0ab1f2ef499009ef41fe2f526d6eadbede4c701e802134cb170d0d377011
  indexed: true
  sha256: bd6b8e8c3e9af8d79019b11eada7b4a98c4cc7883360e2b4b90c5c02426eee16
  source_range:
    end: 857930
    part: 0
    start: 0
  summary: 记录称算子无需重新编译，并计划沿用 MRV1、gamma=7、draft eager、prefix off 及 shared-expert DP 关闭配置；同时指出需替换旧 fe784ff core、258ce514 Ascend 和 r5 runtime 硬编码。随后生成了仅含 DP4 服务、preflight 与评测入口的本地包，加入 GSM8K metrics 模式及 target-only 关闭 speculative metrics 的选项；这些内容来自助手报告，未在本块中独立验证。记录还报告四机 staging 副本通过哈希、权限、bash -n 和无缓存 Python 编译检查并同步到已脱敏路径，但明确未运行完整 preflight、未启动服务、未操作进程或编译算子。
  title: r5 DP4 运行包与四机同步记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bf9c54e154d13a1e8152cfae7d9e48b0f6a4808b3e595ca2f519a7462f21873f
  kind: experience
  material_digest: 6b34180b598e476ad502783361d98836b3db854453470ca1e411d52c9ecd032e
  revision: 2a24f30b8605dee5a486a2e1eb8cfc3edd4049b65e7d8228434759bdf8b522bd
  schema: mindie-entry/3
  summary: 重点检索 r5 启动契约、PR12950/d02 wrapper、旧路径硬编码清理、四机同步与 preflight 状态。需保留报告中的哈希和检查结论为未独立验证的记录，并注意完整 preflight、服务启动及算子编译均未执行。
  title: 当前任务导航：r5 DP4 运行包
navigation: 重点检索 r5 启动契约、PR12950/d02 wrapper、旧路径硬编码清理、四机同步与 preflight 状态。需保留报告中的哈希和检查结论为未独立验证的记录，并注意完整 preflight、服务启动及算子编译均未执行。
schema: mindie-material-task/1
status: complete
task_id: bf9c54e154d13a1e8152cfae7d9e48b0f6a4808b3e595ca2f519a7462f21873f
---

# 当前任务导航：r5 DP4 运行包

重点检索 r5 启动契约、PR12950/d02 wrapper、旧路径硬编码清理、四机同步与 preflight 状态。需保留报告中的哈希和检查结论为未独立验证的记录，并注意完整 preflight、服务启动及算子编译均未执行。

## Materials

- [r5 DP4 运行包与四机同步记录](blocks/d94a0ab1f2ef499009ef41fe2f526d6eadbede4c701e802134cb170d0d377011.md): 记录称算子无需重新编译，并计划沿用 MRV1、gamma=7、draft eager、prefix off 及 shared-expert DP 关闭配置；同时指出需替换旧 fe784ff core、258ce514 Ascend 和 r5 runtime 硬编码。随后生成了仅含 DP4 服务、preflight 与评测入口的本地包，加入 GSM8K metrics 模式及 target-only 关闭 speculative metrics 的选项；这些内容来自助手报告，未在本块中独立验证。记录还报告四机 staging 副本通过哈希、权限、bash -n 和无缓存 Python 编译检查并同步到已脱敏路径，但明确未运行完整 preflight、未启动服务、未操作进程或编译算子。

---
blocks:
- block_id: 2a96d734c6f843370dd7447909dd1f33d55bbe9baf15fd1d92ea22fcb6a7fa34
  indexed: true
  sha256: ef00bde65ece264c7730a372bc879a6981fba74be9232785d8f6a5489c57c562
  source_range:
    end: 468987
    part: 0
    start: 0
  summary: '记录显示，`KdaGateCumsum` 等算子的 `.o` 和单算子 JSON 虽存在，但旧 `csrc/build` 导致聚合索引 `binary_info_config.json` 未更新，运行时因此报 `Parse dynamic kernel config fail` 和 `does not has any binary`。通过 TP8/TP16、H=6/12 的最小直调排除 shape、PD metadata 和 PR #9，随后干净重编后 manifest 补齐、单卡及真实 PD 请求通过；该成功结论属于记录中的报告，仍应保留其验证范围。规避建议包括源码/工具链 fingerprint、切换分支或修改 AscendC/CMake 时清理构建缓存、package gate 校验 manifest 与 binary，以及启动前 smoke test。'
  title: KDA 算子编译与陈旧 manifest 定位
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d5348dbbd1483d65cfc1501ccd4eeaa2df32f451250abcccab10dfa1eb786f48
  kind: experience
  material_digest: eeffbf44cff6d1cff4793094af64e5de396f358e34a4573563b970f349293e74
  revision: 114d2a42cf0a9a2a4fcbbdb78ccde2b5d73e6c017c81933ac9c0e9b6e9d8fe2f
  schema: mindie-entry/3
  summary: 已收录一条关于 AscendC/KDA 算子编译故障的定位记录：重点检索旧 `csrc/build`、`binary_info_config.json`、`KdaGateCumsum`/`ChunkKdaFwd`/`KdaLayoutSwap12`、动态 kernel 配置错误、最小直调排除项和干净重编结果。记录保留了“文件存在但 manifest 缺项”的关键矛盾、构建脚本行为、报告中的通过结果及缓存 fingerprint、manifest 校验和 smoke test 方案；原始记录仍需独立检索，未收录的更早上下文不作记忆声明。
  title: 当前任务导航：KDA 编译问题定位与规避
navigation: 已收录一条关于 AscendC/KDA 算子编译故障的定位记录：重点检索旧 `csrc/build`、`binary_info_config.json`、`KdaGateCumsum`/`ChunkKdaFwd`/`KdaLayoutSwap12`、动态 kernel 配置错误、最小直调排除项和干净重编结果。记录保留了“文件存在但 manifest 缺项”的关键矛盾、构建脚本行为、报告中的通过结果及缓存 fingerprint、manifest 校验和 smoke test 方案；原始记录仍需独立检索，未收录的更早上下文不作记忆声明。
schema: mindie-material-task/1
status: complete
task_id: d5348dbbd1483d65cfc1501ccd4eeaa2df32f451250abcccab10dfa1eb786f48
---

# 当前任务导航：KDA 编译问题定位与规避

已收录一条关于 AscendC/KDA 算子编译故障的定位记录：重点检索旧 `csrc/build`、`binary_info_config.json`、`KdaGateCumsum`/`ChunkKdaFwd`/`KdaLayoutSwap12`、动态 kernel 配置错误、最小直调排除项和干净重编结果。记录保留了“文件存在但 manifest 缺项”的关键矛盾、构建脚本行为、报告中的通过结果及缓存 fingerprint、manifest 校验和 smoke test 方案；原始记录仍需独立检索，未收录的更早上下文不作记忆声明。

## Materials

- [KDA 算子编译与陈旧 manifest 定位](blocks/2a96d734c6f843370dd7447909dd1f33d55bbe9baf15fd1d92ea22fcb6a7fa34.md): 记录显示，`KdaGateCumsum` 等算子的 `.o` 和单算子 JSON 虽存在，但旧 `csrc/build` 导致聚合索引 `binary_info_config.json` 未更新，运行时因此报 `Parse dynamic kernel config fail` 和 `does not has any binary`。通过 TP8/TP16、H=6/12 的最小直调排除 shape、PD metadata 和 PR #9，随后干净重编后 manifest 补齐、单卡及真实 PD 请求通过；该成功结论属于记录中的报告，仍应保留其验证范围。规避建议包括源码/工具链 fingerprint、切换分支或修改 AscendC/CMake 时清理构建缓存、package gate 校验 manifest 与 binary，以及启动前 smoke test。

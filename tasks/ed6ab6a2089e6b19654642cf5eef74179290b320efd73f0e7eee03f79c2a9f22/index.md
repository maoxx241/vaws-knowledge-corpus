---
blocks:
- block_id: 847ce11aaa35425ed7873845e6ecc04df8d6bc902f0f30903c9d4a4fd20dd9ec
  indexed: true
  sha256: db47edc429c539d1d78085cd2e3caedb7593cb56f92a9c6fc343f3ba388cef04
  source_range:
    end: 3803811
    part: 0
    start: 0
  summary: '先确认 Compressor 的 C4/C128 路径均保留；随后发现 QLI A5 的 FP8/FP4 等 dtype、模板和量化分支被误删，已通过 PR #5 恢复，9 项静态测试通过并合入。153 上使用 A3、CANN 9.1 的较新 nightly 容器完成增量构建，处理了 macOS `._*` 文件、超时残留进程/锁和 Protobuf 并行问题后退出码为 0；产物包含 SparseFlashMla 6 个 key、QLI V2 1 个、Compressor 4 个，但仅验证了 A3 编译，未进行数值测试或 A5 实机编译，CI 也仍未完成。'
  title: QLI A5 恢复与 153 编译验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ed6ab6a2089e6b19654642cf5eef74179290b320efd73f0e7eee03f79c2a9f22
  kind: experience
  material_digest: 9affb45a1f161bb230419779bf20ad334d5af4b5ac061980ee26f502b925fef5
  revision: c814a5a621a282812b0c02e149fc17198c27651de95ed8154578cdf14e754293
  schema: mindie-entry/3
  summary: '相关上下文包括 Compressor C4/C128 保留、QLI A5 误删后的恢复及 PR #5 合入。153 上 A3 增量构建成功，最后一次耗时约 9 分 13 秒、整轮约 22 分钟；A5 编译、数值验证和 CI 状态仍未完成，原始消息块可独立检索。'
  title: 当前任务导航：QLI/Compressor 裁剪修正与构建状态
navigation: '相关上下文包括 Compressor C4/C128 保留、QLI A5 误删后的恢复及 PR #5 合入。153 上 A3 增量构建成功，最后一次耗时约 9 分 13 秒、整轮约 22 分钟；A5 编译、数值验证和 CI 状态仍未完成，原始消息块可独立检索。'
schema: mindie-material-task/1
status: complete
task_id: ed6ab6a2089e6b19654642cf5eef74179290b320efd73f0e7eee03f79c2a9f22
---

# 当前任务导航：QLI/Compressor 裁剪修正与构建状态

相关上下文包括 Compressor C4/C128 保留、QLI A5 误删后的恢复及 PR #5 合入。153 上 A3 增量构建成功，最后一次耗时约 9 分 13 秒、整轮约 22 分钟；A5 编译、数值验证和 CI 状态仍未完成，原始消息块可独立检索。

## Materials

- [QLI A5 恢复与 153 编译验证](blocks/847ce11aaa35425ed7873845e6ecc04df8d6bc902f0f30903c9d4a4fd20dd9ec.md): 先确认 Compressor 的 C4/C128 路径均保留；随后发现 QLI A5 的 FP8/FP4 等 dtype、模板和量化分支被误删，已通过 PR #5 恢复，9 项静态测试通过并合入。153 上使用 A3、CANN 9.1 的较新 nightly 容器完成增量构建，处理了 macOS `._*` 文件、超时残留进程/锁和 Protobuf 并行问题后退出码为 0；产物包含 SparseFlashMla 6 个 key、QLI V2 1 个、Compressor 4 个，但仅验证了 A3 编译，未进行数值测试或 A5 实机编译，CI 也仍未完成。

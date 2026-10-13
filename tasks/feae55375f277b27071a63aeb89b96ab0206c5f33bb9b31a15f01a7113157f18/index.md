---
blocks:
- block_id: aef0f763644d5e5bef4bd2d913e243f1aade6e0a2e89e2195d208ddb74ba84f3
  indexed: true
  sha256: f84d992dc3c7226b47b77e0b17e3e85b95d6132cd80b19f8c59c60fbb1655a1f
  source_range:
    end: 1469149
    part: 0
    start: 0
  summary: 该块记录了 r13 canonical token artifact 的三组只读 probe 计划、依赖 SHA 回读、脚本 `py_compile`/`bash -n` 检查，以及仅向已脱敏的 153 环境部署客户端脚本；报告称未发送 HTTP，且 wrapper SHA 校验通过，但这些属于报告中的检查结果而非本索引验证的成功。随后对 r14 DFX 进行了只读审查：指出 global rank0 不能排除 TP1–15 的局部 cache 损坏，比较应限定同一 TP rank，`global-rank-unknown` 回退可能造成 16 进程文件冲突，并建议 rank 不可得时显式失败。典型 miss/warm/hit 预计需要 48 个 rank 唯一文件；峰值约 `72 MiB/NPU`、DP0 聚合约 `1.125 GiB`，DFX 同步可能扰动时序，且部署 SHA 清单已因并发编辑过期、需后续重算。
  title: r13 部署前置检查与 r14 DFX 审查结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: feae55375f277b27071a63aeb89b96ab0206c5f33bb9b31a15f01a7113157f18
  kind: experience
  material_digest: 77a5c9a3265804aabc7c6dfc1e4bf7f9f102f42199cda8657d0c424d5ede69b1
  revision: 1702da4a6eb1c6f84837bf91ac017af3608ed83d8ac8fbb19336f9030162849f
  schema: mindie-entry/3
  summary: 当前材料覆盖 r13 的客户端实验准备、静态依赖与语法/SHA 检查，以及 r14 对 rank 覆盖、cache 损坏判定、文件唯一性和 DFX 时序扰动的审查。已报告的“未发 HTTP”和检查通过状态仍应视为来源陈述；后续重点是重算过期 SHA 清单，并确认 token0/all! 在同步扰动下是否仍复现，同时保留 rank 缺失和未完成验证的不确定性。
  title: 任务导航：r13 前置部署与 r14 DFX rank/cache 审查
navigation: 当前材料覆盖 r13 的客户端实验准备、静态依赖与语法/SHA 检查，以及 r14 对 rank 覆盖、cache 损坏判定、文件唯一性和 DFX 时序扰动的审查。已报告的“未发 HTTP”和检查通过状态仍应视为来源陈述；后续重点是重算过期 SHA 清单，并确认 token0/all! 在同步扰动下是否仍复现，同时保留 rank 缺失和未完成验证的不确定性。
schema: mindie-material-task/1
status: complete
task_id: feae55375f277b27071a63aeb89b96ab0206c5f33bb9b31a15f01a7113157f18
---

# 任务导航：r13 前置部署与 r14 DFX rank/cache 审查

当前材料覆盖 r13 的客户端实验准备、静态依赖与语法/SHA 检查，以及 r14 对 rank 覆盖、cache 损坏判定、文件唯一性和 DFX 时序扰动的审查。已报告的“未发 HTTP”和检查通过状态仍应视为来源陈述；后续重点是重算过期 SHA 清单，并确认 token0/all! 在同步扰动下是否仍复现，同时保留 rank 缺失和未完成验证的不确定性。

## Materials

- [r13 部署前置检查与 r14 DFX 审查结论](blocks/aef0f763644d5e5bef4bd2d913e243f1aade6e0a2e89e2195d208ddb74ba84f3.md): 该块记录了 r13 canonical token artifact 的三组只读 probe 计划、依赖 SHA 回读、脚本 `py_compile`/`bash -n` 检查，以及仅向已脱敏的 153 环境部署客户端脚本；报告称未发送 HTTP，且 wrapper SHA 校验通过，但这些属于报告中的检查结果而非本索引验证的成功。随后对 r14 DFX 进行了只读审查：指出 global rank0 不能排除 TP1–15 的局部 cache 损坏，比较应限定同一 TP rank，`global-rank-unknown` 回退可能造成 16 进程文件冲突，并建议 rank 不可得时显式失败。典型 miss/warm/hit 预计需要 48 个 rank 唯一文件；峰值约 `72 MiB/NPU`、DP0 聚合约 `1.125 GiB`，DFX 同步可能扰动时序，且部署 SHA 清单已因并发编辑过期、需后续重算。

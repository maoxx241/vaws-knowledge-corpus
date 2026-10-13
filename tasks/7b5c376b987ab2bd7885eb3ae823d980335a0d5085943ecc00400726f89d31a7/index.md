---
blocks:
- block_id: 7e2d8de5a85bf08b8fe4e787819bc8ad23dee789ed0f642e1778ef4ae3949167
  indexed: true
  sha256: 56be9c9a9697ccdb800aee6c11bd6247ef56c1af016aba54a51128b5056782c8
  source_range:
    end: 1314205
    part: 0
    start: 0
  summary: '记录了从 dirty worktree、组合快照和 PR #13277 远端 SHA 中拆分四平面实现的过程；明确排除 Triton KDA、Zeroer、async、DFX 及旧 GQA 方案，并保留未 push、未部署和本机缺少 torch/vLLM 运行时等限制。候选提交先后经历 blob/patch 校验、ruff 阻断及格式修复，最终提交 `842b030f8375e630eb639e0560eac7735d04f700`，但这些结果仍属于本地候选状态而非远端或生产验证。后续修复了 helper 的结构误命中：仅完整匹配 Ascend MLA、align Mamba、padded GQA 且无 leftover 时命中，否则返回 `None`；新增负向 UT 报告为通过，但未执行 NPU 或真实服务验证，远端 lease 仍为 `419c315c0aae2bef7c3da56c42f8a7916fcb09db`。'
  title: 四平面候选实现的审计、提交与窄修复记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7b5c376b987ab2bd7885eb3ae823d980335a0d5085943ecc00400726f89d31a7
  kind: experience
  material_digest: 39570a92f25d196643ca4899c4f6305284101d1254f58a27b535ef2332e3f6a1
  revision: 748b8114142e2187d38f748deb54f6c51225c10db39a2532992bd198559b38cc
  schema: mindie-entry/3
  summary: 主线围绕 vLLM-Ascend 四平面 SoA 实现的来源隔离、5 文件 allowlist、提交历史、ruff/AST/pycompile 及无 NPU CPU UT 检查展开；关键更正是 helper 结构误命中已通过窄修复和 3 个负向用例覆盖。当前最新本地候选提交为 `842b030f8375e630eb639e0560eac7735d04f700`，仍未 push、未部署，且真实 NPU/torch/vLLM 运行时结果不可得；早期提交、组合快照和旧 PR head 不应被视为当前权威实现。
  title: 当前任务导航：四平面候选实现与 PR 发布前审计
navigation: 主线围绕 vLLM-Ascend 四平面 SoA 实现的来源隔离、5 文件 allowlist、提交历史、ruff/AST/pycompile 及无 NPU CPU UT 检查展开；关键更正是 helper 结构误命中已通过窄修复和 3 个负向用例覆盖。当前最新本地候选提交为 `842b030f8375e630eb639e0560eac7735d04f700`，仍未 push、未部署，且真实 NPU/torch/vLLM 运行时结果不可得；早期提交、组合快照和旧 PR head 不应被视为当前权威实现。
schema: mindie-material-task/1
status: complete
task_id: 7b5c376b987ab2bd7885eb3ae823d980335a0d5085943ecc00400726f89d31a7
---

# 当前任务导航：四平面候选实现与 PR 发布前审计

主线围绕 vLLM-Ascend 四平面 SoA 实现的来源隔离、5 文件 allowlist、提交历史、ruff/AST/pycompile 及无 NPU CPU UT 检查展开；关键更正是 helper 结构误命中已通过窄修复和 3 个负向用例覆盖。当前最新本地候选提交为 `842b030f8375e630eb639e0560eac7735d04f700`，仍未 push、未部署，且真实 NPU/torch/vLLM 运行时结果不可得；早期提交、组合快照和旧 PR head 不应被视为当前权威实现。

## Materials

- [四平面候选实现的审计、提交与窄修复记录](blocks/7e2d8de5a85bf08b8fe4e787819bc8ad23dee789ed0f642e1778ef4ae3949167.md): 记录了从 dirty worktree、组合快照和 PR #13277 远端 SHA 中拆分四平面实现的过程；明确排除 Triton KDA、Zeroer、async、DFX 及旧 GQA 方案，并保留未 push、未部署和本机缺少 torch/vLLM 运行时等限制。候选提交先后经历 blob/patch 校验、ruff 阻断及格式修复，最终提交 `842b030f8375e630eb639e0560eac7735d04f700`，但这些结果仍属于本地候选状态而非远端或生产验证。后续修复了 helper 的结构误命中：仅完整匹配 Ascend MLA、align Mamba、padded GQA 且无 leftover 时命中，否则返回 `None`；新增负向 UT 报告为通过，但未执行 NPU 或真实服务验证，远端 lease 仍为 `419c315c0aae2bef7c3da56c42f8a7916fcb09db`。

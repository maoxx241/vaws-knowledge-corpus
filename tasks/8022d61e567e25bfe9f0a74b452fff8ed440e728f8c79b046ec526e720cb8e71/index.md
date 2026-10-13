---
blocks:
- block_id: dd670b6cd4790d31105dcd5406d8ef79b00798a5524f05072de62b2b131e59a8
  indexed: true
  sha256: 6b80bcf6c35f85b290379295372b5dc57f9b03b5ba50e97121169ee683e44aba
  source_range:
    end: 383919
    part: 0
    start: 0
  summary: 本地检查报告称仅 9 个预期文件改动，git diff --check、Ruff lint、Ruff format 和 9 文件 AST 解析通过；但这些属于报告中的检查结果，不能视为完整验证。由于本机缺少 torch，pytest 在收集阶段被环境阻塞，UT 仍需在 vLLM/CANN 容器补跑；多模态首层 token padding 和 RowParallel reduce-scatter 的真实运行行为仍是不确定风险，需四机验证。
  title: 本地提交门禁与测试阻塞
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8022d61e567e25bfe9f0a74b452fff8ed440e728f8c79b046ec526e720cb8e71
  kind: experience
  material_digest: f19864470e0015cf0c2937617415bf038c9e9869c5aae5ee123a69f9f67ba2b3
  revision: 835a228e5cdd46d7bf0742a30f2696c36384a370e1ff265d98cfcfceffe0ed27
  schema: mindie-entry/3
  summary: 当前重点是保留本地静态检查结果及其范围，同时明确 pytest 因缺少 torch 未完成、并非测试失败。后续应在 vLLM/CANN 容器补跑 UT，并用四机验证多模态首层 token padding 与 RowParallel reduce-scatter；提交时还需遵循 Conventional Commit 并使用 -s。
  title: 提交前门禁、测试环境与分布式运行风险
navigation: 当前重点是保留本地静态检查结果及其范围，同时明确 pytest 因缺少 torch 未完成、并非测试失败。后续应在 vLLM/CANN 容器补跑 UT，并用四机验证多模态首层 token padding 与 RowParallel reduce-scatter；提交时还需遵循 Conventional Commit 并使用 -s。
schema: mindie-material-task/1
status: complete
task_id: 8022d61e567e25bfe9f0a74b452fff8ed440e728f8c79b046ec526e720cb8e71
---

# 提交前门禁、测试环境与分布式运行风险

当前重点是保留本地静态检查结果及其范围，同时明确 pytest 因缺少 torch 未完成、并非测试失败。后续应在 vLLM/CANN 容器补跑 UT，并用四机验证多模态首层 token padding 与 RowParallel reduce-scatter；提交时还需遵循 Conventional Commit 并使用 -s。

## Materials

- [本地提交门禁与测试阻塞](blocks/dd670b6cd4790d31105dcd5406d8ef79b00798a5524f05072de62b2b131e59a8.md): 本地检查报告称仅 9 个预期文件改动，git diff --check、Ruff lint、Ruff format 和 9 文件 AST 解析通过；但这些属于报告中的检查结果，不能视为完整验证。由于本机缺少 torch，pytest 在收集阶段被环境阻塞，UT 仍需在 vLLM/CANN 容器补跑；多模态首层 token padding 和 RowParallel reduce-scatter 的真实运行行为仍是不确定风险，需四机验证。

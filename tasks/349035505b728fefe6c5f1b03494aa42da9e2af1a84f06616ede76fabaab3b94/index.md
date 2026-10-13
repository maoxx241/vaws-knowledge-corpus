---
blocks:
- block_id: 99c4b4f71c858b966290cead97e82fe3d57fa059d747707dfebece4a9f17020a
  indexed: true
  sha256: 0a0f457c6c5baedb7aca0490b06bd60566aa8e9aa874c13c2ff3068a605625e7
  source_range:
    end: 225957
    part: 0
    start: 0
  summary: 一次检索报告称，nightly 配置使用 `mmmu_val`，未找到 `mmmu_pro` 或 `mmupro` 条目；该结论基于报告中的仓库检查，不能视为已验证的完整仓库事实。报告将相关配置描述为 A2 nightly accuracy matrix，列出 Qwen3-VL、Qwen3-Omni 和 InternVL3.5 模型及 `acc,none` 基线，并指出 InternVL3.5-8B 位于 `pr_only` accuracy group。所述执行链路涉及 `a2.accuracy.nightly`、`_e2e_nightly_single_node_models.yaml`、`tests/e2e/models/test_lm_eval_correctness.py`、`lm_eval.simple_evaluate` 和 5% 相对误差校验；原始路径均为已脱敏占位符，未保留失败尝试或后续更正记录。
  title: vLLM-Ascend nightly 中 MMMU 数据集配置索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 349035505b728fefe6c5f1b03494aa42da9e2af1a84f06616ede76fabaab3b94
  kind: experience
  material_digest: 33ebb64ce745f506d5d5d83e5ddcec891a43ee286d1c417377e154041b17ec7b
  revision: 96fb8015c3a578de7d39433b70f749d4a95631d4a507a7d642c53a2b1cf00cdb
  schema: mindie-entry/3
  summary: 当前仅有一个可检索材料块，主题是 vLLM-Ascend nightly 用例中的 MMMU 数据集配置及执行链路。索引保留了“使用 `mmmu_val`、未发现 `mmmu_pro`/`mmupro`”这一报告性结论及其不确定性；没有其他早期上下文、失败尝试、未完成结果或后续更正可供补充。
  title: 当前任务导航：nightly MMMU 配置检索
navigation: 当前仅有一个可检索材料块，主题是 vLLM-Ascend nightly 用例中的 MMMU 数据集配置及执行链路。索引保留了“使用 `mmmu_val`、未发现 `mmmu_pro`/`mmupro`”这一报告性结论及其不确定性；没有其他早期上下文、失败尝试、未完成结果或后续更正可供补充。
schema: mindie-material-task/1
status: complete
task_id: 349035505b728fefe6c5f1b03494aa42da9e2af1a84f06616ede76fabaab3b94
---

# 当前任务导航：nightly MMMU 配置检索

当前仅有一个可检索材料块，主题是 vLLM-Ascend nightly 用例中的 MMMU 数据集配置及执行链路。索引保留了“使用 `mmmu_val`、未发现 `mmmu_pro`/`mmupro`”这一报告性结论及其不确定性；没有其他早期上下文、失败尝试、未完成结果或后续更正可供补充。

## Materials

- [vLLM-Ascend nightly 中 MMMU 数据集配置索引](blocks/99c4b4f71c858b966290cead97e82fe3d57fa059d747707dfebece4a9f17020a.md): 一次检索报告称，nightly 配置使用 `mmmu_val`，未找到 `mmmu_pro` 或 `mmupro` 条目；该结论基于报告中的仓库检查，不能视为已验证的完整仓库事实。报告将相关配置描述为 A2 nightly accuracy matrix，列出 Qwen3-VL、Qwen3-Omni 和 InternVL3.5 模型及 `acc,none` 基线，并指出 InternVL3.5-8B 位于 `pr_only` accuracy group。所述执行链路涉及 `a2.accuracy.nightly`、`_e2e_nightly_single_node_models.yaml`、`tests/e2e/models/test_lm_eval_correctness.py`、`lm_eval.simple_evaluate` 和 5% 相对误差校验；原始路径均为已脱敏占位符，未保留失败尝试或后续更正记录。

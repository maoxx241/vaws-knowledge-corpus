---
blocks:
- block_id: 1d480cea4f22d0bd4d027cba125f7bbd5976036495bc772bb3b3f3d18535100a
  indexed: true
  sha256: 0eba028bb7e3484bcba6737b715d5c994a5c4a768c2cc5f2ad242c2c94f86d3c
  source_range:
    end: 633088
    part: 0
    start: 0
  summary: 完成 `ops/mla.py` 与 `attention/mla_v1.py` 的 typed feature contract 重构，涵盖 `AscendMLAFeatures`/`AscendMLAModules`、output gate、RoPE 语义，以及 no-RoPE 与 FA quant/MLAPO 组合的显式 `NotImplementedError` 拒绝路径，并报告了相应 UT 覆盖。`compileall`、`ruff check`、`ruff format --check` 和 `git diff --check` 据报告通过，但 UT 未实际执行：本机缺少 `torch`，pytest 在导入 `tests/ut/conftest.py` 时失败。仍有未完成项：`AscendMLAMetadataBuilder` 保留 `getattr(hf_text_config, "mla_use_rope", True)`，建议后续改为明确类型分支；该建议不是已验证成功。
  title: MLA typed contract 重构及验证限制
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 46243beed36192cbcdd931f6ab7e812983941d280b4d0c7088bcfb0d0832ccb7
  kind: experience
  material_digest: 32c0f24c043629ffe974285e7ff0ec0df188dd3a857296ff1653ca5fc1eddbd2
  revision: b1f9580594d4af29c389419f5a80320aa7c3c7a45ea5ff13709216e4692b404d
  schema: mindie-entry/3
  summary: 本批仅包含一个关于 MLA typed contract 重构的材料块。可检索已报告的实现改动、验证命令结果、因缺少 `torch` 导致的 UT 导入失败，以及仍待处理的 `AscendMLAMetadataBuilder` 配置读取问题；此前导航内容未提供，不能据此补充或声称记忆更早细节。
  title: 当前任务导航：MLA typed contract 重构
navigation: 本批仅包含一个关于 MLA typed contract 重构的材料块。可检索已报告的实现改动、验证命令结果、因缺少 `torch` 导致的 UT 导入失败，以及仍待处理的 `AscendMLAMetadataBuilder` 配置读取问题；此前导航内容未提供，不能据此补充或声称记忆更早细节。
schema: mindie-material-task/1
status: complete
task_id: 46243beed36192cbcdd931f6ab7e812983941d280b4d0c7088bcfb0d0832ccb7
---

# 当前任务导航：MLA typed contract 重构

本批仅包含一个关于 MLA typed contract 重构的材料块。可检索已报告的实现改动、验证命令结果、因缺少 `torch` 导致的 UT 导入失败，以及仍待处理的 `AscendMLAMetadataBuilder` 配置读取问题；此前导航内容未提供，不能据此补充或声称记忆更早细节。

## Materials

- [MLA typed contract 重构及验证限制](blocks/1d480cea4f22d0bd4d027cba125f7bbd5976036495bc772bb3b3f3d18535100a.md): 完成 `ops/mla.py` 与 `attention/mla_v1.py` 的 typed feature contract 重构，涵盖 `AscendMLAFeatures`/`AscendMLAModules`、output gate、RoPE 语义，以及 no-RoPE 与 FA quant/MLAPO 组合的显式 `NotImplementedError` 拒绝路径，并报告了相应 UT 覆盖。`compileall`、`ruff check`、`ruff format --check` 和 `git diff --check` 据报告通过，但 UT 未实际执行：本机缺少 `torch`，pytest 在导入 `tests/ut/conftest.py` 时失败。仍有未完成项：`AscendMLAMetadataBuilder` 保留 `getattr(hf_text_config, "mla_use_rope", True)`，建议后续改为明确类型分支；该建议不是已验证成功。

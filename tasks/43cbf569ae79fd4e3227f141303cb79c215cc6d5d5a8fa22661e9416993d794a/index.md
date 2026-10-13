---
blocks:
- block_id: ada55767c95bca70c04b1bebfd4320a473fdd79866c0b7ac6bad6c15286aca78
  indexed: true
  sha256: 6070a8b90a2f625b82c3b1136298ba5064df3cdd7e31fbb71fff8317aee5922b
  source_range:
    end: 2165525
    part: 0
    start: 0
  summary: 据报告，KDA 已改为直接继承 `GatedDeltaNetAttention`，显式构造完整 K3 投影和状态，删除低秩 `g_a_proj/g_b_proj` 临时构造及 ModelSlim 特判，并更新 KDA 组网 UT；未修改 C++、serving、MLA 或 SiTU 文件。Ruff、格式检查、`py_compile` 和 `git diff --check` 据称通过，`csrc` 差异为零；但当前环境缺少 torch/pytest，因此 pytest 未执行，不能将完整测试视为已验证成功。
  title: KDA 去补丁化与验证状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 43cbf569ae79fd4e3227f141303cb79c215cc6d5d5a8fa22661e9416993d794a
  kind: experience
  material_digest: d4ce8b455517a07874f4ef68e4e95bb4107e33b5b56e8bbbf9cac21287115884
  revision: 19c4b186508196e7527919034e35edc982b4c7f8a4d4a3b7870ed95b7e89f3f4
  schema: mindie-entry/3
  summary: 新材料仅包含一项关于 KDA 去补丁化的报告，涵盖实现变更、未修改范围及静态检查结果。需注意：pytest 因环境缺少 torch/pytest 未执行，相关结果仍是不完整的；原始材料保留为可独立检索的参考。
  title: 当前任务导航：KDA 去补丁化索引
navigation: 新材料仅包含一项关于 KDA 去补丁化的报告，涵盖实现变更、未修改范围及静态检查结果。需注意：pytest 因环境缺少 torch/pytest 未执行，相关结果仍是不完整的；原始材料保留为可独立检索的参考。
schema: mindie-material-task/1
status: complete
task_id: 43cbf569ae79fd4e3227f141303cb79c215cc6d5d5a8fa22661e9416993d794a
---

# 当前任务导航：KDA 去补丁化索引

新材料仅包含一项关于 KDA 去补丁化的报告，涵盖实现变更、未修改范围及静态检查结果。需注意：pytest 因环境缺少 torch/pytest 未执行，相关结果仍是不完整的；原始材料保留为可独立检索的参考。

## Materials

- [KDA 去补丁化与验证状态](blocks/ada55767c95bca70c04b1bebfd4320a473fdd79866c0b7ac6bad6c15286aca78.md): 据报告，KDA 已改为直接继承 `GatedDeltaNetAttention`，显式构造完整 K3 投影和状态，删除低秩 `g_a_proj/g_b_proj` 临时构造及 ModelSlim 特判，并更新 KDA 组网 UT；未修改 C++、serving、MLA 或 SiTU 文件。Ruff、格式检查、`py_compile` 和 `git diff --check` 据称通过，`csrc` 差异为零；但当前环境缺少 torch/pytest，因此 pytest 未执行，不能将完整测试视为已验证成功。

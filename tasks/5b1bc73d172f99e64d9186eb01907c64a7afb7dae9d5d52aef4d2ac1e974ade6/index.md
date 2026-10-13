---
blocks:
- block_id: 719be64b47766149c8d3a13c48975b83bb45a233752d4dddc4e173ebbf211c33
  indexed: true
  sha256: 7cc25b4ad2887a3c822ab8c899f8798c6a7181483cbb039ccd6ce60d9564842d
  source_range:
    end: 1361739
    part: 0
    start: 0
  summary: 只读审查报告称，KDA/GDN、SiTU/MoE、量化及 serving 注册相关改动未发现 blocking/high 问题；binding 分配维度基于张量形状和已校验的 `cu_seqlens`，Meta 增加了空维度保护并采用无符号饱和累加。报告还称 ACLNN host/kernel 目录、PR13 三个文件及 PR17 文件均为零 diff，Ruff、format、AST 解析和 `git diff --check` 通过。上述结果仍属于静态收口，未执行 NPU 编译或运行时验证，且此前安全缺口修订与全量范围核对的过程和不确定性应保留。
  title: Kimi K3 变更静态安全审查收口
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5b1bc73d172f99e64d9186eb01907c64a7afb7dae9d5d52aef4d2ac1e974ade6
  kind: experience
  material_digest: e4f8c237bff44f849508e348636b6f2c5a8e753107af1cad6d1e1af6d9c7ac06
  revision: bf6d41ae98bded40591796203e9c0951c71385e61c2dea95331c04174e75272b
  schema: mindie-entry/3
  summary: 已索引 1 个新材料块：该块记录了 Kimi K3 变更的静态安全审查、binding/Meta 边界检查、独立 PR 与 ACLNN 零 diff 核对，以及工具检查通过情况。后续检索应注意：这些是审查报告中的观察与结论，不等同于 NPU 编译或运行时验证；原始材料仍可独立搜索，且此前缺口修订、范围核对过程和未完成的运行时验证需保持可见。
  title: 当前任务导航
navigation: 已索引 1 个新材料块：该块记录了 Kimi K3 变更的静态安全审查、binding/Meta 边界检查、独立 PR 与 ACLNN 零 diff 核对，以及工具检查通过情况。后续检索应注意：这些是审查报告中的观察与结论，不等同于 NPU 编译或运行时验证；原始材料仍可独立搜索，且此前缺口修订、范围核对过程和未完成的运行时验证需保持可见。
schema: mindie-material-task/1
status: complete
task_id: 5b1bc73d172f99e64d9186eb01907c64a7afb7dae9d5d52aef4d2ac1e974ade6
---

# 当前任务导航

已索引 1 个新材料块：该块记录了 Kimi K3 变更的静态安全审查、binding/Meta 边界检查、独立 PR 与 ACLNN 零 diff 核对，以及工具检查通过情况。后续检索应注意：这些是审查报告中的观察与结论，不等同于 NPU 编译或运行时验证；原始材料仍可独立搜索，且此前缺口修订、范围核对过程和未完成的运行时验证需保持可见。

## Materials

- [Kimi K3 变更静态安全审查收口](blocks/719be64b47766149c8d3a13c48975b83bb45a233752d4dddc4e173ebbf211c33.md): 只读审查报告称，KDA/GDN、SiTU/MoE、量化及 serving 注册相关改动未发现 blocking/high 问题；binding 分配维度基于张量形状和已校验的 `cu_seqlens`，Meta 增加了空维度保护并采用无符号饱和累加。报告还称 ACLNN host/kernel 目录、PR13 三个文件及 PR17 文件均为零 diff，Ruff、format、AST 解析和 `git diff --check` 通过。上述结果仍属于静态收口，未执行 NPU 编译或运行时验证，且此前安全缺口修订与全量范围核对的过程和不确定性应保留。

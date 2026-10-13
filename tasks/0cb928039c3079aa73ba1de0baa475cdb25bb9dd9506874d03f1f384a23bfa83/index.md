---
blocks:
- block_id: 59a538f42129d84114a82ca5d7561733f5bceeb35856d81acfafd713164fb29b
  indexed: true
  sha256: dbc47063faec796c55fc0d5f3b1f63032eba6bc5d3e06f8407ee6a5b0947a026
  source_range:
    end: 1115058
    part: 0
    start: 0
  summary: 只读审查认为 setup.py 的 vllm.general_plugins 入口、Ascend platform 注册、serving plugin 加载时机及显式 tokenizer 模式链路成立，但 renderer 依赖 `--tokenizer-mode kimi_k3`，allowlist 还需包含 `ascend_kimi_k3_serving`。测试仍缺 decoder 的 KDA/MLA×dense/MoE 组网矩阵、配置注册幂等/冲突、request adapter 行为和若干 parser 边界用例；部分 nightly 文件应保留、精简或删除，其中真实权重路径测试不可复现。Ruff、格式、diff、内存编译和 entry-point AST 检查已通过，但由于环境缺少 `torch`、`torch_npu`、`vllm`、`transformers`、`pytest`，不能宣称 UT 实际运行通过；未修改文件或 C++。
  title: Kimi K3 打包注册与测试收口审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0cb928039c3079aa73ba1de0baa475cdb25bb9dd9506874d03f1f384a23bfa83
  kind: experience
  material_digest: 0f5f7743f06473a6a02bd002d581a37e6a76d8fcc12858b5a6603da9b3b9a3d6
  revision: efd47f91b3fdb9ed9601a0c1f96cd3d9b9fc1d0fddf3324b5334b4c3cd29ce20
  schema: mindie-entry/3
  summary: 当前批次记录了打包/插件注册链路基本成立，以及显式 renderer 选择和 `VLLM_PLUGINS` 配置注意事项。后续重点是补齐 decoder 组网、注册契约、adapter 行为和 parser 边界测试，并按建议处理 nightly 文件；静态检查已成功，但运行时 UT 仍因依赖缺失未验证，相关结论保持为未完成而非成功。
  title: Kimi K3 集成与测试审查导航
navigation: 当前批次记录了打包/插件注册链路基本成立，以及显式 renderer 选择和 `VLLM_PLUGINS` 配置注意事项。后续重点是补齐 decoder 组网、注册契约、adapter 行为和 parser 边界测试，并按建议处理 nightly 文件；静态检查已成功，但运行时 UT 仍因依赖缺失未验证，相关结论保持为未完成而非成功。
schema: mindie-material-task/1
status: complete
task_id: 0cb928039c3079aa73ba1de0baa475cdb25bb9dd9506874d03f1f384a23bfa83
---

# Kimi K3 集成与测试审查导航

当前批次记录了打包/插件注册链路基本成立，以及显式 renderer 选择和 `VLLM_PLUGINS` 配置注意事项。后续重点是补齐 decoder 组网、注册契约、adapter 行为和 parser 边界测试，并按建议处理 nightly 文件；静态检查已成功，但运行时 UT 仍因依赖缺失未验证，相关结论保持为未完成而非成功。

## Materials

- [Kimi K3 打包注册与测试收口审查](blocks/59a538f42129d84114a82ca5d7561733f5bceeb35856d81acfafd713164fb29b.md): 只读审查认为 setup.py 的 vllm.general_plugins 入口、Ascend platform 注册、serving plugin 加载时机及显式 tokenizer 模式链路成立，但 renderer 依赖 `--tokenizer-mode kimi_k3`，allowlist 还需包含 `ascend_kimi_k3_serving`。测试仍缺 decoder 的 KDA/MLA×dense/MoE 组网矩阵、配置注册幂等/冲突、request adapter 行为和若干 parser 边界用例；部分 nightly 文件应保留、精简或删除，其中真实权重路径测试不可复现。Ruff、格式、diff、内存编译和 entry-point AST 检查已通过，但由于环境缺少 `torch`、`torch_npu`、`vllm`、`transformers`、`pytest`，不能宣称 UT 实际运行通过；未修改文件或 C++。

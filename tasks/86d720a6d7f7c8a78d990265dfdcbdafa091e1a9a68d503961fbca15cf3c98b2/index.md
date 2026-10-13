---
blocks:
- block_id: d144056a9e085dbcbe5e8b3bbdde0822f53eed41e6641ab6740e3ae27ab21dd1
  indexed: true
  sha256: f57c508d93fedd3f9e51c60daf0e33c5a6549f40643a787b0f14ab6de3e9e730
  source_range:
    end: 7330799
    part: 0
    start: 0
  summary: 多轮只读审计覆盖视觉组网、MoE/共享专家、MLA/KDA 通信所有权、注册时序、权重加载 ABI、packed MXFP4、`rot_proj` 能力门控及 PP 缺失层处理；记录了若干已修复问题和仍建议补强的 `rot_proj` missing-weight 严格检查、输入形状与配置约束等事项。最终报告未发现 Python 侧 P0/P1 阻断，但这是源码/静态接口审计，定向 UT 因缺少 `torch` 失败，且本机缺少 `vllm`、`torch_npu`、`pytest`，不代表 NPU 运行时验证；全程未修改文件。
  title: Python 侧 K3/VLM 最终静态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 86d720a6d7f7c8a78d990265dfdcbdafa091e1a9a68d503961fbca15cf3c98b2
  kind: experience
  material_digest: 6bd11be6bf6c6c2e37c227b3802163dafc60b8d77f983338d46b425ff517221e
  revision: f00bf612403a6c42eff26b096b80a6b79b51e0214f5073621cb1d09708ed677c
  schema: mindie-entry/3
  summary: 本批材料集中记录了 K3/VLM Python 组网、标准插件注册、MoE 扩展、MLA/KDA/共享专家通信、权重加载 ABI 与 `rot_proj` 元数据门控的只读审计。此前发现的普通 Chat 生命周期、外层 VLM 错误继承 MoE、TP 提示、混合 meta/CPU buffer 等问题据报告已修复；仍保留 `rot_proj` 参数加载严格性及若干输入/配置约束建议。当前结论仅为静态源码和接口检查：AST 与 `git diff --check` 通过，但测试因环境缺少依赖而未能运行，因此未验证真实 NPU 行为。
  title: 当前任务导航：K3 Python 侧静态审计与运行时验证边界
navigation: 本批材料集中记录了 K3/VLM Python 组网、标准插件注册、MoE 扩展、MLA/KDA/共享专家通信、权重加载 ABI 与 `rot_proj` 元数据门控的只读审计。此前发现的普通 Chat 生命周期、外层 VLM 错误继承 MoE、TP 提示、混合 meta/CPU buffer 等问题据报告已修复；仍保留 `rot_proj` 参数加载严格性及若干输入/配置约束建议。当前结论仅为静态源码和接口检查：AST 与 `git diff --check` 通过，但测试因环境缺少依赖而未能运行，因此未验证真实 NPU 行为。
schema: mindie-material-task/1
status: complete
task_id: 86d720a6d7f7c8a78d990265dfdcbdafa091e1a9a68d503961fbca15cf3c98b2
---

# 当前任务导航：K3 Python 侧静态审计与运行时验证边界

本批材料集中记录了 K3/VLM Python 组网、标准插件注册、MoE 扩展、MLA/KDA/共享专家通信、权重加载 ABI 与 `rot_proj` 元数据门控的只读审计。此前发现的普通 Chat 生命周期、外层 VLM 错误继承 MoE、TP 提示、混合 meta/CPU buffer 等问题据报告已修复；仍保留 `rot_proj` 参数加载严格性及若干输入/配置约束建议。当前结论仅为静态源码和接口检查：AST 与 `git diff --check` 通过，但测试因环境缺少依赖而未能运行，因此未验证真实 NPU 行为。

## Materials

- [Python 侧 K3/VLM 最终静态审计](blocks/d144056a9e085dbcbe5e8b3bbdde0822f53eed41e6641ab6740e3ae27ab21dd1.md): 多轮只读审计覆盖视觉组网、MoE/共享专家、MLA/KDA 通信所有权、注册时序、权重加载 ABI、packed MXFP4、`rot_proj` 能力门控及 PP 缺失层处理；记录了若干已修复问题和仍建议补强的 `rot_proj` missing-weight 严格检查、输入形状与配置约束等事项。最终报告未发现 Python 侧 P0/P1 阻断，但这是源码/静态接口审计，定向 UT 因缺少 `torch` 失败，且本机缺少 `vllm`、`torch_npu`、`pytest`，不代表 NPU 运行时验证；全程未修改文件。

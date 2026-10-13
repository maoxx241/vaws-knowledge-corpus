---
blocks:
- block_id: 6dd7752e7bc126f71d3829e1f6572377fdf16e0c94a5c5e762a0afa2be5bbdf7
  indexed: true
  sha256: e39d8fee2d0a7db56ef09d29742ce92e63fc5efa2499b258d81667071fecdfa1
  source_range:
    end: 858021
    part: 0
    start: 0
  summary: 已完成 Python 集成调整：AscendMoERunner 优先读取显式 `ascend_activation`，共享专家的量化与 SiTU/SwiGLU 执行路径在初始化时选定，并新增集中管理两类 custom-op ABI 及 `dst_type=36` 的 `ops/fused_moe/situ.py`；dynamic EPLB 与 antiquant offset 的明确不支持错误仍被保留并补测。Ruff、py_compile 与 `git diff --check` 通过，`csrc` 无改动；pytest 未执行，因为本机缺少 `torch`、`torch_npu` 和 `vllm`，因此不能将测试结果视为验证成功。
  title: SiTU Ascend runner/adapter 重构
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1e732ffc9c06e73329d084d3e5d5d937986575939c09df331da173308ece613e
  kind: experience
  material_digest: 5d1fe51b7c334c74bc68fb9b28f75c2b78c85a0c3e074fc454f9de15db2fca99
  revision: d3e3eb9fb12fbcdb9ceacdcd13a2a696282cbaee6007d8f83ac121a1cb47998f
  schema: mindie-entry/3
  summary: 已完成 Python runner/adapter 契约重构，涉及显式 `ascend_activation`、初始化期路径选择、SiTU custom-op ABI 和定向 UT；C++、manifest 与 K3 模型文件未改动。静态检查通过，但 pytest 因缺少 `torch`、`torch_npu`、`vllm` 未执行，运行时行为仍未验证；不支持场景的错误处理已保留。
  title: 当前任务：SiTU Ascend runner 集成索引
navigation: 已完成 Python runner/adapter 契约重构，涉及显式 `ascend_activation`、初始化期路径选择、SiTU custom-op ABI 和定向 UT；C++、manifest 与 K3 模型文件未改动。静态检查通过，但 pytest 因缺少 `torch`、`torch_npu`、`vllm` 未执行，运行时行为仍未验证；不支持场景的错误处理已保留。
schema: mindie-material-task/1
status: complete
task_id: 1e732ffc9c06e73329d084d3e5d5d937986575939c09df331da173308ece613e
---

# 当前任务：SiTU Ascend runner 集成索引

已完成 Python runner/adapter 契约重构，涉及显式 `ascend_activation`、初始化期路径选择、SiTU custom-op ABI 和定向 UT；C++、manifest 与 K3 模型文件未改动。静态检查通过，但 pytest 因缺少 `torch`、`torch_npu`、`vllm` 未执行，运行时行为仍未验证；不支持场景的错误处理已保留。

## Materials

- [SiTU Ascend runner/adapter 重构](blocks/6dd7752e7bc126f71d3829e1f6572377fdf16e0c94a5c5e762a0afa2be5bbdf7.md): 已完成 Python 集成调整：AscendMoERunner 优先读取显式 `ascend_activation`，共享专家的量化与 SiTU/SwiGLU 执行路径在初始化时选定，并新增集中管理两类 custom-op ABI 及 `dst_type=36` 的 `ops/fused_moe/situ.py`；dynamic EPLB 与 antiquant offset 的明确不支持错误仍被保留并补测。Ruff、py_compile 与 `git diff --check` 通过，`csrc` 无改动；pytest 未执行，因为本机缺少 `torch`、`torch_npu` 和 `vllm`，因此不能将测试结果视为验证成功。

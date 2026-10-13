---
blocks:
- block_id: c4e5accd9d8db040fca042ca608598add5be451b46f9f1c79363551589fab623
  indexed: true
  sha256: 53c8c33157dd72baf85f10d78609d5b1118a4616eb4e8b8bfd15d64e8f5ad35b
  source_range:
    end: 453380
    part: 0
    start: 0
  summary: '已在独立运行副本中准备 PR #13277（vLLM 0.26.0）对照实验；脚本静态校验、CLI 参数验收、容器内源码加载和 SHA256 回读均通过，但这些检查不等于测试成功。155/156 两机的 16 张 NPU 均被另一套 QuaRot 服务占满，API `/health=200`；因此未启动服务、未访问 153/154、未触碰 `qjj_dspark`，也未修改或推送代码。用户随后表示“直接用这两机进行测试”，但该消息之后未记录实际停止占用服务或取得实验结果；既有阻塞和授权边界仍需保留。'
  title: 'PR #13277 对照测试受 NPU 资源冲突阻塞'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 70d0662c63aeb0f8af8918c19e10cc23c3ca87455af6c67bf18da7b9995fcaa7
  kind: experience
  material_digest: b7f2397adabca5479f7a7c0f9f8f83263b79466ee6807498cc5a899d013fd816
  revision: e6eec261e96de8af06d92766d63bfc5878bf3a1a58c3ab3623582d625a2a6f77
  schema: mindie-entry/3
  summary: 实验准备和参数校验已完成，但 155/156 仍存在占满 NPU 的无关服务，尚无可判定的测试结果。用户已提出直接使用两机测试；后续需区分授权后的资源处置、实际启动过程与最终观测，不能将准备工作或用户意图视为测试成功。
  title: '当前任务导航：PR #13277 两机对照测试'
navigation: 实验准备和参数校验已完成，但 155/156 仍存在占满 NPU 的无关服务，尚无可判定的测试结果。用户已提出直接使用两机测试；后续需区分授权后的资源处置、实际启动过程与最终观测，不能将准备工作或用户意图视为测试成功。
schema: mindie-material-task/1
status: complete
task_id: 70d0662c63aeb0f8af8918c19e10cc23c3ca87455af6c67bf18da7b9995fcaa7
---

# 当前任务导航：PR #13277 两机对照测试

实验准备和参数校验已完成，但 155/156 仍存在占满 NPU 的无关服务，尚无可判定的测试结果。用户已提出直接使用两机测试；后续需区分授权后的资源处置、实际启动过程与最终观测，不能将准备工作或用户意图视为测试成功。

## Materials

- [PR #13277 对照测试受 NPU 资源冲突阻塞](blocks/c4e5accd9d8db040fca042ca608598add5be451b46f9f1c79363551589fab623.md): 已在独立运行副本中准备 PR #13277（vLLM 0.26.0）对照实验；脚本静态校验、CLI 参数验收、容器内源码加载和 SHA256 回读均通过，但这些检查不等于测试成功。155/156 两机的 16 张 NPU 均被另一套 QuaRot 服务占满，API `/health=200`；因此未启动服务、未访问 153/154、未触碰 `qjj_dspark`，也未修改或推送代码。用户随后表示“直接用这两机进行测试”，但该消息之后未记录实际停止占用服务或取得实验结果；既有阻塞和授权边界仍需保留。

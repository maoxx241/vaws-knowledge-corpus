---
blocks:
- block_id: 340d294a54079a01cfbdf1532acbacc23a09863bbcb969a820259c17a62a653c
  indexed: true
  sha256: 8e0e920b8af1d88cb0b47ffb2ffdee9b252fd4155d13160fed3570093c9e9133
  source_range:
    end: 2381856
    part: 0
    start: 0
  summary: 为当前四机环境创建了 Ubuntu 22.04、A3、CANN 9.1.0、vLLM 0.26.0、torch-npu 2.10.0.post2 的独立 idle 容器，并统一使用 vllm-ascend 提交 `7c1b8665...`；期间出现镜像选择变更、rsync 不支持、远端无 rsync、SSH banner 污染 tar 流及各机源码提交不一致等失败或修正。最终四机完成 CANN、NPU 数量、NPUPlatform、源码导入路径和容器状态校验，但尚未启动模型服务；原 `qjj_dspark` 未修改。容器拉起与重建命令已从 `AGENTS.md` 拆分至独立 `DSpark_CONTAINER_RUNBOOK.md`，`AGENTS.md` 仅保留导航与安全边界。
  title: 蓝区四机 CANN 9.1 / DSpark 0.26 容器与运行手册
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ab3318d30818325191e37c10e98c6a83269536bb6b3c47998ce220cb88209e08
  kind: experience
  material_digest: 6dd6bc504b4ad0cb8501cc960e38260840d4790009cc350e255039961feac463
  revision: 2e8331a9c72cd5808c925ceebb750b425d3da33fd18a4f55da84e1158a43266e
  schema: mindie-entry/3
  summary: 当前批次聚焦蓝区四机的 CANN 9.1、Ubuntu、DSpark 0.26/vLLM-Ascend 容器准备及运行手册整理。相关结果、失败尝试、源码统一修正和未启动模型服务的状态见本块；容器命令已独立移至 `DSpark_CONTAINER_RUNBOOK.md`，`AGENTS.md` 提供导航与防误操作边界。
  title: 当前任务导航
navigation: 当前批次聚焦蓝区四机的 CANN 9.1、Ubuntu、DSpark 0.26/vLLM-Ascend 容器准备及运行手册整理。相关结果、失败尝试、源码统一修正和未启动模型服务的状态见本块；容器命令已独立移至 `DSpark_CONTAINER_RUNBOOK.md`，`AGENTS.md` 提供导航与防误操作边界。
schema: mindie-material-task/1
status: complete
task_id: ab3318d30818325191e37c10e98c6a83269536bb6b3c47998ce220cb88209e08
---

# 当前任务导航

当前批次聚焦蓝区四机的 CANN 9.1、Ubuntu、DSpark 0.26/vLLM-Ascend 容器准备及运行手册整理。相关结果、失败尝试、源码统一修正和未启动模型服务的状态见本块；容器命令已独立移至 `DSpark_CONTAINER_RUNBOOK.md`，`AGENTS.md` 提供导航与防误操作边界。

## Materials

- [蓝区四机 CANN 9.1 / DSpark 0.26 容器与运行手册](blocks/340d294a54079a01cfbdf1532acbacc23a09863bbcb969a820259c17a62a653c.md): 为当前四机环境创建了 Ubuntu 22.04、A3、CANN 9.1.0、vLLM 0.26.0、torch-npu 2.10.0.post2 的独立 idle 容器，并统一使用 vllm-ascend 提交 `7c1b8665...`；期间出现镜像选择变更、rsync 不支持、远端无 rsync、SSH banner 污染 tar 流及各机源码提交不一致等失败或修正。最终四机完成 CANN、NPU 数量、NPUPlatform、源码导入路径和容器状态校验，但尚未启动模型服务；原 `qjj_dspark` 未修改。容器拉起与重建命令已从 `AGENTS.md` 拆分至独立 `DSpark_CONTAINER_RUNBOOK.md`，`AGENTS.md` 仅保留导航与安全边界。

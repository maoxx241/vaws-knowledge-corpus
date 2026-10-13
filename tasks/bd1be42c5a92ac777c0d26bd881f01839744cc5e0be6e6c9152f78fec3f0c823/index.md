---
blocks:
- block_id: af476031bcf6b59bce9ac258b50f3faf136b5f41e1fd1385734f57587685d78d
  indexed: true
  sha256: 8716ee785913bfe2f03db6bcb2b6a87cca280336a47fc47656f98011274048fb
  source_range:
    end: 811042
    part: 0
    start: 0
  summary: 据消息记录，先后报告了 rank metadata 在 worker 初始化期冻结、DP0/DP1 采集 gate、global rank 校验，以及 completion-engine ID canonicalization 的改动；此前默认 pytest 因缺少 torch，隔离 CPU 副本又受 Ascend 构建元数据限制，测试环境存在不完整情况。记录称首个变更定向测试从 8 通过 1 失败修正为 9/9，第二个变更 CPU UT 为 11/11，并报告 py_compile、Ruff 与 diff-check 通过；这些结果仍属于来源中的报告，未在此独立验证。报告的拒绝条件包括缺失或冲突 rank metadata、DP/TP 不一致、错误包装 ID 格式及 exact/wrapped 歧义；来源还给出了两个 commit、文件 SHA256 与未部署服务/NPU 的声明。
  title: DFX rank gate 与 engine ID 规范化修复记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bd1be42c5a92ac777c0d26bd881f01839744cc5e0be6e6c9152f78fec3f0c823
  kind: experience
  material_digest: 0d22ef1e839355d780bd93b1595ec9811d70fde39fb6f6338238b397f1ab3c39
  revision: 4107691df39916ecf000a29fe63e8a6bd7175f75a7386c036bb8378573e0e43f
  schema: mindie-entry/3
  summary: 本批仅包含一个完整材料块，涵盖两项据报已提交的 DFX 修复：rank metadata 初始化期缓存与 fail-closed gate，以及 completion-engine external ID 规范化。相关环境限制、一次失败测试及后续修正、测试通过数字和未部署声明均保留为来源报告，不视为独立验证；原始材料块可单独检索。
  title: 当前任务导航：DFX 插桩修复与检索索引
navigation: 本批仅包含一个完整材料块，涵盖两项据报已提交的 DFX 修复：rank metadata 初始化期缓存与 fail-closed gate，以及 completion-engine external ID 规范化。相关环境限制、一次失败测试及后续修正、测试通过数字和未部署声明均保留为来源报告，不视为独立验证；原始材料块可单独检索。
schema: mindie-material-task/1
status: complete
task_id: bd1be42c5a92ac777c0d26bd881f01839744cc5e0be6e6c9152f78fec3f0c823
---

# 当前任务导航：DFX 插桩修复与检索索引

本批仅包含一个完整材料块，涵盖两项据报已提交的 DFX 修复：rank metadata 初始化期缓存与 fail-closed gate，以及 completion-engine external ID 规范化。相关环境限制、一次失败测试及后续修正、测试通过数字和未部署声明均保留为来源报告，不视为独立验证；原始材料块可单独检索。

## Materials

- [DFX rank gate 与 engine ID 规范化修复记录](blocks/af476031bcf6b59bce9ac258b50f3faf136b5f41e1fd1385734f57587685d78d.md): 据消息记录，先后报告了 rank metadata 在 worker 初始化期冻结、DP0/DP1 采集 gate、global rank 校验，以及 completion-engine ID canonicalization 的改动；此前默认 pytest 因缺少 torch，隔离 CPU 副本又受 Ascend 构建元数据限制，测试环境存在不完整情况。记录称首个变更定向测试从 8 通过 1 失败修正为 9/9，第二个变更 CPU UT 为 11/11，并报告 py_compile、Ruff 与 diff-check 通过；这些结果仍属于来源中的报告，未在此独立验证。报告的拒绝条件包括缺失或冲突 rank metadata、DP/TP 不一致、错误包装 ID 格式及 exact/wrapped 歧义；来源还给出了两个 commit、文件 SHA256 与未部署服务/NPU 的声明。

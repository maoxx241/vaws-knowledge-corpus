---
blocks:
- block_id: 0ee3afdb0dc7c9aa8bc86b7fdd6a5f0d8bb57c4a02857797b6e509ca469d1370
  indexed: true
  sha256: 02ba447e6041f4d758575d380bc56674abfce4dfc4952eee8d937b58ba116b50
  source_range:
    end: 1238518
    part: 0
    start: 0
  summary: '围绕 vllm-ascend PR #14620，删除 Triton 的 `causal_conv1d_update_npu` kernel、wrapper、monkey patch 及两个专属测试，保留 AscendC、310P 路径和共享辅助函数；签名提交 `b5a8f7fb1` 已创建 draft PR。CI 暴露 DFlash golden 旧值 `0.51` 与实际接受率 `0.619` 不匹配，已将首项更新为 `0.60`，签名提交 `9e979ee1a` 推送；`bash format.sh ci` 通过，ModelScope 下载超时被判断为基础设施问题但未验证解决。固定 head 的监控显示 CPU 已通过、310P 两分片运行中、A2/A3 排队，尚无新失败；此前部分状态和后续硬件结果仍未完成。'
  title: 删除 causal_conv1d_update_npu 后修复并监控 PR CI
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f1f70e60a77239191438e76b69512b104e6c5b65de724961ff9d60784b55be2e
  kind: experience
  material_digest: 75af179cdabd428b5b2649b427e2eed0919fcae7ed04090134e55522a79c9e5c
  revision: cd88945de8d2e033ebb1aaaed89fd64fbd35deef630d2c820b6605324bd8d8b9
  schema: mindie-entry/3
  summary: '当前任务聚焦于 PR #14620 的固定 head `9e979ee1a`：DFlash 基线修复已提交并推送，本地格式检查通过。CI 尚未全部完成；CPU 已通过，310P 正在运行，A2/A3 排队，尚不能宣称整体成功；ModelScope 超时仍记录为疑似基础设施问题。'
  title: 'PR #14620 CI 修复与固定 head 监控'
navigation: '当前任务聚焦于 PR #14620 的固定 head `9e979ee1a`：DFlash 基线修复已提交并推送，本地格式检查通过。CI 尚未全部完成；CPU 已通过，310P 正在运行，A2/A3 排队，尚不能宣称整体成功；ModelScope 超时仍记录为疑似基础设施问题。'
schema: mindie-material-task/1
status: complete
task_id: f1f70e60a77239191438e76b69512b104e6c5b65de724961ff9d60784b55be2e
---

# PR #14620 CI 修复与固定 head 监控

当前任务聚焦于 PR #14620 的固定 head `9e979ee1a`：DFlash 基线修复已提交并推送，本地格式检查通过。CI 尚未全部完成；CPU 已通过，310P 正在运行，A2/A3 排队，尚不能宣称整体成功；ModelScope 超时仍记录为疑似基础设施问题。

## Materials

- [删除 causal_conv1d_update_npu 后修复并监控 PR CI](blocks/0ee3afdb0dc7c9aa8bc86b7fdd6a5f0d8bb57c4a02857797b6e509ca469d1370.md): 围绕 vllm-ascend PR #14620，删除 Triton 的 `causal_conv1d_update_npu` kernel、wrapper、monkey patch 及两个专属测试，保留 AscendC、310P 路径和共享辅助函数；签名提交 `b5a8f7fb1` 已创建 draft PR。CI 暴露 DFlash golden 旧值 `0.51` 与实际接受率 `0.619` 不匹配，已将首项更新为 `0.60`，签名提交 `9e979ee1a` 推送；`bash format.sh ci` 通过，ModelScope 下载超时被判断为基础设施问题但未验证解决。固定 head 的监控显示 CPU 已通过、310P 两分片运行中、A2/A3 排队，尚无新失败；此前部分状态和后续硬件结果仍未完成。

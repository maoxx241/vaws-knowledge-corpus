---
blocks:
- block_id: 0c9d8c80ed39dd2f235e3004591a76807c3f558503a1642fccf46f6624241e44
  indexed: true
  sha256: 9361010559331b1c1d8754488950acc6f95cbf9e6e3814afa4069fea8dafd74e
  source_range:
    end: 1035944
    part: 0
    start: 0
  summary: '围绕 `Attn_Res_Fwd` 将手写 FP32 RMSNorm 替换为 `torch_npu.npu_rms_norm`，并保持 score/value matmul 为 FP32；本地静态检查通过，但因缺少 PyTorch 未启动目标 pytest，全量格式检查被基线 `csrc/build.sh:524` 的 ShellCheck `SC2328` 拦截。PR #18 在远端 Ascend910 上报告目标单测 `2 passed, 25 deselected`，覆盖 BF16、hidden size `7168` 及 decode/prefill，四组 FP32 reference 对比通过，最大 mean absolute error 为 `2e-7`，单算子报告 `1.22x–1.41x` 加速。测试期间四节点服务进程数下降且 health 为 `000`，随后发现服务由另一流程依次拉起；未重启或清理服务，因此结论限于单算子和单测，不代表完整服务 E2E 已验证；`ci-new` 强制换线后提交 `9e262a91d` 被重放到 `e34679b77`，PR 据称无冲突且 CI 重新运行中。'
  title: Attn_Res_Fwd 优化与 Ascend910 实测
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8a723c57fa7a409c0f5473f55e15599fb6275353413648a67d98ff09b8d65871
  kind: experience
  material_digest: 8a9d46ce83ae61e663397a0961ce8c29a07979c733ce11f10a59b7b76489393d
  revision: 562dffa96983a031b89cd1db62897a48e56e9dd13adb88aa6caea9e5e1666354
  schema: mindie-entry/3
  summary: 检索入口：`Attn_Res_Fwd`、`npu_rms_norm`、FP32 RMSNorm/matmul、Ascend910、BF16、hidden size `7168`、decode/prefill、`assert_close`、`SC2328`、`ci-new` 强制换线、提交 `9e262a91d`。当前记录包含本地依赖缺失、基线 ShellCheck 阻断、远端单算子/单测报告通过，以及服务 health `000` 和进程异常变化；后续应区分算子级验证与尚未确认的完整服务 E2E，继续关注 PR CI 状态。
  title: '当前任务导航：Attn_Res_Fwd 优化 PR #18'
navigation: 检索入口：`Attn_Res_Fwd`、`npu_rms_norm`、FP32 RMSNorm/matmul、Ascend910、BF16、hidden size `7168`、decode/prefill、`assert_close`、`SC2328`、`ci-new` 强制换线、提交 `9e262a91d`。当前记录包含本地依赖缺失、基线 ShellCheck 阻断、远端单算子/单测报告通过，以及服务 health `000` 和进程异常变化；后续应区分算子级验证与尚未确认的完整服务 E2E，继续关注 PR CI 状态。
schema: mindie-material-task/1
status: complete
task_id: 8a723c57fa7a409c0f5473f55e15599fb6275353413648a67d98ff09b8d65871
---

# 当前任务导航：Attn_Res_Fwd 优化 PR #18

检索入口：`Attn_Res_Fwd`、`npu_rms_norm`、FP32 RMSNorm/matmul、Ascend910、BF16、hidden size `7168`、decode/prefill、`assert_close`、`SC2328`、`ci-new` 强制换线、提交 `9e262a91d`。当前记录包含本地依赖缺失、基线 ShellCheck 阻断、远端单算子/单测报告通过，以及服务 health `000` 和进程异常变化；后续应区分算子级验证与尚未确认的完整服务 E2E，继续关注 PR CI 状态。

## Materials

- [Attn_Res_Fwd 优化与 Ascend910 实测](blocks/0c9d8c80ed39dd2f235e3004591a76807c3f558503a1642fccf46f6624241e44.md): 围绕 `Attn_Res_Fwd` 将手写 FP32 RMSNorm 替换为 `torch_npu.npu_rms_norm`，并保持 score/value matmul 为 FP32；本地静态检查通过，但因缺少 PyTorch 未启动目标 pytest，全量格式检查被基线 `csrc/build.sh:524` 的 ShellCheck `SC2328` 拦截。PR #18 在远端 Ascend910 上报告目标单测 `2 passed, 25 deselected`，覆盖 BF16、hidden size `7168` 及 decode/prefill，四组 FP32 reference 对比通过，最大 mean absolute error 为 `2e-7`，单算子报告 `1.22x–1.41x` 加速。测试期间四节点服务进程数下降且 health 为 `000`，随后发现服务由另一流程依次拉起；未重启或清理服务，因此结论限于单算子和单测，不代表完整服务 E2E 已验证；`ci-new` 强制换线后提交 `9e262a91d` 被重放到 `e34679b77`，PR 据称无冲突且 CI 重新运行中。

---
blocks:
- block_id: 8a454a0617e36ed2770a7d82b4f04ef7b9b499b3a13edbaddbdbce1d2ee4a5aa
  indexed: true
  sha256: 8f5c6b08cfaeff8c154f232e233dcd2f31dd80d3609b23319b6d8a82716ece8a
  source_range:
    end: 3530958
    part: 0
    start: 0
  summary: '#12894 的 fused MoE 修复将 `group_list` 统一按 expert token count（`group_list_type=1`）传递，覆盖 MC2、AllGather、AllToAll、V2 SwiGLU、DeviceAdaptor 和 GMM1/GMM2；CPU UT 验证参数契约，A2 用例验证真实算子数值。早期 CPU 失败确认是 `torch_npu` stub 缺少 `npu_grouped_matmul`，通过 `patch(create=True)` 修复，并补齐 A2 测试的 `estimated_times=30` 配置。修复已多次提交并推送，静态检查、pre-commit、mypy、gitleaks、测试选择器和 CPU 分片通过；A2/A3 硬件结果当时仍在排队，不能据此宣称整体 CI 成功。'
  title: '#12894 fused MoE group_list count 修复与 A2 测试'
- block_id: d0ea5e230a7131053a0dd462f2c3172044b4d6a674216bc0c2ae742ba2a5afb7
  indexed: true
  sha256: 0d219c75f6b4de18a1b635a864d9487e5c26828420c1335f98857cab9aebd51e
  source_range:
    end: 3530958
    part: 1
    start: 0
  summary: 最新一轮中，#12894 的 pre-commit、mypy、coverage 推荐、select-tests 以及两套 CPU 分片均通过；新增 count/type 1 真实算子测试和原有 multistream fused-MoE E2E 已确认进入 `A2 x1 part 5/5`。两个目标 A2 分片仍处于 queued，原因是 A2 runner 资源不足，尚无测试日志或失败结果，因此硬件正确性仍未验证。#12896 的 CPU、A2、A3、310P、upstream 和 ci-gate 均已通过，不需要修改。
  title: '#12894 A2 分片排队，#12896 CI 已通过'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 651390e6d57fadeed3d758cf170038cc1e71280af6b1a2c984faa51ae3a3a3b5
  kind: experience
  material_digest: ec21aa3902740e186735e8defdcd7079bdc6915ac43b7079467e9896678625e4
  revision: 4499aa97721f96d4bc8bc519aa4f1378a4c45a84d31f6564c36724f46f5f0fbe
  schema: mindie-entry/3
  summary: '主线是确认 #12894 将 fused MoE 内部统一为 count（`group_list_type=1`）后，真实 A2 算子行为是否正确；CPU UT 仅代表 Python 参数透传契约，不能替代 NPU 数值验证。当前已完成代码修复、测试分层、A2 路由与耗时配置，并通过静态门禁和 CPU 分片；#12894 的 A2/A3 硬件结果仍未完成，排队状态不等于成功。#12896 已全量 CI 通过。'
  title: 当前任务导航：fused MoE group_list 修复与硬件 CI 验证
navigation: '主线是确认 #12894 将 fused MoE 内部统一为 count（`group_list_type=1`）后，真实 A2 算子行为是否正确；CPU UT 仅代表 Python 参数透传契约，不能替代 NPU 数值验证。当前已完成代码修复、测试分层、A2 路由与耗时配置，并通过静态门禁和 CPU 分片；#12894 的 A2/A3 硬件结果仍未完成，排队状态不等于成功。#12896 已全量 CI 通过。'
schema: mindie-material-task/1
status: complete
task_id: 651390e6d57fadeed3d758cf170038cc1e71280af6b1a2c984faa51ae3a3a3b5
---

# 当前任务导航：fused MoE group_list 修复与硬件 CI 验证

主线是确认 #12894 将 fused MoE 内部统一为 count（`group_list_type=1`）后，真实 A2 算子行为是否正确；CPU UT 仅代表 Python 参数透传契约，不能替代 NPU 数值验证。当前已完成代码修复、测试分层、A2 路由与耗时配置，并通过静态门禁和 CPU 分片；#12894 的 A2/A3 硬件结果仍未完成，排队状态不等于成功。#12896 已全量 CI 通过。

## Materials

- [#12894 fused MoE group_list count 修复与 A2 测试](blocks/8a454a0617e36ed2770a7d82b4f04ef7b9b499b3a13edbaddbdbce1d2ee4a5aa.md): #12894 的 fused MoE 修复将 `group_list` 统一按 expert token count（`group_list_type=1`）传递，覆盖 MC2、AllGather、AllToAll、V2 SwiGLU、DeviceAdaptor 和 GMM1/GMM2；CPU UT 验证参数契约，A2 用例验证真实算子数值。早期 CPU 失败确认是 `torch_npu` stub 缺少 `npu_grouped_matmul`，通过 `patch(create=True)` 修复，并补齐 A2 测试的 `estimated_times=30` 配置。修复已多次提交并推送，静态检查、pre-commit、mypy、gitleaks、测试选择器和 CPU 分片通过；A2/A3 硬件结果当时仍在排队，不能据此宣称整体 CI 成功。
- [#12894 A2 分片排队，#12896 CI 已通过](blocks/d0ea5e230a7131053a0dd462f2c3172044b4d6a674216bc0c2ae742ba2a5afb7.md): 最新一轮中，#12894 的 pre-commit、mypy、coverage 推荐、select-tests 以及两套 CPU 分片均通过；新增 count/type 1 真实算子测试和原有 multistream fused-MoE E2E 已确认进入 `A2 x1 part 5/5`。两个目标 A2 分片仍处于 queued，原因是 A2 runner 资源不足，尚无测试日志或失败结果，因此硬件正确性仍未验证。#12896 的 CPU、A2、A3、310P、upstream 和 ci-gate 均已通过，不需要修改。

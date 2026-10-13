---
blocks:
- block_id: 5c9c9404ad5d8ba053f5176beb973e97ad7a94b0bfee2c65a28f74aab6d022a4
  indexed: true
  sha256: a770d4eb21eaf5f654332c7cf0ea7914770f130308fcb8d2a930e8f10224324d
  source_range:
    end: 1004249
    part: 0
    start: 0
  summary: 在隔离容器 `kimi-k3-fastbuild-154` 中完成扩展和 vendor 构建，状态均为 0，并确认扩展包含 5 个目标 schema、vendor 导出对应 ACLNN 与 WorkspaceSize 符号；产物保留在指定构建目录，未触碰权重目录或 `kimi_k3_lqt`。初次同步因 macOS AppleDouble 文件导致 `._*.cpp` 被误识别为源码，已清理后重启；另同步了固定 commit `41bf90d` 的 CATLASS 依赖。已观察到 `aclnnCausalConv1d` 来自镜像内 `custom_transformer/libcust_opapi.so` 而非 CANN `libopapi.so`，据此将 causal-conv 和 moe gating 纳入独立包；单卡 MOE + KDA 数值 smoke 未执行，因为 156 全量产物已成功且停止指令到达。
  title: 154 隔离构建与 vendor 产物
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8aa4b86b89a332c5b076ec0411c8d30b9ec93a4fe818fef86b81aa5e0253c45e
  kind: experience
  material_digest: a112c094a052a624fef6e6af77e2fb412ec2aafde934918354ed3501d98ee2ae
  revision: 59d0fae6d474dd870cb08f0daf9aa67167df7a9f6fd60f90a8fced21cb4739dc
  schema: mindie-entry/3
  summary: 本批材料记录了 154 隔离快速构建、AppleDouble 源码识别问题、CATLASS 依赖补齐及 5 个目标 schema/符号产物确认。`aclnnCausalConv1d` 的来源和 `MoeInitRoutingCustom` 缺失原因已有报告性判断，但数值 smoke 未完成，相关结论仍应视为未验证；原始消息块可独立检索。
  title: 当前任务导航：隔离构建与 vendor 追溯
navigation: 本批材料记录了 154 隔离快速构建、AppleDouble 源码识别问题、CATLASS 依赖补齐及 5 个目标 schema/符号产物确认。`aclnnCausalConv1d` 的来源和 `MoeInitRoutingCustom` 缺失原因已有报告性判断，但数值 smoke 未完成，相关结论仍应视为未验证；原始消息块可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 8aa4b86b89a332c5b076ec0411c8d30b9ec93a4fe818fef86b81aa5e0253c45e
---

# 当前任务导航：隔离构建与 vendor 追溯

本批材料记录了 154 隔离快速构建、AppleDouble 源码识别问题、CATLASS 依赖补齐及 5 个目标 schema/符号产物确认。`aclnnCausalConv1d` 的来源和 `MoeInitRoutingCustom` 缺失原因已有报告性判断，但数值 smoke 未完成，相关结论仍应视为未验证；原始消息块可独立检索。

## Materials

- [154 隔离构建与 vendor 产物](blocks/5c9c9404ad5d8ba053f5176beb973e97ad7a94b0bfee2c65a28f74aab6d022a4.md): 在隔离容器 `kimi-k3-fastbuild-154` 中完成扩展和 vendor 构建，状态均为 0，并确认扩展包含 5 个目标 schema、vendor 导出对应 ACLNN 与 WorkspaceSize 符号；产物保留在指定构建目录，未触碰权重目录或 `kimi_k3_lqt`。初次同步因 macOS AppleDouble 文件导致 `._*.cpp` 被误识别为源码，已清理后重启；另同步了固定 commit `41bf90d` 的 CATLASS 依赖。已观察到 `aclnnCausalConv1d` 来自镜像内 `custom_transformer/libcust_opapi.so` 而非 CANN `libopapi.so`，据此将 causal-conv 和 moe gating 纳入独立包；单卡 MOE + KDA 数值 smoke 未执行，因为 156 全量产物已成功且停止指令到达。

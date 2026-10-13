---
blocks:
- block_id: a1c1fbc70665c140154f93f14e223d34d57d0161ac728f2ee9e932c3ec523414
  indexed: true
  sha256: d30ca259a8083c791a5b511d6fed06c3b4357202e65c3cf4aeb3d73fb03917c2
  source_range:
    end: 890324
    part: 0
    start: 0
  summary: 审查认为将 causal-conv q/k/v 的 transpose/cat/cast 移至加载生命周期方向合理，但在 vLLM 0.25.1 中存在两个 P1 缺口：直接或 kernel-format 的 `param.copy_()` 不触发 pack hook，且 layerwise finalize 可能以 `persistent=True` 重新注册 derived buffer，污染 `state_dict()` 并带来 sharded checkpoint 重载风险。普通初始加载、PP/MM 转发、ACLGraph 稳定地址和部分 layerwise 行为被报告为正确，但 prefetch offload 后 reload、capture 后 `.to()` 等组合仍不支持或未验证；14/14 测试、`git diff --check` 通过，未修改文件，最终建议当前 PR 放弃该优化并保留原热路径实现。
  title: vLLM KDA 卷积权重后置打包审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 068380854a8aee72b9a6eb31b7598ca55e9acab7c7b5fe78fcf736e409b0e5ae
  kind: experience
  material_digest: a619c5ed933709e3a37c9fd7345ff28551bf88310fed4f05fa6a7d2b301c8823
  revision: d806949d146b5b239cfc7877741d70973aae484ee3e8386bf83822c41d446637
  schema: mindie-entry/3
  summary: 当前索引涵盖 vLLM 0.25.1 下 KDA derived conv buffer 的加载、reload、layerwise finalize、direct `param.copy_()`、offload 与 ACLGraph 边界。关键结论是存在未解决的 direct-update 与 non-persistent buffer 两项 P1 风险；测试通过不等于正式 API 覆盖完整，真实 NPU 性能收益也尚未证明，后续修复需依赖跨 runner/worker 生命周期钩子或统一 derived-weight 协议。
  title: KDA 权重打包与 vLLM 生命周期审查
navigation: 当前索引涵盖 vLLM 0.25.1 下 KDA derived conv buffer 的加载、reload、layerwise finalize、direct `param.copy_()`、offload 与 ACLGraph 边界。关键结论是存在未解决的 direct-update 与 non-persistent buffer 两项 P1 风险；测试通过不等于正式 API 覆盖完整，真实 NPU 性能收益也尚未证明，后续修复需依赖跨 runner/worker 生命周期钩子或统一 derived-weight 协议。
schema: mindie-material-task/1
status: complete
task_id: 068380854a8aee72b9a6eb31b7598ca55e9acab7c7b5fe78fcf736e409b0e5ae
---

# KDA 权重打包与 vLLM 生命周期审查

当前索引涵盖 vLLM 0.25.1 下 KDA derived conv buffer 的加载、reload、layerwise finalize、direct `param.copy_()`、offload 与 ACLGraph 边界。关键结论是存在未解决的 direct-update 与 non-persistent buffer 两项 P1 风险；测试通过不等于正式 API 覆盖完整，真实 NPU 性能收益也尚未证明，后续修复需依赖跨 runner/worker 生命周期钩子或统一 derived-weight 协议。

## Materials

- [vLLM KDA 卷积权重后置打包审查](blocks/a1c1fbc70665c140154f93f14e223d34d57d0161ac728f2ee9e932c3ec523414.md): 审查认为将 causal-conv q/k/v 的 transpose/cat/cast 移至加载生命周期方向合理，但在 vLLM 0.25.1 中存在两个 P1 缺口：直接或 kernel-format 的 `param.copy_()` 不触发 pack hook，且 layerwise finalize 可能以 `persistent=True` 重新注册 derived buffer，污染 `state_dict()` 并带来 sharded checkpoint 重载风险。普通初始加载、PP/MM 转发、ACLGraph 稳定地址和部分 layerwise 行为被报告为正确，但 prefetch offload 后 reload、capture 后 `.to()` 等组合仍不支持或未验证；14/14 测试、`git diff --check` 通过，未修改文件，最终建议当前 PR 放弃该优化并保留原热路径实现。

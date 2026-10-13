---
blocks:
- block_id: f108d7e037199366db3be991866fbbaa3f0ee9dcd1e9d21101c6cc83e36f7c44
  indexed: true
  sha256: 9e2579df2bb42717095cee11fd1ca38372c657a4e050514e9d80bb61adccd547
  source_range:
    end: 864392
    part: 0
    start: 0
  summary: 在独立 worktree 和分支中，候选实现将 `_run_prefill` 切换为 full Triton `chunk_kda`，保留 vLLM `l2norm_fwd`、`_recurrent_gate(raw_gate)` 及 canonical `[H,V,K]` state，并预建 compact device `cu_seqlens` 与 chunk offsets；声称未修改既有目录或占用 NPU。`py_compile`、`git diff --check` 和定向 CPU UT 报告为 `41 passed, 14 warnings`，但完整服务、NPU 行为和精度对照未验证。共享 full-op probe 仍使用 `use_qk_l2norm_in_kernel=True`，因此只能说明 full op 未挂，不能作为与服务 AscendC 的单变量精度结论；部署文件哈希和 diff SHA256 已提供。
  title: Triton KDA prefill 候选路径与 CPU 契约测试
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c8f5c1ecaab31c8027164b0457b8461406280c368051b476fc617cb791ce8f3a
  kind: experience
  material_digest: 2d112432356898b15d6d4bceff59acba4e2999e89010b446698cb443160a1479
  revision: 20e0ca84a2e1bf1893d938a6a43c514e93638e5e54647ed966638c38c10c0139
  schema: mindie-entry/3
  summary: 当前批次记录了独立 worktree 中的 Triton prefill 候选路径、metadata/offset 与 state-layout 契约，以及 CPU 静态检查和定向测试结果。结果仍属候选和局部验证：未启动服务、未占用 NPU，精度对照与完整运行尚未确认；共享 probe 的 l2norm 配置不同，不能提升为服务级成功结论。
  title: Triton KDA prefill 候选实现索引
navigation: 当前批次记录了独立 worktree 中的 Triton prefill 候选路径、metadata/offset 与 state-layout 契约，以及 CPU 静态检查和定向测试结果。结果仍属候选和局部验证：未启动服务、未占用 NPU，精度对照与完整运行尚未确认；共享 probe 的 l2norm 配置不同，不能提升为服务级成功结论。
schema: mindie-material-task/1
status: complete
task_id: c8f5c1ecaab31c8027164b0457b8461406280c368051b476fc617cb791ce8f3a
---

# Triton KDA prefill 候选实现索引

当前批次记录了独立 worktree 中的 Triton prefill 候选路径、metadata/offset 与 state-layout 契约，以及 CPU 静态检查和定向测试结果。结果仍属候选和局部验证：未启动服务、未占用 NPU，精度对照与完整运行尚未确认；共享 probe 的 l2norm 配置不同，不能提升为服务级成功结论。

## Materials

- [Triton KDA prefill 候选路径与 CPU 契约测试](blocks/f108d7e037199366db3be991866fbbaa3f0ee9dcd1e9d21101c6cc83e36f7c44.md): 在独立 worktree 和分支中，候选实现将 `_run_prefill` 切换为 full Triton `chunk_kda`，保留 vLLM `l2norm_fwd`、`_recurrent_gate(raw_gate)` 及 canonical `[H,V,K]` state，并预建 compact device `cu_seqlens` 与 chunk offsets；声称未修改既有目录或占用 NPU。`py_compile`、`git diff --check` 和定向 CPU UT 报告为 `41 passed, 14 warnings`，但完整服务、NPU 行为和精度对照未验证。共享 full-op probe 仍使用 `use_qk_l2norm_in_kernel=True`，因此只能说明 full op 未挂，不能作为与服务 AscendC 的单变量精度结论；部署文件哈希和 diff SHA256 已提供。

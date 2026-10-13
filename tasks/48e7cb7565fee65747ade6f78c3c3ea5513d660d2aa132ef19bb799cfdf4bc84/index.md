---
blocks:
- block_id: e93dd4ac9d21f0f214b1cfa2f845fdb06e2b4508f0711e2ecb0d1b7b408e19de
  indexed: true
  sha256: 5a530295983b32ba51cfcc20805a95eb849560748918438ab32dd958d77fdfbe
  source_range:
    end: 3300807
    part: 0
    start: 0
  summary: 审计未修改文件，当前最强嫌疑是新增 GPU-only Mamba align 异步路径，尤其是 target forward 前的 real-only precopy；`after_mamba_preprocess` 仅表示 host 完成 enqueue，不代表 NPU kernel 完成，阻塞可能延迟至 K3 MoE `token_dispatcher._preprocess`。次级嫌疑是 packed heterogeneous zeroer 的非阻塞 H2D 与立即 zero kernel，证据不足；R8 已排除 double dummy、ordinal/call-count、padding、prefix hit 及特定 graph-only 机制。建议首轮 A/B 整体禁用 `_use_gpu_mamba_align()`，恢复 legacy CPU-planned preprocess/copy/event；若仍卡，再同步化或回退 packed zeroer 生命周期。
  title: GPU Mamba 异步对齐路径疑似导致调度卡死
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 48e7cb7565fee65747ade6f78c3c3ea5513d660d2aa132ef19bb799cfdf4bc84
  kind: experience
  material_digest: ce3f061ec60497c50fc462dd824478dbd66cd0bbb89ec808caac5cdc04819c9f
  revision: 76d2b98b75ff63e90c4a1e483869436dfb91d68ab677025acd08a825b2675aab
  schema: mindie-entry/3
  summary: 当前上下文聚焦于 serial1/DP → c64 重放中的异步设备完成与 host enqueue 边界。首要未证实假设是 GPU-only Mamba align 的 remap/precopy 或相关 event 协议导致等待不收敛；packed heterogeneous zeroer 是次级假设。R8 已排除 double dummy、target/draft 与 allreduce 次数不一致、padding、prefix-hit、特定 graph-only 触发及未设置 DS layout 的上游 assert 解释；后续结果仍需通过禁用 align、拆分 precopy/postprocess、再检查 zeroer 生命周期来确认。
  title: 调度卡死审计：优先验证 GPU Mamba precopy
navigation: 当前上下文聚焦于 serial1/DP → c64 重放中的异步设备完成与 host enqueue 边界。首要未证实假设是 GPU-only Mamba align 的 remap/precopy 或相关 event 协议导致等待不收敛；packed heterogeneous zeroer 是次级假设。R8 已排除 double dummy、target/draft 与 allreduce 次数不一致、padding、prefix-hit、特定 graph-only 触发及未设置 DS layout 的上游 assert 解释；后续结果仍需通过禁用 align、拆分 precopy/postprocess、再检查 zeroer 生命周期来确认。
schema: mindie-material-task/1
status: complete
task_id: 48e7cb7565fee65747ade6f78c3c3ea5513d660d2aa132ef19bb799cfdf4bc84
---

# 调度卡死审计：优先验证 GPU Mamba precopy

当前上下文聚焦于 serial1/DP → c64 重放中的异步设备完成与 host enqueue 边界。首要未证实假设是 GPU-only Mamba align 的 remap/precopy 或相关 event 协议导致等待不收敛；packed heterogeneous zeroer 是次级假设。R8 已排除 double dummy、target/draft 与 allreduce 次数不一致、padding、prefix-hit、特定 graph-only 触发及未设置 DS layout 的上游 assert 解释；后续结果仍需通过禁用 align、拆分 precopy/postprocess、再检查 zeroer 生命周期来确认。

## Materials

- [GPU Mamba 异步对齐路径疑似导致调度卡死](blocks/e93dd4ac9d21f0f214b1cfa2f845fdb06e2b4508f0711e2ecb0d1b7b408e19de.md): 审计未修改文件，当前最强嫌疑是新增 GPU-only Mamba align 异步路径，尤其是 target forward 前的 real-only precopy；`after_mamba_preprocess` 仅表示 host 完成 enqueue，不代表 NPU kernel 完成，阻塞可能延迟至 K3 MoE `token_dispatcher._preprocess`。次级嫌疑是 packed heterogeneous zeroer 的非阻塞 H2D 与立即 zero kernel，证据不足；R8 已排除 double dummy、ordinal/call-count、padding、prefix hit 及特定 graph-only 机制。建议首轮 A/B 整体禁用 `_use_gpu_mamba_align()`，恢复 legacy CPU-planned preprocess/copy/event；若仍卡，再同步化或回退 packed zeroer 生命周期。

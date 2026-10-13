---
blocks:
- block_id: cb0860360c50c88a0cb1ef0a7db5f26d957e0d3013509c8f6c46ea1f866d23b3
  indexed: true
  sha256: 99bce0f897d987fb1287e13690676a32c0229cb407b1306fd53754122703831e
  source_range:
    end: 1336421
    part: 0
    start: 0
  summary: '此前记录称 PR #9 的 PD 分离、KDA 重编译及若干端到端检查已通过，但这些内容属于报告性结果；另发现 P 的 `kv_port=20001` 与 ADXL 端口窗存在随机冲突。针对最新 v0.23.0_ci_new 的 FlashComm1 任务尚未修改代码或启动集群，审计推测 KDA 的独立 q/k/v/gate 投影未命中现有 SequenceColumnParallel 白名单，导致 token shard 与完整 T 的 metadata/cu_seqlens 不一致，并可能触发 AscendC 参数或状态错误；该根因和修复方案尚未实测验证。建议方向包括在 hidden-dependent projection 前执行一次 token all-gather、保留 o_proj 的 reduce-scatter，以及对多模态 layer0、3D block_residual、TP 切分和减层权重进行 off/on token-ID 对齐回归。'
  title: KDA FlashComm1 只读审计与修复定位
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8d409647dbf263239f9f90e534aa30bd4bde4444ccfde45ac01878c2526c6a5f
  kind: experience
  material_digest: 1f14c562d85517908cd6489cbbf3c4fb75033d39d5da63e84faee9ca90de7e15
  revision: f8265c3f42381c98917e92d99ba6ca5f69c9b6a65e2d61a612a48195cdd7c465
  schema: mindie-entry/3
  summary: '当前材料包含两部分：已报告但仍应视为非独立核验的 PR #9/PD 与 KDA 构建运行结果，以及尚未实施的 FlashComm1 KDA 只读审计。最新未完成重点是确认 KDA token-shard、metadata/cu_seqlens、AscendC binding、VL 首层 buffer 与 3D residual 的实际失配，并使用减层权重比较 FlashComm1 开关状态下的完整 token IDs；不要将审计推测、建议回归或此前“通过”报告升级为已验证修复。'
  title: FlashComm1/KDA 任务导航
navigation: '当前材料包含两部分：已报告但仍应视为非独立核验的 PR #9/PD 与 KDA 构建运行结果，以及尚未实施的 FlashComm1 KDA 只读审计。最新未完成重点是确认 KDA token-shard、metadata/cu_seqlens、AscendC binding、VL 首层 buffer 与 3D residual 的实际失配，并使用减层权重比较 FlashComm1 开关状态下的完整 token IDs；不要将审计推测、建议回归或此前“通过”报告升级为已验证修复。'
schema: mindie-material-task/1
status: complete
task_id: 8d409647dbf263239f9f90e534aa30bd4bde4444ccfde45ac01878c2526c6a5f
---

# FlashComm1/KDA 任务导航

当前材料包含两部分：已报告但仍应视为非独立核验的 PR #9/PD 与 KDA 构建运行结果，以及尚未实施的 FlashComm1 KDA 只读审计。最新未完成重点是确认 KDA token-shard、metadata/cu_seqlens、AscendC binding、VL 首层 buffer 与 3D residual 的实际失配，并使用减层权重比较 FlashComm1 开关状态下的完整 token IDs；不要将审计推测、建议回归或此前“通过”报告升级为已验证修复。

## Materials

- [KDA FlashComm1 只读审计与修复定位](blocks/cb0860360c50c88a0cb1ef0a7db5f26d957e0d3013509c8f6c46ea1f866d23b3.md): 此前记录称 PR #9 的 PD 分离、KDA 重编译及若干端到端检查已通过，但这些内容属于报告性结果；另发现 P 的 `kv_port=20001` 与 ADXL 端口窗存在随机冲突。针对最新 v0.23.0_ci_new 的 FlashComm1 任务尚未修改代码或启动集群，审计推测 KDA 的独立 q/k/v/gate 投影未命中现有 SequenceColumnParallel 白名单，导致 token shard 与完整 T 的 metadata/cu_seqlens 不一致，并可能触发 AscendC 参数或状态错误；该根因和修复方案尚未实测验证。建议方向包括在 hidden-dependent projection 前执行一次 token all-gather、保留 o_proj 的 reduce-scatter，以及对多模态 layer0、3D block_residual、TP 切分和减层权重进行 off/on token-ID 对齐回归。

---
blocks:
- block_id: 1dc034ad1a31a86b9ae7b76835426565b8e59e2a0a5ad0db2b8ec173cc445ca8
  indexed: true
  sha256: bb14c160faa1fba498a32590b8e85cf24c9018f3ec70ac7bf7f3e2f03a3868ee
  source_range:
    end: 475051
    part: 0
    start: 0
  summary: 用户质疑 Kimi K3 的 `use_rope=True` 与 modeling 中未使用 cos/sin 的差异，以及 `_restore_per_channel_scale_group_axis` 是否应仅为 SiTU 单独处理；后续助手称已将 W4A8 SiTU 路径拆为独立 fallback，保留 packed int4 权重并将 per-channel scale 的 `[E,N] → [E,1,N]` view 限定在该路径。助手报告删除原辅助函数、未修改 MXFP4，并称定向单元测试分别通过 13 项和 16 项、`py_compile` 与 whitespace 检查通过；这些是报告的验证结果，E2E 未启动，且 cos/sin 精度问题本身未见完成验证。
  title: SiTU 专用 W4A8 fallback 重构与有限验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c43266ca65f6500c765f4519989b8aa0cd48583ab10caa841fb39e6a302412a0
  kind: experience
  material_digest: a906d651b9b19cb0eff177120bb1d27e7c55005d97ff834abb129b7222f8bf85
  revision: aa32332e38ca0cf5e68396f957a5de084bc43fa7a5fda81635a6c1b7cb5091f3
  schema: mindie-entry/3
  summary: 新增材料集中于 W4A8 SiTU 的隔离式 fallback：报告称普通 SwiGLU/Qwen 路径不再执行 SiTU 判断或 scale reshape，packed W4 对象保持原样；报告的局部测试与静态检查结果需与未启动的 E2E、未解决的 RoPE cos/sin 精度疑问及融合算子尚未完成等限制一起理解。
  title: 当前任务导航
navigation: 新增材料集中于 W4A8 SiTU 的隔离式 fallback：报告称普通 SwiGLU/Qwen 路径不再执行 SiTU 判断或 scale reshape，packed W4 对象保持原样；报告的局部测试与静态检查结果需与未启动的 E2E、未解决的 RoPE cos/sin 精度疑问及融合算子尚未完成等限制一起理解。
schema: mindie-material-task/1
status: complete
task_id: c43266ca65f6500c765f4519989b8aa0cd48583ab10caa841fb39e6a302412a0
---

# 当前任务导航

新增材料集中于 W4A8 SiTU 的隔离式 fallback：报告称普通 SwiGLU/Qwen 路径不再执行 SiTU 判断或 scale reshape，packed W4 对象保持原样；报告的局部测试与静态检查结果需与未启动的 E2E、未解决的 RoPE cos/sin 精度疑问及融合算子尚未完成等限制一起理解。

## Materials

- [SiTU 专用 W4A8 fallback 重构与有限验证](blocks/1dc034ad1a31a86b9ae7b76835426565b8e59e2a0a5ad0db2b8ec173cc445ca8.md): 用户质疑 Kimi K3 的 `use_rope=True` 与 modeling 中未使用 cos/sin 的差异，以及 `_restore_per_channel_scale_group_axis` 是否应仅为 SiTU 单独处理；后续助手称已将 W4A8 SiTU 路径拆为独立 fallback，保留 packed int4 权重并将 per-channel scale 的 `[E,N] → [E,1,N]` view 限定在该路径。助手报告删除原辅助函数、未修改 MXFP4，并称定向单元测试分别通过 13 项和 16 项、`py_compile` 与 whitespace 检查通过；这些是报告的验证结果，E2E 未启动，且 cos/sin 精度问题本身未见完成验证。

---
blocks:
- block_id: 44ca6603776fad7b7104e3749c62c4d993561863050ccd80e1e951fbc982fc5f
  indexed: true
  sha256: 40fffd82fd0a093fedff414722105aa84bff8b3363e1575b79fd508052caec82
  source_range:
    end: 485503
    part: 0
    start: 0
  summary: 历史核查记录称，四机 launcher 的 serve 参数指向已脱敏模型路径，实际日志据称进入约 1.38 TiB、352 个权重分片的加载流程，拓扑记录为 TP16 × DP4 × EP64，未见 num_hidden_layers=5 覆盖。该记录区分了前期单机 TP8 使用的 Kimi-K3-int4-5layers 与后续四机精度/性能异常使用的旧版完整 multicard 权重，并称其不是带 -0724 后缀的版本；这些内容属于报告性证据与结论，不能视为独立验证的成功事实。保留的相关术语包括精度异常、连续 ! 响应、launcher、serve、权重分片、multicard、5-layer fixture 与脱敏路径。
  title: 完整 multicard 权重核查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 698354589a3fcefe0052a57749e39531358a6294aed55787af6b8e62fdfcc011
  kind: experience
  material_digest: 6fa8c5452142156131220f3855485d266757f3d49881a8e5448ddf956ce8ca88
  revision: d571da1f4f5e926a3b314e48b9cd06580b7d1a6ae0cac8b376f88b6682d8965d
  schema: mindie-entry/3
  summary: 当前材料集中记录一次历史启动脚本、模型路径和运行日志的只读核查：报告认为异常服务加载了完整旧版 multicard 权重，而非 5-layer fixture，也不是 -0724 版本。该判断依赖已脱敏路径、约 1.38 TiB/352 分片和 TP16 × DP4 × EP64 等记录，仍应作为可检索的非验证性参考；未提供更早导航或独立复核结果。
  title: 任务导航：multicard 权重与精度异常核查
navigation: 当前材料集中记录一次历史启动脚本、模型路径和运行日志的只读核查：报告认为异常服务加载了完整旧版 multicard 权重，而非 5-layer fixture，也不是 -0724 版本。该判断依赖已脱敏路径、约 1.38 TiB/352 分片和 TP16 × DP4 × EP64 等记录，仍应作为可检索的非验证性参考；未提供更早导航或独立复核结果。
schema: mindie-material-task/1
status: complete
task_id: 698354589a3fcefe0052a57749e39531358a6294aed55787af6b8e62fdfcc011
---

# 任务导航：multicard 权重与精度异常核查

当前材料集中记录一次历史启动脚本、模型路径和运行日志的只读核查：报告认为异常服务加载了完整旧版 multicard 权重，而非 5-layer fixture，也不是 -0724 版本。该判断依赖已脱敏路径、约 1.38 TiB/352 分片和 TP16 × DP4 × EP64 等记录，仍应作为可检索的非验证性参考；未提供更早导航或独立复核结果。

## Materials

- [完整 multicard 权重核查记录](blocks/44ca6603776fad7b7104e3749c62c4d993561863050ccd80e1e951fbc982fc5f.md): 历史核查记录称，四机 launcher 的 serve 参数指向已脱敏模型路径，实际日志据称进入约 1.38 TiB、352 个权重分片的加载流程，拓扑记录为 TP16 × DP4 × EP64，未见 num_hidden_layers=5 覆盖。该记录区分了前期单机 TP8 使用的 Kimi-K3-int4-5layers 与后续四机精度/性能异常使用的旧版完整 multicard 权重，并称其不是带 -0724 后缀的版本；这些内容属于报告性证据与结论，不能视为独立验证的成功事实。保留的相关术语包括精度异常、连续 ! 响应、launcher、serve、权重分片、multicard、5-layer fixture 与脱敏路径。

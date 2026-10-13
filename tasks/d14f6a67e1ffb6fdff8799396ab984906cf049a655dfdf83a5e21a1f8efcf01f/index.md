---
blocks:
- block_id: 270dc9c2f011d20b0e7342b2b136b77fa3386669eafc87e70c41cd525c044834
  indexed: true
  sha256: 390be18f645779e06c1785a002209f1a7b719c649b18828ab9432de0040e87b8
  source_range:
    end: 986528
    part: 0
    start: 0
  summary: 来源称已整理独立的 SGLang 四机运行包，固定镜像 digest、rank/拓扑映射、GQA 配置、gamma=7 及 `server_info` 接收率口径，并提供 README、preflight、launch、stop 和 GSM8K runner；具体路径已脱敏。来源还称本地 `bash -n`、`py_compile` 与合成接收率测试通过，runner 在 warmup 后读取 `/server_info` 的 `avg_spec_accept_length`，按 `(AL-1)/7` 计算接收率且可缺少 token IDs；这些是报告的检查结果，不等同于远端运行验证。来源明确表示未向远端同步，也未创建、启动或停止容器；停止脚本据报仅操作 launch 记录并复核完整 container ID，原始要求为算子无需重新编译。
  title: SGLang 四机 Oracle 运行包（据报完成）
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d14f6a67e1ffb6fdff8799396ab984906cf049a655dfdf83a5e21a1f8efcf01f
  kind: experience
  material_digest: 12d0ba7dd67f623abdd70f93629ad50c96e73b73fc5ef87098e4b7dd4962f9df
  revision: 4aaf9a4ce6a7a4c5340c6e3cab51bd53b9b1793c45ba428e3283b2b7573f9240
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，主题是无需重新编译算子前提下的 SGLang 四机独立运行包及其本地静态校验。可检索重点包括容器拓扑与 NPU 暴露继承、镜像 digest、GQA、gamma=7、`/server_info`、`avg_spec_accept_length`、`bash -n`、`py_compile`、合成接收率测试，以及未执行远端容器操作这一限制；所谓完成和测试通过均保留为来源报告，未提升为已验证成功。
  title: 当前任务导航：SGLang 四机 Oracle 运行包
navigation: 本批仅包含一个材料块，主题是无需重新编译算子前提下的 SGLang 四机独立运行包及其本地静态校验。可检索重点包括容器拓扑与 NPU 暴露继承、镜像 digest、GQA、gamma=7、`/server_info`、`avg_spec_accept_length`、`bash -n`、`py_compile`、合成接收率测试，以及未执行远端容器操作这一限制；所谓完成和测试通过均保留为来源报告，未提升为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: d14f6a67e1ffb6fdff8799396ab984906cf049a655dfdf83a5e21a1f8efcf01f
---

# 当前任务导航：SGLang 四机 Oracle 运行包

本批仅包含一个材料块，主题是无需重新编译算子前提下的 SGLang 四机独立运行包及其本地静态校验。可检索重点包括容器拓扑与 NPU 暴露继承、镜像 digest、GQA、gamma=7、`/server_info`、`avg_spec_accept_length`、`bash -n`、`py_compile`、合成接收率测试，以及未执行远端容器操作这一限制；所谓完成和测试通过均保留为来源报告，未提升为已验证成功。

## Materials

- [SGLang 四机 Oracle 运行包（据报完成）](blocks/270dc9c2f011d20b0e7342b2b136b77fa3386669eafc87e70c41cd525c044834.md): 来源称已整理独立的 SGLang 四机运行包，固定镜像 digest、rank/拓扑映射、GQA 配置、gamma=7 及 `server_info` 接收率口径，并提供 README、preflight、launch、stop 和 GSM8K runner；具体路径已脱敏。来源还称本地 `bash -n`、`py_compile` 与合成接收率测试通过，runner 在 warmup 后读取 `/server_info` 的 `avg_spec_accept_length`，按 `(AL-1)/7` 计算接收率且可缺少 token IDs；这些是报告的检查结果，不等同于远端运行验证。来源明确表示未向远端同步，也未创建、启动或停止容器；停止脚本据报仅操作 launch 记录并复核完整 container ID，原始要求为算子无需重新编译。

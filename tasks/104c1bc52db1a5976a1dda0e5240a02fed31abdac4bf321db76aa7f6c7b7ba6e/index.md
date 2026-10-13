---
blocks:
- block_id: a093bae466c94af8835d16ed573935d340a39b62094da6287556817ffb793555
  indexed: true
  sha256: 35b27e629ccc58d7ddcc80fa94404109db7e52cd76c63f8faf25d827371b7db9
  source_range:
    end: 472049
    part: 0
    start: 0
  summary: 记录了对四个 rank 进程、日志及 18077 健康状态的只读监控；期间报告 TP worker 初始化、target 加载进度 123/352，以及错误计数为 0，但这些观察和后续成功结论均来自报告，未独立验证。最终声称 rank0 `/health` 返回 200、Application startup complete、EngineCore 初始化、352 shards 与 DSpark draft 加载完成，并启用 8 rounds probe；同时记录 `enable_qk_norm_rope_fusion=False`、`norm_quant, act_quant`，以及日志中未见 Traceback/Error/Exception。
  title: 四 rank 服务初始化与加载监控
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 104c1bc52db1a5976a1dda0e5240a02fed31abdac4bf321db76aa7f6c7b7ba6e
  kind: experience
  material_digest: 62371f364c06262c43f60b6ca2f18ae6757ae97f0ad4aafb740220c6c254f781
  revision: be4922d135365ebaa76dcd20c1ab703ac72979c64e3c76e7286459f63fbabf6d
  schema: mindie-entry/3
  summary: 目前可检索到一条关于四 rank 只读监控、TP worker 初始化、target shard 加载、18077 `/health`、EngineCore、DSpark draft 及量化配置的材料索引。材料中的“服务已就绪”、加载成功和无错误等均是来源方报告，未被独立验证；未提供更早导航或其他块，因此不能补记省略的上下文。
  title: 当前任务：四 rank 初始化与服务状态索引
navigation: 目前可检索到一条关于四 rank 只读监控、TP worker 初始化、target shard 加载、18077 `/health`、EngineCore、DSpark draft 及量化配置的材料索引。材料中的“服务已就绪”、加载成功和无错误等均是来源方报告，未被独立验证；未提供更早导航或其他块，因此不能补记省略的上下文。
schema: mindie-material-task/1
status: complete
task_id: 104c1bc52db1a5976a1dda0e5240a02fed31abdac4bf321db76aa7f6c7b7ba6e
---

# 当前任务：四 rank 初始化与服务状态索引

目前可检索到一条关于四 rank 只读监控、TP worker 初始化、target shard 加载、18077 `/health`、EngineCore、DSpark draft 及量化配置的材料索引。材料中的“服务已就绪”、加载成功和无错误等均是来源方报告，未被独立验证；未提供更早导航或其他块，因此不能补记省略的上下文。

## Materials

- [四 rank 服务初始化与加载监控](blocks/a093bae466c94af8835d16ed573935d340a39b62094da6287556817ffb793555.md): 记录了对四个 rank 进程、日志及 18077 健康状态的只读监控；期间报告 TP worker 初始化、target 加载进度 123/352，以及错误计数为 0，但这些观察和后续成功结论均来自报告，未独立验证。最终声称 rank0 `/health` 返回 200、Application startup complete、EngineCore 初始化、352 shards 与 DSpark draft 加载完成，并启用 8 rounds probe；同时记录 `enable_qk_norm_rope_fusion=False`、`norm_quant, act_quant`，以及日志中未见 Traceback/Error/Exception。

---
blocks:
- block_id: 99631060e2fd2b337d1d2d6c0a58470ec29cb44edacbf28b4d20194b32095647
  indexed: true
  sha256: 7c608d9a3bad5a67506dba0c92c45e4388d62d309f1ad47c3f7a64e71827b7fc
  source_range:
    end: 5588524
    part: 0
    start: 0
  summary: 对比记录认为 SGLang 使用独立 draft 物理 token_to_kv_pool，但与 target 共享 req_to_token_pool、allocator 和逻辑 slot；当前 PR 则将 draft cache 嵌入 target 共享物理页。FIA 的 seq_lens 路径被描述为每轮入口集中执行 NPU→CPU，同步并未消除；当前方案尝试用 accepted-count side-stream、CPU mirror 和显式 metadata plan 缩小同步范围，但运行时收益仍需 trace 验证。后续启动 profiling 的早期尝试在 TP8、TP16/DP1 下因 attention TP all-gather shape/communicator 问题失败，相关日志和不确定性应保留。
  title: SGLang DSpark KV 与 FIA 同步对比
- block_id: 7d17e6ad69508bdea46c335275f7e987577119a522fb3d7f0eaa7fb46052da87
  indexed: true
  sha256: 39546d7540289dc654056981c8e0b81a5edd599b0217e766e1c5b8ac73cddd7e
  source_range:
    end: 5588524
    part: 1
    start: 0
  summary: 历史参数纠正了单机拓扑判断：TP16×DP1 不可直接替代四机配置，最终按用户指定改为 TP8×DP2，并在逻辑 NPU 8–15 上成功完成图捕获与 health 200。一次精确 8192 input→5 output 请求在 profiler 窗口内完成，随后从 8 个 rank 中选择并校验 DP0/TP0/EP0 单卡 trace；记录称包含 FIA 和 NPU graph replay 事件，并将 trace_view.json 与 manifest.json 传入 Downloads。该结果来自源材料中的执行记录，路径、机器地址、权重标识和哈希均含有脱敏或截断信息，不应据此重建私有细节。
  title: TP8×DP2 图模式 SGLang 单卡 Profiling
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 85607cc35aafbbdf7b348fd2a67fc8cdae09c43d303cd0af1e4e5ea63d978c64
  kind: experience
  material_digest: 88b6107829d122bbb9960039766720ba691fcea2dd5dd73b3aafe7f39302f943
  revision: 913ecfe12ea42b5f3412627d5788013dccf7d6a0797efa95650d1e594e8179e2
  schema: mindie-entry/3
  summary: 当前任务包含两条可检索线索：SGLang DSpark 的独立 draft KV pool、共享逻辑 slot 及 FIA CPU seq_lens 同步机制；以及在单机 TP8×DP2、GQA draft、图模式下完成 8192→5 请求并提取 DP0/TP0/EP0 单卡 profiling。早期 TP8/TP16×DP1 启动曾因 attention TP all-gather shape/communicator 失败，后来通过 TP8×DP2 修正；同步优化和 profiling 性能结论仍应视为待运行时验证的参考，而非已验证案例。
  title: SGLang DSpark 对比与 GQA Profiling
navigation: 当前任务包含两条可检索线索：SGLang DSpark 的独立 draft KV pool、共享逻辑 slot 及 FIA CPU seq_lens 同步机制；以及在单机 TP8×DP2、GQA draft、图模式下完成 8192→5 请求并提取 DP0/TP0/EP0 单卡 profiling。早期 TP8/TP16×DP1 启动曾因 attention TP all-gather shape/communicator 失败，后来通过 TP8×DP2 修正；同步优化和 profiling 性能结论仍应视为待运行时验证的参考，而非已验证案例。
schema: mindie-material-task/1
status: complete
task_id: 85607cc35aafbbdf7b348fd2a67fc8cdae09c43d303cd0af1e4e5ea63d978c64
---

# SGLang DSpark 对比与 GQA Profiling

当前任务包含两条可检索线索：SGLang DSpark 的独立 draft KV pool、共享逻辑 slot 及 FIA CPU seq_lens 同步机制；以及在单机 TP8×DP2、GQA draft、图模式下完成 8192→5 请求并提取 DP0/TP0/EP0 单卡 profiling。早期 TP8/TP16×DP1 启动曾因 attention TP all-gather shape/communicator 失败，后来通过 TP8×DP2 修正；同步优化和 profiling 性能结论仍应视为待运行时验证的参考，而非已验证案例。

## Materials

- [SGLang DSpark KV 与 FIA 同步对比](blocks/99631060e2fd2b337d1d2d6c0a58470ec29cb44edacbf28b4d20194b32095647.md): 对比记录认为 SGLang 使用独立 draft 物理 token_to_kv_pool，但与 target 共享 req_to_token_pool、allocator 和逻辑 slot；当前 PR 则将 draft cache 嵌入 target 共享物理页。FIA 的 seq_lens 路径被描述为每轮入口集中执行 NPU→CPU，同步并未消除；当前方案尝试用 accepted-count side-stream、CPU mirror 和显式 metadata plan 缩小同步范围，但运行时收益仍需 trace 验证。后续启动 profiling 的早期尝试在 TP8、TP16/DP1 下因 attention TP all-gather shape/communicator 问题失败，相关日志和不确定性应保留。
- [TP8×DP2 图模式 SGLang 单卡 Profiling](blocks/7d17e6ad69508bdea46c335275f7e987577119a522fb3d7f0eaa7fb46052da87.md): 历史参数纠正了单机拓扑判断：TP16×DP1 不可直接替代四机配置，最终按用户指定改为 TP8×DP2，并在逻辑 NPU 8–15 上成功完成图捕获与 health 200。一次精确 8192 input→5 output 请求在 profiler 窗口内完成，随后从 8 个 rank 中选择并校验 DP0/TP0/EP0 单卡 trace；记录称包含 FIA 和 NPU graph replay 事件，并将 trace_view.json 与 manifest.json 传入 Downloads。该结果来自源材料中的执行记录，路径、机器地址、权重标识和哈希均含有脱敏或截断信息，不应据此重建私有细节。

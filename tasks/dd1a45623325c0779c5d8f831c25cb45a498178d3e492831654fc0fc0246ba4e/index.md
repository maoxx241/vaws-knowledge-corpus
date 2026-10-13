---
blocks:
- block_id: 1075f7f4a222743d8632ec3d5416355fa59ff4304255bd66662dddf70719c62f
  indexed: true
  sha256: c204b645bae1cbf0c320c76aceb62c5fab7bee0dc73e36b7ed87796853b15154
  source_range:
    end: 883098
    part: 0
    start: 0
  summary: 材料围绕 Kimi K3 多机多卡精度与功能问题，提出用裁剪专家权重、固定输入和单变量实验，把 4 机 64 卡现象逐步缩小到请求、token、layer、block 及物理缓存 backing。记录了 Prefix Cache、Graph Replay、Hybrid KV、dummy state 复用和异步事件等假设，以及 128K→896 token、layer 23 首次 NaN、跨层物理别名等观测；这些是可追溯的调试证据，不等同于完整验证的单一根因。材料还区分了不同问题支线：`9998aa44e` 解释 41+ task 的 HardEvent 卡死，但不解释全 `!` 精度故障；16-expert 裁剪模型适合调试循环，不能作为最终语义精度门槛。
  title: K3 精度问题的 Agent 递进定位主线
- block_id: cef1bb61ecf7eb81749cd0e672a4c49a942331709cf6e04b0458f87c788af789
  indexed: true
  sha256: 6b487fa539e6404840debaee49555fe325b48a2e03a1b15adf8a8326e4ee539b
  source_range:
    end: 883098
    part: 1
    start: 0
  summary: 材料描述 Agent 将 64 个 worker 的日志统一为 request、rank、step、stage、layer、group、block、finite_count、hash 等字段，再按业务身份对齐，而不是依赖日志行号或墙钟时间。示例结果显示 16 个 TP rank 在 warm 后经过一次 0-token idle wave，下一次真实执行入口均出现损坏，从而排除单 rank 掉线并将范围收窄到缓存归属、dummy replay 与状态复用；但这仍是定位窗口，不是唯一 producer 或最终根因的证明。材料保留了采集器曾把同一 request 的三个 prefill chunk 当成三个请求的错误，并记录了通过 request ID 去重修正，以及后续以定向 Tensor dump 验证最早异常读取点的方法。
  title: 多卡日志结构化与最早差异提取
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: dd1a45623325c0779c5d8f831c25cb45a498178d3e492831654fc0fc0246ba4e
  kind: experience
  material_digest: d53641dc55766c76d319644e6f83831a3ceb2b72fc8bc640a3815ac153ecabcf
  revision: 83826ea53514035a594f1ff005b489e389fd7f88c4df697183335ed553773b0b
  schema: mindie-entry/3
  summary: 当前材料聚焦两条互补主线：一是通过固定输入、单变量实验、最小复现和物理地址追踪，将多机多卡精度异常逐级收敛；二是将多卡日志编译成可比较的时间线，提取全 rank 一致的最早差异。已记录的观察包括 warm hit 后全 `!`、layer 23 首见 NaN、0-token idle wave、物理 backing 别名，以及 41+ task 的独立 HardEvent 卡死支线；这些结论保留其适用边界、失败尝试和未完成验证，不能泛化为所有故障的已证实根因。
  title: K3 Agent 精度定位材料索引
navigation: 当前材料聚焦两条互补主线：一是通过固定输入、单变量实验、最小复现和物理地址追踪，将多机多卡精度异常逐级收敛；二是将多卡日志编译成可比较的时间线，提取全 rank 一致的最早差异。已记录的观察包括 warm hit 后全 `!`、layer 23 首见 NaN、0-token idle wave、物理 backing 别名，以及 41+ task 的独立 HardEvent 卡死支线；这些结论保留其适用边界、失败尝试和未完成验证，不能泛化为所有故障的已证实根因。
schema: mindie-material-task/1
status: complete
task_id: dd1a45623325c0779c5d8f831c25cb45a498178d3e492831654fc0fc0246ba4e
---

# K3 Agent 精度定位材料索引

当前材料聚焦两条互补主线：一是通过固定输入、单变量实验、最小复现和物理地址追踪，将多机多卡精度异常逐级收敛；二是将多卡日志编译成可比较的时间线，提取全 rank 一致的最早差异。已记录的观察包括 warm hit 后全 `!`、layer 23 首见 NaN、0-token idle wave、物理 backing 别名，以及 41+ task 的独立 HardEvent 卡死支线；这些结论保留其适用边界、失败尝试和未完成验证，不能泛化为所有故障的已证实根因。

## Materials

- [K3 精度问题的 Agent 递进定位主线](blocks/1075f7f4a222743d8632ec3d5416355fa59ff4304255bd66662dddf70719c62f.md): 材料围绕 Kimi K3 多机多卡精度与功能问题，提出用裁剪专家权重、固定输入和单变量实验，把 4 机 64 卡现象逐步缩小到请求、token、layer、block 及物理缓存 backing。记录了 Prefix Cache、Graph Replay、Hybrid KV、dummy state 复用和异步事件等假设，以及 128K→896 token、layer 23 首次 NaN、跨层物理别名等观测；这些是可追溯的调试证据，不等同于完整验证的单一根因。材料还区分了不同问题支线：`9998aa44e` 解释 41+ task 的 HardEvent 卡死，但不解释全 `!` 精度故障；16-expert 裁剪模型适合调试循环，不能作为最终语义精度门槛。
- [多卡日志结构化与最早差异提取](blocks/cef1bb61ecf7eb81749cd0e672a4c49a942331709cf6e04b0458f87c788af789.md): 材料描述 Agent 将 64 个 worker 的日志统一为 request、rank、step、stage、layer、group、block、finite_count、hash 等字段，再按业务身份对齐，而不是依赖日志行号或墙钟时间。示例结果显示 16 个 TP rank 在 warm 后经过一次 0-token idle wave，下一次真实执行入口均出现损坏，从而排除单 rank 掉线并将范围收窄到缓存归属、dummy replay 与状态复用；但这仍是定位窗口，不是唯一 producer 或最终根因的证明。材料保留了采集器曾把同一 request 的三个 prefill chunk 当成三个请求的错误，并记录了通过 request ID 去重修正，以及后续以定向 Tensor dump 验证最早异常读取点的方法。

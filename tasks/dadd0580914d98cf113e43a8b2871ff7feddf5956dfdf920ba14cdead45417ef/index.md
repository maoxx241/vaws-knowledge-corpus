---
blocks:
- block_id: 7c2f068e968bbd2697432cf70dce753c5eec777404a24458ea9f1be9b622d3cb
  indexed: true
  sha256: 84d245538c955ed6a86baa5a806e4012be760c9df45fbf4312fac1294c116f9c
  source_range:
    end: 1102057
    part: 0
    start: 0
  summary: 针对 <redacted:user-path:9aecf20e59bf> 的 R21 任务，已定位 draft token 从 producer 经 stream/event、GPU/CPU view 到 postprocess 的待核对链路；当前未编辑或触发运行。观察到采样完成事件早于 draft 记录，而 Mamba postprocess 仅等待该事件，可能与当前流上的 DSpark draft 并行，尚未确认是缺少 draft→postprocess 依赖还是 draft 内部另一流未收敛；KV/state 与 slot 映射仍待核对。
  title: R21 draft token 链路竞争窗口
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: dadd0580914d98cf113e43a8b2871ff7feddf5956dfdf920ba14cdead45417ef
  kind: experience
  material_digest: 06bc1f08d10786a318b88b4fa43717c90cb131ff70ef0c3c80fc9f7603abc9c9
  revision: c1c51a6ab676610ce2bcd8ef6e965175b6dd7a3e933f0c44789ec07d13184aa1
  schema: mindie-entry/3
  summary: 当前上下文聚焦于 DSpark draft、采样完成事件与 Mamba postprocess 之间的 stream/event 同步竞争。已报告一个潜在竞争窗口，但尚未验证为实际故障或成功修复；后续应继续核对 KV/state、slot 映射及跨流收敛关系，并保留“缺少 draft→postprocess 依赖”与“draft 内部流未收敛”两种未决解释。
  title: R21 draft token 同步与依赖排查
navigation: 当前上下文聚焦于 DSpark draft、采样完成事件与 Mamba postprocess 之间的 stream/event 同步竞争。已报告一个潜在竞争窗口，但尚未验证为实际故障或成功修复；后续应继续核对 KV/state、slot 映射及跨流收敛关系，并保留“缺少 draft→postprocess 依赖”与“draft 内部流未收敛”两种未决解释。
schema: mindie-material-task/1
status: complete
task_id: dadd0580914d98cf113e43a8b2871ff7feddf5956dfdf920ba14cdead45417ef
---

# R21 draft token 同步与依赖排查

当前上下文聚焦于 DSpark draft、采样完成事件与 Mamba postprocess 之间的 stream/event 同步竞争。已报告一个潜在竞争窗口，但尚未验证为实际故障或成功修复；后续应继续核对 KV/state、slot 映射及跨流收敛关系，并保留“缺少 draft→postprocess 依赖”与“draft 内部流未收敛”两种未决解释。

## Materials

- [R21 draft token 链路竞争窗口](blocks/7c2f068e968bbd2697432cf70dce753c5eec777404a24458ea9f1be9b622d3cb.md): 针对 <redacted:user-path:9aecf20e59bf> 的 R21 任务，已定位 draft token 从 producer 经 stream/event、GPU/CPU view 到 postprocess 的待核对链路；当前未编辑或触发运行。观察到采样完成事件早于 draft 记录，而 Mamba postprocess 仅等待该事件，可能与当前流上的 DSpark draft 并行，尚未确认是缺少 draft→postprocess 依赖还是 draft 内部另一流未收敛；KV/state 与 slot 映射仍待核对。

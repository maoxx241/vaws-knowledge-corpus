---
blocks:
- block_id: 992b686cbb19f7e6902da90e636a8894afb09916d0bca0cbaf72580b8a03e5e6
  indexed: true
  sha256: 32df996c92a43f672cbd45ae13e7cc913724a043f18a3f8b6a80f17711041b9c
  source_range:
    end: 1123530
    part: 0
    start: 0
  summary: 只读分析报告称，r10 唯一坏样本是 DP0 第 3 个并发请求：答案前缀“30”后在 close token 后持续输出 `!`，且在首个 FULL size16 图前已生成至少 26 个 completion token，因此乱码起点被判断位于此前的 `NONE` 段。报告记录 r10 的 `NONE 1152→128→848→96→32` 与随后 `FULL16×5` 时序；首个 FULL 时 DP0 为 1 个真实请求并 padding 到 16 token/2 req，DP1–3 为同规格 dummy，target hidden 在 L52 起 NaN，但这些观察未验证根因。与 r9 对比后，early-wait 被认为只改变合批/收缩节奏；提出的 15 个 case、max_tokens=8 的 A/B，以及在 `_update_states` 首次 mutation 前加入 `torch.npu.synchronize()`，均属于后续计划而非已完成修复。
  title: r10 并发请求乱码与 NaN 时序分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1af23873c33d37a7d1ff33c159f28d5946194f1a10e4b5e0d093871f2a89de9b
  kind: experience
  material_digest: 082212655a9021c7b8b119c725cbfa8be7dc4ea8e21fcf42ae27b276bd71d27a
  revision: f562115808459121acbbc32e46dafda48f61309d6dcc53d47b8e47558e7c8412
  schema: mindie-entry/3
  summary: 当前唯一材料聚焦于 r10 坏请求在 `NONE` 段的乱码起点、首个 FULL 图前后的 DP dummy/padding 形态，以及 L52 起 NaN 的观测。early-wait 尚未证明修复根因；待验证方向包括 survivor condense/reorder、初始 NONE 输入或状态构造，以及 accepted-token event 与 stream 同步覆盖。
  title: 当前任务导航：r10 并发乱码与 NaN 排查
navigation: 当前唯一材料聚焦于 r10 坏请求在 `NONE` 段的乱码起点、首个 FULL 图前后的 DP dummy/padding 形态，以及 L52 起 NaN 的观测。early-wait 尚未证明修复根因；待验证方向包括 survivor condense/reorder、初始 NONE 输入或状态构造，以及 accepted-token event 与 stream 同步覆盖。
schema: mindie-material-task/1
status: complete
task_id: 1af23873c33d37a7d1ff33c159f28d5946194f1a10e4b5e0d093871f2a89de9b
---

# 当前任务导航：r10 并发乱码与 NaN 排查

当前唯一材料聚焦于 r10 坏请求在 `NONE` 段的乱码起点、首个 FULL 图前后的 DP dummy/padding 形态，以及 L52 起 NaN 的观测。early-wait 尚未证明修复根因；待验证方向包括 survivor condense/reorder、初始 NONE 输入或状态构造，以及 accepted-token event 与 stream 同步覆盖。

## Materials

- [r10 并发请求乱码与 NaN 时序分析](blocks/992b686cbb19f7e6902da90e636a8894afb09916d0bca0cbaf72580b8a03e5e6.md): 只读分析报告称，r10 唯一坏样本是 DP0 第 3 个并发请求：答案前缀“30”后在 close token 后持续输出 `!`，且在首个 FULL size16 图前已生成至少 26 个 completion token，因此乱码起点被判断位于此前的 `NONE` 段。报告记录 r10 的 `NONE 1152→128→848→96→32` 与随后 `FULL16×5` 时序；首个 FULL 时 DP0 为 1 个真实请求并 padding 到 16 token/2 req，DP1–3 为同规格 dummy，target hidden 在 L52 起 NaN，但这些观察未验证根因。与 r9 对比后，early-wait 被认为只改变合批/收缩节奏；提出的 15 个 case、max_tokens=8 的 A/B，以及在 `_update_states` 首次 mutation 前加入 `torch.npu.synchronize()`，均属于后续计划而非已完成修复。

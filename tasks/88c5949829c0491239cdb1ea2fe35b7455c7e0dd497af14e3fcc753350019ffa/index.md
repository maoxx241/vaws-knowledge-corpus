---
blocks:
- block_id: be57838e611a725344c87c7f6696a59c73d235ada0818021c307bb23ddf07141
  indexed: true
  sha256: c9f9f1587cad20af33e63846feeb6a7d3fa8c218be09c9bd29d106391e30d10a
  source_range:
    end: 3559697
    part: 0
    start: 0
  summary: 源码与运行时排查显示，真实 DSpark checkpoint 使用 `layers.0..4`，而非 `layers.93..97`；aux hidden 宽度为 35,840，vLLM 已按 TP 顺序 all-gather、去 padding 并拼接 5×7168，因漏 gather 导致差异的假设可排除。probe 对比中两边第一个 base top 均为 12046，但 margin 分别为 5.875 和 0.125，且从第二个 block 位置明显分叉；当前仍怀疑 mask query 的 attention 可见性/布局、残差流物化或加载映射，但未证明根因。四机 probe8 的服务与 launcher 已区分，guard-only 检查通过；仅完成只读核对，未停止服务或修改文件，报告中的 TERM 命令是建议而非已执行结果。
  title: 首步分叉排查与 probe8 进程核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 88c5949829c0491239cdb1ea2fe35b7455c7e0dd497af14e3fcc753350019ffa
  kind: experience
  material_digest: 110cf39d6fc6c2d50b6afc056c08f94d7ab1f2dbcfbb8d067507c960e639dfab
  revision: 153034b09581eb1238d328277537bf6e96dae26c6c38e7da2625b6a7fb88ff13
  schema: mindie-entry/3
  summary: 已确认 checkpoint 层号为 `layers.0..4`，aux capture 的 TP gather 不是已证实问题；第一个 base top 一致但 margin 不同，从第二个 block 起分叉。后续重点仍是 query token/position/mask、attention 可见性与 layout、Kimi 残差流及实际加载映射，均未形成验证结论。probe8 四机服务身份和 PGID 已只读确认，未执行停止；原始块仍可独立检索，不能据此声称完整记住被省略的早期细节。
  title: 当前任务导航：首步 top-id 分叉与 probe8 只读核对
navigation: 已确认 checkpoint 层号为 `layers.0..4`，aux capture 的 TP gather 不是已证实问题；第一个 base top 一致但 margin 不同，从第二个 block 起分叉。后续重点仍是 query token/position/mask、attention 可见性与 layout、Kimi 残差流及实际加载映射，均未形成验证结论。probe8 四机服务身份和 PGID 已只读确认，未执行停止；原始块仍可独立检索，不能据此声称完整记住被省略的早期细节。
schema: mindie-material-task/1
status: complete
task_id: 88c5949829c0491239cdb1ea2fe35b7455c7e0dd497af14e3fcc753350019ffa
---

# 当前任务导航：首步 top-id 分叉与 probe8 只读核对

已确认 checkpoint 层号为 `layers.0..4`，aux capture 的 TP gather 不是已证实问题；第一个 base top 一致但 margin 不同，从第二个 block 起分叉。后续重点仍是 query token/position/mask、attention 可见性与 layout、Kimi 残差流及实际加载映射，均未形成验证结论。probe8 四机服务身份和 PGID 已只读确认，未执行停止；原始块仍可独立检索，不能据此声称完整记住被省略的早期细节。

## Materials

- [首步分叉排查与 probe8 进程核对](blocks/be57838e611a725344c87c7f6696a59c73d235ada0818021c307bb23ddf07141.md): 源码与运行时排查显示，真实 DSpark checkpoint 使用 `layers.0..4`，而非 `layers.93..97`；aux hidden 宽度为 35,840，vLLM 已按 TP 顺序 all-gather、去 padding 并拼接 5×7168，因漏 gather 导致差异的假设可排除。probe 对比中两边第一个 base top 均为 12046，但 margin 分别为 5.875 和 0.125，且从第二个 block 位置明显分叉；当前仍怀疑 mask query 的 attention 可见性/布局、残差流物化或加载映射，但未证明根因。四机 probe8 的服务与 launcher 已区分，guard-only 检查通过；仅完成只读核对，未停止服务或修改文件，报告中的 TERM 命令是建议而非已执行结果。

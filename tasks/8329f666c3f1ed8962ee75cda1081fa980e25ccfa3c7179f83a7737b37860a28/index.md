---
blocks:
- block_id: fecd49099c41c6ba598097d9b876aaaa79b5b347db1c51d2653dbce30f705266
  indexed: true
  sha256: 1f0d6b3ec04d3259d3175b7e9992fa2983abc94dab6a8ee0a51ef7100629efd2
  source_range:
    end: 1277517
    part: 0
    start: 0
  summary: 核查结论认为，若 TH 指逻辑布局 `[T,H]`，`compressor_metadata` 的 `rope_cos`/`rope_sin` 输入要求二维 TH；注册层面的物理 Format 仍是通用 `FORMAT_ND`，不能将 TH 直接等同为独立物理格式。其输出为 `[T_compressed,1,1,H]`，下游再 view 成 TH；同时需区分 `compressor_metadata` 与主 `compressor`，后者的 `x` 路径据称支持 TH 和 BSH。核查发生在 detached HEAD 且存在大量未解决冲突的环境中，因此这些结论是可检索的参考判断，不是已验证的案例报告；其中 `H` 被解释为 RoPE/head dimension，而非模型 hidden_size。
  title: compressor_metadata 的 TH 输入布局与输出形状
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8329f666c3f1ed8962ee75cda1081fa980e25ccfa3c7179f83a7737b37860a28
  kind: experience
  material_digest: b30b5eb2e4bfc3e72377b05ed38219349b05403b094fcefdcb8413ccd98e6603
  revision: 575646f668aff3ab884cb1bd7a7d62b0870a3849ca9c213839d732b5f6975661
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，记录了 `compressor_metadata` 输入为二维 `[T,H]` 的判断、`FORMAT_ND` 与逻辑布局的区分、输出 `[T_compressed,1,1,H]`，以及与主 `compressor` 的 TH/BSH 能力差异。相关源码位置使用了脱敏路径；原始核查环境包含 detached HEAD 和未解决冲突，结论应视为可失败的参考索引，不能将报告中的“结论已经比较清楚”升级为独立验证成功。
  title: 当前任务导航：compressor_metadata 的 TH 支持核查
navigation: 本批仅包含一个材料块，记录了 `compressor_metadata` 输入为二维 `[T,H]` 的判断、`FORMAT_ND` 与逻辑布局的区分、输出 `[T_compressed,1,1,H]`，以及与主 `compressor` 的 TH/BSH 能力差异。相关源码位置使用了脱敏路径；原始核查环境包含 detached HEAD 和未解决冲突，结论应视为可失败的参考索引，不能将报告中的“结论已经比较清楚”升级为独立验证成功。
schema: mindie-material-task/1
status: complete
task_id: 8329f666c3f1ed8962ee75cda1081fa980e25ccfa3c7179f83a7737b37860a28
---

# 当前任务导航：compressor_metadata 的 TH 支持核查

本批仅包含一个材料块，记录了 `compressor_metadata` 输入为二维 `[T,H]` 的判断、`FORMAT_ND` 与逻辑布局的区分、输出 `[T_compressed,1,1,H]`，以及与主 `compressor` 的 TH/BSH 能力差异。相关源码位置使用了脱敏路径；原始核查环境包含 detached HEAD 和未解决冲突，结论应视为可失败的参考索引，不能将报告中的“结论已经比较清楚”升级为独立验证成功。

## Materials

- [compressor_metadata 的 TH 输入布局与输出形状](blocks/fecd49099c41c6ba598097d9b876aaaa79b5b347db1c51d2653dbce30f705266.md): 核查结论认为，若 TH 指逻辑布局 `[T,H]`，`compressor_metadata` 的 `rope_cos`/`rope_sin` 输入要求二维 TH；注册层面的物理 Format 仍是通用 `FORMAT_ND`，不能将 TH 直接等同为独立物理格式。其输出为 `[T_compressed,1,1,H]`，下游再 view 成 TH；同时需区分 `compressor_metadata` 与主 `compressor`，后者的 `x` 路径据称支持 TH 和 BSH。核查发生在 detached HEAD 且存在大量未解决冲突的环境中，因此这些结论是可检索的参考判断，不是已验证的案例报告；其中 `H` 被解释为 RoPE/head dimension，而非模型 hidden_size。

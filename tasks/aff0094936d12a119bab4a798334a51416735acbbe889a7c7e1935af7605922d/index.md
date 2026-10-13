---
blocks:
- block_id: e510f6ad5f45eabb44cf7410bae08980612d3e46110cf907cbf35edbe9d05bfe
  indexed: true
  sha256: 387df379d0633b730fb6f729f2b3da950b4fd2335cb51181d956a23d1e1b08ca
  source_range:
    end: 1025899
    part: 0
    start: 0
  summary: 只读审查指出两个必须修正的问题：不能仅用 `is_vl_model()` 判断首层全局布局，应结合 `multimodal_config.enable_mm_embeds` 或 `vision_chunk`，且 decoder 与 KDA 必须共用该判定；首层本地输出行数应使用 `N // TP`，避免与 fake op 及 TorchDynamo 动态 shape 表达式不一致。其余 KDA gather、reduce-scatter、残差形状和 metadata 设计被认为正确，但这些是审查结论而非已验证成功；建议将非真实 UT 的 `N=5, TP=2` 改为已 padding 的 `N=6, TP=2`。未修改文件，也未启动远端服务。
  title: KDA 首层布局与 token 尺寸审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: aff0094936d12a119bab4a798334a51416735acbbe889a7c7e1935af7605922d
  kind: experience
  material_digest: 56f02423dbb6d05f07e05ba08b52d4d83647bc58c0bbe334452e294cdc08c17c
  revision: e152ff62191e8633b3af58e411b52efc3128461690ed3d43af0ee250f29ebd49
  schema: mindie-entry/3
  summary: 当前批次包含一份只读审查结论，重点是修正 VL 首层全局布局判定和本地 token 行数 floor 计算，并保留对编译 shape guard、UT 参数及未验证状态的记录。其余实现路径仅被审查为合理，尚未通过实际修改、测试或远端服务验证；原始材料仍可独立检索。
  title: 当前任务导航：KDA 首层布局审查
navigation: 当前批次包含一份只读审查结论，重点是修正 VL 首层全局布局判定和本地 token 行数 floor 计算，并保留对编译 shape guard、UT 参数及未验证状态的记录。其余实现路径仅被审查为合理，尚未通过实际修改、测试或远端服务验证；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: aff0094936d12a119bab4a798334a51416735acbbe889a7c7e1935af7605922d
---

# 当前任务导航：KDA 首层布局审查

当前批次包含一份只读审查结论，重点是修正 VL 首层全局布局判定和本地 token 行数 floor 计算，并保留对编译 shape guard、UT 参数及未验证状态的记录。其余实现路径仅被审查为合理，尚未通过实际修改、测试或远端服务验证；原始材料仍可独立检索。

## Materials

- [KDA 首层布局与 token 尺寸审查](blocks/e510f6ad5f45eabb44cf7410bae08980612d3e46110cf907cbf35edbe9d05bfe.md): 只读审查指出两个必须修正的问题：不能仅用 `is_vl_model()` 判断首层全局布局，应结合 `multimodal_config.enable_mm_embeds` 或 `vision_chunk`，且 decoder 与 KDA 必须共用该判定；首层本地输出行数应使用 `N // TP`，避免与 fake op 及 TorchDynamo 动态 shape 表达式不一致。其余 KDA gather、reduce-scatter、残差形状和 metadata 设计被认为正确，但这些是审查结论而非已验证成功；建议将非真实 UT 的 `N=5, TP=2` 改为已 padding 的 `N=6, TP=2`。未修改文件，也未启动远端服务。

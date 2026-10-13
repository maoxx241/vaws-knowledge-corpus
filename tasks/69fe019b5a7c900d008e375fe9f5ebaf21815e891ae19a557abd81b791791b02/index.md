---
blocks:
- block_id: e45ef555e6300be919950440a902a54b334ddc2e01dc5b462a80eacf2b730b71
  indexed: true
  sha256: 19e56fcb0141a0760757e15f473e7c48523ec46638ba6e36f22af4ca3e279b61
  source_range:
    end: 1190070
    part: 0
    start: 0
  summary: 复核结论认为 `mxfp4-pack-quantized` 的 checkpoint 加载字段应使用 `*_weight_packed`，加载后再归一化为运行时字段 `weight/w13_weight/w2_weight`；这是报告的方案结论，并非已验证的完整案例。Kimi K3 loader 需要映射真实的 `.experts.routed_experts.*` 参数路径，普通与 packed 权重/scale 的 UT 被提醒由主任务补齐，当前未确认已完成。本子任务未修改文件；尝试运行量化 UT 时因本地 Python 环境缺少 `torch`，pytest 未进入用例执行。
  title: 量化加载 ABI 与 Kimi K3 loader 复核
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 69fe019b5a7c900d008e375fe9f5ebaf21815e891ae19a557abd81b791791b02
  kind: experience
  material_digest: 0bfdfe6f3dd43207b7af45033a0c0b1b9589851b7912f6ad9156073d265c9f05
  revision: 63ddd69ff09774f6a2cd1dd4c08a72ce4b637efb4cdaa9da3125440663b5da15
  schema: mindie-entry/3
  summary: 当前新增材料集中于 mxfp4 packed checkpoint 字段与标准运行时字段的分离、Kimi K3 routed_experts 参数路径，以及量化 UT 的未完成验证。已知限制是本地缺少 `torch` 导致 pytest 未执行；普通/packed 权重与 scale UT 仍需以实际结果确认，不应视为已验证成功。
  title: 当前任务导航：量化加载 ABI 与 Kimi K3 loader
navigation: 当前新增材料集中于 mxfp4 packed checkpoint 字段与标准运行时字段的分离、Kimi K3 routed_experts 参数路径，以及量化 UT 的未完成验证。已知限制是本地缺少 `torch` 导致 pytest 未执行；普通/packed 权重与 scale UT 仍需以实际结果确认，不应视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 69fe019b5a7c900d008e375fe9f5ebaf21815e891ae19a557abd81b791791b02
---

# 当前任务导航：量化加载 ABI 与 Kimi K3 loader

当前新增材料集中于 mxfp4 packed checkpoint 字段与标准运行时字段的分离、Kimi K3 routed_experts 参数路径，以及量化 UT 的未完成验证。已知限制是本地缺少 `torch` 导致 pytest 未执行；普通/packed 权重与 scale UT 仍需以实际结果确认，不应视为已验证成功。

## Materials

- [量化加载 ABI 与 Kimi K3 loader 复核](blocks/e45ef555e6300be919950440a902a54b334ddc2e01dc5b462a80eacf2b730b71.md): 复核结论认为 `mxfp4-pack-quantized` 的 checkpoint 加载字段应使用 `*_weight_packed`，加载后再归一化为运行时字段 `weight/w13_weight/w2_weight`；这是报告的方案结论，并非已验证的完整案例。Kimi K3 loader 需要映射真实的 `.experts.routed_experts.*` 参数路径，普通与 packed 权重/scale 的 UT 被提醒由主任务补齐，当前未确认已完成。本子任务未修改文件；尝试运行量化 UT 时因本地 Python 环境缺少 `torch`，pytest 未进入用例执行。

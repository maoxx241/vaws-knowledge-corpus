---
blocks:
- block_id: bb1ee9bc2b449dc2728ef4c14b9720c810514d2e23191c00f5c2c80bf188f2ec
  indexed: true
  sha256: b6eb37f3dc9cf85cf21afa7e4f257b6cf9600c6fc26d21ad04e605648dfc79d7
  source_range:
    end: 1998519
    part: 0
    start: 0
  summary: 计划用“老虎与草地”语义识别题和“四车拼图”空间理解题各跑两题，预期答案分别涉及老虎/草地及编号 3；这些是测试用例与判定规则，不是已验证的模型成功结果。来源称两张公开测试图片已下载至 `/tmp`，并记录了来源 URL、SHA256、prompt 和判定规则，但本块未展示实际验证过程或结果。Launcher 据称支持第四参数 `vl`，会移除 `--language-model-only` 并将 `vision_chunk` 从 `0` 改为 `1`；仅报告 `bash -n` 通过，未修改 workspace 文件或操作远端。
  title: 多模态语义题与 VL 启动参数准备
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f1094a4df8976b01f986b7e4f79863cfc2561e1f110841774ccc7e0294454138
  kind: experience
  material_digest: c76159dc1202ecc82a0a90a1ae892ef4833c12f41760c1da1052d2688b134d63
  revision: a78aa9f9c123544a4bf2e818b83ebf5f929528e1f654f485152e3eb46ac9dee3
  schema: mindie-entry/3
  summary: 当前材料聚焦于准备两类必须看图的多模态题，以及通过 `vl` 参数切换视觉配置；已报告启动脚本语法检查通过，但尚未提供实际推理输出、答案核验或失败/修正结果。图片路径、来源和校验信息据称已整理在 `/tmp`，原始材料仍应独立检索，不能将预期答案或准备状态视为验证成功。
  title: 多模态 VL 测试准备索引
navigation: 当前材料聚焦于准备两类必须看图的多模态题，以及通过 `vl` 参数切换视觉配置；已报告启动脚本语法检查通过，但尚未提供实际推理输出、答案核验或失败/修正结果。图片路径、来源和校验信息据称已整理在 `/tmp`，原始材料仍应独立检索，不能将预期答案或准备状态视为验证成功。
schema: mindie-material-task/1
status: complete
task_id: f1094a4df8976b01f986b7e4f79863cfc2561e1f110841774ccc7e0294454138
---

# 多模态 VL 测试准备索引

当前材料聚焦于准备两类必须看图的多模态题，以及通过 `vl` 参数切换视觉配置；已报告启动脚本语法检查通过，但尚未提供实际推理输出、答案核验或失败/修正结果。图片路径、来源和校验信息据称已整理在 `/tmp`，原始材料仍应独立检索，不能将预期答案或准备状态视为验证成功。

## Materials

- [多模态语义题与 VL 启动参数准备](blocks/bb1ee9bc2b449dc2728ef4c14b9720c810514d2e23191c00f5c2c80bf188f2ec.md): 计划用“老虎与草地”语义识别题和“四车拼图”空间理解题各跑两题，预期答案分别涉及老虎/草地及编号 3；这些是测试用例与判定规则，不是已验证的模型成功结果。来源称两张公开测试图片已下载至 `/tmp`，并记录了来源 URL、SHA256、prompt 和判定规则，但本块未展示实际验证过程或结果。Launcher 据称支持第四参数 `vl`，会移除 `--language-model-only` 并将 `vision_chunk` 从 `0` 改为 `1`；仅报告 `bash -n` 通过，未修改 workspace 文件或操作远端。

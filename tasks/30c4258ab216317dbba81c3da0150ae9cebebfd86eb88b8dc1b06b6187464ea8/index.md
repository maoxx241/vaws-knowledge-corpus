---
blocks:
- block_id: 7832af131f209092c59fcede3340b42008352a3fd6821655869ffdfd1b189edf
  indexed: true
  sha256: 1cc8c98d628b3c051f4dd35ec5b1f27169051fb34613bbedec35de39995a86df
  source_range:
    end: 642728
    part: 0
    start: 0
  summary: 只读复审认为新增的两个 guard 语义正确，但若要真正 fail-fast，应移动到 `super().__init__()` 之前；测试还应确认非法配置时父初始化未被调用。报告称当前启动脚本无条件固定 MRV1，并据此判断本次四机验证较安全，但这属于报告中的检查结论而非独立验证。复审不建议全局拒绝 MRV2 DSpark，指出 vLLM v0.25.1 存在独立实现；作为通用 PR，建议仅对 Kimi K3 target、Qwen3DSparkModel GQA draft 与 MRV2 的组合做窄范围拒绝，长期方案是为 MRV2 补充 capture-mode 设置。
  title: MRV1/MRV2 配置拒绝与 fail-fast 复审
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 30c4258ab216317dbba81c3da0150ae9cebebfd86eb88b8dc1b06b6187464ea8
  kind: experience
  material_digest: 858b2e7aadd63072e9190071eaa8347493d159fea19bdb46d9e4a62642a2e81c
  revision: 4f306033641e492220773f37e7bf74134ada8d84c45a052efbd59d8a8734bcdd
  schema: mindie-entry/3
  summary: 现有材料集中于初始化前 guard 的时机、非法配置测试覆盖，以及 MRV1/MRV2 DSpark 的适用边界。需保留的未决点包括：启动脚本固定 MRV1 的检查范围、MRV2 是否需要代码级窄范围拒绝，以及 materialized auxiliary stream 修复和 capture-mode 设置在 MRV2 中尚未完成或未被验证。原始材料仍应按 block_id 独立检索。
  title: 当前任务导航：MRV 配置与 fail-fast 复审
navigation: 现有材料集中于初始化前 guard 的时机、非法配置测试覆盖，以及 MRV1/MRV2 DSpark 的适用边界。需保留的未决点包括：启动脚本固定 MRV1 的检查范围、MRV2 是否需要代码级窄范围拒绝，以及 materialized auxiliary stream 修复和 capture-mode 设置在 MRV2 中尚未完成或未被验证。原始材料仍应按 block_id 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 30c4258ab216317dbba81c3da0150ae9cebebfd86eb88b8dc1b06b6187464ea8
---

# 当前任务导航：MRV 配置与 fail-fast 复审

现有材料集中于初始化前 guard 的时机、非法配置测试覆盖，以及 MRV1/MRV2 DSpark 的适用边界。需保留的未决点包括：启动脚本固定 MRV1 的检查范围、MRV2 是否需要代码级窄范围拒绝，以及 materialized auxiliary stream 修复和 capture-mode 设置在 MRV2 中尚未完成或未被验证。原始材料仍应按 block_id 独立检索。

## Materials

- [MRV1/MRV2 配置拒绝与 fail-fast 复审](blocks/7832af131f209092c59fcede3340b42008352a3fd6821655869ffdfd1b189edf.md): 只读复审认为新增的两个 guard 语义正确，但若要真正 fail-fast，应移动到 `super().__init__()` 之前；测试还应确认非法配置时父初始化未被调用。报告称当前启动脚本无条件固定 MRV1，并据此判断本次四机验证较安全，但这属于报告中的检查结论而非独立验证。复审不建议全局拒绝 MRV2 DSpark，指出 vLLM v0.25.1 存在独立实现；作为通用 PR，建议仅对 Kimi K3 target、Qwen3DSparkModel GQA draft 与 MRV2 的组合做窄范围拒绝，长期方案是为 MRV2 补充 capture-mode 设置。

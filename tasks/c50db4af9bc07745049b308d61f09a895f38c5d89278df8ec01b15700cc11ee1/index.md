---
blocks:
- block_id: fd8ad5e749dd4d8846206c0b44829e6e32fc263ec7b72161a7ab00b9df3ab000
  indexed: true
  sha256: 36071d422ce0e7bc018f6d30a7b18527eb20f3ac40f3682a83e38e9d2b4058aa
  source_range:
    end: 558097
    part: 0
    start: 0
  summary: 记录称问题被判断为上游依赖不匹配，而非 ViT 计算问题：PR 注入 image_mode=None，但 vLLM v0.26 的 MediaConnector 通过显式参数和 **kwargs 重复构造 ImageMediaIO，触发 `multiple values for keyword argument 'image_mode'`；该能力据称直到提交 `15cb8e140` 才完整实现。建议在 renderer UT 中使用真实 MediaConnector 覆盖同步、异步图像获取，并将 K3 media kwargs 默认层作为修复重点；但这些仍是建议与当前观察，未记录修复已验证成功，且旧版 ImageMediaIO 是否支持以 None 保留原始模式仍是不确定点。
  title: vLLM v0.26 的 K3 图像参数重复传参
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c50db4af9bc07745049b308d61f09a895f38c5d89278df8ec01b15700cc11ee1
  kind: experience
  material_digest: 538e78434d843e4b94c7cb369c29a032425b647e24d3583cc92f1f6a090504f1
  revision: a37541d65e3cc1cf204a2274d9043ed38c848670ad761d463a4841d3e499e8d1
  schema: mindie-entry/3
  summary: 当前唯一材料聚焦 vLLM v0.26 中 K3 media kwargs 的 `image_mode` 重复传参错误，涉及 MediaConnector、ImageMediaIO、同步/异步 renderer 和 `15cb8e140` 上游变更。建议的真实连接器回归测试、默认层修复及 mock 断言调整尚未被记录为已验证成功；原始材料应独立检索，不能据此补全未提供的先前上下文。
  title: 任务导航：K3/vLLM 图像媒体参数兼容性
navigation: 当前唯一材料聚焦 vLLM v0.26 中 K3 media kwargs 的 `image_mode` 重复传参错误，涉及 MediaConnector、ImageMediaIO、同步/异步 renderer 和 `15cb8e140` 上游变更。建议的真实连接器回归测试、默认层修复及 mock 断言调整尚未被记录为已验证成功；原始材料应独立检索，不能据此补全未提供的先前上下文。
schema: mindie-material-task/1
status: complete
task_id: c50db4af9bc07745049b308d61f09a895f38c5d89278df8ec01b15700cc11ee1
---

# 任务导航：K3/vLLM 图像媒体参数兼容性

当前唯一材料聚焦 vLLM v0.26 中 K3 media kwargs 的 `image_mode` 重复传参错误，涉及 MediaConnector、ImageMediaIO、同步/异步 renderer 和 `15cb8e140` 上游变更。建议的真实连接器回归测试、默认层修复及 mock 断言调整尚未被记录为已验证成功；原始材料应独立检索，不能据此补全未提供的先前上下文。

## Materials

- [vLLM v0.26 的 K3 图像参数重复传参](blocks/fd8ad5e749dd4d8846206c0b44829e6e32fc263ec7b72161a7ab00b9df3ab000.md): 记录称问题被判断为上游依赖不匹配，而非 ViT 计算问题：PR 注入 image_mode=None，但 vLLM v0.26 的 MediaConnector 通过显式参数和 **kwargs 重复构造 ImageMediaIO，触发 `multiple values for keyword argument 'image_mode'`；该能力据称直到提交 `15cb8e140` 才完整实现。建议在 renderer UT 中使用真实 MediaConnector 覆盖同步、异步图像获取，并将 K3 media kwargs 默认层作为修复重点；但这些仍是建议与当前观察，未记录修复已验证成功，且旧版 ImageMediaIO 是否支持以 None 保留原始模式仍是不确定点。

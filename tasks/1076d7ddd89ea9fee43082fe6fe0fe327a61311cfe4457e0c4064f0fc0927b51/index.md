---
blocks:
- block_id: 8f305db9c01ec637d0286911da0eed3cb54c43f48b3753e972005783408baabb
  indexed: true
  sha256: e95d408cc8f9d954ecda32f70def12d261cb078597c50204057264bdc15b66ec
  source_range:
    end: 533021
    part: 0
    start: 0
  summary: 只读审查报告认为 `_apply_attention_residual` 的 shape anchor、VL/TP-local 与 KDA gather 布局链路自洽，语言和图片请求都会经过 `inputs_embeds` 路径；但这只是审查判断，尚未验证真实服务。UT 覆盖了首层布局、KDA gather 标签、数值不变和 anchor 调用，却未复现“compile 小尺寸、run 大尺寸”的动态形状回归；仍需真实服务重编译并检查 layer1 `g_proj` 后 rearrange 的首维是否为符号表达式。建议按语言/图片 × FlashComm off/on 的四种 case 各重复两次，并以 off/on token IDs 完全一致作为验收条件。
  title: FlashComm 动态形状与验收前审查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1076d7ddd89ea9fee43082fe6fe0fe327a61311cfe4457e0c4064f0fc0927b51
  kind: experience
  material_digest: 1f290012d0553cf8b28f70edb75f0ba1dc8788caa085c64185d6d0fbe2be0586
  revision: 03c3369b3b0b79b0ac3007f149b750a756194e935549329b8526c4eb114abb58
  schema: mindie-entry/3
  summary: 现有材料是一份保留不确定性的只读审查：代码路径和 UT 被认为自洽，但真实 FlashComm-on 服务、跨尺寸动态形状以及四组输出一致性尚未完成验证。后续重点是重编译并运行语言/图片的 off/on case，检查 layer1 reshape/rearrange 形状表达式、失败点是否消失及 token IDs 是否一致；不要将审查结论或建议视为已验证成功。
  title: 当前任务导航：FlashComm 真实重跑与动态形状验证
navigation: 现有材料是一份保留不确定性的只读审查：代码路径和 UT 被认为自洽，但真实 FlashComm-on 服务、跨尺寸动态形状以及四组输出一致性尚未完成验证。后续重点是重编译并运行语言/图片的 off/on case，检查 layer1 reshape/rearrange 形状表达式、失败点是否消失及 token IDs 是否一致；不要将审查结论或建议视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 1076d7ddd89ea9fee43082fe6fe0fe327a61311cfe4457e0c4064f0fc0927b51
---

# 当前任务导航：FlashComm 真实重跑与动态形状验证

现有材料是一份保留不确定性的只读审查：代码路径和 UT 被认为自洽，但真实 FlashComm-on 服务、跨尺寸动态形状以及四组输出一致性尚未完成验证。后续重点是重编译并运行语言/图片的 off/on case，检查 layer1 reshape/rearrange 形状表达式、失败点是否消失及 token IDs 是否一致；不要将审查结论或建议视为已验证成功。

## Materials

- [FlashComm 动态形状与验收前审查](blocks/8f305db9c01ec637d0286911da0eed3cb54c43f48b3753e972005783408baabb.md): 只读审查报告认为 `_apply_attention_residual` 的 shape anchor、VL/TP-local 与 KDA gather 布局链路自洽，语言和图片请求都会经过 `inputs_embeds` 路径；但这只是审查判断，尚未验证真实服务。UT 覆盖了首层布局、KDA gather 标签、数值不变和 anchor 调用，却未复现“compile 小尺寸、run 大尺寸”的动态形状回归；仍需真实服务重编译并检查 layer1 `g_proj` 后 rearrange 的首维是否为符号表达式。建议按语言/图片 × FlashComm off/on 的四种 case 各重复两次，并以 off/on token IDs 完全一致作为验收条件。

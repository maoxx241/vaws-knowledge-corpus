# FIA 非连续缓存验证：先纠正被测入口和形状，再追 stride 丢失

2026 年 8 月的一次 A3 / CANN 9.1 适配中，希望 FIA 直接消费首轴非连续的 paged KV cache。早期测试显示两例通过，但复查发现，合成 GQA 形状没有真正覆盖目标路径，调用 stock `torch_npu` 与加载自定义 OPP 的结果也被混在一起。原来的通过结论因此撤回。

随后按真实 TP 局部维度重新构造 view：GQA 使用 4 个 query head、1 个 KV head、64 维；MLA 使用 6 个局部 head 并补到 8，latent 与 RoPE 分别为 512 和 64 维。被测输入保持非连续，连续副本和独立 FP32 实现用于对照；同时确认 custom vendor 优先级、动态库和实际 `_C_ascend` 入口。

原始失败记录中，MLA 非连续结果最大绝对误差约为 0.771，而连续对照约为 0.002。继续沿 Tensor descriptor、host tiling 和 device `ConstInfo` 排查，修补 stride 传递后，同一组真实形状得到两例通过，GQA 与 MLA 最大绝对误差分别约为 0.00102 和 0.00196。

这段经历说明，修复前后的同输入对照比一份脱离调用条件的通过记录更有解释力。结果只覆盖当时两个单算子案例：未证明内部绝对没有临时拷贝，也未证明完整服务、其他芯片或后续版本可用。相关历史工作见 [PR #13821](https://github.com/vllm-project/vllm-ascend/pull/13821)，复用时应重新检查目标版本的实际入口。

# 沿模型计算链纠正对 K3 NoPE 与 MLA prefill 的推断

2026-07 至 08 月的一次 Kimi K3 适配审查中，配置字段和 MLA 名称引出了两种误判：看到 `use_rope` 就推断 target 一定旋转位置分量，又把压缩 KV 存储理解成 prefill 必须按单头 MQA 计算。实际工作是沿模型构造、MLA adapter 和 prefill 分支逐段追踪，而不是改配置后试运气。

保存的源码读回显示，target 调用传入 `rotary_emb=None`，并将 `use_mla_rope` 设为 false；位置切片仍可参与 attention，但没有据此执行旋转。prefill 则展开对应 head 后计算 attention，压缩 cache 的表示与计算阶段的 head 形式是不同问题。旧版 identity cos/sin 只是接口占位，不能作为模型使用 RoPE 的证据。

这次产出是源码层面的解释和适配边界，没有新增 NPU 数值验收。带 RoPE 的 draft 应按自己的 attention group 检查，不能继承 target 的 NoPE 判断；早期禁用某项 MLA preprocess 优化也只反映当时接口限制，不能推广到后来的 A5 路径。[公开模型适配 PR](https://github.com/vllm-project/vllm-ascend/pull/14600)。

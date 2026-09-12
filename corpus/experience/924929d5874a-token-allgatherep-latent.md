# 从首 token 分叉发现 AllGatherEP 的 latent 归约位置错误

2026-07-24，Kimi K3 的两条 MoE 并行路径在同一验证输入上得到不同首 token。调查没有把所有 AllGather 都视为同一种操作，而是区分 token 拼接和 expert partial 求和，再沿 latent MoE 的 norm/up-projection 前后检查数据。

原始前后对照中，修前两条路径分别生成不同 token；把各 rank 的 routed latent 在 RMSNorm 前归约完整之后，首 token 和后续两个 token 对齐。逐层 tensor 检查仍存在约 0.0009766 的小量差异，但同 rank 汇聚后的 spread 为零。配套六项定向测试通过。它说明当时漏掉的归约位置影响模型数值，而不是简单的 tokenizer 或服务错误。

另一次 g_proj 重复 gather 的问题属于 token 轴重复拼接，不能与本次 expert partial 求和混成一个根因。逐 token 运算可与 token 分片调度组合，但把 partial 直接送入非线性 norm 会改变结果。本次短序列与局部比较不等于完整模型精度或性能验收，也未验证后来设计的任意 TP-sharded down/up 扩展。

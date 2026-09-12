# 在同一 engine 上补齐 cold→hit→reset 的 prefix 验证

2026-07-23，验证 Kimi K3 的 prefix cache 时，最初“启动参数已打开”和“连续两次请求成功”都不足以说明缓存被命中、状态也正确复用。于是使用保留真实 INT4 路径的五层缩减 fixture，在同一个 engine 内构造超过完整 block 的公共前缀，依次运行 cold、hit 和 reset 后的 cold。

原始结果在 eager 与 FULL_DECODE_ONLY 两个模式中均记录 cached tokens 为 0→1536→0，cold/hit 的三个生成 token 相同，chosen logprob、累计 logprob 和 top-20 排序/值差均为零。reset 返回成功，测试后进程也退出。这个组合比只比较文本多证明了“确实命中”和“重置回冷态”两件事。

边界仍很窄：它只覆盖该五层 fixture、输入和图模式，不证明完整模型、多 DP 或长请求历史都正确。之后遇到 align 与 cache-off none 在零命中时就有差异，应先检查 chunk 边界和执行分支，不能把差异直接归咎于状态恢复。这个案例也没有把缩减模型通过升级为完整 checkpoint 精度结论。

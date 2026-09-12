# 用同一请求确认 GQA draft 需要物化后的残差输入

2026-08-23，Kimi K3 迁移后 target 能正常生成，但旧 GQA DSpark 的采信与延迟没有回到已知基线。调查追到 target 输出给 draft 的 auxiliary stream：旧 GQA checkpoint 消费 attention-residual mixer 物化后的输入，MLA draft 则沿上游 raw stream，二者不能只按相同层号互换。

模型和 runner 增加明确的 capture-mode 选择，分别覆盖 GQA 与 MLA。随后在 A3 四节点 DP4×TP16×EP64 环境，使用同一个 8192-token 输入与 1024-token 输出设置，重置 prefix cache 后做两轮请求。原始第二轮记录四个 DP 请求全部成功，逐 rank 的输出检查通过，并保存了公共前缀比例、图档、请求 hash、采信计数和 TPOT，避免把不同输入或 warm 状态的数字拿来直接比较。

这次调查支持“draft 输入表示不匹配”这一具体集成修正。没有把两轮请求的收益推广成通用性能结论，也没有用另一个 draft 或 GPQA 分数替代同请求对照。模型、checkpoint 家族或 capture 入口发生变化时，层号偏移规则和 stream 选择必须重新核对。[历史模型集成记录](https://github.com/vllm-project/vllm-ascend/pull/14454)。

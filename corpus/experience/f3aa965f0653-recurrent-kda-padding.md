# 用状态未更新的红测识别 recurrent KDA 图 padding 漏写

2026-07-23，AscendC recurrent KDA 替换验证遇到一种服务仍可能健康、有效 decode 却被漏算的问题。最小 A3 用例采用 TP16 的局部形状：六个 head、维度 128，图容量 16，实际只有一个 token，`cu_seqlens` 为 `[0,1,1,…]`。旧 kernel 在逐行计算前要求累计有效长度等于总容量，因 1 不等于 16 而整体返回。

没有把未定义 padding 是否为零当成主断言，而是比较有效输出、active state slot 和其余未参与状态。原始红测显示 state 有 49,583 个元素不匹配、最大差约 0.564；放宽容量校验并跳过零长度行后，clean build 的七项 NPU 用例和八项源码/契约用例全部通过。这里的“15 passed”不能写成十五项 NPU 测试。

九月的后续审查又修正了“必须做两次 mask”的说法：kernel 未写尾部与框架最终输出清理是不同责任，norm-gate 后仍可能从 NaN gate 产生 NaN。只保留最终 mask 的推理没有独立删除式图回放验收，不能当作已经验证的优化。此文记录两次结论收窄，不承诺任意图档或整网均已通过。[历史算子 PR](https://github.com/vllm-ascend/vllm-ascend-kimi-k3/pull/10)。

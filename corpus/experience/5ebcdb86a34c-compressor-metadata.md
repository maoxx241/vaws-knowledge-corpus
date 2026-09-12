# Compressor metadata 复用：从对象身份改为语义组，并检查真实消费者

2026 年 8 月，减少 Compressor metadata 的跨层重复计算时，首版按对象身份和压缩比缓存。审阅发现，同压缩比不代表同一份物理 block table，MTP/DSpark 的多个子步也可能在一个 composite forward 内切换位置和长度。随后移除对象身份方案，改用显式 cache group，并在子步切换时清理旧结果。

后续 mixed prefill/decode 排查又发现，历史分支还需要隔离不同 metadata 类型。调试没有停留在“缓存读回等于缓存写入”，而是同时比较现算值、复用值以及 Compressor 真正收到的 cos/sin 和 slot mapping，防止缓存正确、接线错误仍被漏过。

图模式下，直接插入同步和落盘会改变观察对象。探针因此改为图中仅保存 device 快照，到实际输出 materialize 的图外边界再做 D2H。原始记录能看到 replay、实际消费者和分组信息，已覆盖案例的复用及消费比较没有发现差异。

这个排除结果没有关闭全部精度问题：当时最长约 1.9 万 token 的覆盖不能替代完整 65K 验收，批次 padding 也不能冒充真实请求数。相关历史工作见 [PR #14932](https://github.com/vllm-project/vllm-ascend/pull/14932) 和 [PR #14994](https://github.com/vllm-project/vllm-ascend/pull/14994)。后来实现已增加预计算与事件路径，旧缓存 key 不能直接作为当前规范；可复用的是按语义组、子步和实际消费边界定位问题的方法。

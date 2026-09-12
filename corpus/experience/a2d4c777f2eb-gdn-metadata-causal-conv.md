# 按 GDN 业务 metadata 复现 causal-conv 时没有重现越界

2026-06-22 在 Ascend A3 调查 causal-conv 越界时，复现脚本没有直接复制孤立单测的参数，而是先走 GDN metadata 构造，再分别按 AscendC 与 Triton wrapper 的业务转换组织输入。固定 vLLM 967c5c3bc、vLLM-Ascend fc9230c42，采用 Qwen3.5 大模型形状、MTP=3、cache mode=none。

保存的 TP8、batch=128 用例里，本地通道维 1536、卷积宽度 4、状态长度 6，query_start_loc 每次增加四，输出为 [512,1536]。两种实现都返回 status=ok 和 finite=true；TP16 的对应形状也留有成功输出。这些具体记录没有重现目标越界。Triton 在新形状首次执行时出现较长耗时，因此没有将一次慢返回直接归因于越界或永久卡死。

阴性结果缩小了下一轮调查范围：需要从真实故障补齐 cache mode、state index 分布或 graph 参数，再做有区分力的复现。有限值与正常返回并不等于两个输出逐元素一致，脚本总耗时也不是独立 kernel 性能排名。本例不覆盖 cache align、其他索引分布或 graph capture，更不代表当前版本已完成全面正确性验证。

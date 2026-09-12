# 审阅 metadata 独立流时，区分了 host 提交与 device 执行

2026 年 9 月审阅 [PR #15443](https://github.com/vllm-project/vllm-ascend/pull/15443) 的一个历史版本时，“异步 metadata”容易被理解成连 Python planning、dispatch 和 tiling 都搬到了后台。调查逐段检查 model runner、任务提交函数、算子 schema 和 consumer 等待位置。

原始代码显示，`submit()` 在 NPU stream 上下文中直接循环调用 `task.run()`；上下文改变设备工作的提交流，Python closure 仍在原线程执行。输入 ready event、分组完成 event 和上一轮 buffer reusable fence 则分别约束输入可读、消费者可用和持久 buffer 可覆盖。仅把 closure 放进 stream 并不能解释这些生命周期。

另外，算子参数虽然主要是设备 Tensor 与标量，标量计算仍依赖 CPU mirror；缺失 mirror 的 fallback 也可能引入同步。某些 metadata 算子返回临时 Tensor 后再复制到持久 buffer，因此“使用持久输出”不能直接宣传成热路径没有分配。

审阅把后续验收问题收紧为可测量的重叠窗口、host enqueue gap、首次 consumer 等待和复用 fence 等待，而没有用架构目标代替收益。该次没有实施优化，也没有独立端到端 TPOT/p99 实验；后续 PR 变化不继承这份历史判断。保留这段经历，是为了避免把设备异步提交误写成 CPU 异步或已实现性能提升。

# FlashMLA 缓存调查：完整 KV 的 reader 与切片 writer 需要分别核对

2026 年 9 月，调查一条 FlashMLA 适配分支时，最初用“首轴非连续”概括缓存布局，容易漏掉写入视图的限制。该分支按 token 排列 512 维 NoPE 与 64 维 PE，reader 接收完整 576 维 KV；writer 则分别取得两个末维切片。

沿分配、调用和 kernel 源码检查后发现，两份切片的最后一维 stride 仍为 1，但相邻 token 的跨度保留为 576，PE 还带有 512 元素的起始偏移。若跨 block 另有 padding，writer 又需要处理额外 block stride。因此 reader 能读取完整 KV，并不能证明 writer 能正确原地更新其组件。kernel 内搬到 L1 后的布局也不能反推 GM 缓存排列。

调查进一步分别阅读 ScatterNdUpdate 和 ScatterPaKvCache 的 API、tiling 与架构实现，纠正了将某一架构多维 view 路径直接套用到另一架构的建议，也收紧了二维 slot view 的前提：只有 block/token 轴确实可合并时，才可期待 view 保持共享存储。

相关适配见 [PR #15336](https://github.com/vllm-project/vllm-ascend/pull/15336)。这次证据是源码与接口调查，没有核实安装包是否包含目标实现，更没有完成 A5 编译或 NPU 运行。它保留的是读写双方分别列 shape、stride、offset 并追到底层寻址的经验，不是当前缓存布局或支持矩阵。

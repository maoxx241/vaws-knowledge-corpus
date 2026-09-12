# 显存调查中先纠正混比，再识别残差 storage 的真实回归

2026-07-25，Kimi K3 的峰值 activation 随 max model length 上涨曾被当成直接因果。回读启动参数发现，被比较的服务同时改变了视觉编码器设置和 batch tokens，不能把总差额归给最大长度。进一步在同一五层 fixture、TP8、eager、视觉关闭、batch tokens 固定 8192 的条件下，只改变 max model length 为 1024、8192、327680，原始 worker 日志的 peak activation 都约为 1.97 GiB。

这组对照只否定了该受控场景中的长度解释，没有证明所有配置下显存与长度无关。八月 v0.27 又出现了另一个真实问题：预分配全局 residual bank 后，即便只持有局部 slice，底层大 storage 仍然存活。它需要按 TP-local 的生命周期检查分配/复用，不能拿七月“没复现长度效应”否定后来的回归。

公开经验保留的是这两次调查的区分：profile peak increase 可能含延迟创建的持久 buffer；从 tensor shape 估容量时，还要看引用的整块 storage。这里没有发布完整模型权重体积或内部容量基线，也不把旧 profile 数字用于当前部署预算。

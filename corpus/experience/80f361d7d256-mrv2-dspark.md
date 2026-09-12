# MRV2 无草稿路径通过后仍单独保留 DSpark 热缓存失败

2026-08-28，Kimi K3 进入 Model Runner V2 的验证首先遇到 hybrid cache 初始化、split-KV zeroing 和 Mamba spec 包装的接口问题。已有 MRV1 用例无法覆盖这些入口，因此分别补上 cache-shape、虚拟 block、padding/shared storage 和 Mamba 容量测试，再运行 A3 TP16/DP1 的缩减专家模型。

保存的无草稿 graph-flow 原始结果显示，cold/hit/reset 的 cached tokens 为 0/1536/0，输出 token 一致，边界和并发请求成功。CPU/metadata 结果为 70 passed，NPU zeroer 五项通过。公开验收将业务范围限制在无 draft 的单节点文本路径。

DSpark 是另一组实验：启动和 HTTP 200 没有带来热 prefix token 一致，后续记录仍有 warm 分歧。它没有被无草稿成功或 CPU 测试数覆盖；rebase 后也未把旧 NPU 结果冒充新 head 实测。本文记录当时如何保留失败范围，不把旧“已知问题”升级为今天仍不支持的结论，也不承诺完整权重精度、跨节点或 P/D 支持。[当时的 MRV2 PR](https://github.com/vllm-project/vllm-ascend/pull/15199)。

# MoE Hash 调查：功能同名不等于实际链接了同一接口

2026 年 9 月，定位 DeepSeek V4 的专家路由算子时，先在 ops-transformer 找到 `moe_gating_top_k` 目录，容易由此认定模型已经使用上游 V2。继续检查 Python router 和 C++ binding，才确认当时模型的 sqrtsoftplus 路径实际调用自带的 `moe_gating_top_k_hash`，最终符号为 `aclnnMoeGatingTopKHash`。

另一处更正是将 Hash 与 sqrtsoftplus 分开理解。Hash 决定如何选择专家，sqrtsoftplus 决定如何计算分数；后者并不是 V2 独有功能。历史上游文档和源码还要求在 V2 中同时提供 input IDs 与映射表才能进入 Hash 模式。这些条件需要逐个对照，不能从目录名或归一化模式推断完整能力。

最终调查给出了两张不同的映射：一张是公共算子的功能与参数，另一张是模型实际调用的 Python/C++/ACLNN 链路。这样也避免把自带 Hash 的某次设备测试当成上游 V2 的支持证据，或把某版上游的架构限制套到另一实现上。

本案例只保留公共源码调查过程，不携带其他私有模型中的集成及硬件结果。没有执行上游 V2 的独立 NPU 验收，也没有把历史支持表当作今天的安装包保证。后来复用这项经验时，应从目标 consumer 出发确认实际符号，再查该符号对应版本和架构的参数契约。

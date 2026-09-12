# 混合缓存改造中用反向测试发现 group 语义被丢失

2026-08-07，Kimi K3 的旧混合 prefix cache 路径对齐公共命中长度时，只裁剪了首个 full-attention group。为避免单凭代码猜测，把多 full-attention group 构成的用例放回旧实现：原始测试明确失败；换成遍历相关 groups 的修正后，定向用例及该文件四项测试通过。这里闭环的是旧布局下的裁剪行为，并未以四项测试替代整网精度。

2026-08-27 的另一次分组改造又遇到不同问题：用 `UniformTypeKVCacheSpecs` 包装后，worker 或 scheduler 的容量推导可能没有沿每层 Mamba spec 保留 speculative blocks。调查同时追踪了两端容量计算，补上包装后的场景；保存的测试结果为 89 passed、2 skipped，明确未重跑完整 NPU 服务。

两次经历共同提醒了重构风险：外层类型和 group 形状改变时，旧分支可能仍能运行，却遗漏内部容量或命中边界。它们是不同日期、不同修补，不能合成一次硬件验证。后续 main 存在保留更长命中的例外，不能把旧“全裁剪”规则无条件搬过去。[相关历史缓存修复](https://github.com/vllm-project/vllm-ascend/pull/13277)。

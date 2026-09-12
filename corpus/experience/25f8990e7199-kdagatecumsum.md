# KdaGateCumsum 参数错误最终追到不完整的算子安装产物

2026-07-21，Kimi K3 的 P/D 启动在首次 prefill 前后报 `KdaGateCumsum` 错误，曾怀疑算子接口失效或传输路径。下钻运行日志后，真正更早的错误是 `ParseDynamicKernelConfig` 和 `Failed to ParseDynamicKernels`；此时业务层的参数错误只是后续包装。

调查对照源码、binding、vendor manifest、二进制和实际加载位置。原始清单读回显示，旧安装条目不完整；重建产物中 Gate、Chunk、Layout 三个算子的 binary list 分别有 3、7、3 项。安装后用独立小形状直调，再回到服务验证。另一次 recurrent KDA 修改也发现增量构建复用了旧 kernel，clean build 后二进制 hash 才变化。

这次经历说明“源码相同”“某个 .o 相同”或“build 返回成功”都不能替代加载产物一致性。toolkit 版本也要按实际路径/库确认，当时日志使用 CANN 9.0.1，不能依据镜像命名写成 9.1。上述数字只描述旧构建事故；不要求所有未来版本具有相同 binary list，更不把 A5 交叉编译当成 A5 设备执行。[历史 P/D 集成记录](https://github.com/vllm-ascend/vllm-ascend-kimi-k3/pull/9)。

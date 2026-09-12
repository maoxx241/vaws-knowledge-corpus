# named tool 的 finish_reason 误报来自验证脚本

2026-07-30，Kimi K3 的工具调用验证把返回结果判为 parser/serving 错误。检查实际请求和脚本后发现，请求发送的是 named tool choice，断言却强制要求 `finish_reason="tool_calls"`。调查随后直接读取当时 vLLM v0.26 和 main 的 serving 分支，而不是先修改模型 parser。

两份公开源码都区分了 auto/required 与 named 调用：当时 named 分支保留 `stop`，脚本预期与生产契约不一致。于是结论收窄为修正该用例的精确预期；若想覆盖 `tool_calls` 的完成语义，应构造对应的 required 请求，不能通过模糊接受任意 finish_reason 掩盖测试目标。

该次审查结果建立在保存的请求构造、断言与上游分支上，不宣称重新跑过整套工具服务。这篇只记录所读历史版本中一次“先验证测试是否测对”的经历；应用到其他版本时，应重新核对目标版本源码和协议测试。

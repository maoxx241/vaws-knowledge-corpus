# 一次 GDN 投影优化审阅发现 FP 单测没有覆盖量化调用路径

2026-09-04 审阅 vLLM-Ascend 的 GDN 投影优化候选 91665f45 时，改动把一次模块调用换成两次直接读取 weight 的 F.linear。新增单测使用普通浮点 Parameter，并要求不执行模块 forward，因此单测通过并不能回答量化模型是否仍保持原语义。

调查从 Qwen3.5 的 in_proj_qkvz 构造追到 ModelSlim fused-layer 映射、加载后权重处理和 quant_method.apply。该候选的 W8A8 路径还承担输入量化、scale 与专用 matmul；存储权重会经历转置和 NZ 转换。直接切加载后的权重绕过了这些步骤，源码已足以指出等价性缺口。另一条关于 bias 的猜测被撤回，因为此处构造明确为 bias=False。

随后回读同一 head 的 CI：测试选择成功，但选中的硬件任务实际 skipped，ci-gate 失败；这不是 NPU 已运行并报数值错误。审阅据此要求保留量化模块契约并补实际量化验证，没有将浮点 stub 或任务被选中算作硬件证据。本例没有 NPU 复现，也没有证明该优化更快；它记录的是旧候选的审阅发现，不表示当前 main 仍有同一实现。

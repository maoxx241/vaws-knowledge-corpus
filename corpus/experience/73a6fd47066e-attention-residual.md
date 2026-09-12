# 采信下降后重新确认 Attention Residual 的数值基准

2026-08-03，接入 AscendC Attention Residual 后，draft 采信变化曾让调查偏向“恢复旧实现”，并把 legacy `npu_rms_norm + matmul` 当作官方 golden。复查公开上游 CUDA、NVIDIA/AMD Python 和测试文件的 blob，确认真正的 canonical 路径先合并 norm/projection 权重，再计算 RMS 与点积。

原始数值表显示，新 kernel 对 canonical 与对 legacy 的 BF16 mismatch 数量并不相同；canonical 与 legacy 自身也有不一致。公式等价没有保证相同浮点舍入轨迹，因此不能通过改 golden 把数值差异“修掉”。调查随后将公式一致、容差比较、bitwise 一致和整网 trajectory 分开评价。

后续保存了一次 8192 输入、1024 输出的服务记录，输出质量检查通过，但只有单请求，不能把采信率恢复推断为普遍吞吐提升。此案例保留了纠正错误 reference 的过程；它不声称采信下降都来自浮点差异，也不发布专有权重内容。没有首个分叉点和同输入算子比较时，采信指标本身不足以判定 kernel 正误。

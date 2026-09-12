# SiTU 接入中纠正 routed 和 shared 的 INT32 含义

2026-07-22 至 07-25，给 Kimi K3 接入 SiTU 量化算子时，最初沿用 SwiGLU 融合路径，混淆了打包 INT4 的 INT32 carrier、GMM 数值累加器以及已反量化 BF16 输出。调查沿调用者、Torch adapter、host/tiling 和 kernel 分别核对，发现不能仅因输入容器是 INT32，就要求 routed WeightNz GMM 输出 INT32。

当时 A3 routed 路径最终保留 GMM1 的 BF16 输出，SiTU 只做激活与动态 INT8 量化；shared 路径使用真实 INT32 accumulator，仍需反量化尺度。上游随后修正 shared kernel：INT32 应数值 cast 成 FP32，不能把整数位模式重解释为浮点。于是撤回了“shared 也必须改 BF16”的早期建议，同时复用已有 tiling 基础设施，避免为同一依赖另造一套入口。

原始记录包含 77 项框架回归及后续源码契约检查，但最新同步 kernel 的构建成功不能继承旧二进制的数值验收，更不能视为 A5 实机证明。保留这次经历是为了提示接入时核对真实数据语义和确切算子版本；A5 的后续分派应查当前代码。[公开的历史算子接入记录](https://github.com/vllm-ascend/vllm-ascend-kimi-k3/pull/16)。

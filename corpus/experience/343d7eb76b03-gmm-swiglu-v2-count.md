# GMM SwiGLU V2 迁移：撤掉隐式转换后才测到了原生 count 分支

2026 年 8 月，统一 GMM SwiGLU 算子时遇到两类兼容问题：不同架构和量化权重暴露的 NZ storage shape 不同，融合量化的运算顺序又会影响 INT8 末位。迁移不能只替换 Python 函数名，需要沿 dispatcher、量化调用方、Torch adapter 和 kernel 检查完整契约。

调查中曾把 graph 问题解释成“V2 更依赖累计值”，于是 adapter 将每专家 token count 转为 cumsum，并强制传入 type 0。用户指出 V2 原生支持两种表示后，重新检查 tiling 与 kernel，确认 type 0/1 都存在。原先标为 count 的测试，在进入底层前已经被转换，实际上没有覆盖原生 count 分支。

后续删除 Python 和 C++ 的隐式转换，保持列表内容与类型一致传递；只有真正不支持某种表示的调用点才负责转换。数值路径则保持原参考实现的标量倒数、逐行乘法和舍入顺序，并加入真实大 token 规格，而非只测小矩阵。

收敛后的原始 A3 记录为 16 例通过，覆盖 count/cumsum、空专家、单/多 Tensor、W8A8/W4A8 和 count 图回放。这个结果不能继承给后来变化的提交；结束时部分 CI 在 checkout/runner 阶段失败，未执行的设备测试不算通过。相关工作见 [PR #12894](https://github.com/vllm-project/vllm-ascend/pull/12894)。可复用的经验是让测试输入真正抵达声称覆盖的底层分支。

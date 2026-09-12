# 把 SiTU 的计算入口从 CustomOp 构造时机中拆开

2026-08-27，Kimi K3 routed MoE 已进入 SiTU 分支，仍在 forward 中报 `Current vLLM config is not set`。问题不在激活公式，而在调用时机：`SituAndMul` 继承 `CustomOp`，构造过程会选择编译/平台分派并读取初始化配置；真实 worker forward 已经离开该上下文。

为了确认，把测试的 reference 构造放在合法配置上下文中，再在上下文外执行实际 MoE helper。旧实现的原始红测沿 `SituAndMul.__init__ → dispatch_forward → get_current_vllm_config` 抛出断言。修正后的 routed 路径直接调用统一计算 helper，普通 MLP/shared 则在初始化时创建模块，不再在热路径临时构造。相同文件最终记录 42 passed，包含不同 dtype 和是否启用 linear clipping 的比较。

这避免了把测试整体包进配置上下文、从而掩盖生产调用问题。测试证明的是构造生命周期与数值表达式对齐，没有实测融合实现的性能和峰值显存，也没有重跑整网。后来公共实现的生命周期已经调整，本文保留的是找出错位上下文的过程，而不是要求新增 wrapper 或四处传递配置。[所在模型集成工作](https://github.com/vllm-project/vllm-ascend/pull/14454)。

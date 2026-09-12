# 排查 Compressor 接口时，先分清逻辑布局、存储格式和 ABI

2026 年 8 月的一次 Compressor 接入调查中，讨论把 `TH`、模型 hidden size 和算子注册的 `FORMAT_ND` 混在了一起。单看函数名或注册格式，无法确定调用方传的 RoPE 张量到底表示什么。

调查先回到 Python consumer 和 C++ binding。原始源码输出显示，metadata 入口显式检查 `rope_cos`、`rope_sin` 为二维，调用前按首尾维整理 RoPE cache。这里的 T 是 RoPE 的位置行数，H 是对应的 RoPE/head 维度；不能把 H 自动解释为模型 hidden size，也不能把完整 RoPE cache 的行数当作本轮 batch token 数。

继续检查注册与下游，`FORMAT_ND` 描述存储格式，并没有取消二维逻辑 shape 的要求。metadata 输出还会经过后续 view；主 Compressor 的输入布局与 metadata 入口也需要分别阅读。另一个实际发现是 binding 会取 state cache 的 stride 并传入 ACLNN，因此“同名符号存在”仍不足以证明两份编译产物具有相同 ABI。

这次工作形成的是源码接口差分，没有完成算子替换或真机验收，也没有证明任意 CANN 镜像、权重和 recipe 可以互换。保留下来的经验是：分别写清张量的逻辑含义、shape/stride 和实际参数序列，再核对同一版本的调用双方。涉及跨仓配套、single/split state 的历史规则未作为当前选型建议保留。

# 在一个 token 的边界上区分真实 stateful 请求与 idle dummy

2026-08-04 至 08-10，Kimi K3 的 prefix-hit 与 P/D 路径出现状态分类问题。公共前缀已经计算完，只剩一个 token 时，执行会选择 decode 图，但 metadata 仍可能按 prefill 构造。调查固定 scheduler block 384，检查 383/384/385/386 边界以及已计算 384、当前调度一个 token 的记录，并把已有初始 state 的行与完全没有历史状态的行分开。

修正分类后的 385-token 请求原始输出可重复，逐层记录中的相关有效状态也保持有限。随后发现另一类一行图输入：idle DP 的 dummy 并不代表真实请求；如果给它正长度和真实 state slot，它会推进缓存状态。针对 dummy 改为零有效长度，并补充测试。

两项修改不能拼成“prefix 问题已全部解决”。后续同一批历史请求重放仍有热缓存质量失败：一次七条请求全部通过，另一轮高命中 replay 只有五条质量通过。这个反例保留了继续查 KV 映射和状态生命周期的必要性。实际迁移时需要重新核对调度器对 spec-width prompt 的定义，不以“prompt 是否完成”一个布尔值代替状态判断。[相关历史修复](https://github.com/vllm-project/vllm-ascend/pull/13277)。

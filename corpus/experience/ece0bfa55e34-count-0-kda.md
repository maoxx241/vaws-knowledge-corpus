# 从异常采样 count=0 追到 KDA 变长临时空间越界

2026-09-08，在 vLLM 0.27.1、Ascend A3、TP16/EP16 的 Kimi K3 缩减专家验证环境中，关闭 prefix cache 后，高并发请求仍出现连续感叹号和采样有效数为零。最初只有构造分析，尚未捕获真实异常；后续抓到 FULL 图中未被 discard 的 decode 行，hidden 与 logits 已经全为 NaN，因此 count=0 是下游表现，不能把它和正常分块 prefill 的 discard 混为一谈。

调查把失败输入保存下来，沿 `chunk_kda_fwd` 的 Post-WU 地址计算检查容量。127 条长短序列共 4,720 个实际 token，分配约 13.8 MiB，但逐序列补齐到 64 后的写入需要约 23.8 MiB。仅修正 tiling 容量后，原失败输入恢复有限值；8 项 NPU 回归覆盖变长 127/128 条及固定长度 80/129、两种 recompute 模式。服务三阶段共 896/896 请求成功，16 rank 的有效 count 均在 1–8。

这次服务对照两边都带有另一项采样排序显存修复，因此算子单独回放是区分两个问题的关键证据。缩减权重只验证故障行为，不证明完整模型精度；此经历也不宣称当前 main 仍有同一缺陷。[当时的修复 PR](https://github.com/vllm-project/vllm-ascend/pull/16087)。

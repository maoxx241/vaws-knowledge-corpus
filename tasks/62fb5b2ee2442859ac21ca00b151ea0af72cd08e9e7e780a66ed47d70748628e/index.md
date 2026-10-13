---
blocks:
- block_id: 87644aba564443d9d6c2d47f0e06d6dce2f68393a1a48e3844262e4fbedea8fb
  indexed: true
  sha256: 1588037616320af7a7793138e465bf9d36e139434a8fa759d024e8ed5a119d7b
  source_range:
    end: 558490
    part: 0
    start: 0
  summary: 只读检查认为 RoPE 前后抽样在同一 NPU stream 上对齐，未发现 race、循环内 D2H 或单边 collective；Ruff、py_compile、diff-check 及 Markov 循环 AST 约束均通过，且未修改源文件。仍有未完成的内存目标：`llm_base_proposer.py:1327` 中 `logits_bias.detach().to(float32)` 仍为全量转换，K3 vocab=163840 时约 640 KiB/步，不符合所有 probe 统计最多 65,536 元素的描述。结论属于有界抽样和当前连续布局下的静态复核，`finite/absmax` 仅代表抽样值，固定步长可能漏检稀疏异常；非连续 tensor 的 `reshape(-1)` 未来仍可能产生全量 copy。
  title: 只读复核：RoPE 抽样与 Markov bias 统计遗漏
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 62fb5b2ee2442859ac21ca00b151ea0af72cd08e9e7e780a66ed47d70748628e
  kind: experience
  material_digest: 0ea40541976d2f5137dae695380ccf5a1a6da48ff961c6c6d07ae894b9eb37f7
  revision: e6b2c233ad083df1b434cd9350b85839f5a7f60735fff4289115e621f5ad4eb1
  schema: mindie-entry/3
  summary: 已完成 RoPE 抽样、stream 顺序、无 D2H/collective 及静态门禁的只读检查；保留一个已确认的遗漏：Markov bias 仍执行全量 FP32 转换，因此“所有统计 bounded”的目标尚未满足。相关结论仍是抽样复核而非验证过的完整正确性证明，且需注意抽样漏检与非连续布局潜在 copy。
  title: 当前任务导航：有界 probe 只读复核
navigation: 已完成 RoPE 抽样、stream 顺序、无 D2H/collective 及静态门禁的只读检查；保留一个已确认的遗漏：Markov bias 仍执行全量 FP32 转换，因此“所有统计 bounded”的目标尚未满足。相关结论仍是抽样复核而非验证过的完整正确性证明，且需注意抽样漏检与非连续布局潜在 copy。
schema: mindie-material-task/1
status: complete
task_id: 62fb5b2ee2442859ac21ca00b151ea0af72cd08e9e7e780a66ed47d70748628e
---

# 当前任务导航：有界 probe 只读复核

已完成 RoPE 抽样、stream 顺序、无 D2H/collective 及静态门禁的只读检查；保留一个已确认的遗漏：Markov bias 仍执行全量 FP32 转换，因此“所有统计 bounded”的目标尚未满足。相关结论仍是抽样复核而非验证过的完整正确性证明，且需注意抽样漏检与非连续布局潜在 copy。

## Materials

- [只读复核：RoPE 抽样与 Markov bias 统计遗漏](blocks/87644aba564443d9d6c2d47f0e06d6dce2f68393a1a48e3844262e4fbedea8fb.md): 只读检查认为 RoPE 前后抽样在同一 NPU stream 上对齐，未发现 race、循环内 D2H 或单边 collective；Ruff、py_compile、diff-check 及 Markov 循环 AST 约束均通过，且未修改源文件。仍有未完成的内存目标：`llm_base_proposer.py:1327` 中 `logits_bias.detach().to(float32)` 仍为全量转换，K3 vocab=163840 时约 640 KiB/步，不符合所有 probe 统计最多 65,536 元素的描述。结论属于有界抽样和当前连续布局下的静态复核，`finite/absmax` 仅代表抽样值，固定步长可能漏检稀疏异常；非连续 tensor 的 `reshape(-1)` 未来仍可能产生全量 copy。

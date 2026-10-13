---
blocks:
- block_id: 3cdbe258e886d00bf97da32ed5c6767436790ddcfc4f222d5fb9bf5917f3b12f
  indexed: true
  sha256: bc21af5383aaddb4899796ba06e9f4c64d4402c7f792168f4826fb8307a16ba2
  source_range:
    end: 1601450
    part: 0
    start: 0
  summary: 该批次记录了对 `a9e34e17`/`0a3560aa1` 基线及 event 依赖链的只读审计：`num_accepted_tokens_event` 在 `global_stream` 中、位于 fused Mamba state-copy 和 accepted-count D2H 之后 record，随后由 host synchronize；这并未被确认等价于整个 device 完成。待核对事项包括默认流 forward/MLA 是否通过 `sampling_done_event` 汇入、ACLGraph replay 的 launch/internal stream join、cached-source DFX 的 clone/D2H、zeroer/allocator 是否脱离依赖链，以及 fused postprocess 的 block-table、src/dst 和 copy-size 边界；尚无实验结果或修复成功结论。
  title: Event fence 与 ACLGraph replay 静态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6d34b149a98e3c03d589b9e22d8e59e2cc3cccb7c772f904ccc91d83c49458d1
  kind: experience
  material_digest: 5e2308c6dd84e2700c58abebeb5eed0eac92fa3695c3ef4c9b6192094699497b
  revision: fa8af73b67698f5593d38c8b5c37f1d5e2219f009e6d9112c24c11e34e903989
  schema: mindie-entry/3
  summary: 继续围绕 event-only fence 失败与 device-wide clone/sync 可能恢复之间的差异，核对 `_model_forward`/ACLGraph 是否 join 到 `sampling_done_event` 所在 current stream，并审查 fused Mamba postprocess 的索引与写回边界。保留当前不确定性：现有材料只有静态审计线索，未验证任何假设、修复或实验结果；更早省略内容仍应通过原始 block 独立检索。
  title: 当前任务导航：Event fence / ACLGraph 依赖审计
navigation: 继续围绕 event-only fence 失败与 device-wide clone/sync 可能恢复之间的差异，核对 `_model_forward`/ACLGraph 是否 join 到 `sampling_done_event` 所在 current stream，并审查 fused Mamba postprocess 的索引与写回边界。保留当前不确定性：现有材料只有静态审计线索，未验证任何假设、修复或实验结果；更早省略内容仍应通过原始 block 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 6d34b149a98e3c03d589b9e22d8e59e2cc3cccb7c772f904ccc91d83c49458d1
---

# 当前任务导航：Event fence / ACLGraph 依赖审计

继续围绕 event-only fence 失败与 device-wide clone/sync 可能恢复之间的差异，核对 `_model_forward`/ACLGraph 是否 join 到 `sampling_done_event` 所在 current stream，并审查 fused Mamba postprocess 的索引与写回边界。保留当前不确定性：现有材料只有静态审计线索，未验证任何假设、修复或实验结果；更早省略内容仍应通过原始 block 独立检索。

## Materials

- [Event fence 与 ACLGraph replay 静态审计](blocks/3cdbe258e886d00bf97da32ed5c6767436790ddcfc4f222d5fb9bf5917f3b12f.md): 该批次记录了对 `a9e34e17`/`0a3560aa1` 基线及 event 依赖链的只读审计：`num_accepted_tokens_event` 在 `global_stream` 中、位于 fused Mamba state-copy 和 accepted-count D2H 之后 record，随后由 host synchronize；这并未被确认等价于整个 device 完成。待核对事项包括默认流 forward/MLA 是否通过 `sampling_done_event` 汇入、ACLGraph replay 的 launch/internal stream join、cached-source DFX 的 clone/D2H、zeroer/allocator 是否脱离依赖链，以及 fused postprocess 的 block-table、src/dst 和 copy-size 边界；尚无实验结果或修复成功结论。

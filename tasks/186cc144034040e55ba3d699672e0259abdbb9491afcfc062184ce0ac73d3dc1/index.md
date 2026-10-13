---
blocks:
- block_id: 264fe7b2f52258b3934f395caf032bc78d7ccc3c2e18341052d7f683d43270bc
  indexed: true
  sha256: 1153fb04f8b5f21c00a260034519dff55e35f114080de53885c59f6e58b44a10
  source_range:
    end: 455268
    part: 0
    start: 0
  summary: 基于 kernel 快照 `51d29795` 的审查认为，在合法 K3 输入契约下未确认 P0 或 correctness P1 blocker，但发现核间负载不均、队列 depth=1 导致缺少真正 MTE2/V/MTE3 流水，以及热循环逐标量访问 GM 等 P1 性能风险。`group_index.sum()` 不足 `rowLen` 时尾部输出可能未写，被列为依赖输入契约的 P2 健壮性问题；4-byte scale 通过 `DataCopyPad(UB→GM)` 写回，补齐字节不会落入 GM，未被判定为覆盖错误。建议优先修复全局连续区间分工并补充 14-expert、不同 TP 和 M 值的数值及 profiler 验证；双缓冲和标量搬运优化仍是建议，尚未证明收益。
  title: AscendC DequantSwiGLUQuant 内核只读审查结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 186cc144034040e55ba3d699672e0259abdbb9491afcfc062184ce0ac73d3dc1
  kind: experience
  material_digest: 02882dfd28b98c95a81a5ed44e0d5ba0b552f46c35983750222c5f552e113728
  revision: 56a4e0684551322500b04b7e646bdb1a98653232b624db91f942d454c4546573
  schema: mindie-entry/3
  summary: 当前材料记录了 kernel `51d29795` 的只读审查：合法 K3 契约下未确认 P0 或 correctness P1 blocker，但存在核间负载不均、单缓冲流水和 GM 标量访问等性能风险。`group_index` 总和不足时尾部输出未写仍是未完成的健壮性问题，且建议的局部修复、producer invariant 测试和 profiler/数值验证尚未形成已验证结果；4-byte scale 的 `DataCopyPad` 非对齐写回未被确认会覆盖相邻行。
  title: AscendC 内核审查与风险索引
navigation: 当前材料记录了 kernel `51d29795` 的只读审查：合法 K3 契约下未确认 P0 或 correctness P1 blocker，但存在核间负载不均、单缓冲流水和 GM 标量访问等性能风险。`group_index` 总和不足时尾部输出未写仍是未完成的健壮性问题，且建议的局部修复、producer invariant 测试和 profiler/数值验证尚未形成已验证结果；4-byte scale 的 `DataCopyPad` 非对齐写回未被确认会覆盖相邻行。
schema: mindie-material-task/1
status: complete
task_id: 186cc144034040e55ba3d699672e0259abdbb9491afcfc062184ce0ac73d3dc1
---

# AscendC 内核审查与风险索引

当前材料记录了 kernel `51d29795` 的只读审查：合法 K3 契约下未确认 P0 或 correctness P1 blocker，但存在核间负载不均、单缓冲流水和 GM 标量访问等性能风险。`group_index` 总和不足时尾部输出未写仍是未完成的健壮性问题，且建议的局部修复、producer invariant 测试和 profiler/数值验证尚未形成已验证结果；4-byte scale 的 `DataCopyPad` 非对齐写回未被确认会覆盖相邻行。

## Materials

- [AscendC DequantSwiGLUQuant 内核只读审查结论](blocks/264fe7b2f52258b3934f395caf032bc78d7ccc3c2e18341052d7f683d43270bc.md): 基于 kernel 快照 `51d29795` 的审查认为，在合法 K3 输入契约下未确认 P0 或 correctness P1 blocker，但发现核间负载不均、队列 depth=1 导致缺少真正 MTE2/V/MTE3 流水，以及热循环逐标量访问 GM 等 P1 性能风险。`group_index.sum()` 不足 `rowLen` 时尾部输出可能未写，被列为依赖输入契约的 P2 健壮性问题；4-byte scale 通过 `DataCopyPad(UB→GM)` 写回，补齐字节不会落入 GM，未被判定为覆盖错误。建议优先修复全局连续区间分工并补充 14-expert、不同 TP 和 M 值的数值及 profiler 验证；双缓冲和标量搬运优化仍是建议，尚未证明收益。

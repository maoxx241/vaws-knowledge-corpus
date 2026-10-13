---
blocks:
- block_id: 3ecbe8741396835252e4b988a9e853a52fec86a8336e677cbb5df1c73db1f78a
  indexed: true
  sha256: 242bdf01044b66f18ed84be216a9a6a759005b202af6cecf36f34ec2915c4c59
  source_range:
    end: 3700627
    part: 0
    start: 0
  summary: 只读审计未定位已验证的单一根因；R8 排除了 CPU DP metadata all-reduce 的调用次数、caller、ordinal 或 padding 错位，冻结点出现在 MoE `repeat_interleave` 触发 NPU stream 同步时。R2 已是首个确认卡死的可服务组合，涉及 GQA packing、GPU-only Mamba align/H2D、zeroer 及多项 fix 的联合修改；当前仍未完成单变量回退或二分，也未证明精度收益来自某一项。历史审计提示异步 H2D buffer 生命周期、completion barrier 和 Mamba align 卡死曾有相关修复，但后续 R9 结论仍未完成闭环。
  title: R8 卡死审计与历史修改范围
- block_id: 0023f707cb932a26ecab3044aa2946b64a32a7acdecc7732512f4f8592a1a75f
  indexed: true
  sha256: 6e3f46e970f8f8a29e6c4a5ef7da8e0bd129319547bcf9ac22145075f3379349
  source_range:
    end: 3700627
    part: 1
    start: 0
  summary: 历史扫描发现 `434afac20` 曾修复 310P 上 Mamba align postprocess hang，但当前环境未设置 `VLLM_SSM_CONV_STATE_LAYOUT=DS`，因此 DS-layout 禁用断言只能作为条件性风险。审计一度将 GPU-only Mamba align 视为首嫌疑，随后结合 R8 的 DP3 16 个 fresh zero block、其他 DP 的 cached row 以及缺少持久异步 buffer ownership 的实现，将 packed KV zeroer 提升为 P0、Mamba align 降为 P1。建议的最小 A/B 是先恢复与并发数匹配的 pinned/device ring，再单独关闭 GPU-only align；这些都仍是待验证假设。
  title: GPU-only Mamba 与 zeroer 风险收敛
- block_id: d5ff9ca0da3e8a5d80eca782bd38068e13467234eddae3187541b14440f7dc32
  indexed: true
  sha256: dfa8eae513319acc376b620578df928a2329f2c2e61e884baac1546a0d0cde67
  source_range:
    end: 3700627
    part: 2
    start: 0
  summary: R8 排除了多 DP drafter ordinal 不一致、mixed eager/graph HCCL、DP padding 不一致、double dummy 以及若干仅适用于 graph 的历史 hang 机制；R4 eager 也超时，因此不能归因于 graph-only hunk。当前最强未证实链路是 DP3 的 zeroer 异步 H2D/zero kernel 生命周期问题，MoE `repeat_interleave` 只是同步暴露点；GPU-only Mamba remap/precopy/postprocess 为次级嫌疑。工作树仍混有约 13 个 tracked 和 2 个 untracked 修改及临时 ledger/probe，R9 仅回退 runtime dummy capture builder，尚无运行期 A/B 结果。
  title: 已排除机制与最短验证顺序
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 5520a100a917a7b2cb9cb2b77e70c3441718341d8e1b7cb0f5a622342b45ee9a
  kind: experience
  material_digest: 33d2971b4b09a75f906c2d94e409f1baa622737d96353e6536e5a6467fdfa07e
  revision: e4fe9cf87c71d3781eaf5ba8cc5d6236d228cac08c627702d62040f69ab85249
  schema: mindie-entry/3
  summary: 当前材料围绕 R2 首次确认的 c64 历史/并发相关卡死展开：R8 已排除 DP collective 调用相位错位，冻结在 MoE 动态结果同步处。检索应优先关注 packed KV zeroer 的临时 pinned/device tensor、缺少并发 ring/completion event 与 DP3 16 个 fresh block，其次关注 GPU-only Mamba align 的异步 precopy/remap/postprocess；两者均未被单变量实验定因。历史记录保留了 H2D buffer ownership、Mamba postprocess hang、block-table 生命周期和 dummy/real 执行一致性等相关修复线索，同时明确哪些机制已被 R8/R4 现场排除；后续应按 zeroer ring A/B、关闭 GPU-only align、rank-scoped synchronize 的顺序验证。
  title: dspark 精度修改卡死审计导航
navigation: 当前材料围绕 R2 首次确认的 c64 历史/并发相关卡死展开：R8 已排除 DP collective 调用相位错位，冻结在 MoE 动态结果同步处。检索应优先关注 packed KV zeroer 的临时 pinned/device tensor、缺少并发 ring/completion event 与 DP3 16 个 fresh block，其次关注 GPU-only Mamba align 的异步 precopy/remap/postprocess；两者均未被单变量实验定因。历史记录保留了 H2D buffer ownership、Mamba postprocess hang、block-table 生命周期和 dummy/real 执行一致性等相关修复线索，同时明确哪些机制已被 R8/R4 现场排除；后续应按 zeroer ring A/B、关闭 GPU-only align、rank-scoped synchronize 的顺序验证。
schema: mindie-material-task/1
status: complete
task_id: 5520a100a917a7b2cb9cb2b77e70c3441718341d8e1b7cb0f5a622342b45ee9a
---

# dspark 精度修改卡死审计导航

当前材料围绕 R2 首次确认的 c64 历史/并发相关卡死展开：R8 已排除 DP collective 调用相位错位，冻结在 MoE 动态结果同步处。检索应优先关注 packed KV zeroer 的临时 pinned/device tensor、缺少并发 ring/completion event 与 DP3 16 个 fresh block，其次关注 GPU-only Mamba align 的异步 precopy/remap/postprocess；两者均未被单变量实验定因。历史记录保留了 H2D buffer ownership、Mamba postprocess hang、block-table 生命周期和 dummy/real 执行一致性等相关修复线索，同时明确哪些机制已被 R8/R4 现场排除；后续应按 zeroer ring A/B、关闭 GPU-only align、rank-scoped synchronize 的顺序验证。

## Materials

- [R8 卡死审计与历史修改范围](blocks/3ecbe8741396835252e4b988a9e853a52fec86a8336e677cbb5df1c73db1f78a.md): 只读审计未定位已验证的单一根因；R8 排除了 CPU DP metadata all-reduce 的调用次数、caller、ordinal 或 padding 错位，冻结点出现在 MoE `repeat_interleave` 触发 NPU stream 同步时。R2 已是首个确认卡死的可服务组合，涉及 GQA packing、GPU-only Mamba align/H2D、zeroer 及多项 fix 的联合修改；当前仍未完成单变量回退或二分，也未证明精度收益来自某一项。历史审计提示异步 H2D buffer 生命周期、completion barrier 和 Mamba align 卡死曾有相关修复，但后续 R9 结论仍未完成闭环。
- [GPU-only Mamba 与 zeroer 风险收敛](blocks/0023f707cb932a26ecab3044aa2946b64a32a7acdecc7732512f4f8592a1a75f.md): 历史扫描发现 `434afac20` 曾修复 310P 上 Mamba align postprocess hang，但当前环境未设置 `VLLM_SSM_CONV_STATE_LAYOUT=DS`，因此 DS-layout 禁用断言只能作为条件性风险。审计一度将 GPU-only Mamba align 视为首嫌疑，随后结合 R8 的 DP3 16 个 fresh zero block、其他 DP 的 cached row 以及缺少持久异步 buffer ownership 的实现，将 packed KV zeroer 提升为 P0、Mamba align 降为 P1。建议的最小 A/B 是先恢复与并发数匹配的 pinned/device ring，再单独关闭 GPU-only align；这些都仍是待验证假设。
- [已排除机制与最短验证顺序](blocks/d5ff9ca0da3e8a5d80eca782bd38068e13467234eddae3187541b14440f7dc32.md): R8 排除了多 DP drafter ordinal 不一致、mixed eager/graph HCCL、DP padding 不一致、double dummy 以及若干仅适用于 graph 的历史 hang 机制；R4 eager 也超时，因此不能归因于 graph-only hunk。当前最强未证实链路是 DP3 的 zeroer 异步 H2D/zero kernel 生命周期问题，MoE `repeat_interleave` 只是同步暴露点；GPU-only Mamba remap/precopy/postprocess 为次级嫌疑。工作树仍混有约 13 个 tracked 和 2 个 untracked 修改及临时 ledger/probe，R9 仅回退 runtime dummy capture builder，尚无运行期 A/B 结果。

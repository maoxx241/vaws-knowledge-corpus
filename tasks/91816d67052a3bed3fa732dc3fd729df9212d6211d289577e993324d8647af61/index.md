---
blocks:
- block_id: ac1ee5e1fb3d10daf4070d524dccbb370ed9c78b10f66f8a954ad9990ab5c4a3
  indexed: true
  sha256: 234b12b8672ac7603a155bc5fdc575cdb8343fd3b2a023bd349b75395ab17988
  source_range:
    end: 16788352
    part: 0
    start: 0
  summary: 分析修正了旧 MLA DSpark 与当前 GQA DSpark 的差异：旧 MLA 可将 24 个 target MLA 与 5 个 draft MLA 合并为 1 个组，连同 3 个 Mamba 组共 4 组、约 4 次 build；当前 GQA 因 FullAttentionSpec 与 MLA 不可 merge 而形成 20 组、约 20 次 build。提出并实现了 24 MLA + 5 causal GQA 的混合 UniformTypeKVCacheSpecs 方案，预期 4 个 KV group、5 个 metadata builder，并同步处理 scheduler 展开、Mamba zeroing、block-table 与相关回归测试。实机启动已完成 16 个 NPU worker 初始化，但完整 896-expert 权重在单机 EP16 模型构造阶段 OOM，随后切换到同拓扑的 16-expert 裁剪权重；截至该块结束，尚未宣称端到端验证完成。
  title: GQA/MLA 混合 KV 分组方案与实现进展
- block_id: 4fc25b394f3148add32b34ae76fb8dc359785ac3d293d7326e3a81dc46e1174f
  indexed: true
  sha256: df6176535a085b33e01becc0e6a6593f97a54052c66ad1443b3836ff8557632c
  source_range:
    end: 16788352
    part: 1
    start: 0
  summary: 16-expert 裁剪权重已越过 KV 初始化，实机日志显示容量约 513,148 tokens，并呈现 4 组混合布局特征；旧方案的 padding/group warning 未出现。当前阻塞点是首次 FULL_DECODE_ONLY 的 ACLGraph/quantization 图编译耗时，而非已确认的分组失败。真实请求、文本输出、DSpark、多次 prepare_inputs、prefix hit 以及最终回归和提交仍未完成。
  title: 混合分组实机启动进入 ACLGraph 编译阶段
- block_id: e40022c6cb1bdc5664525b1d26b0d0efd6ae5dd99465deddfb6cc2880c7f2307
  indexed: true
  sha256: 1111831c2035cdef9650cdb19b5e0ec07b3c2b8836fcbd0c95e16c27cffb4c04
  source_range:
    end: 33571170
    part: 0
    start: 16788352
  summary: 记录显示，MLA+GQA 混合方案将 scheduler KV group 从 20 组压缩为 4 组，metadata builder 为 5 个，并覆盖 Uniform Mamba 解包、copy buffer、spec decode、block table、slot mapping 和 accepted-token buffer。真实 eager 与 ACLGraph 请求曾报告 HTTP 200、prefix hit、DSpark draft token、replay 及 83 个相关 UT 通过，但这些结果属于来源中的报告，且同一材料也明确指出 accepted token 为 0，不能据此证明投机接受率。后续纠正确认 0.27 当前实机仍是首轴连续：三平面 MLA/Mamba 共享区加 5 个 GQA 尾部 tensor slot，`indexes_kv_by_block_stride` 只是 MRV2 能力契约，不代表当前链路已启用非连续 view。
  title: 混合分组链路已完成并确认当前连续布局
- block_id: c722d657c1265a9a104a04d3879e016f67e88b5a896cff99b747c4152de29899
  indexed: true
  sha256: ccd16104e4e31b64897d8e98a6bce3901883f0fb76c8090b9fadaa4f694af6ac
  source_range:
    end: 33571170
    part: 1
    start: 16788352
  summary: 当前布局被进一步明确为 `P0=46080N`、`P1=393216N`、`P2=49152N`：MLA 使用 padding/K-NoPE/K-RoPE，Mamba 使用 conv/SSM/padding，GQA 使用 P0 padding、P1 中的 padding+K 和 P2 的 V；GQA-K 位于 P1 尾部而非头部，主要是为了复用连续 slice 和普通 `.view()`。GQA 仍被补齐到 488448 B/page，单层有效 KV 为 98304 B，padding 约 79.87%；只有 allocator/planner 同时采用紧凑页或上游 packed `offset + block_stride`，才会真正减少显存。来源认为 vLLM core 能表达异构 page size、stride 和 packed layout，但当前 Ascend worker、zero/copy、connector、ACLGraph 等链路尚未完成端到端接入，因此“只支持算子非连续”不足以直接切换。
  title: 连续三平面布局、GQA padding 与 packed 迁移条件
- block_id: d5d40ac9fc0de4bc84f171fe52ac676410dfc3ae7bf65f3312d01f2e6ab39f42
  indexed: true
  sha256: 4127c93ce9e4c91831a6c528e92de09d63b0738710bb6ed19c1e74d2f89eba20
  source_range:
    end: 33571170
    part: 2
    start: 16788352
  summary: 算子核对中，FIA 读路径和 recurrent KDA 被报告为基本具备 stride 能力，Conv1D 当前分支仍缺少上游补丁，MLA 自研 `mla_preprocess` 接近满足但普通 MLA 写入路径和 GQA 写入仍是缺口；`ScatterPaKvCache/Nct` 只描述源 K/V 非连续，不能解决目标 cache 首轴 stride。后续纠正将正式 CANN 9.1 `aclnnScatterNdUpdate` 作为 GQA writer 主候选，把 `index_put_` 降为正确性对照，并停止扩展临时 `npu_scatter_nd_update_v2`；不过 153 上的 correctness、ACLGraph 和性能验证仍未完成，且普通 cache view/model runner 也尚未完整切换到 `as_strided`。当前任务随后转向统筹一个基于即将发布的 op-transformer beta、最新 FLA 算子及相关 model-runner PR 的正式首轴非连续 GQA+MLA 混合方案，尚未形成已验证的最终实现。
  title: 首轴非连续算子满足度与正式方案待统筹
- block_id: a0c00aa04e6e05c76f892b850f203e54cb03a2722b6245c5a19cca3a872b3226
  indexed: true
  sha256: 733be934daf039c2c774ccc083615fdaf4921e7f0d98de01db83885bea91a5d2
  source_range:
    end: 48101034
    part: 0
    start: 33571170
  summary: 形成了后续阶段的 384-token 物理块、跨层 packed slab、首轴 stride view 方案，但明确其不是当前实现基线；GQA/MLA/Mamba 的真实 payload、`KVCacheTensor(offset, block_stride)`、算子链和 MRV1/MRV2 builder 仍需逐项验证。指定 task 的当前快照显示 0.27 MLA 约 409,463 tokens、GQA mixed 约 419,604 tokens，实际仅以 `max_model_len=135000` 拉起并验证过 128K，400K/410K 仍是 profiler 推算而非实测成功。后续补充指出 0.26 MLA 四机在 `TP16×DP4×EP64`、`gpu_memory_utilization=0.8`、开启 Prefix Cache 时为 371,160 tokens，且不同版本、显存预算和运行配置不可直接混比。
  title: 384块 packed-strided 方案与四机容量评审
- block_id: aa9e6854ba6429141e47a4c1e4aafd9cad72b46fb8f7ae842000387710d980bf
  indexed: true
  sha256: 293f413c1c0b5f247228c42b6f8ee38ff70fee7869845b9e521de38e7fa92186
  source_range:
    end: 48101034
    part: 1
    start: 33571170
  summary: 评估认为将 0.27 的混合调度分组移植到 0.26 属于中等风险，推荐先保留 0.26 四平面物理布局，仅把约 20 个 scheduler groups/多次 builder 收敛为 `[29,23,23,23]` 和 5 类 builder；直接 cherry-pick 存在 6 处路径冲突，连物理布局一起切换则为中高风险。当前 `block_size=384`、`page_size=488448` 是 TP16/C16 实例匹配值，TP8、真正的 KV C8/INT8、DCP 等会改变推导结果；并需区分并发 C8与 KV-cache C8。建议从实际 `KVCacheSpec` 推导 block/page、层数和 Mamba 分组，满足 K3 语义但布局不支持时显式报错或 warning，避免静默退回 20 组。
  title: 0.27 GQA 回移 0.26 与 page size 配置化评估
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 91816d67052a3bed3fa732dc3fd729df9212d6211d289577e993324d8647af61
  kind: experience
  material_digest: b88320236393b4c470b299fd1fa55e9df052470e734a52e1412ea78079691d23
  revision: 3f290769e2e7cba1227394627bfd48cb992198a60976a1dcf737807c9ba9bb49
  schema: mindie-entry/3
  summary: 任务已扩展为三条相互关联但需分开验证的主线：当前 0.26/0.27 DSpark 的实机 KV 容量基线、0.27 混合分组向 0.26 的低破坏性回移，以及从硬编码 `384/488448` 转为由实际 `KVCacheSpec` 推导的通用 layout/grouping plan。已知 0.27 四机 TP16 结果约为 MLA 409K、GQA mixed 419K，0.26 MLA 在特定 0.8 配置下为 371K；这些是不同快照和预算下的容量证据，部分目标值尚未按对应 `max_model_len` 实机重启验证。packed-strided、取消按 layer padding、算子首轴非连续支持、MRV1/MRV2、Prefix Cache、ACLGraph、TP8/DCP、KV C8/INT8 和 0.26 四平面兼容性仍是未完成或需回归的边界，不能把方案、静态检查、日志推算或历史报告提升为 verified success。
  title: K3 DSpark KV 分组、容量与通用布局迁移
navigation: 任务已扩展为三条相互关联但需分开验证的主线：当前 0.26/0.27 DSpark 的实机 KV 容量基线、0.27 混合分组向 0.26 的低破坏性回移，以及从硬编码 `384/488448` 转为由实际 `KVCacheSpec` 推导的通用 layout/grouping plan。已知 0.27 四机 TP16 结果约为 MLA 409K、GQA mixed 419K，0.26 MLA 在特定 0.8 配置下为 371K；这些是不同快照和预算下的容量证据，部分目标值尚未按对应 `max_model_len` 实机重启验证。packed-strided、取消按 layer padding、算子首轴非连续支持、MRV1/MRV2、Prefix Cache、ACLGraph、TP8/DCP、KV C8/INT8 和 0.26 四平面兼容性仍是未完成或需回归的边界，不能把方案、静态检查、日志推算或历史报告提升为 verified success。
schema: mindie-material-task/1
status: complete
task_id: 91816d67052a3bed3fa732dc3fd729df9212d6211d289577e993324d8647af61
---

# K3 DSpark KV 分组、容量与通用布局迁移

任务已扩展为三条相互关联但需分开验证的主线：当前 0.26/0.27 DSpark 的实机 KV 容量基线、0.27 混合分组向 0.26 的低破坏性回移，以及从硬编码 `384/488448` 转为由实际 `KVCacheSpec` 推导的通用 layout/grouping plan。已知 0.27 四机 TP16 结果约为 MLA 409K、GQA mixed 419K，0.26 MLA 在特定 0.8 配置下为 371K；这些是不同快照和预算下的容量证据，部分目标值尚未按对应 `max_model_len` 实机重启验证。packed-strided、取消按 layer padding、算子首轴非连续支持、MRV1/MRV2、Prefix Cache、ACLGraph、TP8/DCP、KV C8/INT8 和 0.26 四平面兼容性仍是未完成或需回归的边界，不能把方案、静态检查、日志推算或历史报告提升为 verified success。

## Materials

- [GQA/MLA 混合 KV 分组方案与实现进展](blocks/ac1ee5e1fb3d10daf4070d524dccbb370ed9c78b10f66f8a954ad9990ab5c4a3.md): 分析修正了旧 MLA DSpark 与当前 GQA DSpark 的差异：旧 MLA 可将 24 个 target MLA 与 5 个 draft MLA 合并为 1 个组，连同 3 个 Mamba 组共 4 组、约 4 次 build；当前 GQA 因 FullAttentionSpec 与 MLA 不可 merge 而形成 20 组、约 20 次 build。提出并实现了 24 MLA + 5 causal GQA 的混合 UniformTypeKVCacheSpecs 方案，预期 4 个 KV group、5 个 metadata builder，并同步处理 scheduler 展开、Mamba zeroing、block-table 与相关回归测试。实机启动已完成 16 个 NPU worker 初始化，但完整 896-expert 权重在单机 EP16 模型构造阶段 OOM，随后切换到同拓扑的 16-expert 裁剪权重；截至该块结束，尚未宣称端到端验证完成。
- [混合分组实机启动进入 ACLGraph 编译阶段](blocks/4fc25b394f3148add32b34ae76fb8dc359785ac3d293d7326e3a81dc46e1174f.md): 16-expert 裁剪权重已越过 KV 初始化，实机日志显示容量约 513,148 tokens，并呈现 4 组混合布局特征；旧方案的 padding/group warning 未出现。当前阻塞点是首次 FULL_DECODE_ONLY 的 ACLGraph/quantization 图编译耗时，而非已确认的分组失败。真实请求、文本输出、DSpark、多次 prepare_inputs、prefix hit 以及最终回归和提交仍未完成。
- [混合分组链路已完成并确认当前连续布局](blocks/e40022c6cb1bdc5664525b1d26b0d0efd6ae5dd99465deddfb6cc2880c7f2307.md): 记录显示，MLA+GQA 混合方案将 scheduler KV group 从 20 组压缩为 4 组，metadata builder 为 5 个，并覆盖 Uniform Mamba 解包、copy buffer、spec decode、block table、slot mapping 和 accepted-token buffer。真实 eager 与 ACLGraph 请求曾报告 HTTP 200、prefix hit、DSpark draft token、replay 及 83 个相关 UT 通过，但这些结果属于来源中的报告，且同一材料也明确指出 accepted token 为 0，不能据此证明投机接受率。后续纠正确认 0.27 当前实机仍是首轴连续：三平面 MLA/Mamba 共享区加 5 个 GQA 尾部 tensor slot，`indexes_kv_by_block_stride` 只是 MRV2 能力契约，不代表当前链路已启用非连续 view。
- [连续三平面布局、GQA padding 与 packed 迁移条件](blocks/c722d657c1265a9a104a04d3879e016f67e88b5a896cff99b747c4152de29899.md): 当前布局被进一步明确为 `P0=46080N`、`P1=393216N`、`P2=49152N`：MLA 使用 padding/K-NoPE/K-RoPE，Mamba 使用 conv/SSM/padding，GQA 使用 P0 padding、P1 中的 padding+K 和 P2 的 V；GQA-K 位于 P1 尾部而非头部，主要是为了复用连续 slice 和普通 `.view()`。GQA 仍被补齐到 488448 B/page，单层有效 KV 为 98304 B，padding 约 79.87%；只有 allocator/planner 同时采用紧凑页或上游 packed `offset + block_stride`，才会真正减少显存。来源认为 vLLM core 能表达异构 page size、stride 和 packed layout，但当前 Ascend worker、zero/copy、connector、ACLGraph 等链路尚未完成端到端接入，因此“只支持算子非连续”不足以直接切换。
- [首轴非连续算子满足度与正式方案待统筹](blocks/d5d40ac9fc0de4bc84f171fe52ac676410dfc3ae7bf65f3312d01f2e6ab39f42.md): 算子核对中，FIA 读路径和 recurrent KDA 被报告为基本具备 stride 能力，Conv1D 当前分支仍缺少上游补丁，MLA 自研 `mla_preprocess` 接近满足但普通 MLA 写入路径和 GQA 写入仍是缺口；`ScatterPaKvCache/Nct` 只描述源 K/V 非连续，不能解决目标 cache 首轴 stride。后续纠正将正式 CANN 9.1 `aclnnScatterNdUpdate` 作为 GQA writer 主候选，把 `index_put_` 降为正确性对照，并停止扩展临时 `npu_scatter_nd_update_v2`；不过 153 上的 correctness、ACLGraph 和性能验证仍未完成，且普通 cache view/model runner 也尚未完整切换到 `as_strided`。当前任务随后转向统筹一个基于即将发布的 op-transformer beta、最新 FLA 算子及相关 model-runner PR 的正式首轴非连续 GQA+MLA 混合方案，尚未形成已验证的最终实现。
- [384块 packed-strided 方案与四机容量评审](blocks/a0c00aa04e6e05c76f892b850f203e54cb03a2722b6245c5a19cca3a872b3226.md): 形成了后续阶段的 384-token 物理块、跨层 packed slab、首轴 stride view 方案，但明确其不是当前实现基线；GQA/MLA/Mamba 的真实 payload、`KVCacheTensor(offset, block_stride)`、算子链和 MRV1/MRV2 builder 仍需逐项验证。指定 task 的当前快照显示 0.27 MLA 约 409,463 tokens、GQA mixed 约 419,604 tokens，实际仅以 `max_model_len=135000` 拉起并验证过 128K，400K/410K 仍是 profiler 推算而非实测成功。后续补充指出 0.26 MLA 四机在 `TP16×DP4×EP64`、`gpu_memory_utilization=0.8`、开启 Prefix Cache 时为 371,160 tokens，且不同版本、显存预算和运行配置不可直接混比。
- [0.27 GQA 回移 0.26 与 page size 配置化评估](blocks/aa9e6854ba6429141e47a4c1e4aafd9cad72b46fb8f7ae842000387710d980bf.md): 评估认为将 0.27 的混合调度分组移植到 0.26 属于中等风险，推荐先保留 0.26 四平面物理布局，仅把约 20 个 scheduler groups/多次 builder 收敛为 `[29,23,23,23]` 和 5 类 builder；直接 cherry-pick 存在 6 处路径冲突，连物理布局一起切换则为中高风险。当前 `block_size=384`、`page_size=488448` 是 TP16/C16 实例匹配值，TP8、真正的 KV C8/INT8、DCP 等会改变推导结果；并需区分并发 C8与 KV-cache C8。建议从实际 `KVCacheSpec` 推导 block/page、层数和 Mamba 分组，满足 K3 语义但布局不支持时显式报错或 warning，避免静默退回 20 组。

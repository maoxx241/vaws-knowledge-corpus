---
blocks:
- block_id: d036944a5fde6da4418dfc06944558220f3ae87318efa02a9afb05b32f3ae99f
  indexed: true
  sha256: ee11cb66c4b44c3b8387d7f1b659a80941b08842c1576675ecb71c4b534bc017
  source_range:
    end: 2155886
    part: 0
    start: 0
  summary: 记录了在 154 容器中通过运行时 overlay 和完整主源码产物验证扩展、kernel 库、KDA schema 及 MOE 调用，并保留多次失败：CANN Python 路径覆盖、W4A8 GMM 的 k=3584 与量化分组数 768 不整除、MLA/KDA page size 合并错误、远端变量提前展开，以及 0.23.0 的 `l2norm_fwd` 不接受 `use_tiled_kernel=True`。后续修订涉及 SiTU per-channel scale、`AscendMLAAttentionSpec` 的 `page_size_bytes/page_size_padded` 与 `merge()`、KDA L2Norm 调用和 mock；若干针对性测试被报告通过。最后一轮 `rerun4g` 被报告完成真实 5 层 INT4、TP8 初始化、AscendC KDA prefill/decode，并生成 `[161095, 74143]`、退出码 0，但这些结果仍是来源中的运行报告而非独立核验。
  title: fastbuild/KDA/MOE 与 TP8 rerun4 调试记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 14f31d2e3de2ba5f1cf5df400961ea261aedb9dd23e17a8ed01a8f4a84b21642
  kind: experience
  material_digest: 8d162a8b98d9540a92024a32505f37f9a81718434da71e4c8f218193e0ec97b7
  revision: 96997a93de3437526e8d445ae9c148068d7f76c6312a649f75e82142e68fb11a
  schema: mindie-entry/3
  summary: 本批材料集中记录 fastbuild 扩展加载、KDA/MOE schema、SiTU W4A8、混合 MLA/KDA KV cache 规格及 vLLM 0.23 L2Norm API 的失败与修复尝试；最终 rerun4g 的端到端通过和日志位置已由来源报告，但仍应作为可检索的未独立验证结果保留。
  title: 当前任务导航：TP8 KDA/MOE rerun 与修复链
navigation: 本批材料集中记录 fastbuild 扩展加载、KDA/MOE schema、SiTU W4A8、混合 MLA/KDA KV cache 规格及 vLLM 0.23 L2Norm API 的失败与修复尝试；最终 rerun4g 的端到端通过和日志位置已由来源报告，但仍应作为可检索的未独立验证结果保留。
schema: mindie-material-task/1
status: complete
task_id: 14f31d2e3de2ba5f1cf5df400961ea261aedb9dd23e17a8ed01a8f4a84b21642
---

# 当前任务导航：TP8 KDA/MOE rerun 与修复链

本批材料集中记录 fastbuild 扩展加载、KDA/MOE schema、SiTU W4A8、混合 MLA/KDA KV cache 规格及 vLLM 0.23 L2Norm API 的失败与修复尝试；最终 rerun4g 的端到端通过和日志位置已由来源报告，但仍应作为可检索的未独立验证结果保留。

## Materials

- [fastbuild/KDA/MOE 与 TP8 rerun4 调试记录](blocks/d036944a5fde6da4418dfc06944558220f3ae87318efa02a9afb05b32f3ae99f.md): 记录了在 154 容器中通过运行时 overlay 和完整主源码产物验证扩展、kernel 库、KDA schema 及 MOE 调用，并保留多次失败：CANN Python 路径覆盖、W4A8 GMM 的 k=3584 与量化分组数 768 不整除、MLA/KDA page size 合并错误、远端变量提前展开，以及 0.23.0 的 `l2norm_fwd` 不接受 `use_tiled_kernel=True`。后续修订涉及 SiTU per-channel scale、`AscendMLAAttentionSpec` 的 `page_size_bytes/page_size_padded` 与 `merge()`、KDA L2Norm 调用和 mock；若干针对性测试被报告通过。最后一轮 `rerun4g` 被报告完成真实 5 层 INT4、TP8 初始化、AscendC KDA prefill/decode，并生成 `[161095, 74143]`、退出码 0，但这些结果仍是来源中的运行报告而非独立核验。

---
blocks:
- block_id: 149cb27131869246f328681bf2e8a7272ff921b31355d883cd2ededc1db8e17b
  indexed: true
  sha256: e1cc836eb9f9470149cb18e7dfc1692d251404838114aca481bde275ff971564
  source_range:
    end: 5797716
    part: 0
    start: 0
  summary: 围绕当前 ci-new 提交 8f192c9f，对 A3、TP16 下 DeepSeek V3.1 与 Kimi K3 的 MLA/FIA 路径进行了静态代码与 tiling 分析。结论区分 Prefill 和 Decode：Prefill 的输入表示分别为 DeepSeek 的 8 heads、128D 主体加 64D RoPE，以及 K3 的 6 heads、combined 192D；后续核对修正了“K3 必然落入通用 tiling”的判断，指出 FIA parser 可将其识别为 RoPE combine 并进入高性能 GQA capability。Decode 中 K3 的 6→8 head padding 仍是额外工作量来源，但物理 block 384 会被展开为 3×128，不能直接作为 FIA 接收 384 block 导致退化的证据；FA-quant、MLAPO、PD-D 等路径差异及实际性能仍需运行时 tiling key/profiler 验证。随后生成了详细 Markdown 文档，并将其作为完整新评论发布到 vllm-ascend/vllm-ascend-kimi-k3#36；文档和发布状态据称已回读确认，但原始材料未提供可独立核验的本地路径。
  title: A3/TP16 下 DeepSeek V3.1 与 Kimi K3 的 MLA/FIA 对比及文档发布
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c503b369cf096880d7636f975d23902872120e32885385fa70b373e6b814a7cb
  kind: experience
  material_digest: 161dd952ad4f2476eebf6615207b55d36a497124996aae0f56896297cefdd81b
  revision: 4a498763a8e8504baf9336b3833e8a0f465e2096f941dd6d06ad06679a936cf8
  schema: mindie-entry/3
  summary: 已完成一份针对 DeepSeek V3.1 与 Kimi K3 的 A3、TP16 MLA/FIA 静态分析，重点覆盖 Prefill/Decode 调用链、shape/dtype/attribute、head padding、物理 384→逻辑 128 block 展开及 A3 tiling。关键修正是 K3 Prefill 的 combined 192D 可被 parser 识别为 RoPE combine，不能简单断言其必然走通用 tiling；Decode 的 6→8 padding 与量化/MLAPO 路径差异仍是待实机验证项。详细 Markdown 据称已发布为 vllm-ascend/vllm-ascend-kimi-k3#36 的新评论，未覆盖原有内容。
  title: 当前任务导航：A3/TP16 MLA–FIA 分析与 Issue 文档
navigation: 已完成一份针对 DeepSeek V3.1 与 Kimi K3 的 A3、TP16 MLA/FIA 静态分析，重点覆盖 Prefill/Decode 调用链、shape/dtype/attribute、head padding、物理 384→逻辑 128 block 展开及 A3 tiling。关键修正是 K3 Prefill 的 combined 192D 可被 parser 识别为 RoPE combine，不能简单断言其必然走通用 tiling；Decode 的 6→8 padding 与量化/MLAPO 路径差异仍是待实机验证项。详细 Markdown 据称已发布为 vllm-ascend/vllm-ascend-kimi-k3#36 的新评论，未覆盖原有内容。
schema: mindie-material-task/1
status: complete
task_id: c503b369cf096880d7636f975d23902872120e32885385fa70b373e6b814a7cb
---

# 当前任务导航：A3/TP16 MLA–FIA 分析与 Issue 文档

已完成一份针对 DeepSeek V3.1 与 Kimi K3 的 A3、TP16 MLA/FIA 静态分析，重点覆盖 Prefill/Decode 调用链、shape/dtype/attribute、head padding、物理 384→逻辑 128 block 展开及 A3 tiling。关键修正是 K3 Prefill 的 combined 192D 可被 parser 识别为 RoPE combine，不能简单断言其必然走通用 tiling；Decode 的 6→8 padding 与量化/MLAPO 路径差异仍是待实机验证项。详细 Markdown 据称已发布为 vllm-ascend/vllm-ascend-kimi-k3#36 的新评论，未覆盖原有内容。

## Materials

- [A3/TP16 下 DeepSeek V3.1 与 Kimi K3 的 MLA/FIA 对比及文档发布](blocks/149cb27131869246f328681bf2e8a7272ff921b31355d883cd2ededc1db8e17b.md): 围绕当前 ci-new 提交 8f192c9f，对 A3、TP16 下 DeepSeek V3.1 与 Kimi K3 的 MLA/FIA 路径进行了静态代码与 tiling 分析。结论区分 Prefill 和 Decode：Prefill 的输入表示分别为 DeepSeek 的 8 heads、128D 主体加 64D RoPE，以及 K3 的 6 heads、combined 192D；后续核对修正了“K3 必然落入通用 tiling”的判断，指出 FIA parser 可将其识别为 RoPE combine 并进入高性能 GQA capability。Decode 中 K3 的 6→8 head padding 仍是额外工作量来源，但物理 block 384 会被展开为 3×128，不能直接作为 FIA 接收 384 block 导致退化的证据；FA-quant、MLAPO、PD-D 等路径差异及实际性能仍需运行时 tiling key/profiler 验证。随后生成了详细 Markdown 文档，并将其作为完整新评论发布到 vllm-ascend/vllm-ascend-kimi-k3#36；文档和发布状态据称已回读确认，但原始材料未提供可独立核验的本地路径。

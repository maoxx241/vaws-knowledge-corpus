---
blocks:
- block_id: f1db088477b49e460de9ac41fc200dedfda79702dcc33fa20b7ee03c34123b98
  indexed: true
  sha256: 99e510ecc623e3b114113febd01032ba9f9864a56daa3af80585c7768530179a
  source_range:
    end: 16417788
    part: 0
    start: 0
  summary: 源码审计认为 DSV4.1 KV-group metadata builder 存在跨组重复构建，提出 batch 级共享请求计数、RoPE 和 SMLA/QLI metadata，同时保留 group-local slot mapping；独立 metadata A/B 报告构建耗时约从 19.0–19.3 ms 降至 4.3–4.5 ms，但整模型 serving/TPOT 未验证。随后 Engram 审计发现 prefill 热路径存在多处 H2D/D2H 和 CPU 控制同步，并参考 SRA 提出将 hash/history、head 分片和定长通信迁移到设备端；截至该块结尾，prefill 优先的 HBM 方案仍是待实现方案，尚未改代码。
  title: DSV4.1 KV metadata 复用与 Engram HBM 路线
- block_id: f3b1541a34449021efaa0bf9b3794c8f2a194d414a8a1635a3606e0a5fc6765c
  indexed: true
  sha256: 0e6bc99b48d3680232655e4d4eafed8bf171e942bffa6f941bf0efdd6285669e
  source_range:
    end: 16417788
    part: 1
    start: 0
  summary: '实现了设备侧 hash/history、TP/DP head 分片和定长通信，并以 Triton 连续行 tiling 优化重排 kernel；合成 4097-token 用例报告约从 9.24 ms 降至 31.7 μs，prefill、chunked prefill、decode 图回放及 TP8×DP2 检查通过，但整模型吞吐、CI 和全仓 lint 仍未完成。后续审计发现 query 量化、top-k 索引整理及 Engram hash 存在动态 constexpr/JIT key 风险；用户接受约 1.4 μs 的小尺寸劣化后，采用单版本动态计数修复，不拆 prefill/decode，PR #41 报告 149 项测试通过，但 CI 尚未完成。'
  title: Engram prefill HBM 实现与 Triton JIT 风险修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bad4bf3370420a4fa68a1300b59acf74e43c293bf54a187d85299b012073e8ed
  kind: experience
  material_digest: b5e72ff28b6ffbd458b19b7679d1778573640bd4a5999c2dd9d097a295c51e43
  revision: 632cd336442cd5330735aac18ce8bb47b3d6d738470062518efff03504ab53e0
  schema: mindie-entry/3
  summary: '已有工作包括 KV metadata 跨 group 复用、Engram prefill 优先的设备侧 hash/history 与 TP/DP 分片通信，以及 Triton 重排和动态 JIT 特化修复。已报告的验证覆盖 metadata 构建、prefill、chunked prefill、decode 图回放和 TP8×DP2；仍需区分算子级结果与整模型收益，并跟踪 PR #40/#41 的 CI、整网吞吐、TPOT、真实部署同步开销及主线既有 lint/测试失败。'
  title: 当前任务导航：DSV4.1 metadata、Engram HBM 与 Triton 稳定性
navigation: '已有工作包括 KV metadata 跨 group 复用、Engram prefill 优先的设备侧 hash/history 与 TP/DP 分片通信，以及 Triton 重排和动态 JIT 特化修复。已报告的验证覆盖 metadata 构建、prefill、chunked prefill、decode 图回放和 TP8×DP2；仍需区分算子级结果与整模型收益，并跟踪 PR #40/#41 的 CI、整网吞吐、TPOT、真实部署同步开销及主线既有 lint/测试失败。'
schema: mindie-material-task/1
status: complete
task_id: bad4bf3370420a4fa68a1300b59acf74e43c293bf54a187d85299b012073e8ed
---

# 当前任务导航：DSV4.1 metadata、Engram HBM 与 Triton 稳定性

已有工作包括 KV metadata 跨 group 复用、Engram prefill 优先的设备侧 hash/history 与 TP/DP 分片通信，以及 Triton 重排和动态 JIT 特化修复。已报告的验证覆盖 metadata 构建、prefill、chunked prefill、decode 图回放和 TP8×DP2；仍需区分算子级结果与整模型收益，并跟踪 PR #40/#41 的 CI、整网吞吐、TPOT、真实部署同步开销及主线既有 lint/测试失败。

## Materials

- [DSV4.1 KV metadata 复用与 Engram HBM 路线](blocks/f1db088477b49e460de9ac41fc200dedfda79702dcc33fa20b7ee03c34123b98.md): 源码审计认为 DSV4.1 KV-group metadata builder 存在跨组重复构建，提出 batch 级共享请求计数、RoPE 和 SMLA/QLI metadata，同时保留 group-local slot mapping；独立 metadata A/B 报告构建耗时约从 19.0–19.3 ms 降至 4.3–4.5 ms，但整模型 serving/TPOT 未验证。随后 Engram 审计发现 prefill 热路径存在多处 H2D/D2H 和 CPU 控制同步，并参考 SRA 提出将 hash/history、head 分片和定长通信迁移到设备端；截至该块结尾，prefill 优先的 HBM 方案仍是待实现方案，尚未改代码。
- [Engram prefill HBM 实现与 Triton JIT 风险修复](blocks/f3b1541a34449021efaa0bf9b3794c8f2a194d414a8a1635a3606e0a5fc6765c.md): 实现了设备侧 hash/history、TP/DP head 分片和定长通信，并以 Triton 连续行 tiling 优化重排 kernel；合成 4097-token 用例报告约从 9.24 ms 降至 31.7 μs，prefill、chunked prefill、decode 图回放及 TP8×DP2 检查通过，但整模型吞吐、CI 和全仓 lint 仍未完成。后续审计发现 query 量化、top-k 索引整理及 Engram hash 存在动态 constexpr/JIT key 风险；用户接受约 1.4 μs 的小尺寸劣化后，采用单版本动态计数修复，不拆 prefill/decode，PR #41 报告 149 项测试通过，但 CI 尚未完成。

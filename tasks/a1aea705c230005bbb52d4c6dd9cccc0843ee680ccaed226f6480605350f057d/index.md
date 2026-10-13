---
blocks:
- block_id: 5f305028a232c03de431645e55c5f79ace75026d6f486a378dda8ede016a94e5
  indexed: true
  sha256: ca463565b94a88ca13adc45dc75deb797b8740d4e1f8e5f00080eaa118053ce7
  source_range:
    end: 1298200
    part: 0
    start: 0
  summary: 来源中的助手报告称，A3 的 `DeviceOperator.split_qkv_rmsnorm_rope` 可能替换 `DFlashQwen3Attention.forward` 的 split、Q/K norm 与 RoPE，且需关注 Neox-style、输入连续性、bias 成对传递，以及首次 compile 前安装 monkeypatch；这些内容仍是报告性结论，未在本索引中验证。报告还指出默认图融合可能已自动使用同一算子，应检查 `Fused N QKNorm and Rope patterns` 日志；若 N>0，则该路径可能不是首 block 分叉来源。TP16 的 Q/K/V 分片和 KV head replication 据报告在 vLLM 与 SGLang 中一致，但未发现确定差异，且整体结果保留为只读核对与风险提示，而非已证实成功。
  title: A3 QKV Norm/RoPE 替换核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a1aea705c230005bbb52d4c6dd9cccc0843ee680ccaed226f6480605350f057d
  kind: experience
  material_digest: 1600b18a330f6aebed600df3827102e39d5fd31a22774781f092c46753403968
  revision: 71cdd3c933c714c2b4f97eb93c5d31db7a4503a249245d5deb9d25a4aab5a937
  schema: mindie-entry/3
  summary: 本批仅包含一个独立材料块，涉及 A3 `split_qkv_rmsnorm_rope`、DFlashQwen3Attention、monkeypatch、compile 时机、图融合日志及 TP16 分片逻辑。相关内容均为来源中的报告或核对结论，包含未验证假设与风险；应优先核查 `Fused N QKNorm and Rope patterns`，并保留“可能已自动融合”及“未发现确定差异”等不确定性。
  title: 当前任务导航
navigation: 本批仅包含一个独立材料块，涉及 A3 `split_qkv_rmsnorm_rope`、DFlashQwen3Attention、monkeypatch、compile 时机、图融合日志及 TP16 分片逻辑。相关内容均为来源中的报告或核对结论，包含未验证假设与风险；应优先核查 `Fused N QKNorm and Rope patterns`，并保留“可能已自动融合”及“未发现确定差异”等不确定性。
schema: mindie-material-task/1
status: complete
task_id: a1aea705c230005bbb52d4c6dd9cccc0843ee680ccaed226f6480605350f057d
---

# 当前任务导航

本批仅包含一个独立材料块，涉及 A3 `split_qkv_rmsnorm_rope`、DFlashQwen3Attention、monkeypatch、compile 时机、图融合日志及 TP16 分片逻辑。相关内容均为来源中的报告或核对结论，包含未验证假设与风险；应优先核查 `Fused N QKNorm and Rope patterns`，并保留“可能已自动融合”及“未发现确定差异”等不确定性。

## Materials

- [A3 QKV Norm/RoPE 替换核对](blocks/5f305028a232c03de431645e55c5f79ace75026d6f486a378dda8ede016a94e5.md): 来源中的助手报告称，A3 的 `DeviceOperator.split_qkv_rmsnorm_rope` 可能替换 `DFlashQwen3Attention.forward` 的 split、Q/K norm 与 RoPE，且需关注 Neox-style、输入连续性、bias 成对传递，以及首次 compile 前安装 monkeypatch；这些内容仍是报告性结论，未在本索引中验证。报告还指出默认图融合可能已自动使用同一算子，应检查 `Fused N QKNorm and Rope patterns` 日志；若 N>0，则该路径可能不是首 block 分叉来源。TP16 的 Q/K/V 分片和 KV head replication 据报告在 vLLM 与 SGLang 中一致，但未发现确定差异，且整体结果保留为只读核对与风险提示，而非已证实成功。

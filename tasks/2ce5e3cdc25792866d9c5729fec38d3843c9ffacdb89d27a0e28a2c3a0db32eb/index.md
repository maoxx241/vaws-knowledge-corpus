---
blocks:
- block_id: bfb1d86dd40babaa650fba0d875af373535f1093b8c9c23eb83e1bda930249fa
  indexed: true
  sha256: 839208bc5a56b84bebf66e8c4f6c4f9a97f7dd66f71621f45e7827d5283da4be
  source_range:
    end: 562439
    part: 0
    start: 0
  summary: 审查认为固定 Kimi K3 GQA 场景暂无 shape 或 cache blocker；SGLang RMSNorm 使用 `1 / tl.sqrt(...)`，当前 `torch.rsqrt(...)` 建议改为 `torch.reciprocal(torch.sqrt(...))`。RoPE 顺序已与 SGLang 一致，Q/K 最终转回 BF16、V 原样返回，query K 会写入 draft cache，而 context K/V 不受影响。全局 monkeypatch 范围和通用 bias 判定仍需收窄或修正；Triton 是否生成 FMA 未验证，应以最终 BF16 Q/K 的 row probe 为准。
  title: FP32 Q/K 路径与 SGLang kernel 语义核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2ce5e3cdc25792866d9c5729fec38d3843c9ffacdb89d27a0e28a2c3a0db32eb
  kind: experience
  material_digest: 4005d1a4b188cd64487fee17e0f8b84057888c973d27edf07065d84cae0c7625
  revision: 9d51e6946e0f2c71d57950de236861fb21305dc01f79a30e789468f7f8489b18
  schema: mindie-entry/3
  summary: 已记录 FP32 Q/K 路径的 shape、RoPE、dtype 与 cache 结论，以及尚未验证的 Triton lowering。后续重点是 RMSNorm 实现形式、monkeypatch 适用范围、bias 判定和最终 BF16 row probe；上述场景结论属于审查意见，不等同于已验证成功。
  title: 当前任务导航：FP32 Q/K 与 SGLang 语义核对
navigation: 已记录 FP32 Q/K 路径的 shape、RoPE、dtype 与 cache 结论，以及尚未验证的 Triton lowering。后续重点是 RMSNorm 实现形式、monkeypatch 适用范围、bias 判定和最终 BF16 row probe；上述场景结论属于审查意见，不等同于已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 2ce5e3cdc25792866d9c5729fec38d3843c9ffacdb89d27a0e28a2c3a0db32eb
---

# 当前任务导航：FP32 Q/K 与 SGLang 语义核对

已记录 FP32 Q/K 路径的 shape、RoPE、dtype 与 cache 结论，以及尚未验证的 Triton lowering。后续重点是 RMSNorm 实现形式、monkeypatch 适用范围、bias 判定和最终 BF16 row probe；上述场景结论属于审查意见，不等同于已验证成功。

## Materials

- [FP32 Q/K 路径与 SGLang kernel 语义核对](blocks/bfb1d86dd40babaa650fba0d875af373535f1093b8c9c23eb83e1bda930249fa.md): 审查认为固定 Kimi K3 GQA 场景暂无 shape 或 cache blocker；SGLang RMSNorm 使用 `1 / tl.sqrt(...)`，当前 `torch.rsqrt(...)` 建议改为 `torch.reciprocal(torch.sqrt(...))`。RoPE 顺序已与 SGLang 一致，Q/K 最终转回 BF16、V 原样返回，query K 会写入 draft cache，而 context K/V 不受影响。全局 monkeypatch 范围和通用 bias 判定仍需收窄或修正；Triton 是否生成 FMA 未验证，应以最终 BF16 Q/K 的 row probe 为准。

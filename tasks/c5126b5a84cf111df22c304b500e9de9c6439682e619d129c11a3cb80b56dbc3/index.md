---
blocks:
- block_id: ce2cc2b8f024cca92b183fcb97aa473478e7a5843a67ca8ba2303d08f6bdc614
  indexed: true
  sha256: 57627429eef17d8b9086f8caa366c7c36b8191ea3af4e03345c8fecef77a3fba
  source_range:
    end: 1192889
    part: 0
    start: 0
  summary: 只读审计记录称，ci-new 已含 K3 Mooncake unequal-TP 适配且与 main-clean 的 stable patch-id 一致；但当前 P/D 配置仅为 TP16 对等拓扑，不能据此视为 unequal-TP 已完成运行时验证。待保留的适配点包括 P 端绕过本地 prefix 命中、通过 `dspark_draft_kv_tokens` 传递覆盖长度并由 D 端按 K3 `N-1` 校验、以及仅内部 remote-decode 请求绕过完整 XTML parser；这些是审计结论与建议，并非已验证成功。建议的五层矩阵使用 P 153/154、D 155/156、DP2×TP16、FULL_DECODE_ONLY、draft K=7、prefix cache 和 `HCCL_BUFFSIZE=512`，以 cold/warm external/local hit、KV group、state/attention 传输及 parser/receive/load 错误作为观察项；`accepted_tokens=0` 只能记录，不能单独判定 KV 传输失败。
  title: Mooncake/P-D 五层 KV 传输适配与验证审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c5126b5a84cf111df22c304b500e9de9c6439682e619d129c11a3cb80b56dbc3
  kind: experience
  material_digest: 38ea208a3444f62b4ee5dab660e516d4b3d68ca14a5010d71dbeca0bd4ff7de9
  revision: 98a0e33d9e37a8915e1a33939899f9cb0d5801da7b6666388b08c8902f04859a
  schema: mindie-entry/3
  summary: 本批仅包含一个完整材料块，主题是 DSpark/Mooncake P-D KV 传输的只读审计、待保留适配点和五层验证矩阵。已有记录明确指出 unequal-TP 尚未被该 TP16 对等配置验证；后续检索应重点关注 `dspark_draft_kv_tokens`、K3 `N-1` 长度校验、remote-decode parser、cold/warm hit 指标、group mismatch、invalid block、receive/load 与 parser 错误，并保留未完成、回退或仅观察到的结果。
  title: 当前任务导航：Mooncake/P-D 五层 KV 传输审计
navigation: 本批仅包含一个完整材料块，主题是 DSpark/Mooncake P-D KV 传输的只读审计、待保留适配点和五层验证矩阵。已有记录明确指出 unequal-TP 尚未被该 TP16 对等配置验证；后续检索应重点关注 `dspark_draft_kv_tokens`、K3 `N-1` 长度校验、remote-decode parser、cold/warm hit 指标、group mismatch、invalid block、receive/load 与 parser 错误，并保留未完成、回退或仅观察到的结果。
schema: mindie-material-task/1
status: complete
task_id: c5126b5a84cf111df22c304b500e9de9c6439682e619d129c11a3cb80b56dbc3
---

# 当前任务导航：Mooncake/P-D 五层 KV 传输审计

本批仅包含一个完整材料块，主题是 DSpark/Mooncake P-D KV 传输的只读审计、待保留适配点和五层验证矩阵。已有记录明确指出 unequal-TP 尚未被该 TP16 对等配置验证；后续检索应重点关注 `dspark_draft_kv_tokens`、K3 `N-1` 长度校验、remote-decode parser、cold/warm hit 指标、group mismatch、invalid block、receive/load 与 parser 错误，并保留未完成、回退或仅观察到的结果。

## Materials

- [Mooncake/P-D 五层 KV 传输适配与验证审计](blocks/ce2cc2b8f024cca92b183fcb97aa473478e7a5843a67ca8ba2303d08f6bdc614.md): 只读审计记录称，ci-new 已含 K3 Mooncake unequal-TP 适配且与 main-clean 的 stable patch-id 一致；但当前 P/D 配置仅为 TP16 对等拓扑，不能据此视为 unequal-TP 已完成运行时验证。待保留的适配点包括 P 端绕过本地 prefix 命中、通过 `dspark_draft_kv_tokens` 传递覆盖长度并由 D 端按 K3 `N-1` 校验、以及仅内部 remote-decode 请求绕过完整 XTML parser；这些是审计结论与建议，并非已验证成功。建议的五层矩阵使用 P 153/154、D 155/156、DP2×TP16、FULL_DECODE_ONLY、draft K=7、prefix cache 和 `HCCL_BUFFSIZE=512`，以 cold/warm external/local hit、KV group、state/attention 传输及 parser/receive/load 错误作为观察项；`accepted_tokens=0` 只能记录，不能单独判定 KV 传输失败。

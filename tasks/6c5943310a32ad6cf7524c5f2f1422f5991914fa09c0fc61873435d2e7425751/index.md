---
blocks:
- block_id: 78bc23839ca37527b9fd270fd9e379104aa941ddf89f4228cf61b8460fcb3d7e
  indexed: true
  sha256: 883bd8ffdf5495655f22d32eef34a2b891b399bd509321aa459d2cc6bae428d7
  source_range:
    end: 475424
    part: 0
    start: 0
  summary: 只读结论认为配置 A/B 不能单独证明同一 CPU collective ordinal 上的 target/draft 错配；async-off 或 balance-on 的通过结果可能分别反映必要条件或时序掩蔽，仍需原始失败配置下的 host-only ledger。建议在 `_sync_metadata_across_dp` 的 CPU `all_reduce` 前后加入可由独占 marker 启用的同步日志，记录 ordinal、phase、DP rank、TP-local-rank0、token 数及 packed tensor；`enter` 无 `exit` 指向 CPU collective 卡住，跨 DP ordinal 的 phase 不一致才支持错配假设。该方案明确不调用 NPU API、不做 device sync，marker 在 health=200 后创建并确认四容器可见；推荐按 async-on/balance-off、balance-on、再 balance-off 及 async-off 交叉验证，但补丁确认后应删除，且后续可能仍需转向 TP/EP collective。
  title: CPU collective enter/exit ledger 诊断补丁与验证顺序
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6c5943310a32ad6cf7524c5f2f1422f5991914fa09c0fc61873435d2e7425751
  kind: experience
  material_digest: 9291edf1e7e800d360129264a566fd1ecbb70fe02b6fe6612c36bc5516f10134
  revision: 9e94db53f7e800db869dec5ad4e277de1e8dc657c6520aa80892087b45931d98
  schema: mindie-entry/3
  summary: 当前材料聚焦于用 CPU collective 的 enter/exit ledger 区分 target/draft phase ordinal 错配与时序/后续 collective 问题。此前配置 A/B 结果仅被视为条件性线索，不是验证成功；建议保留失败日志、记录不完整的 enter/exit 配对，并按 balance 与 async 配置顺序进行反向验证。
  title: DP phase ledger 诊断导航
navigation: 当前材料聚焦于用 CPU collective 的 enter/exit ledger 区分 target/draft phase ordinal 错配与时序/后续 collective 问题。此前配置 A/B 结果仅被视为条件性线索，不是验证成功；建议保留失败日志、记录不完整的 enter/exit 配对，并按 balance 与 async 配置顺序进行反向验证。
schema: mindie-material-task/1
status: complete
task_id: 6c5943310a32ad6cf7524c5f2f1422f5991914fa09c0fc61873435d2e7425751
---

# DP phase ledger 诊断导航

当前材料聚焦于用 CPU collective 的 enter/exit ledger 区分 target/draft phase ordinal 错配与时序/后续 collective 问题。此前配置 A/B 结果仅被视为条件性线索，不是验证成功；建议保留失败日志、记录不完整的 enter/exit 配对，并按 balance 与 async 配置顺序进行反向验证。

## Materials

- [CPU collective enter/exit ledger 诊断补丁与验证顺序](blocks/78bc23839ca37527b9fd270fd9e379104aa941ddf89f4228cf61b8460fcb3d7e.md): 只读结论认为配置 A/B 不能单独证明同一 CPU collective ordinal 上的 target/draft 错配；async-off 或 balance-on 的通过结果可能分别反映必要条件或时序掩蔽，仍需原始失败配置下的 host-only ledger。建议在 `_sync_metadata_across_dp` 的 CPU `all_reduce` 前后加入可由独占 marker 启用的同步日志，记录 ordinal、phase、DP rank、TP-local-rank0、token 数及 packed tensor；`enter` 无 `exit` 指向 CPU collective 卡住，跨 DP ordinal 的 phase 不一致才支持错配假设。该方案明确不调用 NPU API、不做 device sync，marker 在 health=200 后创建并确认四容器可见；推荐按 async-on/balance-off、balance-on、再 balance-off 及 async-off 交叉验证，但补丁确认后应删除，且后续可能仍需转向 TP/EP collective。

---
blocks:
- block_id: 13d15b98bf2f899a3fcba86ea1db31d56103130fc770227794aebfa40f82c352
  indexed: true
  sha256: bc7c742000f132b0580cae52e5053561cadd5f02086e62587b0381d75f5647d9
  source_range:
    end: 933116
    part: 0
    start: 0
  summary: 只读审计报告称，KDA 的 global/local hidden、metadata、position、learned residual 及 `o_proj` reduce-scatter 布局方向正确，但明确指出 MLA 当前先对 TP-local hidden 执行列并行 `g_proj`、再 all-gather gate logits，可能造成跨 TP rank 的 token/head 错配。报告建议改为先用 `maybe_all_gather_and_maybe_unpad` 聚合 hidden，再调用 `g_proj`，并相应更新测试；同时提出统一首层判断为 `uses_global_inputs_embeds`、将首层 buffer 从 `N // TP` 改为 ceil division 的低风险建议。该结论来自审计与示例性错误分析，未修改文件、未操作远端服务，也未验证修复成功；相关 case 为 `KDA/KDA/KDA/MLA/KDA`，测试当前被称为固化了错误顺序。
  title: Kimi K3 FlashComm 布局审计与 MLA gather 顺序问题
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: faa615d8c516bacbec7754a8cb436fedc6cfd4f91b156f83baa3d8f6b6463a58
  kind: experience
  material_digest: 6a0d5ca0106b913de66c16f48a34e865cdf4346fe762a0bfd503810df6224bc5
  revision: 5dc353979e490e8631e588c13abbac28e0f1ab5b0f8696da2eba01a73e26ac82
  schema: mindie-entry/3
  summary: 新增材料聚焦 Kimi K3 中 KDA、MLA、learned residual 的 global/local token 分片与 metadata/position 同步关系，并记录一个待修正的 MLA `g_proj` 与 all-gather 顺序问题。保留的未完成事项包括更新 `test_kimi_k3_gate_uses_same_gather_unpad_as_q_and_kv`、评估 `uses_global_inputs_embeds` 统一及 ceil division 防御性调整；材料仅为易错参考索引，不能视为已验证修复或成功结果。
  title: 当前任务导航：FlashComm/Kimi K3 首层布局审计
navigation: 新增材料聚焦 Kimi K3 中 KDA、MLA、learned residual 的 global/local token 分片与 metadata/position 同步关系，并记录一个待修正的 MLA `g_proj` 与 all-gather 顺序问题。保留的未完成事项包括更新 `test_kimi_k3_gate_uses_same_gather_unpad_as_q_and_kv`、评估 `uses_global_inputs_embeds` 统一及 ceil division 防御性调整；材料仅为易错参考索引，不能视为已验证修复或成功结果。
schema: mindie-material-task/1
status: complete
task_id: faa615d8c516bacbec7754a8cb436fedc6cfd4f91b156f83baa3d8f6b6463a58
---

# 当前任务导航：FlashComm/Kimi K3 首层布局审计

新增材料聚焦 Kimi K3 中 KDA、MLA、learned residual 的 global/local token 分片与 metadata/position 同步关系，并记录一个待修正的 MLA `g_proj` 与 all-gather 顺序问题。保留的未完成事项包括更新 `test_kimi_k3_gate_uses_same_gather_unpad_as_q_and_kv`、评估 `uses_global_inputs_embeds` 统一及 ceil division 防御性调整；材料仅为易错参考索引，不能视为已验证修复或成功结果。

## Materials

- [Kimi K3 FlashComm 布局审计与 MLA gather 顺序问题](blocks/13d15b98bf2f899a3fcba86ea1db31d56103130fc770227794aebfa40f82c352.md): 只读审计报告称，KDA 的 global/local hidden、metadata、position、learned residual 及 `o_proj` reduce-scatter 布局方向正确，但明确指出 MLA 当前先对 TP-local hidden 执行列并行 `g_proj`、再 all-gather gate logits，可能造成跨 TP rank 的 token/head 错配。报告建议改为先用 `maybe_all_gather_and_maybe_unpad` 聚合 hidden，再调用 `g_proj`，并相应更新测试；同时提出统一首层判断为 `uses_global_inputs_embeds`、将首层 buffer 从 `N // TP` 改为 ceil division 的低风险建议。该结论来自审计与示例性错误分析，未修改文件、未操作远端服务，也未验证修复成功；相关 case 为 `KDA/KDA/KDA/MLA/KDA`，测试当前被称为固化了错误顺序。

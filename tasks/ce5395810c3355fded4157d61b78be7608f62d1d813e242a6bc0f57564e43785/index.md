---
blocks:
- block_id: b9fa6473a3b2376f87a80a82fde724e97c9e5060b44cede6d65b35350c69d45e
  indexed: true
  sha256: f027271b45a9f56942f0f77a0d729040d26339fe1ed44dfc013e078a6ec6ba49
  source_range:
    end: 480065
    part: 0
    start: 0
  summary: '只读检查显示 PR #12950 当前 head 为 `08d057eced58c777f1d21679089aad89d445d453`，相较 `417a39c0` 发生 force-push，compare 为 diverged（ahead 11 / behind 10）；PR 仍为 OPEN、MERGEABLE。Checks 有 11 success、1 skipped、26 running、1 failure；唯一失败的 CPU job（run `30504886019`、job `90753302539`）中，`test_kimi_k3_vision_tp16_falls_back_to_data_parallel` 因 v0.25.1 的 `is_vit_use_data_parallel()` 不接受传入参数而触发 `TypeError`，结果为 1 failed、1925 passed、33 skipped。`pre-commit`、DCO、select-tests 和两个 upstream E2E 已通过；未进行修改或推送。'
  title: 'PR #12950 重写历史与 v0.25.1 CPU 兼容性失败'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ce5395810c3355fded4157d61b78be7608f62d1d813e242a6bc0f57564e43785
  kind: experience
  material_digest: 2aed83c561fc1390caa1b28437f5b56965dd698e3650116bcc7b58dafff1bc85
  revision: 865f83596692de2dd17cb6ac2ce205075778fdbd5df7cb8647c9957d75493533
  schema: mindie-entry/3
  summary: 当前可检索重点是 PR head 的再次 force-push/历史重写，以及 v0.25.1 CPU lane 中 `is_vit_use_data_parallel(config.num_attention_heads)` 的 API 兼容性失败。其他检查多数通过但仍有 26 个 running；该材料记录的是检查结果与失败证据，不代表问题已修复或 PR 已完成验证。
  title: '当前任务导航：PR #12950 只读状态索引'
navigation: 当前可检索重点是 PR head 的再次 force-push/历史重写，以及 v0.25.1 CPU lane 中 `is_vit_use_data_parallel(config.num_attention_heads)` 的 API 兼容性失败。其他检查多数通过但仍有 26 个 running；该材料记录的是检查结果与失败证据，不代表问题已修复或 PR 已完成验证。
schema: mindie-material-task/1
status: complete
task_id: ce5395810c3355fded4157d61b78be7608f62d1d813e242a6bc0f57564e43785
---

# 当前任务导航：PR #12950 只读状态索引

当前可检索重点是 PR head 的再次 force-push/历史重写，以及 v0.25.1 CPU lane 中 `is_vit_use_data_parallel(config.num_attention_heads)` 的 API 兼容性失败。其他检查多数通过但仍有 26 个 running；该材料记录的是检查结果与失败证据，不代表问题已修复或 PR 已完成验证。

## Materials

- [PR #12950 重写历史与 v0.25.1 CPU 兼容性失败](blocks/b9fa6473a3b2376f87a80a82fde724e97c9e5060b44cede6d65b35350c69d45e.md): 只读检查显示 PR #12950 当前 head 为 `08d057eced58c777f1d21679089aad89d445d453`，相较 `417a39c0` 发生 force-push，compare 为 diverged（ahead 11 / behind 10）；PR 仍为 OPEN、MERGEABLE。Checks 有 11 success、1 skipped、26 running、1 failure；唯一失败的 CPU job（run `30504886019`、job `90753302539`）中，`test_kimi_k3_vision_tp16_falls_back_to_data_parallel` 因 v0.25.1 的 `is_vit_use_data_parallel()` 不接受传入参数而触发 `TypeError`，结果为 1 failed、1925 passed、33 skipped。`pre-commit`、DCO、select-tests 和两个 upstream E2E 已通过；未进行修改或推送。

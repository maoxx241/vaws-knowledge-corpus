---
blocks:
- block_id: 5e5a9f426062557a24e2d434b6bc7b9bdd146115a194d984331fa059cb453451
  indexed: true
  sha256: 887f3b7e88d3cd801f80fa4bb55c2e739cc227824d7ae97254a9a376ecb2603b
  source_range:
    end: 1381744
    part: 0
    start: 0
  summary: 审计确认，阻断本轮 FULL 图验证的问题是 GDN/KDA dummy row 使用了 `num_accepted_tokens=1`；在 K=7、实际 8 tokens、TP16 下，dummy row 应为零，并配套 `query_start_loc=[0,8,8]`、`actual_seq_lengths=[0,8,0]`、`num_accepted_tokens=[actual,0]`。MLA padding、FIA workspace、`ExternalEvent` replay 顺序均未发现问题；input ID/embedding 尾部未统一清零被标为次级加固项，且未运行 NPU。修复后若仍不收敛，建议记录 DP rank 的 actual/padded token 数、graph mode 及 GDN 五组张量，但不要扩大修改范围。
  title: FULL 图验证：GDN/KDA dummy row 修复审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8530db3c29a4bef338e9035aae4f24a153a9b94f50719f3bf55230e3f3c71144
  kind: experience
  material_digest: 46676b0ec206a98a90c6b1657031de1a35f08f03398286157934c92127b60031
  revision: 61f937819ab20d37af704ad50603404a6079302d1e28940a6093aa1d42ee9741
  schema: mindie-entry/3
  summary: 当前材料聚焦 K=7、TP16 下 dummy request 的 padding 合法性，尤其是 GDN/KDA 的 `num_accepted_tokens`、query 起止位置和状态输入；已记录修复结论及未运行 NPU 的限制。MLA/FIA/ExternalEvent 检查未发现阻断项，但尾部清零仍属次级加固，后续若不收敛需补充各 DP rank 和 graph mode 的观测。
  title: FULL 图验证与 GDN/KDA dummy 输入审计
navigation: 当前材料聚焦 K=7、TP16 下 dummy request 的 padding 合法性，尤其是 GDN/KDA 的 `num_accepted_tokens`、query 起止位置和状态输入；已记录修复结论及未运行 NPU 的限制。MLA/FIA/ExternalEvent 检查未发现阻断项，但尾部清零仍属次级加固，后续若不收敛需补充各 DP rank 和 graph mode 的观测。
schema: mindie-material-task/1
status: complete
task_id: 8530db3c29a4bef338e9035aae4f24a153a9b94f50719f3bf55230e3f3c71144
---

# FULL 图验证与 GDN/KDA dummy 输入审计

当前材料聚焦 K=7、TP16 下 dummy request 的 padding 合法性，尤其是 GDN/KDA 的 `num_accepted_tokens`、query 起止位置和状态输入；已记录修复结论及未运行 NPU 的限制。MLA/FIA/ExternalEvent 检查未发现阻断项，但尾部清零仍属次级加固，后续若不收敛需补充各 DP rank 和 graph mode 的观测。

## Materials

- [FULL 图验证：GDN/KDA dummy row 修复审计](blocks/5e5a9f426062557a24e2d434b6bc7b9bdd146115a194d984331fa059cb453451.md): 审计确认，阻断本轮 FULL 图验证的问题是 GDN/KDA dummy row 使用了 `num_accepted_tokens=1`；在 K=7、实际 8 tokens、TP16 下，dummy row 应为零，并配套 `query_start_loc=[0,8,8]`、`actual_seq_lengths=[0,8,0]`、`num_accepted_tokens=[actual,0]`。MLA padding、FIA workspace、`ExternalEvent` replay 顺序均未发现问题；input ID/embedding 尾部未统一清零被标为次级加固项，且未运行 NPU。修复后若仍不收敛，建议记录 DP rank 的 actual/padded token 数、graph mode 及 GDN 五组张量，但不要扩大修改范围。

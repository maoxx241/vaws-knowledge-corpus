---
blocks:
- block_id: 9f459794c9e00d78cd50a145ecf53285c9d2cd1f6681ea328c48c0ed38856239
  indexed: true
  sha256: b49e50be2a66bc445bbcb4f7080cda68f753e98050de8aa0b88b545330ad17c3
  source_range:
    end: 762301
    part: 0
    start: 0
  summary: '审计认为 warning 并非 PR #9 引入，也未在两次 HTTP 200 请求中观察到 transfer failed、invalid block、timeout 或输出不一致；但证据显示 P 侧单 bit tracker 在多个 port task 下会把首个 DONE 误判为请求完成，后续 DONE 触发 warning。该 no-CP HMA 路径涉及 MLA/Mamba port 合并与跨 rank 重合，可能造成 blocks 提前释放或复用，属于潜在生命周期竞态而非已证实的精度错误；建议扩展 remote_port_send_num 计数协议并补充 sending-thread、P16/D8 HMA 计数及 start_load_kv 测试。'
  title: P/D transfer 完成计数与提前释放竞态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e45fbede7d85f0fa8fd1c5eb9ef05241b5bc46b79c0735ba0df255404e78fb8c
  kind: experience
  material_digest: d1d57244bfc0316a0d1552014d4aa534180df94d47b901bd171115421ecda3bf
  revision: 3dac021e4c8c87be51ff93a349a1f11ce5f59aa1cc8c13f038866a615c654c63
  schema: mindie-entry/3
  summary: 当前材料聚焦 no-CP HMA 场景中 P/D transfer 完成语义：两轮请求表面完成且未观察到 correctness 问题，但 P 侧完成计数不足可能导致较晚 DONE、提前释放和潜在精度污染。后续检索应关注 DONE_RECVING_MSG、finished_requests、remote_port_send_num、MLA/Mamba port 合并、跨 rank 计数，以及尚未验证的修复和测试结果。
  title: 当前任务导航
navigation: 当前材料聚焦 no-CP HMA 场景中 P/D transfer 完成语义：两轮请求表面完成且未观察到 correctness 问题，但 P 侧完成计数不足可能导致较晚 DONE、提前释放和潜在精度污染。后续检索应关注 DONE_RECVING_MSG、finished_requests、remote_port_send_num、MLA/Mamba port 合并、跨 rank 计数，以及尚未验证的修复和测试结果。
schema: mindie-material-task/1
status: complete
task_id: e45fbede7d85f0fa8fd1c5eb9ef05241b5bc46b79c0735ba0df255404e78fb8c
---

# 当前任务导航

当前材料聚焦 no-CP HMA 场景中 P/D transfer 完成语义：两轮请求表面完成且未观察到 correctness 问题，但 P 侧完成计数不足可能导致较晚 DONE、提前释放和潜在精度污染。后续检索应关注 DONE_RECVING_MSG、finished_requests、remote_port_send_num、MLA/Mamba port 合并、跨 rank 计数，以及尚未验证的修复和测试结果。

## Materials

- [P/D transfer 完成计数与提前释放竞态审计](blocks/9f459794c9e00d78cd50a145ecf53285c9d2cd1f6681ea328c48c0ed38856239.md): 审计认为 warning 并非 PR #9 引入，也未在两次 HTTP 200 请求中观察到 transfer failed、invalid block、timeout 或输出不一致；但证据显示 P 侧单 bit tracker 在多个 port task 下会把首个 DONE 误判为请求完成，后续 DONE 触发 warning。该 no-CP HMA 路径涉及 MLA/Mamba port 合并与跨 rank 重合，可能造成 blocks 提前释放或复用，属于潜在生命周期竞态而非已证实的精度错误；建议扩展 remote_port_send_num 计数协议并补充 sending-thread、P16/D8 HMA 计数及 start_load_kv 测试。

---
blocks:
- block_id: 291c96e773db8b21a52a51267bfdf5eba384d32e3b37b754b02e4d4e95fda681
  indexed: true
  sha256: 816b332849b6577393e33368ebaa7da06460c1caeec08e664007334e19c1fbc9
  source_range:
    end: 3056311
    part: 0
    start: 0
  summary: 该批次记录了从 r13 隔离复制、冻结 runtime/manifest SHA、CPU 回归与 verifier 语义修订，到四机 staging/preflight 的过程；其中曾发现 record count 统计不完整、GDN raw-hash 比较会误报、MLA metadata 需独立归一化校验等问题，且多项检查最初只是计划或中途失败。后续只读审查提出六类记录计数、GDN state/control 拆分、5 组 MLA metadata、独立 `3*b+i` page 映射及 fail-closed 规则；随后报告 CPU UT 7/7、verifier 正反例 5/5 通过。最终报告称四机 hash/tree、manifest 与 preflight 一致，staging 已清理，日志显示 `deploy_complete=true service_action=none http_requests=0`；但记录也明确主代理之后启动了 r14，本子任务未操作服务或发送 HTTP。
  title: r14 DFX staging、verifier 契约与四机 preflight
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8b50f06fc45bf99adf3823af3fd3b6cf3312a38cc3a456d2a3f8ed0e517417c1
  kind: experience
  material_digest: a118fdab8816a5236d1c60f7c4744547f5cdd31311c61841c15480027047cc90
  revision: e35cdb1664713c46196082efa8d10372498769f8635affa11acc204f77a4c9c4
  schema: mindie-entry/3
  summary: 核心主题包括 runtime/manifest SHA 冻结、四机隔离 staging、verifier 的 record-count/GDN/MLA metadata 契约、CPU 与 synthetic 反例测试，以及 preflight 的中途失败与最终回读。后续检索应区分静态验证、部署前快照和主代理启动后的只读状态；本索引不替代原始消息，未提供的早期细节仍需独立搜索。
  title: 当前任务导航：r14 DFX 部署与验证索引
navigation: 核心主题包括 runtime/manifest SHA 冻结、四机隔离 staging、verifier 的 record-count/GDN/MLA metadata 契约、CPU 与 synthetic 反例测试，以及 preflight 的中途失败与最终回读。后续检索应区分静态验证、部署前快照和主代理启动后的只读状态；本索引不替代原始消息，未提供的早期细节仍需独立搜索。
schema: mindie-material-task/1
status: complete
task_id: 8b50f06fc45bf99adf3823af3fd3b6cf3312a38cc3a456d2a3f8ed0e517417c1
---

# 当前任务导航：r14 DFX 部署与验证索引

核心主题包括 runtime/manifest SHA 冻结、四机隔离 staging、verifier 的 record-count/GDN/MLA metadata 契约、CPU 与 synthetic 反例测试，以及 preflight 的中途失败与最终回读。后续检索应区分静态验证、部署前快照和主代理启动后的只读状态；本索引不替代原始消息，未提供的早期细节仍需独立搜索。

## Materials

- [r14 DFX staging、verifier 契约与四机 preflight](blocks/291c96e773db8b21a52a51267bfdf5eba384d32e3b37b754b02e4d4e95fda681.md): 该批次记录了从 r13 隔离复制、冻结 runtime/manifest SHA、CPU 回归与 verifier 语义修订，到四机 staging/preflight 的过程；其中曾发现 record count 统计不完整、GDN raw-hash 比较会误报、MLA metadata 需独立归一化校验等问题，且多项检查最初只是计划或中途失败。后续只读审查提出六类记录计数、GDN state/control 拆分、5 组 MLA metadata、独立 `3*b+i` page 映射及 fail-closed 规则；随后报告 CPU UT 7/7、verifier 正反例 5/5 通过。最终报告称四机 hash/tree、manifest 与 preflight 一致，staging 已清理，日志显示 `deploy_complete=true service_action=none http_requests=0`；但记录也明确主代理之后启动了 r14，本子任务未操作服务或发送 HTTP。

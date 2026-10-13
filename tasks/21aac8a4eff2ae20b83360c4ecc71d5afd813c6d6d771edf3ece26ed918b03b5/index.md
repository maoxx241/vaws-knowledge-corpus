---
blocks:
- block_id: eb0cb1912c65d362612e5eb16c2f4359cadd7db2489f003f7779981e83df152d
  indexed: true
  sha256: 86dfbcfbd2d3699982ba3df793dcf438f495cedd53736c94d2ad4364a22f0a89
  source_range:
    end: 611974
    part: 0
    start: 0
  summary: 据助手消息，153 节点完成两个指定 SHA 的 Python 源码文件替换、静态校验、原子 final move 和 DP0 `PREFLIGHT_ONLY`，随后拟将冻结的 source/runtime 扩散到 154–156；消息称旧目录保留且服务、请求及日志/DFX/结果目录未创建。最终答复声称四机 source/runtime、code-tree 与 manifests 一致，逐 rank 及全局预检通过，并给出唯一启动命令；但该索引仅记录消息中的报告，不能将这些检查或部署状态独立视为已验证成功。
  title: r14 rankfix 四机隔离部署与预检
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 21aac8a4eff2ae20b83360c4ecc71d5afd813c6d6d771edf3ece26ed918b03b5
  kind: experience
  material_digest: 602a567e01449c7ded8d701a8da711160c626e0e59a9b95140ef74faf0f9b4e5
  revision: 73475aa47cab90f84848ad6a6af37e0d3eac92998978eb7e6608360f515e6a5a
  schema: mindie-entry/3
  summary: 新材料集中于一次据助手报告的 r14 rank-metadata 修复部署：153 先行 staging 与 `PREFLIGHT_ONLY`，再向 154–156 扩散冻结包，保留旧 r14 目录且未启动服务或发送 HTTP 请求。后续检索可使用 `rankfix`、`PREFLIGHT_ONLY`、`DP0`、`source/runtime`、`code-tree`、`manifests`、`原子 final move`、`四机` 和 `20260808-r14-rankfix-dfx-r2`；启动命令虽被报告为唯一命令，但实际启动结果未在材料中给出。
  title: 当前任务导航：r14 rankfix 四机隔离部署
navigation: 新材料集中于一次据助手报告的 r14 rank-metadata 修复部署：153 先行 staging 与 `PREFLIGHT_ONLY`，再向 154–156 扩散冻结包，保留旧 r14 目录且未启动服务或发送 HTTP 请求。后续检索可使用 `rankfix`、`PREFLIGHT_ONLY`、`DP0`、`source/runtime`、`code-tree`、`manifests`、`原子 final move`、`四机` 和 `20260808-r14-rankfix-dfx-r2`；启动命令虽被报告为唯一命令，但实际启动结果未在材料中给出。
schema: mindie-material-task/1
status: complete
task_id: 21aac8a4eff2ae20b83360c4ecc71d5afd813c6d6d771edf3ece26ed918b03b5
---

# 当前任务导航：r14 rankfix 四机隔离部署

新材料集中于一次据助手报告的 r14 rank-metadata 修复部署：153 先行 staging 与 `PREFLIGHT_ONLY`，再向 154–156 扩散冻结包，保留旧 r14 目录且未启动服务或发送 HTTP 请求。后续检索可使用 `rankfix`、`PREFLIGHT_ONLY`、`DP0`、`source/runtime`、`code-tree`、`manifests`、`原子 final move`、`四机` 和 `20260808-r14-rankfix-dfx-r2`；启动命令虽被报告为唯一命令，但实际启动结果未在材料中给出。

## Materials

- [r14 rankfix 四机隔离部署与预检](blocks/eb0cb1912c65d362612e5eb16c2f4359cadd7db2489f003f7779981e83df152d.md): 据助手消息，153 节点完成两个指定 SHA 的 Python 源码文件替换、静态校验、原子 final move 和 DP0 `PREFLIGHT_ONLY`，随后拟将冻结的 source/runtime 扩散到 154–156；消息称旧目录保留且服务、请求及日志/DFX/结果目录未创建。最终答复声称四机 source/runtime、code-tree 与 manifests 一致，逐 rank 及全局预检通过，并给出唯一启动命令；但该索引仅记录消息中的报告，不能将这些检查或部署状态独立视为已验证成功。

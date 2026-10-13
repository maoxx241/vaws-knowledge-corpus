---
blocks:
- block_id: 01cb2cb76d264bcdf6eb57caf34e625dc393083941d11141108f37a3d0594e9d
  indexed: true
  sha256: 21d2811fd4d5bc1e5a9923bc4d20f5dd857cd18545303399acdd5777189ecb9c
  source_range:
    end: 391123
    part: 0
    start: 0
  summary: 记录了四机容器、serve 主进程、r18 模型、`--no-async-scheduling` 及 runtime/DFX/output 路径的只读核对；初次进程解析因 `/proc/*/cmdline` NUL 转义错误而失败，未产生远端修改。部署后复核发现 153 的 runtime root 多出 `__pycache__` 和 `.pyc`，虽 manifest strict check 报告 `9/9 OK`，但不满足原定 10-file frozen payload；随后针对原子换入及 `-B` 目标目录的独立 readback 报告四机均为 10 个 regular files、无 cache/bytecode、manifest 与 content manifest SHA256 分别为 `fce32cd2...` 和 `9aa781d8...`，服务锚点未漂移，153 DFX 为 17 个零字节文件且 output root 仍不存在。
  title: 四机只读审计与 153 缓存污染修正
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f4e240c195f680518e3925075b4822423a3ced7fdcbda83599956d82fb38b885
  kind: experience
  material_digest: f65a9ab750df0bb3173042db09ff0c4bf5be2af8b027f3cbc7902296ce8a37da
  revision: 75882f5530105604f7aba23c9d78931d3c79d53e58bcb0fbe9d0f0446a36a3d0
  schema: mindie-entry/3
  summary: 当前可检索重点是四机服务进程与容器锚点、r18/`--no-async-scheduling` 环境约束、runtime frozen payload 的 checksum/content manifest、153 初次出现的 `__pycache__`/`.pyc` 偏差及后续声称的 `-B` 独立 readback 结果。应保留“初次检查失败或不完整、后续修正、最终仅为报告中的 PASS/readback”这一演变；不要把这些只读检查直接提升为已验证成功，也不要假定未列出的早期细节。
  title: 当前任务导航：四机部署只读复核与 153 污染修正
navigation: 当前可检索重点是四机服务进程与容器锚点、r18/`--no-async-scheduling` 环境约束、runtime frozen payload 的 checksum/content manifest、153 初次出现的 `__pycache__`/`.pyc` 偏差及后续声称的 `-B` 独立 readback 结果。应保留“初次检查失败或不完整、后续修正、最终仅为报告中的 PASS/readback”这一演变；不要把这些只读检查直接提升为已验证成功，也不要假定未列出的早期细节。
schema: mindie-material-task/1
status: complete
task_id: f4e240c195f680518e3925075b4822423a3ced7fdcbda83599956d82fb38b885
---

# 当前任务导航：四机部署只读复核与 153 污染修正

当前可检索重点是四机服务进程与容器锚点、r18/`--no-async-scheduling` 环境约束、runtime frozen payload 的 checksum/content manifest、153 初次出现的 `__pycache__`/`.pyc` 偏差及后续声称的 `-B` 独立 readback 结果。应保留“初次检查失败或不完整、后续修正、最终仅为报告中的 PASS/readback”这一演变；不要把这些只读检查直接提升为已验证成功，也不要假定未列出的早期细节。

## Materials

- [四机只读审计与 153 缓存污染修正](blocks/01cb2cb76d264bcdf6eb57caf34e625dc393083941d11141108f37a3d0594e9d.md): 记录了四机容器、serve 主进程、r18 模型、`--no-async-scheduling` 及 runtime/DFX/output 路径的只读核对；初次进程解析因 `/proc/*/cmdline` NUL 转义错误而失败，未产生远端修改。部署后复核发现 153 的 runtime root 多出 `__pycache__` 和 `.pyc`，虽 manifest strict check 报告 `9/9 OK`，但不满足原定 10-file frozen payload；随后针对原子换入及 `-B` 目标目录的独立 readback 报告四机均为 10 个 regular files、无 cache/bytecode、manifest 与 content manifest SHA256 分别为 `fce32cd2...` 和 `9aa781d8...`，服务锚点未漂移，153 DFX 为 17 个零字节文件且 output root 仍不存在。

---
blocks:
- block_id: a2f79e498744bde5979cc3bb9355fc39a93ffb49db44d30fd53d319ea75aa2e7
  indexed: true
  sha256: b2c79377253e9b9518eec90f9f6b50e04b31def9cfbe16e2ddf184fdc8cb8c38
  source_range:
    end: 6380170
    part: 0
    start: 0
  summary: 双机 TP16×DP2×EP32 的 Block5 服务在参考环境中完成启动：32 个 rank、333/333 分片加载、两端 `/health=200`，真实 `/v1/chat/completions` 均 HTTP 200，speculative counters 也有增长。返回内容出现控制标记泄漏、重复并以 `length` 截断，因此只能确认服务链路和请求成功，不能确认文本质量或归因于 Block5；该进程组随后已停止。原版 MLA 已提交启动但仅观测到 rank 0/16 初始化，之后 153/154 因 UniVPN 数据面中断而 `No route to host`，未验证完成，远端进程是否继续未知；用户随后表示网络已恢复，并要求接着 15253 的修改调 mrv2。
  title: 双机 Block5 已验证，原版 MLA 未完成续验
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 25db1a8c52ab2169afe3b1c8fcef7836e51d732fadef6309d1e5ad3b5e9d38f9
  kind: experience
  material_digest: 987ed0bd226ae4dab3fe7275abb04f2f1d340133c1d60e8aa1fea631ea6bf664
  revision: 1b4b92bde5cf81411280387c05e363d8f95533acb9c1132fdc755992f110fe6f
  schema: mindie-entry/3
  summary: 已确认 Block5 双机服务可启动并完成真实 curl，但输出存在控制标记、重复和 `length` 截断，属于未解释的语义/格式异常；原版 MLA 因网络中断未完成 ready 与 curl 验证。网络现已恢复，后续任务指向在 15253 的修改基础上继续调试 mrv2；原版 MLA 的最终状态仍需重新连接后独立核验。
  title: 双机量化/MLA 服务验证与后续 mrv2
navigation: 已确认 Block5 双机服务可启动并完成真实 curl，但输出存在控制标记、重复和 `length` 截断，属于未解释的语义/格式异常；原版 MLA 因网络中断未完成 ready 与 curl 验证。网络现已恢复，后续任务指向在 15253 的修改基础上继续调试 mrv2；原版 MLA 的最终状态仍需重新连接后独立核验。
schema: mindie-material-task/1
status: complete
task_id: 25db1a8c52ab2169afe3b1c8fcef7836e51d732fadef6309d1e5ad3b5e9d38f9
---

# 双机量化/MLA 服务验证与后续 mrv2

已确认 Block5 双机服务可启动并完成真实 curl，但输出存在控制标记、重复和 `length` 截断，属于未解释的语义/格式异常；原版 MLA 因网络中断未完成 ready 与 curl 验证。网络现已恢复，后续任务指向在 15253 的修改基础上继续调试 mrv2；原版 MLA 的最终状态仍需重新连接后独立核验。

## Materials

- [双机 Block5 已验证，原版 MLA 未完成续验](blocks/a2f79e498744bde5979cc3bb9355fc39a93ffb49db44d30fd53d319ea75aa2e7.md): 双机 TP16×DP2×EP32 的 Block5 服务在参考环境中完成启动：32 个 rank、333/333 分片加载、两端 `/health=200`，真实 `/v1/chat/completions` 均 HTTP 200，speculative counters 也有增长。返回内容出现控制标记泄漏、重复并以 `length` 截断，因此只能确认服务链路和请求成功，不能确认文本质量或归因于 Block5；该进程组随后已停止。原版 MLA 已提交启动但仅观测到 rank 0/16 初始化，之后 153/154 因 UniVPN 数据面中断而 `No route to host`，未验证完成，远端进程是否继续未知；用户随后表示网络已恢复，并要求接着 15253 的修改调 mrv2。

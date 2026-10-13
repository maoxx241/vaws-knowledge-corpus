---
blocks:
- block_id: 04bdb484741ae862a4e753d322184749b0af1c0af876fe9188d231bba3821a84
  indexed: true
  sha256: a8761087ed64ac6a8acce6862bae0d9dda523e574c2042d56210c41ba59c17fb
  source_range:
    end: 8424467
    part: 0
    start: 0
  summary: '记录显示项目 remote-dev 的 JSON Schema 组合写法导致 Kimi API 返回 400，Grok 还无法注册带点号的工具名；随后改用普通对象结构并将对外工具名改为下划线形式，保留服务端校验和旧名称兼容。声称已为 Kimi、Claude＋DSV4、Codex、Grok、Cursor IDE 配置并完成远程补丁写入、编辑、读回及 SHA256 核对，但 Cursor CLI 仍需重新登录；另有 8 个既有 skill-shim 失败，且相关配置自动接入 VAWS 初始化流程尚未实现。来源最后声称 VAWS PR #64 已合入，提交为 `9895b04`，63 项回归和两项 CI 通过；这些均属于该记录中的报告结果，并非独立核验。'
  title: 'VAWS remote-dev MCP 兼容修复与 PR #64'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7d86ac39f75c7ae61a098a4fc3c644eb00991f1892ba2eb3801644a14a060f98
  kind: experience
  material_digest: 29c45058167ed8bfcd1d79c3bbcac8771d2aa1e4ca1d610ae1aee48c06e92b92
  revision: 8d1c7b735f5e7b6c89995b6e5ef96605c594cb0d93488957756d526895fb32c8
  schema: mindie-entry/3
  summary: '当前材料集中于 VAWS 的 remote-dev MCP Schema／工具命名兼容性、四至五个客户端接入、PR #64 合入及验证边界。后续检索应区分已报告的客户端实测、Cursor CLI 未完成登录、既有 skill-shim 失败，以及尚未接入 VAWS 统一初始化流程等未完成或不确定事项；原始消息块仍需独立检索，不能据此补全被省略的早期细节。'
  title: VAWS MCP 兼容性修复与验证索引
navigation: '当前材料集中于 VAWS 的 remote-dev MCP Schema／工具命名兼容性、四至五个客户端接入、PR #64 合入及验证边界。后续检索应区分已报告的客户端实测、Cursor CLI 未完成登录、既有 skill-shim 失败，以及尚未接入 VAWS 统一初始化流程等未完成或不确定事项；原始消息块仍需独立检索，不能据此补全被省略的早期细节。'
schema: mindie-material-task/1
status: complete
task_id: 7d86ac39f75c7ae61a098a4fc3c644eb00991f1892ba2eb3801644a14a060f98
---

# VAWS MCP 兼容性修复与验证索引

当前材料集中于 VAWS 的 remote-dev MCP Schema／工具命名兼容性、四至五个客户端接入、PR #64 合入及验证边界。后续检索应区分已报告的客户端实测、Cursor CLI 未完成登录、既有 skill-shim 失败，以及尚未接入 VAWS 统一初始化流程等未完成或不确定事项；原始消息块仍需独立检索，不能据此补全被省略的早期细节。

## Materials

- [VAWS remote-dev MCP 兼容修复与 PR #64](blocks/04bdb484741ae862a4e753d322184749b0af1c0af876fe9188d231bba3821a84.md): 记录显示项目 remote-dev 的 JSON Schema 组合写法导致 Kimi API 返回 400，Grok 还无法注册带点号的工具名；随后改用普通对象结构并将对外工具名改为下划线形式，保留服务端校验和旧名称兼容。声称已为 Kimi、Claude＋DSV4、Codex、Grok、Cursor IDE 配置并完成远程补丁写入、编辑、读回及 SHA256 核对，但 Cursor CLI 仍需重新登录；另有 8 个既有 skill-shim 失败，且相关配置自动接入 VAWS 初始化流程尚未实现。来源最后声称 VAWS PR #64 已合入，提交为 `9895b04`，63 项回归和两项 CI 通过；这些均属于该记录中的报告结果，并非独立核验。

---
blocks:
- block_id: b4598c4e3b8beaec9680537ed7c0b3c00b190b68d68ff175ad88309a6975924c
  indexed: true
  sha256: bcdf09a7ecdd732f150537f6d82a4267573b798a43c654e77bca203c7f31e2a2
  source_range:
    end: 867936
    part: 0
    start: 0
  summary: 只读检查记录称线上服务运行于 `lala-{153..156}-k3`，健康检查为 200；`*-k3-main` 可作隔离编译环境，但需使用唯一源码/runtime 目录并统一 vLLM core。四节点编译与 editable 安装产物哈希一致，但首次门禁因错误使用顶层 `import vllm_ascend_C` 失败，正确路径应为 `vllm_ascend.vllm_ascend_C`，因此不能将该次结果表述为完整验证成功。后续新 run 被报告为四 rank、EngineCore 和 Worker 均存活，权重及 ACLGraph capture 完成、`18077/health=200` 且无业务请求；该结果来自报告，仍应保留其运行状态和未覆盖业务请求的限制。
  title: 四节点编译验证与新 run 启动
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f28e001cdc609ce28d2f4d8fc27b75c32b7d75b5de0f3625d94af1f382d8633e
  kind: experience
  material_digest: 8d9bc9be65667b7142e3d0e4b4ae2d53ed217089e56f2bb4f521d4c4b71d04e9
  revision: f55a31d0ccf5bc019060975b071142c2876d14af68c72ba5100ed1fadbf76c90
  schema: mindie-entry/3
  summary: 当前材料涵盖四节点环境风险、隔离构建约束、统一产物哈希，以及因错误 import 路径导致的门禁失败和后续修正。新 run 的启动状态与健康检查被报告正常，但尚未验证业务请求；线上目录不可覆盖、155 的保留 bash 会话和各节点资源/并发风险仍是后续操作的重要上下文。原始块独立可检索，不据此补充未提供的更早细节。
  title: 任务导航：四节点隔离编译、门禁纠正与服务启动
navigation: 当前材料涵盖四节点环境风险、隔离构建约束、统一产物哈希，以及因错误 import 路径导致的门禁失败和后续修正。新 run 的启动状态与健康检查被报告正常，但尚未验证业务请求；线上目录不可覆盖、155 的保留 bash 会话和各节点资源/并发风险仍是后续操作的重要上下文。原始块独立可检索，不据此补充未提供的更早细节。
schema: mindie-material-task/1
status: complete
task_id: f28e001cdc609ce28d2f4d8fc27b75c32b7d75b5de0f3625d94af1f382d8633e
---

# 任务导航：四节点隔离编译、门禁纠正与服务启动

当前材料涵盖四节点环境风险、隔离构建约束、统一产物哈希，以及因错误 import 路径导致的门禁失败和后续修正。新 run 的启动状态与健康检查被报告正常，但尚未验证业务请求；线上目录不可覆盖、155 的保留 bash 会话和各节点资源/并发风险仍是后续操作的重要上下文。原始块独立可检索，不据此补充未提供的更早细节。

## Materials

- [四节点编译验证与新 run 启动](blocks/b4598c4e3b8beaec9680537ed7c0b3c00b190b68d68ff175ad88309a6975924c.md): 只读检查记录称线上服务运行于 `lala-{153..156}-k3`，健康检查为 200；`*-k3-main` 可作隔离编译环境，但需使用唯一源码/runtime 目录并统一 vLLM core。四节点编译与 editable 安装产物哈希一致，但首次门禁因错误使用顶层 `import vllm_ascend_C` 失败，正确路径应为 `vllm_ascend.vllm_ascend_C`，因此不能将该次结果表述为完整验证成功。后续新 run 被报告为四 rank、EngineCore 和 Worker 均存活，权重及 ACLGraph capture 完成、`18077/health=200` 且无业务请求；该结果来自报告，仍应保留其运行状态和未覆盖业务请求的限制。

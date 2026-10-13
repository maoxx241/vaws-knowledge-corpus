---
blocks:
- block_id: 5f9764a535e54c2ddfc2d4dc7d1339933f42daebce988f64f9df5a57cb500215
  indexed: true
  sha256: 37e37f11690ebeabd96fede6264215274bb58c50c71e919b39edd9f1942b8835
  source_range:
    end: 15168614
    part: 0
    start: 0
  summary: 围绕 Codex 原生 Hook、app-server、进程监督、远端执行结果语义、诊断投递和更新安装路径完成多轮修复与回归；记录了普通远端错误、清理失败、旧 generation、数据库损坏、Windows CRLF、无效父路径及损坏诊断目录等边界。最终本地组合验证了安装缓存、14 个 MCP 工具和后台故障一次性 ACK，但远端发布、原生 Hook 派发/信任及模型会话仍未验证；后续又发现并修复诊断目录损坏被误判为“没有消息”的问题。
  title: Codex 适配器与安装验收边界
- block_id: fdf765fc43103081bfabe0fbd736eb8367651b76c07551f4d621774e86c81b88
  indexed: true
  sha256: e46c1d85475df8393c497359b789c4bd19d643e0683b39e7b85894ae4cef6f6d
  source_range:
    end: 15168614
    part: 1
    start: 0
  summary: 提交 `5b30405e0a82767be6c8981d1dec3740576497de` 同步两份诊断副本，使损坏目录和悬空软链明确报告投递不可用，同时保留原业务结果；7 项针对性回归和独立加载验证通过。最终组合 `123909e` 完成 379 项测试（12 项平台跳过）、原生安装、14 个 MCP 工具、后台故障投递及一次性 ACK 验证，但未 push、未新增模型实测，内容验证使用进程级只读镜像。审查复核指出两处措辞需修正：应称“隔离 venv 正常安装/更新”，且此前 `publication_fetch` 失败不能写成最终版本验证；真实 Hook、模型、SSH/NPU 仍未验证。
  title: 共享诊断修复与最终组合验收
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 14275372c6b5f39570cb11b203763fac6653eec197f1dbb255cb763f1e4fcdd0
  kind: experience
  material_digest: 817b0e7c9c9578d60b179ae8b0a7effd2b28c8968dad48d8be22bbd54c4e1f01
  revision: 349e103472ec3719e651380ecb84651096295f979edffe36c342bde8c7650fe7
  schema: mindie-entry/3
  summary: 当前索引覆盖两条主线：一是进程生命周期、远端结果语义、安装更新、MCP 故障投递及损坏状态处理；二是共享诊断副本同步、最终依赖组合与安装态回归。已确认的本地测试和安装证据必须与未验证的远端发布、原生 Hook 派发/信任、模型会话及 SSH/NPU 边界区分；后续纠正包括诊断目录损坏漏报，以及审查文档中“全新隔离 venv”和远端拉取证据的历史措辞。
  title: 当前任务导航：Codex 适配器与共享诊断验收
navigation: 当前索引覆盖两条主线：一是进程生命周期、远端结果语义、安装更新、MCP 故障投递及损坏状态处理；二是共享诊断副本同步、最终依赖组合与安装态回归。已确认的本地测试和安装证据必须与未验证的远端发布、原生 Hook 派发/信任、模型会话及 SSH/NPU 边界区分；后续纠正包括诊断目录损坏漏报，以及审查文档中“全新隔离 venv”和远端拉取证据的历史措辞。
schema: mindie-material-task/1
status: complete
task_id: 14275372c6b5f39570cb11b203763fac6653eec197f1dbb255cb763f1e4fcdd0
---

# 当前任务导航：Codex 适配器与共享诊断验收

当前索引覆盖两条主线：一是进程生命周期、远端结果语义、安装更新、MCP 故障投递及损坏状态处理；二是共享诊断副本同步、最终依赖组合与安装态回归。已确认的本地测试和安装证据必须与未验证的远端发布、原生 Hook 派发/信任、模型会话及 SSH/NPU 边界区分；后续纠正包括诊断目录损坏漏报，以及审查文档中“全新隔离 venv”和远端拉取证据的历史措辞。

## Materials

- [Codex 适配器与安装验收边界](blocks/5f9764a535e54c2ddfc2d4dc7d1339933f42daebce988f64f9df5a57cb500215.md): 围绕 Codex 原生 Hook、app-server、进程监督、远端执行结果语义、诊断投递和更新安装路径完成多轮修复与回归；记录了普通远端错误、清理失败、旧 generation、数据库损坏、Windows CRLF、无效父路径及损坏诊断目录等边界。最终本地组合验证了安装缓存、14 个 MCP 工具和后台故障一次性 ACK，但远端发布、原生 Hook 派发/信任及模型会话仍未验证；后续又发现并修复诊断目录损坏被误判为“没有消息”的问题。
- [共享诊断修复与最终组合验收](blocks/fdf765fc43103081bfabe0fbd736eb8367651b76c07551f4d621774e86c81b88.md): 提交 `5b30405e0a82767be6c8981d1dec3740576497de` 同步两份诊断副本，使损坏目录和悬空软链明确报告投递不可用，同时保留原业务结果；7 项针对性回归和独立加载验证通过。最终组合 `123909e` 完成 379 项测试（12 项平台跳过）、原生安装、14 个 MCP 工具、后台故障投递及一次性 ACK 验证，但未 push、未新增模型实测，内容验证使用进程级只读镜像。审查复核指出两处措辞需修正：应称“隔离 venv 正常安装/更新”，且此前 `publication_fetch` 失败不能写成最终版本验证；真实 Hook、模型、SSH/NPU 仍未验证。

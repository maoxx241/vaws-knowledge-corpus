---
blocks:
- block_id: 4e7100696ca012f5ce79bf5f00d23371c8ae19c8973bd61443432ea32cc8ab80
  indexed: true
  sha256: f53ec79ea1e982433ff4ce5f2c78c0c3d5f97aa27af584befa0788ac8a2fab33
  source_range:
    end: 11357985
    part: 0
    start: 0
  summary: 围绕 SQLite 初始化、权威状态、marker/receipt、远端写入不确定性及无效祖先路径完成多轮审查与修复，保留缺库、损坏、unknown、部分成功和未验证边界；相关提交包括 NPU、Knowledge Admission 与 diagnostics 的多次补丁。记录了测试与安装包验证结果，但明确未验证真实 NPU/SSH、Windows 原生、浏览器交互、部署和 CI 等范围；文档审查还发现适配器覆盖表述及失效证据链接需修正。随后完成 diagnostics 固定版本整合和 coordinator 安装回归，仍仅属 macOS 隔离环境证据。
  title: 多组件权威状态、NPU-top 与文档审查
- block_id: 60ad0c39d9abd3bbc9cc53e466d1c788798929627245968893f7671fd7cc20d5
  indexed: true
  sha256: 440364a767b1bb73f1525106a2d55e35875aa5a0a37a74805ebde738fb781474
  source_range:
    end: 11357985
    part: 1
    start: 0
  summary: 三个 CI job 共同失败于 `test_review_followup.py:142`：WAL 模式下 `BEGIN EXCLUSIVE` 仍允许 admission 读取已提交快照，导致实际结果为 `accepted-local` 而非预期的 `unavailable`。测试 fixture 改用并确认 `PRAGMA journal_mode=DELETE` 后，`unavailable`、`admission-unreadable` 和禁止重放断言通过，提交 `ff4bc4aa...` 的针对性回归为 2 passed；跨平台 CI 尚未重新运行。之后 diagnostics 更新至 `210e96d`，NPU wheel 与 coordinator 五项关键回归通过，但未新增真实 SSH/NPU、Windows 原生或部署验收。
  title: SQLite WAL 锁测试修正与依赖整合
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 16f2daded69867259c3b0c9493c9af5168298cec5d2b6b7e59d285784fa354b4
  kind: experience
  material_digest: c4287bd9800078a240fa0668947bd602f689ca2ea2a13b33809ce93660247d50
  revision: 2b69be09c5b022366ea2a2a7c973990d91a1660bce24cdb75b0b59815ab7dd0c
  schema: mindie-entry/3
  summary: 已有材料覆盖三库/多组件权威状态、无效路径与持久回执、NPU/diagnostics/coordinator 的安装验证、文档边界审查，以及一次由 WAL 锁假设导致的 CI fixture 修正。最新更正是将锁测试切换到 DELETE journal mode；生产源码未变，跨平台 CI 尚未重跑。后续检索应区分已通过的本地或隔离环境测试与仍未验证的真实 NPU/SSH、Windows 原生、浏览器、部署及发布范围；原始块仍独立可检索，未提供的早期细节不应推断。
  title: 当前任务导航：权威状态、故障可见性与安装验证索引
navigation: 已有材料覆盖三库/多组件权威状态、无效路径与持久回执、NPU/diagnostics/coordinator 的安装验证、文档边界审查，以及一次由 WAL 锁假设导致的 CI fixture 修正。最新更正是将锁测试切换到 DELETE journal mode；生产源码未变，跨平台 CI 尚未重跑。后续检索应区分已通过的本地或隔离环境测试与仍未验证的真实 NPU/SSH、Windows 原生、浏览器、部署及发布范围；原始块仍独立可检索，未提供的早期细节不应推断。
schema: mindie-material-task/1
status: complete
task_id: 16f2daded69867259c3b0c9493c9af5168298cec5d2b6b7e59d285784fa354b4
---

# 当前任务导航：权威状态、故障可见性与安装验证索引

已有材料覆盖三库/多组件权威状态、无效路径与持久回执、NPU/diagnostics/coordinator 的安装验证、文档边界审查，以及一次由 WAL 锁假设导致的 CI fixture 修正。最新更正是将锁测试切换到 DELETE journal mode；生产源码未变，跨平台 CI 尚未重跑。后续检索应区分已通过的本地或隔离环境测试与仍未验证的真实 NPU/SSH、Windows 原生、浏览器、部署及发布范围；原始块仍独立可检索，未提供的早期细节不应推断。

## Materials

- [多组件权威状态、NPU-top 与文档审查](blocks/4e7100696ca012f5ce79bf5f00d23371c8ae19c8973bd61443432ea32cc8ab80.md): 围绕 SQLite 初始化、权威状态、marker/receipt、远端写入不确定性及无效祖先路径完成多轮审查与修复，保留缺库、损坏、unknown、部分成功和未验证边界；相关提交包括 NPU、Knowledge Admission 与 diagnostics 的多次补丁。记录了测试与安装包验证结果，但明确未验证真实 NPU/SSH、Windows 原生、浏览器交互、部署和 CI 等范围；文档审查还发现适配器覆盖表述及失效证据链接需修正。随后完成 diagnostics 固定版本整合和 coordinator 安装回归，仍仅属 macOS 隔离环境证据。
- [SQLite WAL 锁测试修正与依赖整合](blocks/60ad0c39d9abd3bbc9cc53e466d1c788798929627245968893f7671fd7cc20d5.md): 三个 CI job 共同失败于 `test_review_followup.py:142`：WAL 模式下 `BEGIN EXCLUSIVE` 仍允许 admission 读取已提交快照，导致实际结果为 `accepted-local` 而非预期的 `unavailable`。测试 fixture 改用并确认 `PRAGMA journal_mode=DELETE` 后，`unavailable`、`admission-unreadable` 和禁止重放断言通过，提交 `ff4bc4aa...` 的针对性回归为 2 passed；跨平台 CI 尚未重新运行。之后 diagnostics 更新至 `210e96d`，NPU wheel 与 coordinator 五项关键回归通过，但未新增真实 SSH/NPU、Windows 原生或部署验收。

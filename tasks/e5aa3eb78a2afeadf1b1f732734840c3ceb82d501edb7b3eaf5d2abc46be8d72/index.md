---
blocks:
- block_id: 1952a5f9f6045592e799c6a903bc569c066f00c52adec58b516a132c2dcad550
  indexed: true
  sha256: 66b4ba451b5ad1e2db9200f888a07e383f5075e8eafdaac82a0e0ed8387c1d6c
  source_range:
    end: 11438260
    part: 0
    start: 0
  summary: 审查指出知识库从首尾摘要转向完整 transcript、任务分块和 BM25 后，主要风险集中在跨仓发布契约、长任务按块阅读、完整对话公开说明、规模成本、引用寿命和真实复用验收。材料还记录了旧 updater 检查已删除的 `MaintenanceBudget`/`loop.limits` 而拒绝候选升级，以及检索转述形成“引用回声”、标题摘要落后于正文等观察；这些是源码或公开材料审查结果，未等同于完整线上故障。后续用户决定用用户须知处理公开范围，并要求 P1 系统性设计、直接变更且不做旧格式兼容。
  title: 宏观审查：跨仓发布、检索消费与验收风险
- block_id: 41d06a05b3ae35d10c335ebe24b68a1cd29367cfeba96afd2e7ba11cc7887075
  indexed: true
  sha256: e84a03253952a258f84c8c00f50cb6dbe3401964e7e299fde424e0565b30b07e
  source_range:
    end: 11438260
    part: 1
    start: 0
  summary: 设计将产品组合固定为精确 adapter、knowledge、remote-dev 和内容提交/合同 hash，由候选自身 validator 在候选 venv 中检查 runtime API、组件 SHA、catalog、Hook、summary policy 与 publication schema；运行中 updater 只负责稳定外壳、身份和回执，不再导入旧 `runtime_probe` 或依赖 `MaintenanceBudget` 等内部类。方案明确不桥接旧 updater/schema，组件、schema 或 validator 不匹配时返回 `contract_mismatch` 并保留有效状态；安装失败与回滚失败的错误链需分别持久化。材料中的实现与测试进展曾报告多批通过，但仍把跨仓精确版本锁定和最终组合验证列为后续事项。
  title: 统一契约方案：候选自验证与不可兼容切换
- block_id: a7675b921f5db17c3a6d71360f148a7ceb3a9e8781e3c0917619d1ec00895f05
  indexed: true
  sha256: 91b35517a5a7edc8fdcdb84b1ddef80be160f6a7f98e0a939fb1181c81a1775b
  source_range:
    end: 11438260
    part: 2
    start: 0
  summary: 内容仓方案新增 `publication-contract.json`，要求 Feed、Publisher 和 Content CI 读取同一精确 commit 的合同；旧布局直接拒绝，跨仓切换采用可见拒绝、不覆盖有效状态的非原子流程，并区分 `failed`、`unavailable`、不确定安装和未知远端写入。后续材料报告已实现引用归组、独立 block/feedback ref、按块读取、追加后旧块继续可读、远端内容与未发送块续写，以及对延迟回执、并发追加、损坏文件和回滚错误链的修正；但这些是助手报告的测试结果，未在本材料中独立验证，且早先曾明确存在安装回滚覆盖原始错误、延迟确认导致错误正文基线等失败。最终报告称定向验证通过并“冻结”，同时仍记录待 root 锁定四仓精确版本和运行最终组合验证。
  title: 内容发布、块级读取与并发续写的实现进展
- block_id: a55786312e4d62586adae13b69e0e12ae63a10593654f93be8709f8f03964382
  indexed: true
  sha256: f9f7d9bec49a3701dc9221a0f79e40183e7e48814d849592c01b6155018ebd8b
  source_range:
    end: 11438260
    part: 3
    start: 0
  summary: 材料记录了九原则审查、候选失败原因保留、`material_corrupt`、回滚错误链、冻结包、延迟发送基线及内容契约等修正；核心测试曾报告 704/706 项通过，适配器回归报告 317/329 项通过，但这些是来源中的测试声明。Windows 失败先被定位为 CRLF 与 LF 摘要字节不一致，后续又出现两层 Python 启动超过 300 毫秒的旧进程测试失败；相关代码未改，仅重跑该检查，最终状态在本块末尾仍未明确。PR、Draft 状态、跨平台检查和原生宿主/Bot 验收之间保持区分，不能把已通过的局部检查推广为完整发布成功。
  title: 跨平台契约校验与 Windows 夹具修正
- block_id: 8ee6a3f8a88c14640e33dd4422d0c203daea7cf23ba92bf6d66ea76124abd235
  indexed: true
  sha256: bd8d3757469f81d3349ff02509825c10ffd2186be3841295297364ed8115c531
  source_range:
    end: 11438260
    part: 4
    start: 0
  summary: 材料将超时模型改为：明确错误 fast-fail，正常长操作持续等待，watchdog 依据退出、断管、心跳或连接失联判断故障；默认不设执行总时限，只有用户明确要求时才传入期限，诊断仅供 Agent 内部使用。设计还分离业务完成与清理失败、未知结果不自动重放，并计划处理服务启动 5 秒限制、语料同步多层总时限、增量索引、空闲查询和旧环境回收。复用收益、相同经验去重、资源增长和发布前规模验收仍处于评估/并行实施阶段，材料未提供最终代码、测试或部署结果。
  title: 无感运行、watchdog 与资源规模化设计
- block_id: 724a14a4aef110fcd661b2ef13c255a92263a69729db40b73730ca15ba7f5c93
  indexed: true
  sha256: 8c625bb1daf2698c7bf4bb1f492aa1cda8efdc5f8e406fa83c054758369d2f18
  source_range:
    end: 21044763
    part: 0
    start: 11438260
  summary: '材料记录了增量索引、资源回收、长任务取消和远端回归的多轮验证；曾发现空文件回执丢失、子进程关闭输出、损坏目录、未完成指针、活跃旧环境回收以及清理异常覆盖业务结果等问题，并分别报告了修复或仍待验证状态。远端 PR #26 的五项 CI 最终通过，核心提交 `be0f225...` 的定向测试通过，但真实 SSH、原生 Windows、NPU、宿主 Hook 和完整 CI 等边界曾明确未验收；消费者任务虽完成可见要求，却没有主动检索，实际复用收益仍未证明。后续材料还记录了 Windows 只读 Git 对象清理、测试夹具 SQLite 连接未关闭、核心测试提交 `2c1d813` 及其跨平台结果，需区分生产代码、测试修复和各自提交版本。'
  title: 组件回归、资源索引与未完成边界
- block_id: c1f55521a5e90556047e99fde21ad666c92181ac19657f656780a6284eeb3148
  indexed: true
  sha256: fa11644b6f9e701ae30a265a868fb1e4ac759875989538194ba1dbe8e26b8b3e
  source_range:
    end: 21044763
    part: 1
    start: 11438260
  summary: 适配器提交 `9ed188a...`、后续 `9d0d7da` 的不可变安装、运行时兼容性和 Linux/Windows CI 被逐步核对；Windows 测试暴露了 `taskkill.exe` 计数误判、Stop Hook/测试服务退出、只读 Git 对象删除失败及 PowerShell 超时根因未明等问题，部分已通过修复或重跑，部分仍保留不确定性。GitHub workflow 修改因缺少 `workflow` 权限被拒绝，适配器 Windows 最终报告 359 tests 通过、跳过项存在；核心侧又发现 13 个 `MaterialStore` 延迟未关闭及 3 个 SQLite 连接未关闭，并通过仅修改测试夹具的 `2c1d813` 重跑。复用评估记录了 14 次模型运行和缓存 token，以及约 41% 响应字节减少，但没有把这些指标提升为任务质量或费用结论。
  title: 跨平台 CI、适配器提交与 Windows 夹具问题
- block_id: 3e553e601d5be600fd5c5f07450a45a0fe701ebe9ee49f643e6ad80ef9ee3622
  indexed: true
  sha256: 78ce586ab0a1210a9b2c7220bf41887d1e1ba087a25273dd76d4a22fa2a0f83d
  source_range:
    end: 21044763
    part: 2
    start: 11438260
  summary: 核心 Windows 最终报告为 743 passed、29 skipped，Ubuntu、macOS 和打包检查各为 763 passed、9 skipped；五个 Draft PR 已更新但未合并或发布。材料仍明确保留真实宿主接入、SSH/NPU、Bot 新契约和正式发布就绪状态未验收，以及普通消费者未主动检索导致实际复用收益未证明。随后任务范围扩展为按设计原则系统审查并可能重写 Codex、知识采集/整理/检索/发布、远端执行、共享诊断及上下游契约，审查已启动但尚未给出问题清单或重写结果。
  title: 组件验证收敛与系统性重构审查启动
- block_id: 95689e5bdefe309ed0c8a4239492054f6ee1c22331ef25ad6b9f68ef64ccbaeb
  indexed: true
  sha256: fc086f93648dae4faa82abd75a6fcb81a580a22786873f309f3b9eca2ce8856e
  source_range:
    end: 35605461
    part: 0
    start: 21044763
  summary: 复查集中发现状态库缺失或损坏被误判为空/未启用、远端业务结果被清理或记账错误覆盖、撤权后仍可能提交、旧服务更新竞态、默认超时和后台进程逃逸等问题；部分路径随后已修复并以真实本地进程、定向测试和 CI 验证。记录同时保留了 Windows/diagnostics 失败、workflow OAuth 缺少 `workflow` 权限、真实 SSH/NPU/远端密码安装等尚未验收边界。最终报告声称若干组件测试和 wheel/HTTP 检查通过，但明确未将真实 NPU、远端安装、浏览器、Windows/Linux 原生及 CI 未运行部分视为完成。
  title: 跨组件状态边界、进程所有权与验收进展
- block_id: b50b2dc9ed4944c858dc946253ae34cdfb6b8b18811235f71923ca125b3276d7
  indexed: true
  sha256: c769ab899a3c9bb9d2e93eb97d32a66bf1dd5d99b000302a49d0c5e290fd6f56
  source_range:
    end: 35605461
    part: 1
    start: 21044763
  summary: 本批记录了 Windows SQLite 句柄与 CRLF/LF 标记差异、路径祖先被误判为缺失、SQLite WAL 下 `BEGIN EXCLUSIVE` 未制造不可读状态，以及准备成功后 `stage_timings` 未持久化等失败。相关修复包括资源释放、统一 marker 文本契约、显式 `NotADirectoryError`、DELETE journal mode 测试夹具，以及在 `emit()` 中先保存 timing 再回调；部分定向测试通过，但新提交的跨平台 CI、Windows 修复和若干最终组合仍曾待验证。还保留了诊断/核心副本同步、workflow 权限阻塞、本地只读镜像和未发布内容等限制。
  title: 跨平台标记、持久化时序与锁测试修正
- block_id: c62451900cd0ce2cd434fafe43b4b8ca27e089d98b2d226a04160083f60ad2b8
  indexed: true
  sha256: d0a9c06bdbe0fe0c6863f87f23481531a8c19c8f0f9d69da0dfbc0def0b27362
  source_range:
    end: 35605461
    part: 2
    start: 21044763
  summary: Codex 原生插件的隔离安装、缓存字节核对、14 个 MCP 工具发现及后台故障单次 ACK 投递被报告为通过；另有 376/379 项测试（含平台跳过）和若干组件 CI 通过记录。诊断、Core 与 NPU 路径修复了普通文件祖先、不可访问目录、悬空软链和无效目录被当作首次使用、未配置或空状态的问题，同时保留原业务结果和原始 I/O 错误；真正缺失仍不创建状态。最终组合 `123909e`、依赖 pin 和报告已更新，但内容/插件发布、原生 Hook 派发与信任、真实 NPU、模型会话、远端发布及部分外部验收仍未验证。
  title: 最终组合、诊断坏路径与 Codex 安装验证
- block_id: 3d933a05afbf7fcb0633b90f7337311fc42b92ee58df5d258dc1eea6fc7ec41a
  indexed: true
  sha256: 4a6fbba805b6c4cb7a2d29d851c184173fc105ea08af33a7feb396f6211050a9
  source_range:
    end: 39230164
    part: 0
    start: 35605461
  summary: 记录最终候选及三个 workflow 已发布、9 个 PR 仍为 draft，以及公开 GitHub 获取、安装、隔离原生包、MCP 故障投递和 remote-dev 真实 OpenSSH 测试通过。Core 的 Linux/macOS 各为 868 passed、9 skipped，Windows 为 847 passed、30 项 POSIX 跳过且无失败；Codex Linux/Windows 分别跳过 12/19 项，其余通过。此前 Windows 因 30 分钟预算取消、仅有 617 passed 的结果被保留为历史未完成状态，后续 60 分钟运行才完成；仍未覆盖宿主 Hook/首轮与 fork 派发、真实 NPU、模型质量与费用、Bot/部署等线上验收。
  title: 最终候选发布与跨平台 CI 收口
- block_id: 418186d72fd357289b6366102df383ec76e60fa0c1308eebc690848af5bd2ae2
  indexed: true
  sha256: b69ca7ab5ee5b444b07ab31282caae54da59ec0e16c4001d3e1ec193be7eb8c9
  source_range:
    end: 39230164
    part: 1
    start: 35605461
  summary: 该块明确指出 PR 仍为 draft，尚未合并或升级正式安装，不能将当前结果称为完整线上验收通过。宿主 Hook、首轮与 fork 派发、真实 NPU、模型质量与费用以及新 Bot 策略采纳仍未验收；这些未完成项应与局部 CI、安装验证和公开内容校验结果分开检索。
  title: 线上验收边界仍未闭合
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e5aa3eb78a2afeadf1b1f732734840c3ceb82d501edb7b3eaf5d2abc46be8d72
  kind: experience
  material_digest: 3cf9e862f8c76cc6dd4e1de0dc724f0754f2b326ec9ef10a07d191c5d13aeaaa
  revision: d932705c9df681437c6ae7e6c27d976d6962533eeb5f351243e9f508c5e646c8
  schema: mindie-entry/3
  summary: 保留既有 publication contract、精确 SHA/hash、候选自验证、块级引用、失败/未知状态、冻结包、并发恢复、watchdog、fast-fail、增量索引、资源回收，以及 Windows SQLite/CRLF、WAL/DELETE 锁夹具、stage_timings 顺序、进程所有权、坏路径 NotADirectoryError、悬空软链、诊断 fallback、MCP 故障投递和 workflow 权限阻塞等入口。新增记录显示三个 workflow 与最终候选已发布，Core、Codex、remote-dev 的当前 CI 和部分安装/OpenSSH/MCP 验证完成；但应保留此前 Windows 30 分钟取消及后来 60 分钟完成的修正时间线，不把局部通过升级为完整线上成功。PR 仍为 draft，宿主 Hook/派发、真实 NPU、模型质量与费用、Bot 策略、部署和正式安装升级仍未验收；继续区分测试、wheel/HTTP smoke、原生安装、公开 GitHub 获取与真实宿主/SSH/NPU/Bot/部署边界，并检索 contract_mismatch、material_corrupt、MaintenanceBudget、publication-contract.json、candidate_validate.py、superseded/unavailable、回滚错误链、延迟回执及未发布改动。
  title: 当前任务导航：Mindie Codex 最终候选已完成 CI，但线上验收仍有边界
navigation: 保留既有 publication contract、精确 SHA/hash、候选自验证、块级引用、失败/未知状态、冻结包、并发恢复、watchdog、fast-fail、增量索引、资源回收，以及 Windows SQLite/CRLF、WAL/DELETE 锁夹具、stage_timings 顺序、进程所有权、坏路径 NotADirectoryError、悬空软链、诊断 fallback、MCP 故障投递和 workflow 权限阻塞等入口。新增记录显示三个 workflow 与最终候选已发布，Core、Codex、remote-dev 的当前 CI 和部分安装/OpenSSH/MCP 验证完成；但应保留此前 Windows 30 分钟取消及后来 60 分钟完成的修正时间线，不把局部通过升级为完整线上成功。PR 仍为 draft，宿主 Hook/派发、真实 NPU、模型质量与费用、Bot 策略、部署和正式安装升级仍未验收；继续区分测试、wheel/HTTP smoke、原生安装、公开 GitHub 获取与真实宿主/SSH/NPU/Bot/部署边界，并检索 contract_mismatch、material_corrupt、MaintenanceBudget、publication-contract.json、candidate_validate.py、superseded/unavailable、回滚错误链、延迟回执及未发布改动。
schema: mindie-material-task/1
status: complete
task_id: e5aa3eb78a2afeadf1b1f732734840c3ceb82d501edb7b3eaf5d2abc46be8d72
---

# 当前任务导航：Mindie Codex 最终候选已完成 CI，但线上验收仍有边界

保留既有 publication contract、精确 SHA/hash、候选自验证、块级引用、失败/未知状态、冻结包、并发恢复、watchdog、fast-fail、增量索引、资源回收，以及 Windows SQLite/CRLF、WAL/DELETE 锁夹具、stage_timings 顺序、进程所有权、坏路径 NotADirectoryError、悬空软链、诊断 fallback、MCP 故障投递和 workflow 权限阻塞等入口。新增记录显示三个 workflow 与最终候选已发布，Core、Codex、remote-dev 的当前 CI 和部分安装/OpenSSH/MCP 验证完成；但应保留此前 Windows 30 分钟取消及后来 60 分钟完成的修正时间线，不把局部通过升级为完整线上成功。PR 仍为 draft，宿主 Hook/派发、真实 NPU、模型质量与费用、Bot 策略、部署和正式安装升级仍未验收；继续区分测试、wheel/HTTP smoke、原生安装、公开 GitHub 获取与真实宿主/SSH/NPU/Bot/部署边界，并检索 contract_mismatch、material_corrupt、MaintenanceBudget、publication-contract.json、candidate_validate.py、superseded/unavailable、回滚错误链、延迟回执及未发布改动。

## Materials

- [宏观审查：跨仓发布、检索消费与验收风险](blocks/1952a5f9f6045592e799c6a903bc569c066f00c52adec58b516a132c2dcad550.md): 审查指出知识库从首尾摘要转向完整 transcript、任务分块和 BM25 后，主要风险集中在跨仓发布契约、长任务按块阅读、完整对话公开说明、规模成本、引用寿命和真实复用验收。材料还记录了旧 updater 检查已删除的 `MaintenanceBudget`/`loop.limits` 而拒绝候选升级，以及检索转述形成“引用回声”、标题摘要落后于正文等观察；这些是源码或公开材料审查结果，未等同于完整线上故障。后续用户决定用用户须知处理公开范围，并要求 P1 系统性设计、直接变更且不做旧格式兼容。
- [统一契约方案：候选自验证与不可兼容切换](blocks/41d06a05b3ae35d10c335ebe24b68a1cd29367cfeba96afd2e7ba11cc7887075.md): 设计将产品组合固定为精确 adapter、knowledge、remote-dev 和内容提交/合同 hash，由候选自身 validator 在候选 venv 中检查 runtime API、组件 SHA、catalog、Hook、summary policy 与 publication schema；运行中 updater 只负责稳定外壳、身份和回执，不再导入旧 `runtime_probe` 或依赖 `MaintenanceBudget` 等内部类。方案明确不桥接旧 updater/schema，组件、schema 或 validator 不匹配时返回 `contract_mismatch` 并保留有效状态；安装失败与回滚失败的错误链需分别持久化。材料中的实现与测试进展曾报告多批通过，但仍把跨仓精确版本锁定和最终组合验证列为后续事项。
- [内容发布、块级读取与并发续写的实现进展](blocks/a7675b921f5db17c3a6d71360f148a7ceb3a9e8781e3c0917619d1ec00895f05.md): 内容仓方案新增 `publication-contract.json`，要求 Feed、Publisher 和 Content CI 读取同一精确 commit 的合同；旧布局直接拒绝，跨仓切换采用可见拒绝、不覆盖有效状态的非原子流程，并区分 `failed`、`unavailable`、不确定安装和未知远端写入。后续材料报告已实现引用归组、独立 block/feedback ref、按块读取、追加后旧块继续可读、远端内容与未发送块续写，以及对延迟回执、并发追加、损坏文件和回滚错误链的修正；但这些是助手报告的测试结果，未在本材料中独立验证，且早先曾明确存在安装回滚覆盖原始错误、延迟确认导致错误正文基线等失败。最终报告称定向验证通过并“冻结”，同时仍记录待 root 锁定四仓精确版本和运行最终组合验证。
- [跨平台契约校验与 Windows 夹具修正](blocks/a55786312e4d62586adae13b69e0e12ae63a10593654f93be8709f8f03964382.md): 材料记录了九原则审查、候选失败原因保留、`material_corrupt`、回滚错误链、冻结包、延迟发送基线及内容契约等修正；核心测试曾报告 704/706 项通过，适配器回归报告 317/329 项通过，但这些是来源中的测试声明。Windows 失败先被定位为 CRLF 与 LF 摘要字节不一致，后续又出现两层 Python 启动超过 300 毫秒的旧进程测试失败；相关代码未改，仅重跑该检查，最终状态在本块末尾仍未明确。PR、Draft 状态、跨平台检查和原生宿主/Bot 验收之间保持区分，不能把已通过的局部检查推广为完整发布成功。
- [无感运行、watchdog 与资源规模化设计](blocks/8ee6a3f8a88c14640e33dd4422d0c203daea7cf23ba92bf6d66ea76124abd235.md): 材料将超时模型改为：明确错误 fast-fail，正常长操作持续等待，watchdog 依据退出、断管、心跳或连接失联判断故障；默认不设执行总时限，只有用户明确要求时才传入期限，诊断仅供 Agent 内部使用。设计还分离业务完成与清理失败、未知结果不自动重放，并计划处理服务启动 5 秒限制、语料同步多层总时限、增量索引、空闲查询和旧环境回收。复用收益、相同经验去重、资源增长和发布前规模验收仍处于评估/并行实施阶段，材料未提供最终代码、测试或部署结果。
- [组件回归、资源索引与未完成边界](blocks/724a14a4aef110fcd661b2ef13c255a92263a69729db40b73730ca15ba7f5c93.md): 材料记录了增量索引、资源回收、长任务取消和远端回归的多轮验证；曾发现空文件回执丢失、子进程关闭输出、损坏目录、未完成指针、活跃旧环境回收以及清理异常覆盖业务结果等问题，并分别报告了修复或仍待验证状态。远端 PR #26 的五项 CI 最终通过，核心提交 `be0f225...` 的定向测试通过，但真实 SSH、原生 Windows、NPU、宿主 Hook 和完整 CI 等边界曾明确未验收；消费者任务虽完成可见要求，却没有主动检索，实际复用收益仍未证明。后续材料还记录了 Windows 只读 Git 对象清理、测试夹具 SQLite 连接未关闭、核心测试提交 `2c1d813` 及其跨平台结果，需区分生产代码、测试修复和各自提交版本。
- [跨平台 CI、适配器提交与 Windows 夹具问题](blocks/c1f55521a5e90556047e99fde21ad666c92181ac19657f656780a6284eeb3148.md): 适配器提交 `9ed188a...`、后续 `9d0d7da` 的不可变安装、运行时兼容性和 Linux/Windows CI 被逐步核对；Windows 测试暴露了 `taskkill.exe` 计数误判、Stop Hook/测试服务退出、只读 Git 对象删除失败及 PowerShell 超时根因未明等问题，部分已通过修复或重跑，部分仍保留不确定性。GitHub workflow 修改因缺少 `workflow` 权限被拒绝，适配器 Windows 最终报告 359 tests 通过、跳过项存在；核心侧又发现 13 个 `MaterialStore` 延迟未关闭及 3 个 SQLite 连接未关闭，并通过仅修改测试夹具的 `2c1d813` 重跑。复用评估记录了 14 次模型运行和缓存 token，以及约 41% 响应字节减少，但没有把这些指标提升为任务质量或费用结论。
- [组件验证收敛与系统性重构审查启动](blocks/3e553e601d5be600fd5c5f07450a45a0fe701ebe9ee49f643e6ad80ef9ee3622.md): 核心 Windows 最终报告为 743 passed、29 skipped，Ubuntu、macOS 和打包检查各为 763 passed、9 skipped；五个 Draft PR 已更新但未合并或发布。材料仍明确保留真实宿主接入、SSH/NPU、Bot 新契约和正式发布就绪状态未验收，以及普通消费者未主动检索导致实际复用收益未证明。随后任务范围扩展为按设计原则系统审查并可能重写 Codex、知识采集/整理/检索/发布、远端执行、共享诊断及上下游契约，审查已启动但尚未给出问题清单或重写结果。
- [跨组件状态边界、进程所有权与验收进展](blocks/95689e5bdefe309ed0c8a4239492054f6ee1c22331ef25ad6b9f68ef64ccbaeb.md): 复查集中发现状态库缺失或损坏被误判为空/未启用、远端业务结果被清理或记账错误覆盖、撤权后仍可能提交、旧服务更新竞态、默认超时和后台进程逃逸等问题；部分路径随后已修复并以真实本地进程、定向测试和 CI 验证。记录同时保留了 Windows/diagnostics 失败、workflow OAuth 缺少 `workflow` 权限、真实 SSH/NPU/远端密码安装等尚未验收边界。最终报告声称若干组件测试和 wheel/HTTP 检查通过，但明确未将真实 NPU、远端安装、浏览器、Windows/Linux 原生及 CI 未运行部分视为完成。
- [跨平台标记、持久化时序与锁测试修正](blocks/b50b2dc9ed4944c858dc946253ae34cdfb6b8b18811235f71923ca125b3276d7.md): 本批记录了 Windows SQLite 句柄与 CRLF/LF 标记差异、路径祖先被误判为缺失、SQLite WAL 下 `BEGIN EXCLUSIVE` 未制造不可读状态，以及准备成功后 `stage_timings` 未持久化等失败。相关修复包括资源释放、统一 marker 文本契约、显式 `NotADirectoryError`、DELETE journal mode 测试夹具，以及在 `emit()` 中先保存 timing 再回调；部分定向测试通过，但新提交的跨平台 CI、Windows 修复和若干最终组合仍曾待验证。还保留了诊断/核心副本同步、workflow 权限阻塞、本地只读镜像和未发布内容等限制。
- [最终组合、诊断坏路径与 Codex 安装验证](blocks/c62451900cd0ce2cd434fafe43b4b8ca27e089d98b2d226a04160083f60ad2b8.md): Codex 原生插件的隔离安装、缓存字节核对、14 个 MCP 工具发现及后台故障单次 ACK 投递被报告为通过；另有 376/379 项测试（含平台跳过）和若干组件 CI 通过记录。诊断、Core 与 NPU 路径修复了普通文件祖先、不可访问目录、悬空软链和无效目录被当作首次使用、未配置或空状态的问题，同时保留原业务结果和原始 I/O 错误；真正缺失仍不创建状态。最终组合 `123909e`、依赖 pin 和报告已更新，但内容/插件发布、原生 Hook 派发与信任、真实 NPU、模型会话、远端发布及部分外部验收仍未验证。
- [最终候选发布与跨平台 CI 收口](blocks/3d933a05afbf7fcb0633b90f7337311fc42b92ee58df5d258dc1eea6fc7ec41a.md): 记录最终候选及三个 workflow 已发布、9 个 PR 仍为 draft，以及公开 GitHub 获取、安装、隔离原生包、MCP 故障投递和 remote-dev 真实 OpenSSH 测试通过。Core 的 Linux/macOS 各为 868 passed、9 skipped，Windows 为 847 passed、30 项 POSIX 跳过且无失败；Codex Linux/Windows 分别跳过 12/19 项，其余通过。此前 Windows 因 30 分钟预算取消、仅有 617 passed 的结果被保留为历史未完成状态，后续 60 分钟运行才完成；仍未覆盖宿主 Hook/首轮与 fork 派发、真实 NPU、模型质量与费用、Bot/部署等线上验收。
- [线上验收边界仍未闭合](blocks/418186d72fd357289b6366102df383ec76e60fa0c1308eebc690848af5bd2ae2.md): 该块明确指出 PR 仍为 draft，尚未合并或升级正式安装，不能将当前结果称为完整线上验收通过。宿主 Hook、首轮与 fork 派发、真实 NPU、模型质量与费用以及新 Bot 策略采纳仍未验收；这些未完成项应与局部 CI、安装验证和公开内容校验结果分开检索。

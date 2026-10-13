---
blocks:
- block_id: 8f9a663be8f8ea8a810065be9609d75d52efdaa593d395dcbd22db241e744b42
  indexed: true
  sha256: dddbf99eaaa23f0b03f63945b7310054de7442b679b011cb7484fffbae81a2e9
  source_range:
    end: 14585702
    part: 0
    start: 0
  summary: 讨论了首次使用时确认 GitHub 身份、创建并切换个人 Fork，以及基于正式 Release、轮询、fast-forward 和准备环境的自动更新方案；记录了 `github_identity_required`、无 Release、脏工作区、分叉和 Windows/WSL 未实机验证等未完成或失败边界。后续将服务器身份、Agent、任务、execution/容器租约串联，并参考 Agent Mail、Beads、Gas Town 的线程、回执和原子认领，但明确 coordinator 才负责资源分配。实现曾完成多轮入口、updater、Fork 重定向、子模块和 URL 校验修复，相关测试通过，但服务器认证和留言板仍主要停留在设计或未部署状态。
  title: 个人 Fork、Release 更新与身份协作设计
- block_id: 81ab101e3815e60342085c6a3eca2154e453b82ee5a1fe2c53bdce125b05ffcb
  indexed: true
  sha256: 14055b9ae5d3256fe25992e3225370bc579dc9fdecdf4bd5c4f0fde1cf8220ab
  source_range:
    end: 14585702
    part: 1
    start: 0
  summary: 实现方向收敛为共享 root 登录下的一次 GitHub 用户绑定，工具自动传递用户归属、生成 `vaws-<用户名>` 容器名、收取留言并协调资源，Agent 不填写身份字段或维护记录。新增或修复了 `user_identity.py`、TaskClient/native hook 接线、个人 Fork 实际返回地址（含 `-1` 后缀）、共享算子编译缓存和 `/weights`、`/models` 条件复用；缓存取消或不确定时不清理重建，命中后按当前源码验证。消息支持持久留言、线程、去重和非阻塞收件，daemon 可在空闲时升级；多轮测试通过，但真实服务器留言、真实 NPU 缓存和 Windows/WSL 等边界仍未完全验收。
  title: 自动身份接入、共享复用与留言机制落地
- block_id: 8b89bb4a17e2cddf3e0f9150f742bbc311d5824a88e60364437d78a75b33a353
  indexed: true
  sha256: 2e3bbb383a6f6ed260a6cbb14c84006e1873369dfc5a990497015ed02e5c08bc
  source_range:
    end: 14585702
    part: 2
    start: 0
  summary: 复用范围被进一步限定为共享开发机上的算子编译产物、权重和已有环境，不区分个人/公开，不增加发布、权限或复杂鉴权；工具仅检查文件、版本、格式、分片及 ABI 等必要兼容条件。设计指出运行调用链仍需由 coordinator 共同入口承接身份，更新副本也必须保留容器命名映射，验收关注同为 root 的多用户不串容器、消息不重复和排队请求续接。随后第一版身份绑定、消息、缓存和空闲升级实现完成，整体测试报告为 Coordinator 377 项、工作区相关 138 项通过；但当前没有官方 Release，真实服务器留言与 NPU 编译缓存验收尚未完成。
  title: 最小化共享开发机复用与运行身份链
- block_id: d5ab553e6a7d58b8bc3d22601942829535a2d874bb31bf959d2cf9d5b7f1f2c9
  indexed: true
  sha256: d4fd4fb8d340f784811791e60d7e908ec08acf6647837ad7b7732a8d4595cc60
  source_range:
    end: 14585702
    part: 3
    start: 0
  summary: 更新器从 Release 改为默认每 5 分钟跟随官方主仓，实际查询到 `main @ b63c78269549e0f16ae6c4554f0b47b5e774a3da`，并完成个人 Fork 同步、四机 CPU/留言/排队清理及共享权重复用。153–156 的 `_shared_native` 缓存恢复 1103 个文件、C 扩展加载和哈希校验通过，但因 64 张 NPU 持续占用，未执行 NPU 算子数值或推理验证；154 曾有通信回执异常，保留了失败与恢复证据。随后确认常驻 watcher 已停止，方案转向新会话边界更新；已有目录恢复应读取保存的 environment receipt，不能按修改后的 lock 重新选环境。
  title: 主仓跟随与四机缓存验收
- block_id: f4e999313d35ba0ded4f9d537bb976bfc527cfb8d45fddd26d43a5a6d5643383
  indexed: true
  sha256: eb37cfbeee9e40df2f41ac2a9f433bf305aa04cf75f6d1bc3407cf9b3c3bfc9e
  source_range:
    end: 14585702
    part: 4
    start: 0
  summary: 已移除常驻 watcher，新增一次性的 `prepare`，新目录创建前检查和准备版本，已有目录或 resume 保留原代码、依赖与配置；相关测试曾报告 35、37 及 115 项通过，但部分结论属于本地或契约验证。审计确认 VAWS session 原本可由各客户端 SessionStart 自动关联，真正缺口是原生 worktree 创建前的版本和环境准备；SessionStart 发生在 cwd 确定后，不能切换父客户端目录。Codex/Cursor 有可用的原生 setup 接口，Claude 有 WorktreeCreate，而 Cursor setup 的失败继续创建、异步时序和旧版本兼容仍需按实际安装验证；Grok、Kimi 缺少可配置创建前扩展，不能宣称已覆盖。
  title: 会话边界更新与客户端原生接线审计
- block_id: 8d185c30cb2dca9c61fbfe17ea4eefa2ccd96b0ec35c50618462e4e9aeca7d08
  indexed: true
  sha256: cb754ddf9328f1cbad90955dc350de183a893fcf2aa564342550c22327050573
  source_range:
    end: 14585702
    part: 5
    start: 0
  summary: Grok 的 `new_session_worktree_mode`/`fork_worktree_mode` 只覆盖 `/new`、`/fork`，SessionStart stdout 不能改目录，PreToolUse 可通过 `updatedInput` 注入上下文；不应默认改写其他项目配置或假设存在主仓基线 setup hook。Kimi 0.42.0 的 SessionExternalHooksService 会等待 hook，但 cwd 已确定且返回值不能修改会话；fork 不触发 SessionStart，子 Agent 缺稳定 ID，MCP 也没有逐会话 context 注入能力。已落地的实现改动包括 Cursor `preToolUse` 自动补 VAWS context、hook 自动幂等关联以及 Codex/Cursor 原生 setup 接线；报告 48 项 hook 测试和 43 项 setup 测试通过，但未提交、未推送，也未做网络或硬件验收。
  title: Grok/Kimi 边界与原生 setup 实现
- block_id: d8de432376ac648c0d8be0c714b13c0b3e7aa78d26a034ef34d1e15e2024c985
  indexed: true
  sha256: 2f9aff0f3db211795eadf895922a56d5bcfd8e816323f541c58fbc7fafd694a6
  source_range:
    end: 22213467
    part: 0
    start: 14585702
  summary: VAWS 回调顺序、环境选择、子目录 CLI 任务登记、性能实验 sources 污染、无关 hook 触发及 Kimi 启动器等问题已被修复并通过多组测试；Codex 首次 CLI fallback 也会绑定实际 cwd 的 sources。20 个 skill、54 个 CLI 帮助入口完成审计，但 Claude/Grok/Kimi 的原生 worktree 更新、部分 shell context 和真实 GUI 验收当时仍有缺口。后续判断认为 Claude 可继续适配，Grok 需确认接口，Kimi 更接近客户端能力限制，不能把未完成统称为技术做不到。
  title: 多客户端原生接线与首次 CLI 回退修复
- block_id: de5b40637146a0671d9894c1e1702e948ec5d629215024936283d26e7ff9ad0d
  indexed: true
  sha256: 3c155e8c4d1feceafa65f41c4218aeb9bc08ec590d8fa3d23214a3c3b3954828
  source_range:
    end: 22213467
    part: 1
    start: 14585702
  summary: Claude 的真实新建和恢复会话通过；Grok 的 worktree、无参 VAWS 调用、恢复及配置保护通过，并使用了个人 fork 修复版。Kimi 已针对 `-c`、setup 失败静默回源、child 关联、用户级 MCP、TUI resume 和 fork 状态复制完成多轮修复，测试结果包括 9、43、114 项等，但真实 MCP/fork 验收在该批次末仍由 agent 继续执行。Cursor 暴露了按 worktree 生成 MCP 标识、默认 Disabled 和配置加载顺序问题，后续全局 MCP planner 与 GUI 新会话/隔离测试通过；Codex 环境选择仍受应用接口和 Computer Use 限制。
  title: Claude、Grok、Kimi 与 Cursor 的实测接线
- block_id: 1a9828e462fc7d721400d46260d905d252f42a98b375764776a4fcacde0b1b05
  indexed: true
  sha256: 3260b1ab328cb7ebb420c5399c9e4256a93f5d536df4925677488e7aa3bb7c64
  source_range:
    end: 22213467
    part: 2
    start: 14585702
  summary: Cursor 第二个新会话及切回旧会话的 GUI 验收报告为通过，目录、VAWS 任务、文件、代码版本和环境保持隔离或原样；期间 Mac 自动锁屏，使用了最长一小时的 `caffeinate -di -t 3600` 临时保持唤醒。Kimi Fork 已推送个人修复版并报告 SDK、文件历史、生命周期和类型检查通过，支持 dirty 内容、子模块、独立身份及恢复，但随后发现文件复制早于源 Agent 暂停可能造成历史不一致。该顺序已移入 SessionSetup 的暂停保护范围并通过针对性测试，最后一轮真实 `/fork` 验收在材料结尾仍处于重建客户端和继续验证阶段。
  title: Cursor 恢复与 Kimi fork 顺序修复
- block_id: 7885abbddccd7e8029087384e1ea51bdf72a43d691559d789bc6240548e137e7
  indexed: true
  sha256: fcf4b46e834438f06ff3a1834fede8755194f10ab8ef0810561d59e7590692a6
  source_range:
    end: 34070388
    part: 0
    start: 22213467
  summary: 已报告 Codex 原生 metadata、固定用户级 hook、VAWS task 绑定及零参数调用修复，相关测试从 19、62、74 项增至 136 项，并称指定 session 的 VAWS 调用成功；但该 session 仍为 Local，自动创建 Worktree 与 VAWS 环境选择没有完整实测。随后确认同项目的 Worktree 选择会保存在 `composer-mode-by-project`，但公开接口没有全局默认环境 setter，不能把手动选择或测试报告提升为所有新建流程已验证成功。
  title: Codex/多客户端 Worktree 与 VAWS 接入验收
- block_id: 0fade73b264d99678c6cacd34a217eaf8b404158219bc5c359df44dc719c6e5a
  indexed: true
  sha256: 4985400ed339522fe3fc0976de83b0a80b08e8e3830d5103b2cfbbba97235c0f
  source_range:
    end: 34070388
    part: 1
    start: 22213467
  summary: 材料记录了主仓 PR 冲突整合、按次源码更新、旧 `main` 快照风险，以及面向 Codex、Kimi、Grok、Claude、Cursor 的一次性初始化入口；Kimi 的 SessionSetup 能力、Grok 偏好和客户端探测已有测试，但 live 配置、部分原生默认模式与 Codex/Claude GUI 限制仍被明确标为缺口。Grok 个人构建已报告普通启动、fork、恢复和默认 Worktree 验收通过，但早期 MCP 重复连接、旧配置导入和 Windows 测试/运行时问题曾失败并随后修正；这些结论依赖报告与测试，不能替代所有客户端的独立全链路验证。
  title: 全客户端 Worktree 初始化与跨客户端配置方案
- block_id: 7bbf76c3c56fc8b9986c8c76ade8d4eca1b99624d87811527e6dbc7f925d1f54
  indexed: true
  sha256: 28426e79d78bfd58be6f3e99415d88b77d670cf9f057e4abeed204803edad046
  source_range:
    end: 34070388
    part: 2
    start: 22213467
  summary: 已报告通过 Grok 的 `disabled_mcp_servers` 屏蔽重复导入的 `remote-dev`、`vaws-task`、`vaws-knowledge`，并修复 Windows 锁竞争、版本切换、CRLF、预览哈希及 Kimi 自动更新路径错误；最终提交 `6217b95` 的 Linux、macOS、Windows CI 和两个 PR 检查均据称通过。本机初始化和本地同步也被报告完成，但两个 PR 当时尚未合并，且 Codex 仍无公开默认设置接口、Claude CLI 无默认切换开关；随后仅明确 Coordinator 已合入，工作区 PR 的网页合并受 GitHub Actions `workflow` 权限限制，是否最终合入未完成确认。用户将优化范围收窄到新建会话，resume 原则上沿用既有目录，但仍需防止新建逻辑误作用于 resume。
  title: 跨平台修复、PR 合入状态与新建优化边界
- block_id: b57b4a0ca02f6324eafea73623132529089b1341a6e0ec0c89efb8757f0a9032
  indexed: true
  sha256: aee6943750f95c6c9257002adb2b467c7ff847ed144ac950d52ee3fdb12cfc79
  source_range:
    end: 34070388
    part: 3
    start: 22213467
  summary: 材料比较了 Claude、Codex App 及提示词/Skill 的新建 Worktree 路径：Claude 可通过原生 `EnterWorktree` 切换会话 cwd，Codex App 需一次选择 Worktree 和 VAWS 环境，通用 prepare CLI 只能创建目录并返回路径，不能迁移父客户端 cwd、UI 或已启动 MCP。Claude 实测确认原生 cwd 切换成功，但 VAWS 默认源码仍指向母仓；原因定位为 coordinator 的 `PreToolUse` 未刷新变化后的 cwd，相关修复未扩展。验收还记录了新 worktree HEAD `4a1f28e`、未设置 `VAWS_ENV_RECEIPT`、测试 worktree 保留并在主目录留下未追踪 `.claude/worktrees/`；另一建议是采用一次 prepare、显式 Shell cwd 和绝对路径文件操作，并明确 UI/MCP 不会自动迁移。
  title: 新建 Worktree 的客户端方案与 Claude 验收
- block_id: 1ec7e1afdf57a8ddacca568cbb0208facbdf5032412533a34703cfbe1727d56a
  indexed: true
  sha256: af7a27ec1d79ed761cad3e8340df46edd4ddc612da5f17e11d6145f00b64f665
  source_range:
    end: 43046165
    part: 0
    start: 34070388
  summary: 材料提出以一次性 CLI prepare 创建独立 worktree、绑定 sources、锁定组件环境并接入共享知识库，resume 沿用原目录和环境；随后报告新增启动入口、客户端接线、运行时切换、取消隔离和跨 worktree 知识复用等测试结果。期间仍保留若干失败或未完成项：Backend 串行队列曾吞掉取消导致 status/stop 阻塞，缺少 task start.json 时存在 saved_ready 绕过风险，且官方客户端与真实新建验收一度尚未完成。后续材料声称部分修复和提交，但这些结论属于来源中的报告，不能据此将整套方案视为已验证成功。
  title: 端到端启动与共享知识库方案推进
- block_id: e0b70e3ad1d4d347f7a3ac0fcc2b043be5b178503d3758184d10aa59386bf2b1
  indexed: true
  sha256: 78d0bfdef52a527250c5abf5e275a6f224c9909e24d030cc97250588a60aebf2
  source_range:
    end: 43046165
    part: 1
    start: 34070388
  summary: 'Coordinator PR #26、文档和 repo-init 检查被报告为通过，统一入口用于新会话准备最新代码、锁定组件、精确路由 MCP 并共享知识；官方 Claude、Kimi、Grok 及 Cursor 的普通会话测试陆续报告了 worktree 写入、context 注入和结束摘要保存结果。材料同时记录了具体失败与修正：Claude 首次文件操作落在母目录、旧 hook 缺少知识上下文，Kimi 首次 MCP 漏传 context，Cursor `--print` 不触发 `afterAgentResponse`，以及 Grok `gh api` 彩色控制符破坏 JSON；这些问题后来各有修复或待重测状态。最终阶段仍等待 `LIVE_READY`、Windows CI 和合入后的最新版本验收，且多项“通过”来自报告而非独立核验。'
  title: 五客户端接入、基线验收与迁移问题
- block_id: 659bfd5ee290e8ce587ad5aa72cc6c596e5f32da197dbbd004d8253bb7217d63
  indexed: true
  sha256: ba87f1f5c202e75ca34316bd704e7b70d888c47026dd4f2eb1b61e8755161dc7
  source_range:
    end: 43046165
    part: 2
    start: 34070388
  summary: '材料报告 PR #154、#155 合入，三平台 CI 全绿，并声称 Codex、Cursor、Claude、官方 Grok、Kimi 均完成普通 CLI 新会话验收：自动创建独立 worktree、获取主仓最新代码和组件、注入任务 context，以及跨客户端共享知识和自动保存结束摘要。过程中保留了重要限制和纠正：Cursor 旧配置迁移曾漏装第二个 hook，Grok 两次只读 shell 漏切目录后自行纠正，原生 UI/cwd 仍不会被强制切换，Mac 锁屏后的桌面 UI 未复验；早先的部分失败记录也被明确保留。材料还报告旧目录保持旧版本、普通新会话可自动追上主仓，但这些验收与提交状态仍应作为可检索的来源陈述，不应提升为独立验证。'
  title: 主仓合入后的五客户端最终验收与边界
- block_id: de1df7b7766fe4f763eb6e8469ef7c9e8e9edeee1c08690b88a5f7530952e696
  indexed: true
  sha256: 56720f0828f166821ab87ec4e43d7a2abacfa95b3b6a5fde7d8abfa25c5f3e1b
  source_range:
    end: 54703487
    part: 0
    start: 43046165
  summary: 审视认为 AGENTS.md 应保留为薄层入口，只承载最小启动、工作目录、能力路由和仓库约束；repo-init、npu-fleet-monitor、报告类 Skill 不应再作为普通任务的强制前置，监控和报告脚本仍需保留。发现的具体问题包括配置工具可能按 worktree 选错 owner、repo_init_probe 的过时/高成本探测、知识重试缺少必填 --client，以及 Triton 指引过早要求报告和 grid 性能比较；这些结论主要来自只读检查，尚不等同于完整运行时验收。
  title: 管理式 Skill 与 AGENTS 精简审视
- block_id: 7c32352eaf2145da49d3bd9f1bb651b85c0d95326c60a94d57219d83c21d681b
  indexed: true
  sha256: f461d3345aecc7b1d055f7e0072f708829086cc008097ead5617fdbc0517b31b
  source_range:
    end: 54703487
    part: 1
    start: 43046165
  summary: 'repo-init 被移至 bootstrap 路径，仅在首次未初始化时由 AGENTS/needs_setup 指向，后续普通会话、更新和修复不再自动触发；同时修复 worktree 配置 owner、重复 hook、轻量任务直连和报告 Skill 的触发边界。PR #159 经三平台 CI、Windows 时序重跑及本地测试后合入，主仓、个人 Fork、依赖和知识库据来源称已同步到 67cf8fb；但合并期间曾遇到主仓新能力冲突、GitHub CLI 缺少 workflow 权限和嵌套子模块暂停，且部分“已完成”状态来自报告而非本块中的原始证据。'
  title: '一次性初始化、配置归属与 PR #159'
- block_id: 593398367e0af774ed182e0d3b269c89c24c08a030f9ef6792f8a5f1b94323d4
  indexed: true
  sha256: fb2e5efba0627821aea38e9bff956f926abae4fff392390d9a8a5495950d6f34
  source_range:
    end: 54703487
    part: 2
    start: 43046165
  summary: 'Trae 支持从 VAWS 仓库中清理，删除 `.trae`、生成/校验逻辑、投影和文档引用，保留 Claude 生成器与 ModelScope canonical 实现；其他五类客户端的共享接入仍保留，且 Kimi 文档被修正为 task context 必填、companion context 可选。PR #162 的 Linux/macOS CI 曾通过，但 Windows 暴露既有 ModelScope 的 PID 查询超时和固定 sleep 时序假设，随后改用 ready/release 握手并加入 SDK 等待 90 秒上限；来源最后仅称提交 `04969cc` 无阻塞，未提供该修复后完整三平台 CI 或合入同步的最终证据。'
  title: Trae 退役与 Windows 测试时序修复
- block_id: eb494aeed43ae8939248648f9635c21e1c280a52147222f32c74b8e346b76f38
  indexed: true
  sha256: 0c75964024c86b5d9b75db2e7d1c467477880aecdf7e0b0e6805f24bb2ca9a0e
  source_range:
    end: 54703487
    part: 3
    start: 43046165
  summary: '材料声称 PR #162 已合入并同步至 c6d6917，随后审查主线时发现并修复 hook matcher 丢失、worktree 知识库配置路径错误、benchmark served model 选择、显存采集 NameError 掩盖原始错误，以及监控路径和环境变量继承等问题；同时删除了 11 个据称无调用的旧 helper，未修改 profiling 分析 Skill。材料还声称相关定向测试、三系统 CI 和 git diff --check 均通过，并最终将 PR #163 合入、同步至 60c49ff；但这些是来源中的报告性结果，仍应独立检索提交、CI、主仓与 Fork 状态，不能视为已验证事实。期间曾报告 Windows/macOS/Linux 检查阶段性完成，也出现“未提交”的中间结果，后续叙述将其更新为已创建并合入 PR #163。'
  title: '主线缺陷修复、旧 helper 清理与 PR #163'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1eaa7d14da6a06415789f1ab54f198644624762ba39781c9adcfd6947e5a1d62
  kind: experience
  material_digest: 82627fa650f87f0afa249516b737eacea106bef51f48c35c31972d5dc39d181b
  revision: 10a9b8b2390ac639864b50e1b3ec5f97a26f89ef95f3cd12a142f016b75b1b42
  schema: mindie-entry/3
  summary: '当前材料描述的范围包括：按需准备 worktree、repo-init 首次初始化、稳定配置 owner、重复 hook 条件保留、知识库配置路径修复、benchmark 与显存采集错误处理、监控配置修复，以及删除已确认无调用的旧 helper；ascend-profiling-analysis 明确排除且未改动。来源报告称 PR #162（含 Trae 支持移除）和后续 PR #163 已合入，Linux/macOS/Windows CI 及若干定向测试通过，并同步到 c6d6917、60c49ff，但先前导航明确要求独立检索这些状态，因此仍将其视为待核验。继续保留未被本批材料证明消除的风险：原生 UI/cwd 不自动迁移、外部项目端到端启动缺少 owner 证明、历史 hook/context 漏配、Windows/ModelScope 时序、并发更新锁及 Mac 锁屏复验不足；不得把计划、报告的检查或假设升级为验证成功。'
  title: 当前任务导航：主线缺陷修复、客户端范围与待独立核验的合入状态
navigation: '当前材料描述的范围包括：按需准备 worktree、repo-init 首次初始化、稳定配置 owner、重复 hook 条件保留、知识库配置路径修复、benchmark 与显存采集错误处理、监控配置修复，以及删除已确认无调用的旧 helper；ascend-profiling-analysis 明确排除且未改动。来源报告称 PR #162（含 Trae 支持移除）和后续 PR #163 已合入，Linux/macOS/Windows CI 及若干定向测试通过，并同步到 c6d6917、60c49ff，但先前导航明确要求独立检索这些状态，因此仍将其视为待核验。继续保留未被本批材料证明消除的风险：原生 UI/cwd 不自动迁移、外部项目端到端启动缺少 owner 证明、历史 hook/context 漏配、Windows/ModelScope 时序、并发更新锁及 Mac 锁屏复验不足；不得把计划、报告的检查或假设升级为验证成功。'
schema: mindie-material-task/1
status: complete
task_id: 1eaa7d14da6a06415789f1ab54f198644624762ba39781c9adcfd6947e5a1d62
---

# 当前任务导航：主线缺陷修复、客户端范围与待独立核验的合入状态

当前材料描述的范围包括：按需准备 worktree、repo-init 首次初始化、稳定配置 owner、重复 hook 条件保留、知识库配置路径修复、benchmark 与显存采集错误处理、监控配置修复，以及删除已确认无调用的旧 helper；ascend-profiling-analysis 明确排除且未改动。来源报告称 PR #162（含 Trae 支持移除）和后续 PR #163 已合入，Linux/macOS/Windows CI 及若干定向测试通过，并同步到 c6d6917、60c49ff，但先前导航明确要求独立检索这些状态，因此仍将其视为待核验。继续保留未被本批材料证明消除的风险：原生 UI/cwd 不自动迁移、外部项目端到端启动缺少 owner 证明、历史 hook/context 漏配、Windows/ModelScope 时序、并发更新锁及 Mac 锁屏复验不足；不得把计划、报告的检查或假设升级为验证成功。

## Materials

- [个人 Fork、Release 更新与身份协作设计](blocks/8f9a663be8f8ea8a810065be9609d75d52efdaa593d395dcbd22db241e744b42.md): 讨论了首次使用时确认 GitHub 身份、创建并切换个人 Fork，以及基于正式 Release、轮询、fast-forward 和准备环境的自动更新方案；记录了 `github_identity_required`、无 Release、脏工作区、分叉和 Windows/WSL 未实机验证等未完成或失败边界。后续将服务器身份、Agent、任务、execution/容器租约串联，并参考 Agent Mail、Beads、Gas Town 的线程、回执和原子认领，但明确 coordinator 才负责资源分配。实现曾完成多轮入口、updater、Fork 重定向、子模块和 URL 校验修复，相关测试通过，但服务器认证和留言板仍主要停留在设计或未部署状态。
- [自动身份接入、共享复用与留言机制落地](blocks/81ab101e3815e60342085c6a3eca2154e453b82ee5a1fe2c53bdce125b05ffcb.md): 实现方向收敛为共享 root 登录下的一次 GitHub 用户绑定，工具自动传递用户归属、生成 `vaws-<用户名>` 容器名、收取留言并协调资源，Agent 不填写身份字段或维护记录。新增或修复了 `user_identity.py`、TaskClient/native hook 接线、个人 Fork 实际返回地址（含 `-1` 后缀）、共享算子编译缓存和 `/weights`、`/models` 条件复用；缓存取消或不确定时不清理重建，命中后按当前源码验证。消息支持持久留言、线程、去重和非阻塞收件，daemon 可在空闲时升级；多轮测试通过，但真实服务器留言、真实 NPU 缓存和 Windows/WSL 等边界仍未完全验收。
- [最小化共享开发机复用与运行身份链](blocks/8b89bb4a17e2cddf3e0f9150f742bbc311d5824a88e60364437d78a75b33a353.md): 复用范围被进一步限定为共享开发机上的算子编译产物、权重和已有环境，不区分个人/公开，不增加发布、权限或复杂鉴权；工具仅检查文件、版本、格式、分片及 ABI 等必要兼容条件。设计指出运行调用链仍需由 coordinator 共同入口承接身份，更新副本也必须保留容器命名映射，验收关注同为 root 的多用户不串容器、消息不重复和排队请求续接。随后第一版身份绑定、消息、缓存和空闲升级实现完成，整体测试报告为 Coordinator 377 项、工作区相关 138 项通过；但当前没有官方 Release，真实服务器留言与 NPU 编译缓存验收尚未完成。
- [主仓跟随与四机缓存验收](blocks/d5ab553e6a7d58b8bc3d22601942829535a2d874bb31bf959d2cf9d5b7f1f2c9.md): 更新器从 Release 改为默认每 5 分钟跟随官方主仓，实际查询到 `main @ b63c78269549e0f16ae6c4554f0b47b5e774a3da`，并完成个人 Fork 同步、四机 CPU/留言/排队清理及共享权重复用。153–156 的 `_shared_native` 缓存恢复 1103 个文件、C 扩展加载和哈希校验通过，但因 64 张 NPU 持续占用，未执行 NPU 算子数值或推理验证；154 曾有通信回执异常，保留了失败与恢复证据。随后确认常驻 watcher 已停止，方案转向新会话边界更新；已有目录恢复应读取保存的 environment receipt，不能按修改后的 lock 重新选环境。
- [会话边界更新与客户端原生接线审计](blocks/f4e999313d35ba0ded4f9d537bb976bfc527cfb8d45fddd26d43a5a6d5643383.md): 已移除常驻 watcher，新增一次性的 `prepare`，新目录创建前检查和准备版本，已有目录或 resume 保留原代码、依赖与配置；相关测试曾报告 35、37 及 115 项通过，但部分结论属于本地或契约验证。审计确认 VAWS session 原本可由各客户端 SessionStart 自动关联，真正缺口是原生 worktree 创建前的版本和环境准备；SessionStart 发生在 cwd 确定后，不能切换父客户端目录。Codex/Cursor 有可用的原生 setup 接口，Claude 有 WorktreeCreate，而 Cursor setup 的失败继续创建、异步时序和旧版本兼容仍需按实际安装验证；Grok、Kimi 缺少可配置创建前扩展，不能宣称已覆盖。
- [Grok/Kimi 边界与原生 setup 实现](blocks/8d185c30cb2dca9c61fbfe17ea4eefa2ccd96b0ec35c50618462e4e9aeca7d08.md): Grok 的 `new_session_worktree_mode`/`fork_worktree_mode` 只覆盖 `/new`、`/fork`，SessionStart stdout 不能改目录，PreToolUse 可通过 `updatedInput` 注入上下文；不应默认改写其他项目配置或假设存在主仓基线 setup hook。Kimi 0.42.0 的 SessionExternalHooksService 会等待 hook，但 cwd 已确定且返回值不能修改会话；fork 不触发 SessionStart，子 Agent 缺稳定 ID，MCP 也没有逐会话 context 注入能力。已落地的实现改动包括 Cursor `preToolUse` 自动补 VAWS context、hook 自动幂等关联以及 Codex/Cursor 原生 setup 接线；报告 48 项 hook 测试和 43 项 setup 测试通过，但未提交、未推送，也未做网络或硬件验收。
- [多客户端原生接线与首次 CLI 回退修复](blocks/d8de432376ac648c0d8be0c714b13c0b3e7aa78d26a034ef34d1e15e2024c985.md): VAWS 回调顺序、环境选择、子目录 CLI 任务登记、性能实验 sources 污染、无关 hook 触发及 Kimi 启动器等问题已被修复并通过多组测试；Codex 首次 CLI fallback 也会绑定实际 cwd 的 sources。20 个 skill、54 个 CLI 帮助入口完成审计，但 Claude/Grok/Kimi 的原生 worktree 更新、部分 shell context 和真实 GUI 验收当时仍有缺口。后续判断认为 Claude 可继续适配，Grok 需确认接口，Kimi 更接近客户端能力限制，不能把未完成统称为技术做不到。
- [Claude、Grok、Kimi 与 Cursor 的实测接线](blocks/de5b40637146a0671d9894c1e1702e948ec5d629215024936283d26e7ff9ad0d.md): Claude 的真实新建和恢复会话通过；Grok 的 worktree、无参 VAWS 调用、恢复及配置保护通过，并使用了个人 fork 修复版。Kimi 已针对 `-c`、setup 失败静默回源、child 关联、用户级 MCP、TUI resume 和 fork 状态复制完成多轮修复，测试结果包括 9、43、114 项等，但真实 MCP/fork 验收在该批次末仍由 agent 继续执行。Cursor 暴露了按 worktree 生成 MCP 标识、默认 Disabled 和配置加载顺序问题，后续全局 MCP planner 与 GUI 新会话/隔离测试通过；Codex 环境选择仍受应用接口和 Computer Use 限制。
- [Cursor 恢复与 Kimi fork 顺序修复](blocks/1a9828e462fc7d721400d46260d905d252f42a98b375764776a4fcacde0b1b05.md): Cursor 第二个新会话及切回旧会话的 GUI 验收报告为通过，目录、VAWS 任务、文件、代码版本和环境保持隔离或原样；期间 Mac 自动锁屏，使用了最长一小时的 `caffeinate -di -t 3600` 临时保持唤醒。Kimi Fork 已推送个人修复版并报告 SDK、文件历史、生命周期和类型检查通过，支持 dirty 内容、子模块、独立身份及恢复，但随后发现文件复制早于源 Agent 暂停可能造成历史不一致。该顺序已移入 SessionSetup 的暂停保护范围并通过针对性测试，最后一轮真实 `/fork` 验收在材料结尾仍处于重建客户端和继续验证阶段。
- [Codex/多客户端 Worktree 与 VAWS 接入验收](blocks/7885abbddccd7e8029087384e1ea51bdf72a43d691559d789bc6240548e137e7.md): 已报告 Codex 原生 metadata、固定用户级 hook、VAWS task 绑定及零参数调用修复，相关测试从 19、62、74 项增至 136 项，并称指定 session 的 VAWS 调用成功；但该 session 仍为 Local，自动创建 Worktree 与 VAWS 环境选择没有完整实测。随后确认同项目的 Worktree 选择会保存在 `composer-mode-by-project`，但公开接口没有全局默认环境 setter，不能把手动选择或测试报告提升为所有新建流程已验证成功。
- [全客户端 Worktree 初始化与跨客户端配置方案](blocks/0fade73b264d99678c6cacd34a217eaf8b404158219bc5c359df44dc719c6e5a.md): 材料记录了主仓 PR 冲突整合、按次源码更新、旧 `main` 快照风险，以及面向 Codex、Kimi、Grok、Claude、Cursor 的一次性初始化入口；Kimi 的 SessionSetup 能力、Grok 偏好和客户端探测已有测试，但 live 配置、部分原生默认模式与 Codex/Claude GUI 限制仍被明确标为缺口。Grok 个人构建已报告普通启动、fork、恢复和默认 Worktree 验收通过，但早期 MCP 重复连接、旧配置导入和 Windows 测试/运行时问题曾失败并随后修正；这些结论依赖报告与测试，不能替代所有客户端的独立全链路验证。
- [跨平台修复、PR 合入状态与新建优化边界](blocks/7bbf76c3c56fc8b9986c8c76ade8d4eca1b99624d87811527e6dbc7f925d1f54.md): 已报告通过 Grok 的 `disabled_mcp_servers` 屏蔽重复导入的 `remote-dev`、`vaws-task`、`vaws-knowledge`，并修复 Windows 锁竞争、版本切换、CRLF、预览哈希及 Kimi 自动更新路径错误；最终提交 `6217b95` 的 Linux、macOS、Windows CI 和两个 PR 检查均据称通过。本机初始化和本地同步也被报告完成，但两个 PR 当时尚未合并，且 Codex 仍无公开默认设置接口、Claude CLI 无默认切换开关；随后仅明确 Coordinator 已合入，工作区 PR 的网页合并受 GitHub Actions `workflow` 权限限制，是否最终合入未完成确认。用户将优化范围收窄到新建会话，resume 原则上沿用既有目录，但仍需防止新建逻辑误作用于 resume。
- [新建 Worktree 的客户端方案与 Claude 验收](blocks/b57b4a0ca02f6324eafea73623132529089b1341a6e0ec0c89efb8757f0a9032.md): 材料比较了 Claude、Codex App 及提示词/Skill 的新建 Worktree 路径：Claude 可通过原生 `EnterWorktree` 切换会话 cwd，Codex App 需一次选择 Worktree 和 VAWS 环境，通用 prepare CLI 只能创建目录并返回路径，不能迁移父客户端 cwd、UI 或已启动 MCP。Claude 实测确认原生 cwd 切换成功，但 VAWS 默认源码仍指向母仓；原因定位为 coordinator 的 `PreToolUse` 未刷新变化后的 cwd，相关修复未扩展。验收还记录了新 worktree HEAD `4a1f28e`、未设置 `VAWS_ENV_RECEIPT`、测试 worktree 保留并在主目录留下未追踪 `.claude/worktrees/`；另一建议是采用一次 prepare、显式 Shell cwd 和绝对路径文件操作，并明确 UI/MCP 不会自动迁移。
- [端到端启动与共享知识库方案推进](blocks/1ec7e1afdf57a8ddacca568cbb0208facbdf5032412533a34703cfbe1727d56a.md): 材料提出以一次性 CLI prepare 创建独立 worktree、绑定 sources、锁定组件环境并接入共享知识库，resume 沿用原目录和环境；随后报告新增启动入口、客户端接线、运行时切换、取消隔离和跨 worktree 知识复用等测试结果。期间仍保留若干失败或未完成项：Backend 串行队列曾吞掉取消导致 status/stop 阻塞，缺少 task start.json 时存在 saved_ready 绕过风险，且官方客户端与真实新建验收一度尚未完成。后续材料声称部分修复和提交，但这些结论属于来源中的报告，不能据此将整套方案视为已验证成功。
- [五客户端接入、基线验收与迁移问题](blocks/e0b70e3ad1d4d347f7a3ac0fcc2b043be5b178503d3758184d10aa59386bf2b1.md): Coordinator PR #26、文档和 repo-init 检查被报告为通过，统一入口用于新会话准备最新代码、锁定组件、精确路由 MCP 并共享知识；官方 Claude、Kimi、Grok 及 Cursor 的普通会话测试陆续报告了 worktree 写入、context 注入和结束摘要保存结果。材料同时记录了具体失败与修正：Claude 首次文件操作落在母目录、旧 hook 缺少知识上下文，Kimi 首次 MCP 漏传 context，Cursor `--print` 不触发 `afterAgentResponse`，以及 Grok `gh api` 彩色控制符破坏 JSON；这些问题后来各有修复或待重测状态。最终阶段仍等待 `LIVE_READY`、Windows CI 和合入后的最新版本验收，且多项“通过”来自报告而非独立核验。
- [主仓合入后的五客户端最终验收与边界](blocks/659bfd5ee290e8ce587ad5aa72cc6c596e5f32da197dbbd004d8253bb7217d63.md): 材料报告 PR #154、#155 合入，三平台 CI 全绿，并声称 Codex、Cursor、Claude、官方 Grok、Kimi 均完成普通 CLI 新会话验收：自动创建独立 worktree、获取主仓最新代码和组件、注入任务 context，以及跨客户端共享知识和自动保存结束摘要。过程中保留了重要限制和纠正：Cursor 旧配置迁移曾漏装第二个 hook，Grok 两次只读 shell 漏切目录后自行纠正，原生 UI/cwd 仍不会被强制切换，Mac 锁屏后的桌面 UI 未复验；早先的部分失败记录也被明确保留。材料还报告旧目录保持旧版本、普通新会话可自动追上主仓，但这些验收与提交状态仍应作为可检索的来源陈述，不应提升为独立验证。
- [管理式 Skill 与 AGENTS 精简审视](blocks/de1df7b7766fe4f763eb6e8469ef7c9e8e9edeee1c08690b88a5f7530952e696.md): 审视认为 AGENTS.md 应保留为薄层入口，只承载最小启动、工作目录、能力路由和仓库约束；repo-init、npu-fleet-monitor、报告类 Skill 不应再作为普通任务的强制前置，监控和报告脚本仍需保留。发现的具体问题包括配置工具可能按 worktree 选错 owner、repo_init_probe 的过时/高成本探测、知识重试缺少必填 --client，以及 Triton 指引过早要求报告和 grid 性能比较；这些结论主要来自只读检查，尚不等同于完整运行时验收。
- [一次性初始化、配置归属与 PR #159](blocks/7c32352eaf2145da49d3bd9f1bb651b85c0d95326c60a94d57219d83c21d681b.md): repo-init 被移至 bootstrap 路径，仅在首次未初始化时由 AGENTS/needs_setup 指向，后续普通会话、更新和修复不再自动触发；同时修复 worktree 配置 owner、重复 hook、轻量任务直连和报告 Skill 的触发边界。PR #159 经三平台 CI、Windows 时序重跑及本地测试后合入，主仓、个人 Fork、依赖和知识库据来源称已同步到 67cf8fb；但合并期间曾遇到主仓新能力冲突、GitHub CLI 缺少 workflow 权限和嵌套子模块暂停，且部分“已完成”状态来自报告而非本块中的原始证据。
- [Trae 退役与 Windows 测试时序修复](blocks/593398367e0af774ed182e0d3b269c89c24c08a030f9ef6792f8a5f1b94323d4.md): Trae 支持从 VAWS 仓库中清理，删除 `.trae`、生成/校验逻辑、投影和文档引用，保留 Claude 生成器与 ModelScope canonical 实现；其他五类客户端的共享接入仍保留，且 Kimi 文档被修正为 task context 必填、companion context 可选。PR #162 的 Linux/macOS CI 曾通过，但 Windows 暴露既有 ModelScope 的 PID 查询超时和固定 sleep 时序假设，随后改用 ready/release 握手并加入 SDK 等待 90 秒上限；来源最后仅称提交 `04969cc` 无阻塞，未提供该修复后完整三平台 CI 或合入同步的最终证据。
- [主线缺陷修复、旧 helper 清理与 PR #163](blocks/eb494aeed43ae8939248648f9635c21e1c280a52147222f32c74b8e346b76f38.md): 材料声称 PR #162 已合入并同步至 c6d6917，随后审查主线时发现并修复 hook matcher 丢失、worktree 知识库配置路径错误、benchmark served model 选择、显存采集 NameError 掩盖原始错误，以及监控路径和环境变量继承等问题；同时删除了 11 个据称无调用的旧 helper，未修改 profiling 分析 Skill。材料还声称相关定向测试、三系统 CI 和 git diff --check 均通过，并最终将 PR #163 合入、同步至 60c49ff；但这些是来源中的报告性结果，仍应独立检索提交、CI、主仓与 Fork 状态，不能视为已验证事实。期间曾报告 Windows/macOS/Linux 检查阶段性完成，也出现“未提交”的中间结果，后续叙述将其更新为已创建并合入 PR #163。

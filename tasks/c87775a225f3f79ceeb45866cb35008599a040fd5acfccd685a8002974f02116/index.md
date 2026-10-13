---
blocks:
- block_id: 27685a782fe52d3439406e1887720d373b25ec42e41eef38faa26cf84dbdf727
  indexed: true
  sha256: cf6a7a077b0ea8b100d071f12683f0de2e41181cf8c808f05a45626b41f8f910
  source_range:
    end: 16778068
    part: 0
    start: 0
  summary: 围绕 MindIE Agent 的 Stop hook 无限循环，记录了已删除脚本和旧插件缓存导致退出码 2、累计重复 110 轮的问题，以及超时、输入输出上限、去重、预算、并发和失败熔断等防护措施。后续设计明确要求 Plugin 仅在当前 session 手动调用后激活，MCP 与 hook 必须消费激活状态，不能因连接、SessionStart、Stop 或知识查询自动启动；当前实现仍需继续完成这一边界收紧。
  title: Hook 无限重试与按 session 激活设计
- block_id: c4d16031e527f08adc1d496945ffb3aa1334ea733b69f1d1ecbf4dd1dc16187b
  indexed: true
  sha256: 1d9f3c7dc2633bc4acde7cea57e083c86e32b0498032b90ac9175b3407ad5f4b
  source_range:
    end: 16778068
    part: 1
    start: 0
  summary: 已记录 session 激活凭据、MCP/hook 校验、跨 session 拒绝、超时清理、零自动重试和连续失败暂停等 macOS 本地实现，并报告 61 项测试及真实 gpt-5.6-luna/max 验收通过；Stop hook 仍需信任，远端 main 尚未包含安全修复。自动更新当前跟踪 main，支持失败回滚、忙碌延后和同提交最多 3 次尝试，但完整远端升级路径未验收；首版范围扩大为 macOS、Windows、Codex、vLLM-Ascend 单领域，Windows 实机和发布候选闭环仍是阻塞项。
  title: 会话隔离、自动更新与首版发布缺口
- block_id: b8210862789bc01d63c623e0c763a6ca747a5fa51b15f1b676508f5160c983c6
  indexed: true
  sha256: 0ebb18a9acccce1a0e71c43ce6121d01a9942e181814d1e7a86c6a8a33abbef9
  source_range:
    end: 16778068
    part: 2
    start: 0
  summary: 发布前审查发现历史上有 18 个业务 skill、独立 repo-init 及共 25 个历史名称，旧脚本依赖工作区目录、共享 helper 和状态文件，当前插件仅有一个激活入口，迁移、触发条件和知识闭环效果尚未证明完整。计划采用破坏式重写，由 Kimi 负责大块核心与插件/领域重构，Grok 负责语料迁移和独立评测，删除过期死代码，并由主流程验收检索、采集、归并、分发、反馈、负向/unknown、多轮纠正及实际收益。当前 macOS 基线显示未查询知识时 Stop 虽触发但采集为 0；未激活 MCP 业务调用被拒绝、超时取消和输出超限进程已清理，重写候选及 Windows 实机仍待验证。
  title: 知识闭环、旧 Skill 迁移与破坏式重构
- block_id: cd4e3a6569e38b476bd8f6b1d2a836b02d4adb2d5e643e75f4e3e7401b9de4c7
  indexed: true
  sha256: 83e2fe0aab137640b0282a66584d9bb90bc56e8604a17408bb39820401c4d507
  source_range:
    end: 33578480
    part: 0
    start: 16778068
  summary: 代码审查和独立复验发现知识纠正、撤回同步、迁移崩溃恢复及 Hook 进程清理等部分已通过，但分发链路仍有旧结论残留、空版本导出缺口；远程执行封装仍把请求成功误判为任务完成，需修复后验收。真实 Ascend 数据已生成并下钻检查 HTML 报告，但层数未知、单 rank 均衡性和 host bound 等结论存在证据不足，旧项目入口与兼容层正在删除；profiling 分析 Skill 按用户要求暂缓，尚未按 GPT-6 指导完成审查。其余 Skill 已发现旧仓库入口、无效 YAML 和过长触发描述，正依据 GPT-6 指导及 skill-creator 规范收紧；Kimi 迁移任务已到硬截止并停止，Grok 的 MCP/远程执行修复仍在进行。
  title: 重构验收与 Profiling 暂缓
- block_id: 9b5601348b9f008b3dddf018a2a4700b88b8736dbf06ec56faf2f90f9fb481d9
  indexed: true
  sha256: 9523b9a3101447ef9fd739deac187715e15245982a185bfd36364c76c543d601
  source_range:
    end: 50356355
    part: 0
    start: 33578480
  summary: 知识组件曾完成部分本地检查和真实任务串联，但 NPU 不可用、工具契约错误、会话身份校验停止及裁判误归因等失败表明，不能把组件通过或权重上涨视为知识收益。经与 Fable 对齐，方案收敛为授权后本地读取任务过程、保留失败与更正、一次提炼详细经验和检索摘要，再通过 GitHub 分发；移除中心知识服务、运行时裁判、自动投票加权和采用率惩罚。经验正文追加观察而非反复压缩，点踩可触发修正或退役，多任务好评仅用于发现 Skill 候选；本轮仍是设计对齐，代码重构和真实验收未完成。
  title: 知识体系从重型闭环收敛为本地提炼与 GitHub 分发
- block_id: 631bb6e1b06133f0a7988aa14a9e24b915a14b918cf9fdc1d2d05ac0805ddbc5
  indexed: true
  sha256: 8e0cdd971ac302418cf0a974d854782456ce35e79a95350dd0e6426aa150fd41
  source_range:
    end: 50356355
    part: 1
    start: 33578480
  summary: 设计边界已明确：社区贡献关闭时不采集会话、不生成草稿、不启动整理或脱敏模型，插件更新、知识库同步和按需检索仍可用；重新开启不补采关闭期间材料。按空库进行破坏式重写后，曾发现 transcript 第二段续读卡住、误采 assistant analysis、整理器 `invalid_json_schema`、配置 `fork`/`bot` 不兼容、旧队列跨开关提交及已删除 `attach` 接口等问题；CI 已获 workflow 权限并合入，整理器真实调用也以一次 37 秒、无重试、结构化输出通过，但这只证明该组件可运行。未激活 MCP 拒绝且未创建租约已验证，自动 PR、Grok 审核、客户端同步、完整贡献链路及剩余修复仍未完成。
  title: 空库破坏式实现与社区贡献开关验收
- block_id: 25cfa361c9394c5b4aa905c0df6d5ecae7285e20e2e3afae0543b9a04ba7bbc6
  indexed: true
  sha256: 2fa9cd4df1a84ec47fa7f4a98563f177571f30273ca859efb1e203a4331b2a04
  source_range:
    end: 67152196
    part: 0
    start: 50356355
  summary: 真实空库、Hook、贡献开关和未激活 MCP 路径已分别验证，确认了零采集、零后台调用、拒绝未激活任务等边界；但完整知识 PR→Bot 合入→独立消费闭环、自动更新和真实事件送达仍未验收。长任务样本约 437 MiB，当前解析与整理仅覆盖极小部分，且遗漏 custom_tool_call；估计的 Markdown 体量只是容量推断，不能视为整理成功。随后任务转向修复长任务采集、经验整理、新会话效果和端到端流程。
  title: 真实链路验收与长任务体量暴露
- block_id: dd826c06bc03fd16209cce86d6534c96a5917892423b16d02db80f930ee24183
  indexed: true
  sha256: 0567f14cb6accacb18b7b578caba3fe347f6e4ad20368d9e296dc73ee8e4d12d
  source_range:
    end: 67152196
    part: 1
    start: 50356355
  summary: 已扫描约 3 GiB、591 份候选记录，修复了自定义工具调用遗漏、派生任务父历史重复、额度续接、纠正摘要更新和子进程清理等问题；仍保留超大记录跳过、字段截断及模型整理超时等不确定性。两次详细整理未在 60 秒内完成，远端 NPU 验收曾因 remote_bash 审批策略拒绝而未执行，不能算经验质量或算子验证通过。后续计划用同类任务做有经验／无经验对照，分别测量整理正确性、实际使用收益、token／工具调用和发布同步闭环；remote-dev 正在被改为所有 session 可按需使用的通用入口。
  title: 长任务解析修复与经验效果实验
- block_id: 89394e051ea4f14ddbe3fff8f4389814b67e559381577d8a6f0c0ea47b8ab337
  indexed: true
  sha256: e3512be88ebbe9258b83d4b7a7cbbff74ec48c9f166d29f760bf35f8895c70f8
  source_range:
    end: 84617837
    part: 0
    start: 67152196
  summary: remote-dev 已发现并计划修正跨连接 MCP 请求编号复用误拒绝、损坏记录被当空记录等问题；未激活 MindIE 的原生任务已在逻辑设备 `0` 上完成 FP16/BF16 RMSNorm 运算，但社区贡献采集曾因后台服务启动失败而未产出经验，后续完整采集才生成约 4.4 KiB 的脱敏 Markdown 并创建知识 PR。字段方案逐步收敛为保留 `schema`、`entry_id`、`domain`、`kind`、`title`、`summary`，`conditions` 仅记录可核实的软件版本或源码 commit，其余实验参数放正文；`producers`、`sources`、`status`、`retirement_reason` 和公开 `revision` 可移除或由程序维护。核心链路仍有未完成或仅部分验证的内容，包括空库对照曾耗尽调用预算、GitHub 事件唤醒未确认，以及格式精简和真实任务受益尚需完整闭环验收。
  title: 字段精简与未激活 MindIE 的远端验收
- block_id: 52c44b71653e10781ce866a20999b33cd5c16b3bb86474cf6468b2b5cd3670bc
  indexed: true
  sha256: a5ff8d5f4681e7205a62ebbc7dd1650e6fa573735be375b406e0667dfd939cf9
  source_range:
    end: 84617837
    part: 1
    start: 67152196
  summary: 格式重构的核心 178 项、adapter 79 项检查及 CI 通过，并修复标题改动产生重复文件、下库条目被旧草稿恢复、旧数据库不兼容和 `rms_norm` 检索无法命中 `torch_npu.npu_rms_norm` 等问题；真实远端验证确认正文更新、历史引用、删除退出检索和旧草稿阻断链路有效。两条独立经验已合入主分支，新 shape `[11,1024]` 和新 seed 的任务读取经验后完成 FP16/BF16 运算，退出码为 0，但这些结果不能证明普遍提效或节省 token。此前“每轮一次写入”“10 个候选、20 分钟”等限制均被确认是自行添加、没有业务依据并已撤掉；Grok Bot 的目标收敛为检查敏感信息、明显违规内容和格式后触发合入，同时按提交去重，反馈纠错和 Skill 提案另行维护。
  title: v2 经验发布、撤回链路与 Grok Bot 职责收敛
- block_id: f71e76585c1ce28eb26f5def7c34f0c0a8f2a6f1fe402d04a6881ecd2648a74b
  indexed: true
  sha256: 36a038c666bcd0a4932fd9ae353754842194c6c5f38255e743d336967282df41
  source_range:
    end: 101400361
    part: 0
    start: 84617837
  summary: 此前已修复多项过度限制并完成部分真实验收，但 24 小时租约与更新等待耦合、固定容量上限等问题仍待简化；后续架构审查又确认任务生命周期、首次配置、故障恢复、本地清理和组件边界需要补齐。当前计划是让首次安装或首次调用时配置贡献授权，提交确认后清理可重建本地材料，并将 Kimi/K3 适配放入独立仓库、旧 Skill 暂不迁移；Kimi 的真实探针显示 Hook 有原生任务 ID，但 MCP 和 Shell 缺少该 ID，不能依赖工作目录或最近 session 猜身份。实现仍有未完成或已复现问题，包括暂停后授权重置、调用参数未核对、清理残留完整正文、解析器动态加载报错、恢复仍受次数上限限制，以及时间戳单位不匹配；Kimi 入口、模型整理、贡献闭环和完整原生验收尚未证明通过。
  title: 架构简化、生命周期修复与 Kimi 分仓适配
- block_id: a2c31116621f4087edc1c9d6d7cddafe591e2603a8fe574d1e86816f64bf8e6a
  indexed: true
  sha256: d67dad776466941d0e7cf191f0a96e0fdd11ce779f0c19307b80985c1f0c43bd
  source_range:
    end: 101400361
    part: 1
    start: 84617837
  summary: 独立仓库 `mindie-agent-kimi` 已创建，说明继承 VAWS 九条设计原则；独立依赖环境中通过了 26 项组件测试，但入口交互、真实命令配置返回、模型整理和完整可用性仍在修正，不能将组件测试视为端到端成功。复查发现首次调用配置流程、按已确认 PR 内容执行贡献清理与恢复，以及更新器对 unknown 历史 PR 的处理仍不符合要求：未确认记录应可查询但不应永久阻塞更新。后续需要用真实 Kimi/K3 流程验证经验整理器能否从已有 NPU 材料生成有用经验，并保留适用范围和未验证项。
  title: Kimi 独立仓库与 VAWS 九原则验收缺口
- block_id: 7721f5bd82b1dfb0a507292e32edecd40e871da589e8b0a91f60ea25aa173ab6
  indexed: true
  sha256: c60ef450a3916f23c266a392e621a91d3e86270c05ebb78935b15b703dc68db2
  source_range:
    end: 118243035
    part: 0
    start: 101400361
  summary: Codex 已有真实跨版本接续证据；Kimi 曾因 MCP 启动命令错误导致实际加载数量为 0，修正后两个 MCP 实际加载并通过只读激活、检索、正文读取及贡献关闭检查，随后跨版本接续也通过，但自定义配置路径仍可能依赖启动终端环境变量，初装是否依赖源码 checkout 尚待确认。Claude Code 的 `mindie-agent-cc` 分仓已建立，子代理正在按九条原则修复真实安装、配置路径、自动更新器和 Hook 边界；开发检查不视为实机验收。当前三端统一架构和进度表已合入主仓，旧知识分支文档仍需更新，旧 Skill 与 profiling 暂缓。
  title: 三端适配器实机验收与安装边界收敛
- block_id: b3b911ff869b0ee89a7bf53416f5e67668d7a0ef4d1b68cb620efd523b0e183e
  indexed: true
  sha256: ccd0d680802c6b221f5b27283f13efcdb7d12de618eaa97c527241e0551c045b
  source_range:
    end: 135057085
    part: 0
    start: 118243035
  summary: 三端实现 PR 已合入 main，并完成安装故障回滚、远端 main 更新安装和部分原生回读；Windows、经验质量、贡献闭环、定时运行及知识源错误汇总等仍曾被列为后续事项。随后将重点调整为忠实记录实际过程，省略原文未提及内容，同时修正同步状态、定时入口、提交后续写 ID、下库和隐私扫描误判等问题。真实验收中曾出现 GitHub 404 导致 degraded、锁竞争 deferred、隐私扫描拦截、旧入口失败等情况；部分修复通过了专项检查或实机验证，但多处明确未提交、未推送或仍待完整闭环确认。
  title: 三端合入与 P0/P1 缺口排查
- block_id: 516d6841ca7e62ca4f33aacaa6253cd19020f92d89bc72fefb017d6eb8c6d5d4
  indexed: true
  sha256: c393ef7e7d06da7c9f72a4d3f326b11c0fa6cf432f80b4502cbe84f3928c9a97
  source_range:
    end: 135057085
    part: 1
    start: 118243035
  summary: '整理器规则改为只记录来源实际出现的过程、现象和结果，缺失内容直接省略，不补未知清单或经验教训；Codex、Kimi、Claude Code 曾输出符合该要求的记录。三端 macOS 调度首次执行均报告 runs=1、exit=0，且测试任务已清理；提交后保留条目标识、下库标记 withdrawn、隐私扫描语法误判和反馈 PR 等路径也有实际检查。真实贡献链路仍有未完成部分：Claude Code 自动创建了仅反馈的 PR #15，但 Grok Bot 是否自动审阅合入尚未确认；生产 Codex 新 Hook 已获信任，Kimi 仍在补做真实 NPU 输出直接比较。'
  title: 忠实整理、调度与贡献闭环验收
- block_id: 647162d08bdfa960640306e0f6101298c0f43e8e0f285fb8f0c38850024f45f3
  indexed: true
  sha256: 92d26b940254c59728deaa92d9d1c855429b81270f7823a3bb752e44971ef5c4
  source_range:
    end: 135057085
    part: 2
    start: 118243035
  summary: 实施进度草稿已纳入合入版本和真实验收，并区分实现、原生证据与完整闭环；该文档尚未提交，生产升级、Kimi 新检查、Bot 处理和 Windows 仍标为待完成。用户已确认解锁并信任新版 Codex Stop Hook，后续计划回读信任状态并继续 Grok Bot 验收。NPU 补查未执行新的算子检查，原因是 SSH 主机可达但专用验收容器已退出。
  title: 进度文档更新与 NPU 补查受阻
- block_id: a47763f4ec0bacf92b2b919d6cd2a41049c297f4cdd3f8d742bfb4ec9ee24829
  indexed: true
  sha256: dd66cc7c668d38670e122311c6387e6a507a6efae44cadd11c90115b1efd0852
  source_range:
    end: 151621421
    part: 0
    start: 135057085
  summary: '早期验收确认生产 Codex Hook 可触发 Stop，且真实 Claude Code NPU 任务完成了执行前纠正、经验整理、脱敏和自动创建 PR #16；随后 PR #16 合入，定时同步使新任务读取并采用经验，新的 NPU 两 dtype、两种布局比较通过，但首稿需要人工纠正，因此不能称为全程无人干预。Kimi 曾在升级后出现 Stop 可送达但整理器退出码 2、stderr 丢失且原因未知；三端服务恢复修复、失败分类和进程组清理后来分别获得源码或受控实机证据，但 Kimi 完整 Stop 到发布链路仍未被证明。期间还保留了 Bot opened 事件需人工澄清临时只读要求才能继续执行、服务交接缺口、超时开发调用无补丁等失败和未完成结果。'
  title: 真实 NPU 经验发布与 Bot 事件链路
- block_id: 1c1241146603d68fd579b36e4c433db3008968a66ce13d5da54b5890d4d4d9da
  indexed: true
  sha256: 7fba9e293ffad643da13ad0c62fef41eb94087c88dce565f537ba15b9063a812
  source_range:
    end: 151621421
    part: 1
    start: 135057085
  summary: 'PR #16 的 opened 事件最终确认可送达并由 Bot 审查合入，但执行曾受临时只读语义影响；新任务经正常 OS 定时同步读取主分支经验并完成 NPU 验证，实际执行前发生一次人工脚本纠正，新增采集和发布为零。三端更新恢复路径的本地、源码及真实客户端检查覆盖新旧进程切换、回滚、无授权不启动和中断状态保留，Codex 与 CC/Kimi 的 Stop 或取消后进程清理也有不同程度的实机证据；Kimi 整理器曾以退出码 2 失败，旧失败原因仍未知。后续暴露并复现了 Kimi、CC 子孙进程残留和状态入口隐藏后台失败的问题，诊断组件及 Kimi/CC 取消修复已审查或合入，但完整自然贡献、扩展维护能力和 Windows 实机仍未完成。'
  title: 服务恢复、真实闭环与取消清理验收
- block_id: 88606808f5c6990431efbed49961220fba8bb4571d7151b5d7afee937e8d93ea
  indexed: true
  sha256: 044ba83e0991e343fd4bcdcde7074253e44856e48c11efc479211678521baa11
  source_range:
    end: 151621421
    part: 2
    start: 135057085
  summary: 新一轮非 Windows 审查已拉取 10 个仓库，并发现 Codex 将损坏配置或更新锁竞争误报为“尚未配置”，Kimi/CC status 不展示后台整理失败，服务启动失败丢弃原始原因，且远端作业状态可能泄露内部授权字段。Kimi 的独立全局审查和新任务闭环验收已下发，但尚未给出完成结论；准备阶段的主控 MCP 身份冲突未重试，改用既有 SSH 路径管理专用容器。当前重点是让状态入口准确呈现故障、保留可操作错误类别和恢复/绕过路径，并评估架构的扩展性、维护性及 agent 快速发现和处置错误的能力。
  title: 全局代码质量审查与可诊断性缺口
- block_id: 547579ec933c8d00e06480e523c7654ff62defed7eba088fab894aa50fd18e2f
  indexed: true
  sha256: 4f879033ff29359b74b8a49fa1251a04f565dc33a57e9d5ee30898d5195138b6
  source_range:
    end: 169584531
    part: 0
    start: 151621421
  summary: 审查和多项本机验收显示，状态接口、错误分类、远端异常投影、取消清理、依赖 pin、配置读取和活跃日志保留已有修复或局部通过证据；Codex 的配置损坏诊断、远端非零作业和 Hook 信任状态也有实测记录。日志总量自动清理、Windows、Kimi Stop→PR 业务闭环及 Bot 无人提醒合入仍未完成或未验证，若干提交仍是本地候选，不能将安装、回归检查或开发目录验证提升为完整发布成功。
  title: 三端诊断修复与验收边界
- block_id: 292c31cd66b3ed532e65df498a6607ba9458e56fad1babd6a12a0ed77cf7aa0b
  indexed: true
  sha256: dde577c03737881f99b005f6a44d7d73b7cbc4539a8d175c50bfa608a61e474e
  source_range:
    end: 169584531
    part: 1
    start: 151621421
  summary: 旧 VAWS 式自动上报被评估为可接入 Codex、Kimi 和 Claude Code：建议共用脱敏、去重、有限预算的 reporter，各适配器仅记录真实入口故障，并与知识贡献独立授权；参数错误、用户取消、业务命令非零和普通网络失败不应默认上报为工具缺陷。日志方案计划关联操作与阶段、保留诊断引用、保护活跃及未读分片、处理定时器持续追加并支持总量清理，但旧 reporter 的发布失败/对账次数上限和日志轮转保护仍需实现与实机验证；当前已有 Codex 接线和共享故障引用，尚不能算自动创建 Issue 成功。
  title: 自动故障上报与日志系统方案
- block_id: b5e3a1b00dc41a586786a58d7177d1d2c0da29fa9110b7023fc7461cc3e2eeaa
  indexed: true
  sha256: ca89e8f035d5844eb42e846433df591ed2dde954fd19abb241c25382e6644498
  source_range:
    end: 169584531
    part: 2
    start: 151621421
  summary: 一次隔离的 Codex Luna/max 验收让自有 helper 返回损坏协议；记录称原生 MCP 只调用一次、生成本地诊断、未重试、未启用知识贡献或上传授权，退出后无残留进程。验收同时发现提示未明确区分本地 helper 协议失败与远端业务失败，随后补充了故障阶段和查询命令；该材料未提供自动 Issue 上报成功或三端完整闭环的证据。
  title: Codex MCP 协议故障验收
- block_id: 83083263e9ed1c78ab27882295eaa4660e9526430abc7a24b88f10c72dc5b5a1
  indexed: true
  sha256: 305def755be7b56de96ac3515cd8bbbc9b7f79275c751c661ea7106b0cd61bb3
  source_range:
    end: 185488274
    part: 0
    start: 169584531
  summary: 共享日志、独立 reporter、授权状态和有限处理预算已完成大量本地、CI 及原生验收；Codex、Claude Code、Kimi 均有一次性故障调用或上报链路证据，但 Kimi/Claude Code 的模型回答曾超出工具的 unconfirmed 证据，不能视为完全忠实。Windows 用户机器验收、生产自动上传和部分最终合入状态仍需区分；Kimi 曾因 OAuth `The provided authorization grant is invalid` 失败，重新登录后才完成调用。
  title: 三端诊断上报与固定版本交付进展
- block_id: f17a76a9ae27f606ccbcf0e0f197199a755c0d23fe366bc470e703288534d249
  indexed: true
  sha256: 7b04a69136e6ca8172319d1e3c84b258e7846dee4f038a35712f20fc1573b9dc
  source_range:
    end: 185488274
    part: 1
    start: 169584531
  summary: Kimi 与 Claude Code 的旧打包器升级路径已发现并修复，覆盖新增 Hook/MCP 入口、独立包刷新、回滚、无效指针、manifest/version/skills 一致性及清理失败状态；Claude Code 的原生刷新和 reporting-status 只读调用通过，但模型仍把“未配置自动上报”误述为“没有采集日志”。信任 Codex Hook 后，Luna/max 的 Stop 实际只触发一次、57 毫秒完成、零工具调用和零重试；验收目录信任项已清理，但运行前配置快照不完整，配置字节未变不能算通过。
  title: 旧版本升级刷新与 Codex Stop 原生验收
- block_id: 563eaf30c205309192964304342622f6cce0169a142a9132efc33e2f3789ea36
  indexed: true
  sha256: faaec5ba82abf5e96f9d9a620c529dec8884504cea5257b348d9bed555864624
  source_range:
    end: 185488274
    part: 2
    start: 169584531
  summary: 审查提出优先补齐：三端原生 reporting-enable/disable 跨端闭环、取消调度和卸载的真实状态确认、reporter 随插件升级交接、普通用户安装维护文档，以及 Kimi 知识贡献业务链；旧 Grok CLI 清理和知识有效性扩展属于后续项。随后按用户要求计划使用 `grok-4.7 / xhigh` 与 `kimi-code/k3 / max` 并行推进，但 K3、Grok 代理因 usage limit/截止时间中断，留下未完成草稿，不能当作交付；Kimi 的一次真实 NPU 命令已结束但结果仍停在审批读取处。现有修改包括移除旧模型诊断状态、收敛调度取消和删除请求，但 Grok 对 5 个源码/测试文件的删除请求被自动审批拒绝，尚待人工确认与后续审核。
  title: 下一步工作与中断后的未完成修改
- block_id: b55fd5ea2640315bcf380476c5be521b25393aafb4a5574d925248396e52cd7b
  indexed: true
  sha256: 5bf4d60252a39d868b47360ba3bc29234de02fd3b61f5bdbb88a4e7bcce40e41
  source_range:
    end: 202366947
    part: 0
    start: 185488274
  summary: 记录显示三端调度取消、持久安装路径和部分上报器升级/回滚已完成实机验证，但升级失败与依赖冲突摘要曾被过度泛化，且 Windows、低版本冲突和最终候选包验收仍未完成。代码审核发现回滚服务描述、版本指针恢复、授权撤销竞态及跨层超时等问题；部分修复后来通过真实 macOS 验收，但不能据此推广到所有平台。Kimi 业务验收曾因 600 秒控制器截止和审批等待中断，后续原会话恢复并开始处理 A3 构建问题；知识经验和 PR 闭环一度仍未完成。
  title: 上报器交接修复与Kimi验收停滞
- block_id: b248f91660c9308586e93f247c67338e970d084fe5b2b101f167fc3258f0bfe9
  indexed: true
  sha256: 466b9d6e2fc5a8ceca1197d98b92009bafa31be21672a4fee3284aa63efb0428
  source_range:
    end: 202366947
    part: 1
    start: 185488274
  summary: 旧 diagnostics 文件经用户确认后删除，完整本地检查记录为 246 项通过、3 项平台相关跳过；该结果不等于所有平台或最终安装包均已验收。diagnostics、knowledge、remote-dev 及三端适配器后来分别出现合入和 CI 通过记录，但最终候选包、Kimi 经验生成、自动知识 PR 与新任务检索闭环仍需按实际证据确认。Kimi 先前的 600 秒中断来自验收控制器：业务约第 158 秒才开始，审批等待占用较多时间，最后的 torch_npu 导入审批未处理，不能归因于产品或远端工具失败。
  title: 发布前清理与验收边界
- block_id: a9bec6ef81c6c1ec399f31d0aa0073788dcbbc5d3fcf0c16fce108c1bca75de4
  indexed: true
  sha256: abd07fac5e59d97403c8b76adf23ebd314aafd3fc62e756c3d7324d08b97f6ae
  source_range:
    end: 202366947
    part: 2
    start: 185488274
  summary: 'Kimi K3/max 使用私仓固定提交和独立分支，以原生 `--auto` 持续开发 A3/A5 统一接口；社区贡献已开启，正式组件和三端适配器已合入，安装回读也有记录。初期 A3 构建因找不到 `cann_compat.h` 失败，原会话在主机重启后曾只完成上下文压缩、停在输入界面，随后发送 `continue` 才重新执行工具；因此此前“已恢复”的判断曾过早。最新记录称 Kimi 已于 23:07 将 A3 实现合入私仓 PR #4、会话空闲，但 A3 正确性、Graph、性能原始日志及知识自动 PR 尚待核对，不能把提交合入当作算子验收成功。'
  title: A3 RMSNorm+DynamicQuant任务进展
- block_id: 5b68f5f856d2040856812a5b7bb58efcf4db31de8952cd760ce56bde0e153fcf
  indexed: true
  sha256: f8760627f55368781448ad470a7b3044d61586acdca9336c7c4c3e4f0815ae5e
  source_range:
    end: 220625205
    part: 0
    start: 202366947
  summary: 审核确认共性根因包括 Kimi 仅读日志尾部 256 KiB 导致长轮次身份丢失、服务恢复入口缺失，以及 Stop 将投递失败静默吞掉；后续方案改为会话通知、共享游标去重、按需唤醒服务，并区分 accepted、unavailable、unknown 等状态。还发现分段读取、半条 JSONL、结果保存失败、授权 epoch、CAS 游标、裸 PID、apply-pending 活跃计数和超长输出等边界风险，均只有代码审查或隔离实验依据，未据此宣称产品成功。Kimi 已交付候选并通过 146 项组件检查，但真实闭环仍待共享核心最终修正和原生验收。
  title: 三端采集闭环的系统性修复审查
- block_id: 6ba335d55d86f2e7d2eb700fcc2e013b01a1f00230989acd6063ae03883ee3ad
  indexed: true
  sha256: da4eb536f282a773cc9ebc997fe003d13b5571fcc4c2305632f38a6aa285b7af
  source_range:
    end: 220625205
    part: 1
    start: 202366947
  summary: 修复设计要求业务不阻塞但后台失败可见，复用现有采集记录和模型执行记录，恢复时只续接失败阶段、不重复调用模型，并区分贡献关闭与配置暂时不可读。审核和实验确认可复用 attempt、游标、进程锁及结果记录，同时发现旧轮次误用、迟到事件重绑新授权、DB/Hook 故障吞掉、apply-pending 无活动计数等 P1 风险；多数材料仍是静态审核，未运行模型或 Hook。三端验收脚本和候选方案已准备，但候选安装、真实 Stop 和自动 PR 仍未在该阶段完成验证。
  title: 共享核心修复方案与验收准备
- block_id: fd26ca9634d3f9d0951ddf2f1d346880101aae11643713ff49bdac7af7dd6b13
  indexed: true
  sha256: 63478ecf9491e43716124077c725a9cb031bec9a73600d6c56dc29fdf4454cbe
  source_range:
    end: 220625205
    part: 2
    start: 202366947
  summary: 共享核心和 Kimi 回归分别通过 266 项和 146 项检查；Claude Code 实机会验证未启用不采集、query/Stop 按需恢复服务并完成整理，期间曾把验收脚本误认成服务，后已核对真实进程和采集记录。CC 的 OFF Stop 约 32/38 ms 且无新增采集、整理或唤醒，但 ON→OFF 转换及前六轮未重放，不能扩大为完整验收。Kimi 长会话真实 Stop 已落采集记录并唤醒退出的知识服务，证明采集和唤醒路径走通；随后发现生成草稿泄露私仓代码细节，已关闭该测试任务，outbox 和 PR 均为 0，因此脱敏与自动发布闭环仍未通过。
  title: 三端原生验收与脱敏失败
- block_id: 59ce872f7e5fcc61057a5b5486fbe68c48ebd9061ca5fe63a1df9fc7369ba892
  indexed: true
  sha256: f38db3d2774ccb34093b88fb04030fe0b61912b2574b3519767831f0035e71be
  source_range:
    end: 238531835
    part: 0
    start: 220625205
  summary: 共享核心、Kimi 和 Claude Code 的持久接收、单次唤醒、游标去重、错误状态记录及公开内容边界已有合入或实机证据；Kimi 真实任务已生成并合入知识 PR，新的 Claude Code 会话检索并使用了该经验，但回答有一处 dtype 顺序推断过强。Codex 原生 Stop 仍受宿主 Hook 为 `modified` 和信任确认限制，Windows 也未验收；自动 PR、脱敏、内容复用和服务恢复不能据此推广为全端闭环。静态审查又发现管理入口错代、同 SHA 失败缺少公开恢复出口、导出占位导致本地材料卡死，以及把 `batched_revision` 当作已上传基线而误判首次发布失败等问题；这些修改当时尚未实施。
  title: 三端真实发布与架构缺口审查
- block_id: 868d8bb497205950aea7307eb2978de9954160f5db1231fb46e89586e77389e9
  indexed: true
  sha256: e810f7ae7fcad53dac598324ef54f475466d72ba4826ee7b29943fb18302baae
  source_range:
    end: 238531835
    part: 1
    start: 220625205
  summary: '后续设计修订认为无需额外的撤销或决定不贡献流程，用户也不应维护内部 batch；开放投稿保持为本地脱敏后直接提交公共 PR，Bot 负责补充检查、必要修订和合入，未采用独立私有审核仓。当前已确认单篇正文、文件读取、打包、检索及整库容量存在旧门槛，长任务应通过增量处理和保存进度继续，而非截断或停止贡献；远端 Bot 脱敏后的正文应覆盖本地已发布副本，本地只保留未提交增量。网络、插件更新、同步和 PR 提交应由后台保存进度、退避并静默恢复，提交结果不确定时先查询远端，不能把恢复责任转给 CLI 或重跑模型；PR #208 仅合入了生命周期约定，产品代码仍在开发，尚未安装候选或完成实机验收。'
  title: 开放投稿下的容量、同步与静默恢复方案
- block_id: de9064c8449e6aa7fee6101671e752aa800c01fcc0f5f885ce74c0343c0c4f67
  indexed: true
  sha256: 7c61f6514cee76a81f7f963fb2e6dc48167d3c24ca692dc31b415c973767bfec
  source_range:
    end: 256832100
    part: 0
    start: 238531835
  summary: 围绕本地脱敏后直接提交公共 PR，持续修订长正文分页、规模限制、Bot 改稿后的增量、断网恢复、提交回执和 fork/公共仓同步；已保留失败尝试、未完成候选及本地合成数据边界。三端适配器曾通过组件检查并完成旧入口升级验证，但核心贡献闭环、自动恢复、Windows 和完整原生验收仍未全部证明；期间复现并修正或待修正了 HTTP 403、pip 网络故障、代理断连、响应预算、缓存失效、命令行长度及远端正文回退等问题。
  title: 三端开放投稿与长经验链路的开发审查
- block_id: 8599d7bf48853baefae69ad52637be9234fe80479971727c33258ac76d74b1fe
  indexed: true
  sha256: 6a08b6baa184bf33973d5958d012f1e5b6bce43271251dbd72f1bc5b120dbe3f
  source_range:
    end: 256832100
    part: 1
    start: 238531835
  summary: B 候选完成了三端知识响应预算调整，针对 Unicode 和转义数据实测最大响应约 817,407 bytes，预算设为 1 MiB；正文继续通过 next_offset 分页读取。12,050 条、约 48 MiB 的合成知识已在本地规模检查中同步成功，但正式依赖 pin 后的分页测试、公共投稿、Bot 改稿续写、网络恢复和新会话自然检索仍待验收；审核还发现 fork 落后、未知提交结果、PR 读取失败及远端删除内容回流等缺陷，修复尚在整合。
  title: B 候选分页预算与远端恢复验收准备
- block_id: 979246a10ccb5d50eb21211648304eae4ebc504a0b2a44d45a1f8c5716e65351
  indexed: true
  sha256: e28c28b18c95cb44b8574b29779ba4717d2fcfe71b864fd553c9e6330d4867f1
  source_range:
    end: 256832100
    part: 2
    start: 238531835
  summary: 只读核对真实 GitHub 接口发现，PR 列表接口使用 `merged_at` 表示合入，而详情接口才返回 `merged`；现有代码可能因此把已合入 PR 误判为拒绝。开发测试尚未覆盖这一 API 差异，当前正在修正，未新建测试 PR。
  title: GitHub PR 合入状态字段差异
- block_id: 66225a37c8322e3a29523bf63e3e3075a7cdca6ac2624147bf3607c0a80a4177
  indexed: true
  sha256: 362a4bf478e1012deffd6d41901a90a830aa3dbe0077798a4ef8aa70e2b975e9
  source_range:
    end: 273989826
    part: 0
    start: 256832100
  summary: 组件和部分 CI 检查多次通过，真实 RPC 将大库查询从约 12 秒降至约 17–42 毫秒，并验证了长正文分页和中断续接；但并发更新可能写回旧版本条件或错误删除重新可见条目，相关问题已复现，核心修复尚未合入。Windows 的长路径、UTF-8、Git 超时和 macOS 时序测试也曾失败后修正，FTS 能力探测发现过“输出 OK 但退出码为 1”却被误判成功的问题，现已加入真实子进程检查。原生业务闭环、自动恢复、Bot 更新、Windows 实机和语义正确性仍未完成，且出现了正式 A 版 MCP 进程的身份校验拒绝。
  title: 大库检索索引修复与跨平台验收仍受并发问题阻塞
- block_id: c03e059431c7ca1062e5dc334352e81f90af7912187259ce439c23337eaa374a
  indexed: true
  sha256: d2a34e40f42600543f234956b62557a1e15bab151fb8d8c157e4d5a019d3d833
  source_range:
    end: 273989826
    part: 1
    start: 256832100
  summary: Windows CI 曾达到 317 passed、29 skipped，macOS 相关时序夹具也完成修正；SQLite 3.54.0 的 FTS5 前提和六条 setup/update 探测路径已在准备环境中通过，但这只是能力前提，不等于产品功能验收。大库查询约 17–42 毫秒、316 KB 正文可分段读取，然而并发更新、撤下、草稿清理和远端正文优先规则仍需复验，核心尚未冻结合入。NPU 8 被其他 vLLM Worker 占用，因此只准备了只读 Infra 调查，未启动模型、Hook、贡献或远端写入；还记录了身份冲突/重复请求拒绝和 Bot 界面锁定等环境限制。
  title: FTS 运行时探测已修正，索引一致性与真实闭环仍待验收
- block_id: ad84915e78072cf9ecf8d7b59579fb862667c6401b4dc72428ec6746cf8e08bb
  indexed: true
  sha256: 06955d36f29b8a4bcc46844ae76662abe738223cea423aeb6bb3b02bfa59425f
  source_range:
    end: 273989826
    part: 2
    start: 256832100
  summary: 核心候选已推到现有 PR，Linux CI 已通过，macOS 和 Windows CI 尚在运行；并发一致性修复完成前仍不合入。身份拒绝已确认发生在正式 A 版 MCP 进程，而非旧进程残留，当前继续核对请求记录，不能把候选或 Linux 结果视为完整产品闭环成功。
  title: 候选已推送 PR，CI 与正式 MCP 身份拒绝仍在核对
- block_id: f7a2ca580badf45b4ec599b4f4615c93ab9a51bc48bc570c2bb8f66f0a1b310e
  indexed: true
  sha256: 3003555ef77e4fb16083e1465ebabbd73c70682e61a044b95f2f9f7f62579cbd
  source_range:
    end: 286537974
    part: 0
    start: 273989826
  summary: '已确认 Codex remote 去重键应加入连接标识，并修正线程 ID 与会话树 ID 被错误强制相等的问题；同连接重复请求仍拒绝，跨连接复用数字 request.id 可执行，但 MCP reload 尚未证明实际更换连接。核心、三个适配器和公共校验器已合入，三端断网更新恢复、大库 FTS5/SQLite 检索、父子会话隔离及 Kimi 长轮次采集已有真实通过记录；Windows 实机、Bot 更新和完整自然闭环仍未完成。真实贡献曾因整理器 120 秒超时导致第二段永久跳过，随后发现隔离调度器改写 HOME 造成 Git 凭据不可见，修正后同一份脱敏经验已提交知识 PR #19，但超时段有限恢复和新会话复用仍未收尾。'
  title: 核心与三端改动合入，真实恢复多项通过但闭环仍有缺口
- block_id: e72af50a88e1f8223f3ac6eaf5c9e6a2e6f50f5ddd0b088958a7a82507f392af
  indexed: true
  sha256: a04f96e89b6a5b694532cfc63403fc99855878dfd87e25ab2501814e43e909e8
  source_range:
    end: 286537974
    part: 1
    start: 273989826
  summary: '三端定时更新在下载失败后保留旧版、退避期间不重复下载，并由后续 OS 调度自动安装新版；但一次重连验收因参数嵌套格式错误被工具拒绝，不能据此判定重连成功。长轮次采集已覆盖超过 256 KiB 的旧漏捕获条件，部分整理成功并产生 PR #19，但第二段整理超时后造成经验缺失，现有根因诊断、有限重试和持久化恢复方案尚未完成。应以暂停后的稳定基线为准：架构文档、共享知识组件、三个适配器、公共校验器和 PR #19 已合入；一份有输入字节字段过滤、扫描边界及 Windows 适用性问题的日志补丁未合入，Bot、原生 Hook 验收和自然跨会话检索仍待后续。'
  title: 暂停验收并收敛基线，保留超时遗漏与未成熟补丁
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c87775a225f3f79ceeb45866cb35008599a040fd5acfccd685a8002974f02116
  kind: experience
  material_digest: ba36db8f3f820557f35fe1b720ff3ffd78b09bf09db60ad1915cb3fb580fd99d
  revision: 6bf66009110128df7e8c9bd7a62733a84bcf621fec45729a3978f55429fe870d
  schema: mindie-entry/3
  summary: '当前已从持续开发和验收转为暂停后的系统盘点：架构文档、共享知识组件、Codex/Kimi/CC 三端适配器、公共校验器及脱敏经验 PR #19 已合入；三端断网更新恢复、Codex 大库检索、父子身份隔离和 Kimi 长轮次采集已有真实通过证据。仍不可宣称完整闭环成功：整理器超时会使后续段落遗漏，有限恢复与诊断改进未完成；MCP reload 未证明连接更换，重连验收曾因错误参数提前失败，Bot 更新、Windows 实机、原生 Hook 和自然跨会话复用未收尾。未合入的日志补丁及其输入字节过滤、扫描边界和 Windows 适用性问题应继续保留并独立检索，后续优先确定超时失败恢复语义，再完成一次真实投稿与新会话读取闭环。'
  title: 稳定基线已合入，超时恢复与真实闭环仍待分阶段收口
navigation: '当前已从持续开发和验收转为暂停后的系统盘点：架构文档、共享知识组件、Codex/Kimi/CC 三端适配器、公共校验器及脱敏经验 PR #19 已合入；三端断网更新恢复、Codex 大库检索、父子身份隔离和 Kimi 长轮次采集已有真实通过证据。仍不可宣称完整闭环成功：整理器超时会使后续段落遗漏，有限恢复与诊断改进未完成；MCP reload 未证明连接更换，重连验收曾因错误参数提前失败，Bot 更新、Windows 实机、原生 Hook 和自然跨会话复用未收尾。未合入的日志补丁及其输入字节过滤、扫描边界和 Windows 适用性问题应继续保留并独立检索，后续优先确定超时失败恢复语义，再完成一次真实投稿与新会话读取闭环。'
schema: mindie-material-task/1
status: complete
task_id: c87775a225f3f79ceeb45866cb35008599a040fd5acfccd685a8002974f02116
---

# 稳定基线已合入，超时恢复与真实闭环仍待分阶段收口

当前已从持续开发和验收转为暂停后的系统盘点：架构文档、共享知识组件、Codex/Kimi/CC 三端适配器、公共校验器及脱敏经验 PR #19 已合入；三端断网更新恢复、Codex 大库检索、父子身份隔离和 Kimi 长轮次采集已有真实通过证据。仍不可宣称完整闭环成功：整理器超时会使后续段落遗漏，有限恢复与诊断改进未完成；MCP reload 未证明连接更换，重连验收曾因错误参数提前失败，Bot 更新、Windows 实机、原生 Hook 和自然跨会话复用未收尾。未合入的日志补丁及其输入字节过滤、扫描边界和 Windows 适用性问题应继续保留并独立检索，后续优先确定超时失败恢复语义，再完成一次真实投稿与新会话读取闭环。

## Materials

- [Hook 无限重试与按 session 激活设计](blocks/27685a782fe52d3439406e1887720d373b25ec42e41eef38faa26cf84dbdf727.md): 围绕 MindIE Agent 的 Stop hook 无限循环，记录了已删除脚本和旧插件缓存导致退出码 2、累计重复 110 轮的问题，以及超时、输入输出上限、去重、预算、并发和失败熔断等防护措施。后续设计明确要求 Plugin 仅在当前 session 手动调用后激活，MCP 与 hook 必须消费激活状态，不能因连接、SessionStart、Stop 或知识查询自动启动；当前实现仍需继续完成这一边界收紧。
- [会话隔离、自动更新与首版发布缺口](blocks/c4d16031e527f08adc1d496945ffb3aa1334ea733b69f1d1ecbf4dd1dc16187b.md): 已记录 session 激活凭据、MCP/hook 校验、跨 session 拒绝、超时清理、零自动重试和连续失败暂停等 macOS 本地实现，并报告 61 项测试及真实 gpt-5.6-luna/max 验收通过；Stop hook 仍需信任，远端 main 尚未包含安全修复。自动更新当前跟踪 main，支持失败回滚、忙碌延后和同提交最多 3 次尝试，但完整远端升级路径未验收；首版范围扩大为 macOS、Windows、Codex、vLLM-Ascend 单领域，Windows 实机和发布候选闭环仍是阻塞项。
- [知识闭环、旧 Skill 迁移与破坏式重构](blocks/b8210862789bc01d63c623e0c763a6ca747a5fa51b15f1b676508f5160c983c6.md): 发布前审查发现历史上有 18 个业务 skill、独立 repo-init 及共 25 个历史名称，旧脚本依赖工作区目录、共享 helper 和状态文件，当前插件仅有一个激活入口，迁移、触发条件和知识闭环效果尚未证明完整。计划采用破坏式重写，由 Kimi 负责大块核心与插件/领域重构，Grok 负责语料迁移和独立评测，删除过期死代码，并由主流程验收检索、采集、归并、分发、反馈、负向/unknown、多轮纠正及实际收益。当前 macOS 基线显示未查询知识时 Stop 虽触发但采集为 0；未激活 MCP 业务调用被拒绝、超时取消和输出超限进程已清理，重写候选及 Windows 实机仍待验证。
- [重构验收与 Profiling 暂缓](blocks/cd4e3a6569e38b476bd8f6b1d2a836b02d4adb2d5e643e75f4e3e7401b9de4c7.md): 代码审查和独立复验发现知识纠正、撤回同步、迁移崩溃恢复及 Hook 进程清理等部分已通过，但分发链路仍有旧结论残留、空版本导出缺口；远程执行封装仍把请求成功误判为任务完成，需修复后验收。真实 Ascend 数据已生成并下钻检查 HTML 报告，但层数未知、单 rank 均衡性和 host bound 等结论存在证据不足，旧项目入口与兼容层正在删除；profiling 分析 Skill 按用户要求暂缓，尚未按 GPT-6 指导完成审查。其余 Skill 已发现旧仓库入口、无效 YAML 和过长触发描述，正依据 GPT-6 指导及 skill-creator 规范收紧；Kimi 迁移任务已到硬截止并停止，Grok 的 MCP/远程执行修复仍在进行。
- [知识体系从重型闭环收敛为本地提炼与 GitHub 分发](blocks/9b5601348b9f008b3dddf018a2a4700b88b8736dbf06ec56faf2f90f9fb481d9.md): 知识组件曾完成部分本地检查和真实任务串联，但 NPU 不可用、工具契约错误、会话身份校验停止及裁判误归因等失败表明，不能把组件通过或权重上涨视为知识收益。经与 Fable 对齐，方案收敛为授权后本地读取任务过程、保留失败与更正、一次提炼详细经验和检索摘要，再通过 GitHub 分发；移除中心知识服务、运行时裁判、自动投票加权和采用率惩罚。经验正文追加观察而非反复压缩，点踩可触发修正或退役，多任务好评仅用于发现 Skill 候选；本轮仍是设计对齐，代码重构和真实验收未完成。
- [空库破坏式实现与社区贡献开关验收](blocks/631bb6e1b06133f0a7988aa14a9e24b915a14b918cf9fdc1d2d05ac0805ddbc5.md): 设计边界已明确：社区贡献关闭时不采集会话、不生成草稿、不启动整理或脱敏模型，插件更新、知识库同步和按需检索仍可用；重新开启不补采关闭期间材料。按空库进行破坏式重写后，曾发现 transcript 第二段续读卡住、误采 assistant analysis、整理器 `invalid_json_schema`、配置 `fork`/`bot` 不兼容、旧队列跨开关提交及已删除 `attach` 接口等问题；CI 已获 workflow 权限并合入，整理器真实调用也以一次 37 秒、无重试、结构化输出通过，但这只证明该组件可运行。未激活 MCP 拒绝且未创建租约已验证，自动 PR、Grok 审核、客户端同步、完整贡献链路及剩余修复仍未完成。
- [真实链路验收与长任务体量暴露](blocks/25cfa361c9394c5b4aa905c0df6d5ecae7285e20e2e3afae0543b9a04ba7bbc6.md): 真实空库、Hook、贡献开关和未激活 MCP 路径已分别验证，确认了零采集、零后台调用、拒绝未激活任务等边界；但完整知识 PR→Bot 合入→独立消费闭环、自动更新和真实事件送达仍未验收。长任务样本约 437 MiB，当前解析与整理仅覆盖极小部分，且遗漏 custom_tool_call；估计的 Markdown 体量只是容量推断，不能视为整理成功。随后任务转向修复长任务采集、经验整理、新会话效果和端到端流程。
- [长任务解析修复与经验效果实验](blocks/dd826c06bc03fd16209cce86d6534c96a5917892423b16d02db80f930ee24183.md): 已扫描约 3 GiB、591 份候选记录，修复了自定义工具调用遗漏、派生任务父历史重复、额度续接、纠正摘要更新和子进程清理等问题；仍保留超大记录跳过、字段截断及模型整理超时等不确定性。两次详细整理未在 60 秒内完成，远端 NPU 验收曾因 remote_bash 审批策略拒绝而未执行，不能算经验质量或算子验证通过。后续计划用同类任务做有经验／无经验对照，分别测量整理正确性、实际使用收益、token／工具调用和发布同步闭环；remote-dev 正在被改为所有 session 可按需使用的通用入口。
- [字段精简与未激活 MindIE 的远端验收](blocks/89394e051ea4f14ddbe3fff8f4389814b67e559381577d8a6f0c0ea47b8ab337.md): remote-dev 已发现并计划修正跨连接 MCP 请求编号复用误拒绝、损坏记录被当空记录等问题；未激活 MindIE 的原生任务已在逻辑设备 `0` 上完成 FP16/BF16 RMSNorm 运算，但社区贡献采集曾因后台服务启动失败而未产出经验，后续完整采集才生成约 4.4 KiB 的脱敏 Markdown 并创建知识 PR。字段方案逐步收敛为保留 `schema`、`entry_id`、`domain`、`kind`、`title`、`summary`，`conditions` 仅记录可核实的软件版本或源码 commit，其余实验参数放正文；`producers`、`sources`、`status`、`retirement_reason` 和公开 `revision` 可移除或由程序维护。核心链路仍有未完成或仅部分验证的内容，包括空库对照曾耗尽调用预算、GitHub 事件唤醒未确认，以及格式精简和真实任务受益尚需完整闭环验收。
- [v2 经验发布、撤回链路与 Grok Bot 职责收敛](blocks/52c44b71653e10781ce866a20999b33cd5c16b3bb86474cf6468b2b5cd3670bc.md): 格式重构的核心 178 项、adapter 79 项检查及 CI 通过，并修复标题改动产生重复文件、下库条目被旧草稿恢复、旧数据库不兼容和 `rms_norm` 检索无法命中 `torch_npu.npu_rms_norm` 等问题；真实远端验证确认正文更新、历史引用、删除退出检索和旧草稿阻断链路有效。两条独立经验已合入主分支，新 shape `[11,1024]` 和新 seed 的任务读取经验后完成 FP16/BF16 运算，退出码为 0，但这些结果不能证明普遍提效或节省 token。此前“每轮一次写入”“10 个候选、20 分钟”等限制均被确认是自行添加、没有业务依据并已撤掉；Grok Bot 的目标收敛为检查敏感信息、明显违规内容和格式后触发合入，同时按提交去重，反馈纠错和 Skill 提案另行维护。
- [架构简化、生命周期修复与 Kimi 分仓适配](blocks/f71e76585c1ce28eb26f5def7c34f0c0a8f2a6f1fe402d04a6881ecd2648a74b.md): 此前已修复多项过度限制并完成部分真实验收，但 24 小时租约与更新等待耦合、固定容量上限等问题仍待简化；后续架构审查又确认任务生命周期、首次配置、故障恢复、本地清理和组件边界需要补齐。当前计划是让首次安装或首次调用时配置贡献授权，提交确认后清理可重建本地材料，并将 Kimi/K3 适配放入独立仓库、旧 Skill 暂不迁移；Kimi 的真实探针显示 Hook 有原生任务 ID，但 MCP 和 Shell 缺少该 ID，不能依赖工作目录或最近 session 猜身份。实现仍有未完成或已复现问题，包括暂停后授权重置、调用参数未核对、清理残留完整正文、解析器动态加载报错、恢复仍受次数上限限制，以及时间戳单位不匹配；Kimi 入口、模型整理、贡献闭环和完整原生验收尚未证明通过。
- [Kimi 独立仓库与 VAWS 九原则验收缺口](blocks/a2c31116621f4087edc1c9d6d7cddafe591e2603a8fe574d1e86816f64bf8e6a.md): 独立仓库 `mindie-agent-kimi` 已创建，说明继承 VAWS 九条设计原则；独立依赖环境中通过了 26 项组件测试，但入口交互、真实命令配置返回、模型整理和完整可用性仍在修正，不能将组件测试视为端到端成功。复查发现首次调用配置流程、按已确认 PR 内容执行贡献清理与恢复，以及更新器对 unknown 历史 PR 的处理仍不符合要求：未确认记录应可查询但不应永久阻塞更新。后续需要用真实 Kimi/K3 流程验证经验整理器能否从已有 NPU 材料生成有用经验，并保留适用范围和未验证项。
- [三端适配器实机验收与安装边界收敛](blocks/7721f5bd82b1dfb0a507292e32edecd40e871da589e8b0a91f60ea25aa173ab6.md): Codex 已有真实跨版本接续证据；Kimi 曾因 MCP 启动命令错误导致实际加载数量为 0，修正后两个 MCP 实际加载并通过只读激活、检索、正文读取及贡献关闭检查，随后跨版本接续也通过，但自定义配置路径仍可能依赖启动终端环境变量，初装是否依赖源码 checkout 尚待确认。Claude Code 的 `mindie-agent-cc` 分仓已建立，子代理正在按九条原则修复真实安装、配置路径、自动更新器和 Hook 边界；开发检查不视为实机验收。当前三端统一架构和进度表已合入主仓，旧知识分支文档仍需更新，旧 Skill 与 profiling 暂缓。
- [三端合入与 P0/P1 缺口排查](blocks/b3b911ff869b0ee89a7bf53416f5e67668d7a0ef4d1b68cb620efd523b0e183e.md): 三端实现 PR 已合入 main，并完成安装故障回滚、远端 main 更新安装和部分原生回读；Windows、经验质量、贡献闭环、定时运行及知识源错误汇总等仍曾被列为后续事项。随后将重点调整为忠实记录实际过程，省略原文未提及内容，同时修正同步状态、定时入口、提交后续写 ID、下库和隐私扫描误判等问题。真实验收中曾出现 GitHub 404 导致 degraded、锁竞争 deferred、隐私扫描拦截、旧入口失败等情况；部分修复通过了专项检查或实机验证，但多处明确未提交、未推送或仍待完整闭环确认。
- [忠实整理、调度与贡献闭环验收](blocks/516d6841ca7e62ca4f33aacaa6253cd19020f92d89bc72fefb017d6eb8c6d5d4.md): 整理器规则改为只记录来源实际出现的过程、现象和结果，缺失内容直接省略，不补未知清单或经验教训；Codex、Kimi、Claude Code 曾输出符合该要求的记录。三端 macOS 调度首次执行均报告 runs=1、exit=0，且测试任务已清理；提交后保留条目标识、下库标记 withdrawn、隐私扫描语法误判和反馈 PR 等路径也有实际检查。真实贡献链路仍有未完成部分：Claude Code 自动创建了仅反馈的 PR #15，但 Grok Bot 是否自动审阅合入尚未确认；生产 Codex 新 Hook 已获信任，Kimi 仍在补做真实 NPU 输出直接比较。
- [进度文档更新与 NPU 补查受阻](blocks/647162d08bdfa960640306e0f6101298c0f43e8e0f285fb8f0c38850024f45f3.md): 实施进度草稿已纳入合入版本和真实验收，并区分实现、原生证据与完整闭环；该文档尚未提交，生产升级、Kimi 新检查、Bot 处理和 Windows 仍标为待完成。用户已确认解锁并信任新版 Codex Stop Hook，后续计划回读信任状态并继续 Grok Bot 验收。NPU 补查未执行新的算子检查，原因是 SSH 主机可达但专用验收容器已退出。
- [真实 NPU 经验发布与 Bot 事件链路](blocks/a47763f4ec0bacf92b2b919d6cd2a41049c297f4cdd3f8d742bfb4ec9ee24829.md): 早期验收确认生产 Codex Hook 可触发 Stop，且真实 Claude Code NPU 任务完成了执行前纠正、经验整理、脱敏和自动创建 PR #16；随后 PR #16 合入，定时同步使新任务读取并采用经验，新的 NPU 两 dtype、两种布局比较通过，但首稿需要人工纠正，因此不能称为全程无人干预。Kimi 曾在升级后出现 Stop 可送达但整理器退出码 2、stderr 丢失且原因未知；三端服务恢复修复、失败分类和进程组清理后来分别获得源码或受控实机证据，但 Kimi 完整 Stop 到发布链路仍未被证明。期间还保留了 Bot opened 事件需人工澄清临时只读要求才能继续执行、服务交接缺口、超时开发调用无补丁等失败和未完成结果。
- [服务恢复、真实闭环与取消清理验收](blocks/1c1241146603d68fd579b36e4c433db3008968a66ce13d5da54b5890d4d4d9da.md): PR #16 的 opened 事件最终确认可送达并由 Bot 审查合入，但执行曾受临时只读语义影响；新任务经正常 OS 定时同步读取主分支经验并完成 NPU 验证，实际执行前发生一次人工脚本纠正，新增采集和发布为零。三端更新恢复路径的本地、源码及真实客户端检查覆盖新旧进程切换、回滚、无授权不启动和中断状态保留，Codex 与 CC/Kimi 的 Stop 或取消后进程清理也有不同程度的实机证据；Kimi 整理器曾以退出码 2 失败，旧失败原因仍未知。后续暴露并复现了 Kimi、CC 子孙进程残留和状态入口隐藏后台失败的问题，诊断组件及 Kimi/CC 取消修复已审查或合入，但完整自然贡献、扩展维护能力和 Windows 实机仍未完成。
- [全局代码质量审查与可诊断性缺口](blocks/88606808f5c6990431efbed49961220fba8bb4571d7151b5d7afee937e8d93ea.md): 新一轮非 Windows 审查已拉取 10 个仓库，并发现 Codex 将损坏配置或更新锁竞争误报为“尚未配置”，Kimi/CC status 不展示后台整理失败，服务启动失败丢弃原始原因，且远端作业状态可能泄露内部授权字段。Kimi 的独立全局审查和新任务闭环验收已下发，但尚未给出完成结论；准备阶段的主控 MCP 身份冲突未重试，改用既有 SSH 路径管理专用容器。当前重点是让状态入口准确呈现故障、保留可操作错误类别和恢复/绕过路径，并评估架构的扩展性、维护性及 agent 快速发现和处置错误的能力。
- [三端诊断修复与验收边界](blocks/547579ec933c8d00e06480e523c7654ff62defed7eba088fab894aa50fd18e2f.md): 审查和多项本机验收显示，状态接口、错误分类、远端异常投影、取消清理、依赖 pin、配置读取和活跃日志保留已有修复或局部通过证据；Codex 的配置损坏诊断、远端非零作业和 Hook 信任状态也有实测记录。日志总量自动清理、Windows、Kimi Stop→PR 业务闭环及 Bot 无人提醒合入仍未完成或未验证，若干提交仍是本地候选，不能将安装、回归检查或开发目录验证提升为完整发布成功。
- [自动故障上报与日志系统方案](blocks/292c31cd66b3ed532e65df498a6607ba9458e56fad1babd6a12a0ed77cf7aa0b.md): 旧 VAWS 式自动上报被评估为可接入 Codex、Kimi 和 Claude Code：建议共用脱敏、去重、有限预算的 reporter，各适配器仅记录真实入口故障，并与知识贡献独立授权；参数错误、用户取消、业务命令非零和普通网络失败不应默认上报为工具缺陷。日志方案计划关联操作与阶段、保留诊断引用、保护活跃及未读分片、处理定时器持续追加并支持总量清理，但旧 reporter 的发布失败/对账次数上限和日志轮转保护仍需实现与实机验证；当前已有 Codex 接线和共享故障引用，尚不能算自动创建 Issue 成功。
- [Codex MCP 协议故障验收](blocks/b5e3a1b00dc41a586786a58d7177d1d2c0da29fa9110b7023fc7461cc3e2eeaa.md): 一次隔离的 Codex Luna/max 验收让自有 helper 返回损坏协议；记录称原生 MCP 只调用一次、生成本地诊断、未重试、未启用知识贡献或上传授权，退出后无残留进程。验收同时发现提示未明确区分本地 helper 协议失败与远端业务失败，随后补充了故障阶段和查询命令；该材料未提供自动 Issue 上报成功或三端完整闭环的证据。
- [三端诊断上报与固定版本交付进展](blocks/83083263e9ed1c78ab27882295eaa4660e9526430abc7a24b88f10c72dc5b5a1.md): 共享日志、独立 reporter、授权状态和有限处理预算已完成大量本地、CI 及原生验收；Codex、Claude Code、Kimi 均有一次性故障调用或上报链路证据，但 Kimi/Claude Code 的模型回答曾超出工具的 unconfirmed 证据，不能视为完全忠实。Windows 用户机器验收、生产自动上传和部分最终合入状态仍需区分；Kimi 曾因 OAuth `The provided authorization grant is invalid` 失败，重新登录后才完成调用。
- [旧版本升级刷新与 Codex Stop 原生验收](blocks/f17a76a9ae27f606ccbcf0e0f197199a755c0d23fe366bc470e703288534d249.md): Kimi 与 Claude Code 的旧打包器升级路径已发现并修复，覆盖新增 Hook/MCP 入口、独立包刷新、回滚、无效指针、manifest/version/skills 一致性及清理失败状态；Claude Code 的原生刷新和 reporting-status 只读调用通过，但模型仍把“未配置自动上报”误述为“没有采集日志”。信任 Codex Hook 后，Luna/max 的 Stop 实际只触发一次、57 毫秒完成、零工具调用和零重试；验收目录信任项已清理，但运行前配置快照不完整，配置字节未变不能算通过。
- [下一步工作与中断后的未完成修改](blocks/563eaf30c205309192964304342622f6cce0169a142a9132efc33e2f3789ea36.md): 审查提出优先补齐：三端原生 reporting-enable/disable 跨端闭环、取消调度和卸载的真实状态确认、reporter 随插件升级交接、普通用户安装维护文档，以及 Kimi 知识贡献业务链；旧 Grok CLI 清理和知识有效性扩展属于后续项。随后按用户要求计划使用 `grok-4.7 / xhigh` 与 `kimi-code/k3 / max` 并行推进，但 K3、Grok 代理因 usage limit/截止时间中断，留下未完成草稿，不能当作交付；Kimi 的一次真实 NPU 命令已结束但结果仍停在审批读取处。现有修改包括移除旧模型诊断状态、收敛调度取消和删除请求，但 Grok 对 5 个源码/测试文件的删除请求被自动审批拒绝，尚待人工确认与后续审核。
- [上报器交接修复与Kimi验收停滞](blocks/b55fd5ea2640315bcf380476c5be521b25393aafb4a5574d925248396e52cd7b.md): 记录显示三端调度取消、持久安装路径和部分上报器升级/回滚已完成实机验证，但升级失败与依赖冲突摘要曾被过度泛化，且 Windows、低版本冲突和最终候选包验收仍未完成。代码审核发现回滚服务描述、版本指针恢复、授权撤销竞态及跨层超时等问题；部分修复后来通过真实 macOS 验收，但不能据此推广到所有平台。Kimi 业务验收曾因 600 秒控制器截止和审批等待中断，后续原会话恢复并开始处理 A3 构建问题；知识经验和 PR 闭环一度仍未完成。
- [发布前清理与验收边界](blocks/b248f91660c9308586e93f247c67338e970d084fe5b2b101f167fc3258f0bfe9.md): 旧 diagnostics 文件经用户确认后删除，完整本地检查记录为 246 项通过、3 项平台相关跳过；该结果不等于所有平台或最终安装包均已验收。diagnostics、knowledge、remote-dev 及三端适配器后来分别出现合入和 CI 通过记录，但最终候选包、Kimi 经验生成、自动知识 PR 与新任务检索闭环仍需按实际证据确认。Kimi 先前的 600 秒中断来自验收控制器：业务约第 158 秒才开始，审批等待占用较多时间，最后的 torch_npu 导入审批未处理，不能归因于产品或远端工具失败。
- [A3 RMSNorm+DynamicQuant任务进展](blocks/a9bec6ef81c6c1ec399f31d0aa0073788dcbbc5d3fcf0c16fce108c1bca75de4.md): Kimi K3/max 使用私仓固定提交和独立分支，以原生 `--auto` 持续开发 A3/A5 统一接口；社区贡献已开启，正式组件和三端适配器已合入，安装回读也有记录。初期 A3 构建因找不到 `cann_compat.h` 失败，原会话在主机重启后曾只完成上下文压缩、停在输入界面，随后发送 `continue` 才重新执行工具；因此此前“已恢复”的判断曾过早。最新记录称 Kimi 已于 23:07 将 A3 实现合入私仓 PR #4、会话空闲，但 A3 正确性、Graph、性能原始日志及知识自动 PR 尚待核对，不能把提交合入当作算子验收成功。
- [三端采集闭环的系统性修复审查](blocks/5b68f5f856d2040856812a5b7bb58efcf4db31de8952cd760ce56bde0e153fcf.md): 审核确认共性根因包括 Kimi 仅读日志尾部 256 KiB 导致长轮次身份丢失、服务恢复入口缺失，以及 Stop 将投递失败静默吞掉；后续方案改为会话通知、共享游标去重、按需唤醒服务，并区分 accepted、unavailable、unknown 等状态。还发现分段读取、半条 JSONL、结果保存失败、授权 epoch、CAS 游标、裸 PID、apply-pending 活跃计数和超长输出等边界风险，均只有代码审查或隔离实验依据，未据此宣称产品成功。Kimi 已交付候选并通过 146 项组件检查，但真实闭环仍待共享核心最终修正和原生验收。
- [共享核心修复方案与验收准备](blocks/6ba335d55d86f2e7d2eb700fcc2e013b01a1f00230989acd6063ae03883ee3ad.md): 修复设计要求业务不阻塞但后台失败可见，复用现有采集记录和模型执行记录，恢复时只续接失败阶段、不重复调用模型，并区分贡献关闭与配置暂时不可读。审核和实验确认可复用 attempt、游标、进程锁及结果记录，同时发现旧轮次误用、迟到事件重绑新授权、DB/Hook 故障吞掉、apply-pending 无活动计数等 P1 风险；多数材料仍是静态审核，未运行模型或 Hook。三端验收脚本和候选方案已准备，但候选安装、真实 Stop 和自动 PR 仍未在该阶段完成验证。
- [三端原生验收与脱敏失败](blocks/fd26ca9634d3f9d0951ddf2f1d346880101aae11643713ff49bdac7af7dd6b13.md): 共享核心和 Kimi 回归分别通过 266 项和 146 项检查；Claude Code 实机会验证未启用不采集、query/Stop 按需恢复服务并完成整理，期间曾把验收脚本误认成服务，后已核对真实进程和采集记录。CC 的 OFF Stop 约 32/38 ms 且无新增采集、整理或唤醒，但 ON→OFF 转换及前六轮未重放，不能扩大为完整验收。Kimi 长会话真实 Stop 已落采集记录并唤醒退出的知识服务，证明采集和唤醒路径走通；随后发现生成草稿泄露私仓代码细节，已关闭该测试任务，outbox 和 PR 均为 0，因此脱敏与自动发布闭环仍未通过。
- [三端真实发布与架构缺口审查](blocks/59ce872f7e5fcc61057a5b5486fbe68c48ebd9061ca5fe63a1df9fc7369ba892.md): 共享核心、Kimi 和 Claude Code 的持久接收、单次唤醒、游标去重、错误状态记录及公开内容边界已有合入或实机证据；Kimi 真实任务已生成并合入知识 PR，新的 Claude Code 会话检索并使用了该经验，但回答有一处 dtype 顺序推断过强。Codex 原生 Stop 仍受宿主 Hook 为 `modified` 和信任确认限制，Windows 也未验收；自动 PR、脱敏、内容复用和服务恢复不能据此推广为全端闭环。静态审查又发现管理入口错代、同 SHA 失败缺少公开恢复出口、导出占位导致本地材料卡死，以及把 `batched_revision` 当作已上传基线而误判首次发布失败等问题；这些修改当时尚未实施。
- [开放投稿下的容量、同步与静默恢复方案](blocks/868d8bb497205950aea7307eb2978de9954160f5db1231fb46e89586e77389e9.md): 后续设计修订认为无需额外的撤销或决定不贡献流程，用户也不应维护内部 batch；开放投稿保持为本地脱敏后直接提交公共 PR，Bot 负责补充检查、必要修订和合入，未采用独立私有审核仓。当前已确认单篇正文、文件读取、打包、检索及整库容量存在旧门槛，长任务应通过增量处理和保存进度继续，而非截断或停止贡献；远端 Bot 脱敏后的正文应覆盖本地已发布副本，本地只保留未提交增量。网络、插件更新、同步和 PR 提交应由后台保存进度、退避并静默恢复，提交结果不确定时先查询远端，不能把恢复责任转给 CLI 或重跑模型；PR #208 仅合入了生命周期约定，产品代码仍在开发，尚未安装候选或完成实机验收。
- [三端开放投稿与长经验链路的开发审查](blocks/de9064c8449e6aa7fee6101671e752aa800c01fcc0f5f885ce74c0343c0c4f67.md): 围绕本地脱敏后直接提交公共 PR，持续修订长正文分页、规模限制、Bot 改稿后的增量、断网恢复、提交回执和 fork/公共仓同步；已保留失败尝试、未完成候选及本地合成数据边界。三端适配器曾通过组件检查并完成旧入口升级验证，但核心贡献闭环、自动恢复、Windows 和完整原生验收仍未全部证明；期间复现并修正或待修正了 HTTP 403、pip 网络故障、代理断连、响应预算、缓存失效、命令行长度及远端正文回退等问题。
- [B 候选分页预算与远端恢复验收准备](blocks/8599d7bf48853baefae69ad52637be9234fe80479971727c33258ac76d74b1fe.md): B 候选完成了三端知识响应预算调整，针对 Unicode 和转义数据实测最大响应约 817,407 bytes，预算设为 1 MiB；正文继续通过 next_offset 分页读取。12,050 条、约 48 MiB 的合成知识已在本地规模检查中同步成功，但正式依赖 pin 后的分页测试、公共投稿、Bot 改稿续写、网络恢复和新会话自然检索仍待验收；审核还发现 fork 落后、未知提交结果、PR 读取失败及远端删除内容回流等缺陷，修复尚在整合。
- [GitHub PR 合入状态字段差异](blocks/979246a10ccb5d50eb21211648304eae4ebc504a0b2a44d45a1f8c5716e65351.md): 只读核对真实 GitHub 接口发现，PR 列表接口使用 `merged_at` 表示合入，而详情接口才返回 `merged`；现有代码可能因此把已合入 PR 误判为拒绝。开发测试尚未覆盖这一 API 差异，当前正在修正，未新建测试 PR。
- [大库检索索引修复与跨平台验收仍受并发问题阻塞](blocks/66225a37c8322e3a29523bf63e3e3075a7cdca6ac2624147bf3607c0a80a4177.md): 组件和部分 CI 检查多次通过，真实 RPC 将大库查询从约 12 秒降至约 17–42 毫秒，并验证了长正文分页和中断续接；但并发更新可能写回旧版本条件或错误删除重新可见条目，相关问题已复现，核心修复尚未合入。Windows 的长路径、UTF-8、Git 超时和 macOS 时序测试也曾失败后修正，FTS 能力探测发现过“输出 OK 但退出码为 1”却被误判成功的问题，现已加入真实子进程检查。原生业务闭环、自动恢复、Bot 更新、Windows 实机和语义正确性仍未完成，且出现了正式 A 版 MCP 进程的身份校验拒绝。
- [FTS 运行时探测已修正，索引一致性与真实闭环仍待验收](blocks/c03e059431c7ca1062e5dc334352e81f90af7912187259ce439c23337eaa374a.md): Windows CI 曾达到 317 passed、29 skipped，macOS 相关时序夹具也完成修正；SQLite 3.54.0 的 FTS5 前提和六条 setup/update 探测路径已在准备环境中通过，但这只是能力前提，不等于产品功能验收。大库查询约 17–42 毫秒、316 KB 正文可分段读取，然而并发更新、撤下、草稿清理和远端正文优先规则仍需复验，核心尚未冻结合入。NPU 8 被其他 vLLM Worker 占用，因此只准备了只读 Infra 调查，未启动模型、Hook、贡献或远端写入；还记录了身份冲突/重复请求拒绝和 Bot 界面锁定等环境限制。
- [候选已推送 PR，CI 与正式 MCP 身份拒绝仍在核对](blocks/ad84915e78072cf9ecf8d7b59579fb862667c6401b4dc72428ec6746cf8e08bb.md): 核心候选已推到现有 PR，Linux CI 已通过，macOS 和 Windows CI 尚在运行；并发一致性修复完成前仍不合入。身份拒绝已确认发生在正式 A 版 MCP 进程，而非旧进程残留，当前继续核对请求记录，不能把候选或 Linux 结果视为完整产品闭环成功。
- [核心与三端改动合入，真实恢复多项通过但闭环仍有缺口](blocks/f7a2ca580badf45b4ec599b4f4615c93ab9a51bc48bc570c2bb8f66f0a1b310e.md): 已确认 Codex remote 去重键应加入连接标识，并修正线程 ID 与会话树 ID 被错误强制相等的问题；同连接重复请求仍拒绝，跨连接复用数字 request.id 可执行，但 MCP reload 尚未证明实际更换连接。核心、三个适配器和公共校验器已合入，三端断网更新恢复、大库 FTS5/SQLite 检索、父子会话隔离及 Kimi 长轮次采集已有真实通过记录；Windows 实机、Bot 更新和完整自然闭环仍未完成。真实贡献曾因整理器 120 秒超时导致第二段永久跳过，随后发现隔离调度器改写 HOME 造成 Git 凭据不可见，修正后同一份脱敏经验已提交知识 PR #19，但超时段有限恢复和新会话复用仍未收尾。
- [暂停验收并收敛基线，保留超时遗漏与未成熟补丁](blocks/e72af50a88e1f8223f3ac6eaf5c9e6a2e6f50f5ddd0b088958a7a82507f392af.md): 三端定时更新在下载失败后保留旧版、退避期间不重复下载，并由后续 OS 调度自动安装新版；但一次重连验收因参数嵌套格式错误被工具拒绝，不能据此判定重连成功。长轮次采集已覆盖超过 256 KiB 的旧漏捕获条件，部分整理成功并产生 PR #19，但第二段整理超时后造成经验缺失，现有根因诊断、有限重试和持久化恢复方案尚未完成。应以暂停后的稳定基线为准：架构文档、共享知识组件、三个适配器、公共校验器和 PR #19 已合入；一份有输入字节字段过滤、扫描边界及 Windows 适用性问题的日志补丁未合入，Bot、原生 Hook 验收和自然跨会话检索仍待后续。

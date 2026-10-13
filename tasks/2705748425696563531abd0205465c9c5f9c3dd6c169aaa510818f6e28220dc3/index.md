---
blocks:
- block_id: b16d8c460df500b1d762ae63208b438877e5f59ccb1fe6caec1c19fe62890e25
  indexed: true
  sha256: 6a532c3e4deafba38c3407cc639963f7fa3404fe6b7de763a21e35085f0e0747
  source_range:
    end: 16822460
    part: 0
    start: 0
  summary: '审查认为 PR #66 主要通过 session、worktree、租约和状态隔离改善当前 VAWS 仓库的受管多任务并行，但跨工作区调度、业务仓解耦、K3 四机生命周期和若干租约安全问题原本仍未解决。讨论逐步形成独立协调 MCP、Session/Environment/Run+Lease 分层、按运行申请 NPU、复用已准备容器与编译产物、每轮修改后重新启动服务的方向；常驻模型服务池被用户指出不适合开发场景，后续明确排除。实施过程中报告已修复误回收、空租约启动、源码快照同步、共享清单、构建输入校验等问题，并称 CI 通过，但完整 K3 四机和 Ascend 实机验收仍不能据此视为完成。'
  title: 'PR #66：以 session 为核心的并行与资源协调设计'
- block_id: e55641bd4ee698cdd3c77d6bcae1b9cdb8a3cdd5869fcbe8bb4a15d0d031a71f
  indexed: true
  sha256: 0cc5fa59426b02f45393c85162c8635c3f58f0c0acfd5bf3b66c66a338c26d7b
  source_range:
    end: 16822460
    part: 1
    start: 0
  summary: 监控缓存不能直接用于发卡：默认采样间隔、前端故障、服务重启和管理机休眠都会影响时效，因此资源分配必须由共享的租约权威实时确认。建议以独立 MCP 提供申请、排队、续租、释放及让卡协商，vaws-top 仅作展示；机器与 session 解耦为 Session、Environment、Run+Lease 三层，并通过 remote-dev 增量同步代码、按构建输入复用 CANN/torch/vLLM/VA/算子产物。环境池应让 Agent 领取现成容器后直接启动本次代码服务，仍需模型加载和验证；缓存不匹配、缺失元数据或原生依赖变化必须明确失败，不能悄悄使用旧产物。
  title: 独立协调 MCP 与就绪运行环境池
- block_id: c45a3f6b88477453cd2bd4f5144a2ebc0914b2d5ba2dd05695a108901f7977ba
  indexed: true
  sha256: 2ec14a721159556a63df5643ad342e83124c6a3f710da90f14a37a1ec49da940
  source_range:
    end: 16822460
    part: 2
    start: 0
  summary: 'PR #67 已合入 main 并被吸收到 #66：共享 Git common-dir 下的 machine-inventory 与 parity 入口得到统一，但共享清单不等于共享资源分配，独立管理根、客户端和跨 clone 的竞争仍需统一协调权威。随后在 K3 154 机器上报告完成五类客户端的 MCP 调用、排队交接、双卡隔离、代码同步后重启和产物复用测试，并修复 Docker PID 探测、子模块内容缓存失效及 npu-smi A3 进程误判等问题；153 按高优先级任务保持未动。最终报告称提交 20f536c、90 项针对性测试和 4 项 CI 通过，计划文档已移除，但完整 K3 模型、TP16 和四机验收仍未覆盖，当前任务随后转向梳理整体资源管理与生命周期架构。'
  title: '吸收 PR #67 后的实现与 154 实机验收'
- block_id: af4699c5c93b87387d253468b07d0930ad9cefeae395b0c37154029e60e2e542
  indexed: true
  sha256: eb8c2d8086741822d9039799c070a3d5a6c7ed349fb9f511197eee1cc1bbec89
  source_range:
    end: 30674938
    part: 0
    start: 16822460
  summary: 材料提出以 VAWS session 作为稳定任务身份，与多个 Agent 工具 session 建立 1:N 关系；worktree 负责代码隔离，远端 Machine/Runtime、Job/Service 和 NPU 租约按需关联。新工具 session 默认创建新的 VAWS session，resume 原工具 session 时沿用原 VAWS session，子 agent 需通过明确父上下文继承任务归属；用户不应直接感知底层 ID 和清理顺序。实现与实机验收状态多次修正：Grok 的嵌套 tool_input context_file 注入问题已有报告修复，Kimi 原生 MCP 仍是 fallback/transport gap，Cursor 与 Codex 分别受 trust/approval 或 hook review 阻断，三任务并行实机验收及完整自动回收仍未被这批材料证明。
  title: VAWS 任务、工具会话与远端资源生命周期设计
- block_id: 1fa8fd9de7f021382efe9fcb5106f880986c9f57a2b94f92300cada948d95d1d
  indexed: true
  sha256: 9802dfbf5c7f161f9a8e1692f3bd53ca1b8c36b259ce83e0479df97ae7ba29a7
  source_range:
    end: 30674938
    part: 1
    start: 16822460
  summary: 'Kimi Code 0.38.0 的 native 新会话与两次 resume 被报告为保持同一 task/context，但主会话和子 agent 没有 mcp__* 工具；通过 Bash import mcp.tools.call_tool 的 remote_bash、remote_write/read 和 SHA256 操作属于 fallback，不能算严格 client-side MCP pass。Kimi 首次远端 cwd 不存在时返回 REMOTE_DEV_CWD_NOT_FOUND，按允许的 /tmp recovery 后重试成功；配置已恢复，未改核心实现、依赖、服务或 NPU。Cursor Agent 2026.08.25-3e8eec8 返回 remote-dev: not loaded (needs approval)，真实 workspace trust 阻断，明确未使用 --trust、--approve-mcps、--yolo 或 -f；后续 knowledge_query 无 verified match，根因仍不确定，建议由用户在交互会话完成正常 trust/MCP approval 后复验。'
  title: Kimi 与 Cursor 客户端验收边界及 MCP 阻断
- block_id: 48d4b8ff434194027c517410fd95fe9308bfb050f1f569a813e90cd6aac1637a
  indexed: true
  sha256: cd0356dcb0f742309ad5dc651ba82ccee83e83d92926355cee6479349631c697
  source_range:
    end: 30674938
    part: 2
    start: 16822460
  summary: 153 上报告已准备一个未启动 NPU/kernel、未启动 vLLM 服务的独立 Runtime：容器 vaws-agent46945、运行目录 /opt/vaws-pr66-runtime-a、CPU 版 torch 与指定 vLLM/vllm-ascend/CANN/SOC 版本，并完成 prepare_runtime attest、smoke/import 和 SSH 46168 登录验证。记录包含建议 runtime_id pr66-153-runtime-a、服务端口 18868、独立 sshd 配置与 PID、环境和构建指纹、artifact/attestation 哈希及私有 runtime-summary.json；来源材料保留了路径、地址和密钥指纹的脱敏占位符。当前只证明环境准备和传输链路可用，未证明 NPU 服务启动、资源租约注册、三任务并行互不影响或完整生命周期回收；材料明确未改共享源码/依赖、未提交，也未使用默认 sshd/root/.ssh。
  title: 153 预备 Runtime 与独立 SSH 传输环境记录
- block_id: 2703af6a587ad299938c312692d687c2edaf3d08e3157d09e2da9defa26c7d96
  indexed: true
  sha256: 6ca60ffa3400f5271506349c9681178b78a8eee186a13c8895880b941a4a3f40
  source_range:
    end: 30674938
    part: 3
    start: 16822460
  summary: A/B/C 均曾通过 production facade 运行真实 NPU probe，返回 HTTP 200、sum=16777216.0 和一致 checksum；A 停止后以 v2 重启，B/C 的 PID、快照和结果据报保持不变。期间发现 `setsid + env -u VAWS_REMOTE_JOB_TOKEN` 可能使后代逃出 marker/PGID 跟踪并导致错误释放风险；该风险随后促成 subreaper 跟踪与 lease 保留设计。五客户端原生验收仍不完整：Claude/Grok 有限路径通过，Kimi 使用 Bash fallback，Cursor 受 trust/approval 阻塞，Codex hook review 未完成。
  title: 三任务 NPU 生命周期与清理漏洞
- block_id: 859f602f0786faab70d4ab02e7161897ff06e7b71983b0ea493408afb69f85b4
  indexed: true
  sha256: 449fccc84453c129743b89c3a8cb1b19bb21ac2653513dc860e23df55a3c849f
  source_range:
    end: 30674938
    part: 4
    start: 16822460
  summary: A2、B、C 均据报通过受控 finish 回收，lease released、runtime_returned=true、remote processes 为空，C 的退出码为 -15，且 worktree 保留。C 随后恢复独立 46168 SSH listener，严格 host-key 检查连接成功；默认 SSHD 保持运行，未重复 NPU 测试。文档修订明确区分 native lifecycle、resource-layer contract 和显式 public adapter，并保留 Kimi/Cursor/Codex 的未通过边界；candidate-2 归档不能复用最新源码。
  title: 任务回收、SSH 恢复与验收边界修订
- block_id: c30057660e91d6ac100fe0aa57cd5870441655c0bcd29e20f865e6a328148766
  indexed: true
  sha256: ee76f54874704c46357282ea939b1e03da611927456b489832562d0f0866640c
  source_range:
    end: 30674938
    part: 5
    start: 16822460
  summary: 曾有 review 误判 `activate()` 会接受已消失 supervisor；后续更正为现有 `process_guard_busy` 条件会正确拒绝 activation，未修改实现。candidate-3/4 CPU 回归据报分别为 185 passed，普通 `nobody` 用户的 managed_jobs 测试为 6 passed；A/B/C 又完成 subreaper 监督下的真实 NPU 请求，setsid/脱离原 PGID 的 A daemon 仍被跟踪并可回收。A、B、C 最终均通过受控停止或 finish 释放资源，保留了初次启动拒绝、未知状态和清理过程等不确定性。
  title: supervisor 竞态更正与最终监督复测
- block_id: c9ca0360fbc0f2141d9fc5a81efd26593b375fa3edcbcbd35aec44bc2ea0f9ff
  indexed: true
  sha256: 9009b6d72787bf7102e656e6474edc18eb5e37b74f5ea018446d5aeaaa37540e
  source_range:
    end: 30674938
    part: 6
    start: 16822460
  summary: 'C 的二次 finish 最终落为 finished；仅终止并清理本轮生成的 46168 listener、authorized_keys 和 host 私钥，默认 SSHD、容器、runtime、源码、产物、日志及本机 known_hosts 保留。PR #66 据报已推送提交 `c56de3c`，185 项远端回归、6 项普通用户测试和四项 CI 被报告通过，8 个执行释放租约且临时 SSH/MCP 接入回收。PR 明确说明 Claude/Grok 有限原生路径通过，但 Kimi 原生 MCP、Codex hooks、Cursor Agent 仍未完成，不能视为完整 K3 模型验收。'
  title: 最终清理、CI 与 PR 更新
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2705748425696563531abd0205465c9c5f9c3dd6c169aaa510818f6e28220dc3
  kind: experience
  material_digest: 23c9d8cded21b9a03ade98d3217f6274fb8677a8ff9801c5c5757ab43e3a8c4c
  revision: eec35d89e2ca2f7dba5e2e4cde233a5ef7b3ffac0a49713aab1de3a1c35c72c6
  schema: mindie-entry/3
  summary: '当前记录显示，VAWS 任务身份、工具 session、worktree、runtime/job/service 与 NPU lease 需要分别追踪；resume 应复用既有 session，child 需显式父上下文。153/154 的 A/B/C 显式 public-adapter NPU substrate probe 曾返回一致计算结果，随后完成 subreaper、setsid/清除环境变量后代跟踪、停止/finish、runtime 归还与临时 SSH 清理；185 项 CPU 回归、普通用户测试和四项 CI 均为报告结果，仍应保留证据路径与“报告而非独立核验”的属性。客户端边界必须持续区分：Claude/Grok 有限 native lifecycle 证据；Kimi native MCP 未证实且使用 Bash fallback；Cursor 受 trust/MCP approval 阻塞；Codex hooks 仍需 review；显式 adapter 不等同自动 hook 或 K3 模型正确性。后续检索应优先核对 supervisor 丢失/unknown、lease 保留与人工 reconciliation，以及 PR #66 提交后的 CI、清理 receipt 和未完成客户端验收；不要把准备、设计规范、报告性检查或单次 HTTP/NPU probe升级为全面成功。'
  title: 'PR #66 VAWS 生命周期、监督回收与客户端验收边界'
navigation: '当前记录显示，VAWS 任务身份、工具 session、worktree、runtime/job/service 与 NPU lease 需要分别追踪；resume 应复用既有 session，child 需显式父上下文。153/154 的 A/B/C 显式 public-adapter NPU substrate probe 曾返回一致计算结果，随后完成 subreaper、setsid/清除环境变量后代跟踪、停止/finish、runtime 归还与临时 SSH 清理；185 项 CPU 回归、普通用户测试和四项 CI 均为报告结果，仍应保留证据路径与“报告而非独立核验”的属性。客户端边界必须持续区分：Claude/Grok 有限 native lifecycle 证据；Kimi native MCP 未证实且使用 Bash fallback；Cursor 受 trust/MCP approval 阻塞；Codex hooks 仍需 review；显式 adapter 不等同自动 hook 或 K3 模型正确性。后续检索应优先核对 supervisor 丢失/unknown、lease 保留与人工 reconciliation，以及 PR #66 提交后的 CI、清理 receipt 和未完成客户端验收；不要把准备、设计规范、报告性检查或单次 HTTP/NPU probe升级为全面成功。'
schema: mindie-material-task/1
status: complete
task_id: 2705748425696563531abd0205465c9c5f9c3dd6c169aaa510818f6e28220dc3
---

# PR #66 VAWS 生命周期、监督回收与客户端验收边界

当前记录显示，VAWS 任务身份、工具 session、worktree、runtime/job/service 与 NPU lease 需要分别追踪；resume 应复用既有 session，child 需显式父上下文。153/154 的 A/B/C 显式 public-adapter NPU substrate probe 曾返回一致计算结果，随后完成 subreaper、setsid/清除环境变量后代跟踪、停止/finish、runtime 归还与临时 SSH 清理；185 项 CPU 回归、普通用户测试和四项 CI 均为报告结果，仍应保留证据路径与“报告而非独立核验”的属性。客户端边界必须持续区分：Claude/Grok 有限 native lifecycle 证据；Kimi native MCP 未证实且使用 Bash fallback；Cursor 受 trust/MCP approval 阻塞；Codex hooks 仍需 review；显式 adapter 不等同自动 hook 或 K3 模型正确性。后续检索应优先核对 supervisor 丢失/unknown、lease 保留与人工 reconciliation，以及 PR #66 提交后的 CI、清理 receipt 和未完成客户端验收；不要把准备、设计规范、报告性检查或单次 HTTP/NPU probe升级为全面成功。

## Materials

- [PR #66：以 session 为核心的并行与资源协调设计](blocks/b16d8c460df500b1d762ae63208b438877e5f59ccb1fe6caec1c19fe62890e25.md): 审查认为 PR #66 主要通过 session、worktree、租约和状态隔离改善当前 VAWS 仓库的受管多任务并行，但跨工作区调度、业务仓解耦、K3 四机生命周期和若干租约安全问题原本仍未解决。讨论逐步形成独立协调 MCP、Session/Environment/Run+Lease 分层、按运行申请 NPU、复用已准备容器与编译产物、每轮修改后重新启动服务的方向；常驻模型服务池被用户指出不适合开发场景，后续明确排除。实施过程中报告已修复误回收、空租约启动、源码快照同步、共享清单、构建输入校验等问题，并称 CI 通过，但完整 K3 四机和 Ascend 实机验收仍不能据此视为完成。
- [独立协调 MCP 与就绪运行环境池](blocks/e55641bd4ee698cdd3c77d6bcae1b9cdb8a3cdd5869fcbe8bb4a15d0d031a71f.md): 监控缓存不能直接用于发卡：默认采样间隔、前端故障、服务重启和管理机休眠都会影响时效，因此资源分配必须由共享的租约权威实时确认。建议以独立 MCP 提供申请、排队、续租、释放及让卡协商，vaws-top 仅作展示；机器与 session 解耦为 Session、Environment、Run+Lease 三层，并通过 remote-dev 增量同步代码、按构建输入复用 CANN/torch/vLLM/VA/算子产物。环境池应让 Agent 领取现成容器后直接启动本次代码服务，仍需模型加载和验证；缓存不匹配、缺失元数据或原生依赖变化必须明确失败，不能悄悄使用旧产物。
- [吸收 PR #67 后的实现与 154 实机验收](blocks/c45a3f6b88477453cd2bd4f5144a2ebc0914b2d5ba2dd05695a108901f7977ba.md): PR #67 已合入 main 并被吸收到 #66：共享 Git common-dir 下的 machine-inventory 与 parity 入口得到统一，但共享清单不等于共享资源分配，独立管理根、客户端和跨 clone 的竞争仍需统一协调权威。随后在 K3 154 机器上报告完成五类客户端的 MCP 调用、排队交接、双卡隔离、代码同步后重启和产物复用测试，并修复 Docker PID 探测、子模块内容缓存失效及 npu-smi A3 进程误判等问题；153 按高优先级任务保持未动。最终报告称提交 20f536c、90 项针对性测试和 4 项 CI 通过，计划文档已移除，但完整 K3 模型、TP16 和四机验收仍未覆盖，当前任务随后转向梳理整体资源管理与生命周期架构。
- [VAWS 任务、工具会话与远端资源生命周期设计](blocks/af4699c5c93b87387d253468b07d0930ad9cefeae395b0c37154029e60e2e542.md): 材料提出以 VAWS session 作为稳定任务身份，与多个 Agent 工具 session 建立 1:N 关系；worktree 负责代码隔离，远端 Machine/Runtime、Job/Service 和 NPU 租约按需关联。新工具 session 默认创建新的 VAWS session，resume 原工具 session 时沿用原 VAWS session，子 agent 需通过明确父上下文继承任务归属；用户不应直接感知底层 ID 和清理顺序。实现与实机验收状态多次修正：Grok 的嵌套 tool_input context_file 注入问题已有报告修复，Kimi 原生 MCP 仍是 fallback/transport gap，Cursor 与 Codex 分别受 trust/approval 或 hook review 阻断，三任务并行实机验收及完整自动回收仍未被这批材料证明。
- [Kimi 与 Cursor 客户端验收边界及 MCP 阻断](blocks/1fa8fd9de7f021382efe9fcb5106f880986c9f57a2b94f92300cada948d95d1d.md): Kimi Code 0.38.0 的 native 新会话与两次 resume 被报告为保持同一 task/context，但主会话和子 agent 没有 mcp__* 工具；通过 Bash import mcp.tools.call_tool 的 remote_bash、remote_write/read 和 SHA256 操作属于 fallback，不能算严格 client-side MCP pass。Kimi 首次远端 cwd 不存在时返回 REMOTE_DEV_CWD_NOT_FOUND，按允许的 /tmp recovery 后重试成功；配置已恢复，未改核心实现、依赖、服务或 NPU。Cursor Agent 2026.08.25-3e8eec8 返回 remote-dev: not loaded (needs approval)，真实 workspace trust 阻断，明确未使用 --trust、--approve-mcps、--yolo 或 -f；后续 knowledge_query 无 verified match，根因仍不确定，建议由用户在交互会话完成正常 trust/MCP approval 后复验。
- [153 预备 Runtime 与独立 SSH 传输环境记录](blocks/48d4b8ff434194027c517410fd95fe9308bfb050f1f569a813e90cd6aac1637a.md): 153 上报告已准备一个未启动 NPU/kernel、未启动 vLLM 服务的独立 Runtime：容器 vaws-agent46945、运行目录 /opt/vaws-pr66-runtime-a、CPU 版 torch 与指定 vLLM/vllm-ascend/CANN/SOC 版本，并完成 prepare_runtime attest、smoke/import 和 SSH 46168 登录验证。记录包含建议 runtime_id pr66-153-runtime-a、服务端口 18868、独立 sshd 配置与 PID、环境和构建指纹、artifact/attestation 哈希及私有 runtime-summary.json；来源材料保留了路径、地址和密钥指纹的脱敏占位符。当前只证明环境准备和传输链路可用，未证明 NPU 服务启动、资源租约注册、三任务并行互不影响或完整生命周期回收；材料明确未改共享源码/依赖、未提交，也未使用默认 sshd/root/.ssh。
- [三任务 NPU 生命周期与清理漏洞](blocks/2703af6a587ad299938c312692d687c2edaf3d08e3157d09e2da9defa26c7d96.md): A/B/C 均曾通过 production facade 运行真实 NPU probe，返回 HTTP 200、sum=16777216.0 和一致 checksum；A 停止后以 v2 重启，B/C 的 PID、快照和结果据报保持不变。期间发现 `setsid + env -u VAWS_REMOTE_JOB_TOKEN` 可能使后代逃出 marker/PGID 跟踪并导致错误释放风险；该风险随后促成 subreaper 跟踪与 lease 保留设计。五客户端原生验收仍不完整：Claude/Grok 有限路径通过，Kimi 使用 Bash fallback，Cursor 受 trust/approval 阻塞，Codex hook review 未完成。
- [任务回收、SSH 恢复与验收边界修订](blocks/859f602f0786faab70d4ab02e7161897ff06e7b71983b0ea493408afb69f85b4.md): A2、B、C 均据报通过受控 finish 回收，lease released、runtime_returned=true、remote processes 为空，C 的退出码为 -15，且 worktree 保留。C 随后恢复独立 46168 SSH listener，严格 host-key 检查连接成功；默认 SSHD 保持运行，未重复 NPU 测试。文档修订明确区分 native lifecycle、resource-layer contract 和显式 public adapter，并保留 Kimi/Cursor/Codex 的未通过边界；candidate-2 归档不能复用最新源码。
- [supervisor 竞态更正与最终监督复测](blocks/c30057660e91d6ac100fe0aa57cd5870441655c0bcd29e20f865e6a328148766.md): 曾有 review 误判 `activate()` 会接受已消失 supervisor；后续更正为现有 `process_guard_busy` 条件会正确拒绝 activation，未修改实现。candidate-3/4 CPU 回归据报分别为 185 passed，普通 `nobody` 用户的 managed_jobs 测试为 6 passed；A/B/C 又完成 subreaper 监督下的真实 NPU 请求，setsid/脱离原 PGID 的 A daemon 仍被跟踪并可回收。A、B、C 最终均通过受控停止或 finish 释放资源，保留了初次启动拒绝、未知状态和清理过程等不确定性。
- [最终清理、CI 与 PR 更新](blocks/c9ca0360fbc0f2141d9fc5a81efd26593b375fa3edcbcbd35aec44bc2ea0f9ff.md): C 的二次 finish 最终落为 finished；仅终止并清理本轮生成的 46168 listener、authorized_keys 和 host 私钥，默认 SSHD、容器、runtime、源码、产物、日志及本机 known_hosts 保留。PR #66 据报已推送提交 `c56de3c`，185 项远端回归、6 项普通用户测试和四项 CI 被报告通过，8 个执行释放租约且临时 SSH/MCP 接入回收。PR 明确说明 Claude/Grok 有限原生路径通过，但 Kimi 原生 MCP、Codex hooks、Cursor Agent 仍未完成，不能视为完整 K3 模型验收。

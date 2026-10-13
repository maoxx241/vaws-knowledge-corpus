---
blocks:
- block_id: 853abafe8009d20df94faf841feacb68f4a348d97a9bde1f8292776003dc913c
  indexed: true
  sha256: ea9c3eb170bfe731a425f90babb9ee60bfcc6c18e32c2db4a7458c35892101ab
  source_range:
    end: 16789558
    part: 0
    start: 0
  summary: 用户要求并行推进前两个修复，单独建立 Kimi 与 Claude Code（mindie-agent-cc）适配边界，并继承旧 vaws 的九条设计原则；Kimi 原生验收允许使用 K3，所有测试须在实机完成。记录显示 Claude Code 已完成隔离安装、远端调用、任务/fork 授权隔离、升级回滚、默认审批及 macOS 定时触发验收，但贡献采集整理、PR 闭环、领域经验质量、Windows 和生产升级仍未完整验收；Kimi 曾因旧启动文件不识别新配置参数导致调度失败，修正入口后首轮执行成功。知识库下库、withdrawn 精确引用和多端调度已有实测证据，但占位源同步失败、OAuth-only 未验收、Bot 审阅合入及反馈闭环等限制仍需保留，不能将报告中的成功扩大为完整闭环。
  title: 多端插件适配与原生验收状态
- block_id: 69406b46ed0638ba6d9020129861de625a55738dbba739f95339d1a2eec92fdc
  indexed: true
  sha256: 175b863fb91093254db814e6055286d1abb724bf8b250b9a2e62ee48b4f339e9
  source_range:
    end: 16789558
    part: 1
    start: 0
  summary: 当前任务要求只读核对授权、冻结、停服和启动之间的锁顺序。重点风险包括更新或回滚后恢复服务发生死锁、误认旧进程，以及恢复流程意外扩大授权范围；该块尚未提供检查结果或验证结论。
  title: 授权与服务生命周期锁顺序检查
- block_id: 72fc381cb84a3c27e54648675313a6d1059278f695fe6397dda5b9886570e177
  indexed: true
  sha256: 3d15c3a2c755a1a1b535fdb232751516a79dbd0ce94467a44b06f1eb3ff751e2
  source_range:
    end: 34427135
    part: 0
    start: 16789558
  summary: 源码审核与真实本机验收覆盖了更新停服/恢复、Kimi 与 Claude Code 进程组取消、远端错误输出和状态分类；曾发现异步停服、旧 helper、恢复配置、进程残留、授权字段泄露及 MCP 取消阻塞等风险，部分已修复或实测通过，部分仍未完成。真实 CC 升级、服务恢复、Stop 整理器执行及共享远端输出窄修已有证据，但整理器命令行采样缺失、MCP 取消接线和若干发布/rebase 工作仍不能视为完成。受控进程实验确认适配器独立进程组会留下内部子进程，Claude Code 最小修复验证通过；后续整理器退出码分类、状态入口和 NPU 显式开关继续按不泄露敏感信息、不新增重试原则验收。
  title: 服务交接、进程组取消与远端错误边界
- block_id: 923f63657921780c5db0abfe24c026085bafd7a15a578e6a3c2fb6f039ab125d
  indexed: true
  sha256: 10dbd9dc392e8ed432277700334af040ff1a7277d07621e406f87e2b6926d133
  source_range:
    end: 34427135
    part: 1
    start: 16789558
  summary: 受控进程验证确认整理器五类失败可被 core 准确分类并记录耗时，成功结果正常返回，且未泄露 stderr/敏感内容或遗留子进程；共享状态 API、MCP 取消等并行事项仍明确未完成。Codex 状态入口、远端异常安全分类与任务范围隔离已通过离线/真实 CLI 检查，136 项集成检查和编译检查通过；安装器已锁定官方仓库精确 SHA，旧解释器会在写配置前拒绝，但这些结果不等于完整发布闭环。当前新工作转向 Kimi/Claude Code 默认关闭、用户显式启用的故障上报薄接入，同时需收敛定时器无限追加日志和 Kimi 原始网页输出，并保留正在进行的本地实现调用及未验收状态。
  title: 状态诊断、依赖锁定与故障上报接入
- block_id: 62d2b37957962825d7b11482714f9f62ac0f1c905e20cae91db344c73bc1736b
  indexed: true
  sha256: f2b02d8092e6f533de1a5b7b54143fc7b9fd81cc31030d74dc807e7c303b1b76
  source_range:
    end: 51520969
    part: 0
    start: 34427135
  summary: '完成了旧打包器升级兼容和原生包刷新：隔离 Claude 环境中出现三个 reporting 命令，两个 MCP 连接成功；最终 `/mindie-agent:reporting-status` 经 UserPromptExpansion 实际送达，2.53 秒返回“未配置上报”，无工具调用、授权或上传。过程中保留了验收脚本缺少 MCP SDK、旧包缺少 reporting matcher、同版本缓存不会刷新等失败和限制；PR #7 已更新推送，仍等待 root 合入。'
  title: 原生包刷新与 reporting 命令验收
- block_id: 5c538179e6d94b97674580467df3dd91704f5017c8e40df77d672b903afea8bc
  indexed: true
  sha256: c755f7c7eb42799f5054bf79a2d2a5d19e3af87651f49cd82a1b8d9a06e906b6
  source_range:
    end: 51520969
    part: 1
    start: 34427135
  summary: 最终 HEAD `e9259e09941835cea002c24e95c1318231c0d5c4` 的 88 项组件检查和 CI 通过，原生更新刷新九个命令，唯一 DSV4 `reporting-status` 调用用时 2.526 秒且零重试；配置已恢复、进程已清理。后续只读审查发现原生 `reporting-enable` 的完整跨端接线证据不足，Kimi/Claude 取消调度可能吞掉 `launchctl bootout` 失败并误报 `unscheduled`，普通用户持久安装说明也需补齐。清理旧 Grok/Bot 授权入口的实施曾因自动审批拒绝删除五个旧源码和测试文件而未完成，该拒绝及文件范围已保留，不能视为清理成功。
  title: 三端卸载边界与安装文档缺口审查
- block_id: 58512a0326c53e1825a9420ca408d324b7c2eef028d1b15b58c8346c7d2d9bbe
  indexed: true
  sha256: 9733d9eeb8788ce5aad379099da8048638e1da0d1342fe63db7abfd632b919a5
  source_range:
    end: 68893448
    part: 0
    start: 51520969
  summary: 记录了旧文件删除被拒后由工具自行换命令、五个文件恢复，以及文档任务因旧提示路径和超时未交付；新版上报队列保留已发布 Issue 回执和“写入结果未知”记录。静态审核发现三项回滚/授权缺陷：新服务描述可能阻断旧进程回滚、记录失败后版本指针可能未恢复、撤销授权与启动之间存在竞态；这些仅为发现的问题，未改文件。Kimi 持久安装、卸载和三端官方依赖检查有实机或 CI 证据，但 Claude Code 验收曾因观察脚本计入自身进程而失败，后续修正后仅验证关闭状态，原生 ON→OFF 仍未验；DSV4 单样本整理保留公开 API 与数值但遗漏外层 python3 行，不能推出保密保证。跨宿主检索复用找到了 Kimi 经验并正确引用多数参数，但将 dtype 顺序表述过强，该局限被保留。
  title: 多端安装发布与回滚验收记录
- block_id: b6091750f20677bbf368f0256ea95e82d8934c2083c62c207191b39ade4b1e21
  indexed: true
  sha256: 4d818adec30493c90069f14f47cb4ae35b3410bbac5432834e50fe0d932cf155
  source_range:
    end: 68893448
    part: 1
    start: 51520969
  summary: 验收准备强调区分组件故障注入与真实宿主证据，并指出日志超过 64 KiB 不等于累计经验正文超过 64 KiB，也不能用填充文本制造公共经验；另提醒仍需核对 10,000 条检索和 128 KiB 文件门槛。已实现并验证仅作用于指定隔离进程的 HTTPS CONNECT 网络开关：断连后恢复，公开 GitHub 请求正常；全局代理未修改。Claude Code 两阶段升级驱动仍在补齐，候选尚未执行模型验收，投稿方式拟采用本地脱敏后直接公共 PR，敏感测试内容不应进入公共库。
  title: 隔离网络开关与 CC 验收准备
- block_id: 1be2487217c2f52b4b6883e15088daaca604e4b5ae62f076832f655f8f1de63b
  indexed: true
  sha256: b2181d9dff47619dec5105277b8cfe0bf49603611be0e6818c12ca308f7e9c12
  source_range:
    end: 83794781
    part: 0
    start: 68893448
  summary: 网络探针区分了真实 `ls-remote` 解析成功与 `fetch` 被阻断，放通后同一 SHA 下载成功；Kimi、CC 的驱动完成语法或静态检查，但尚未调用模型、注册调度或执行 B 验收。审查发现公共 CI 仍固定校验器 `40063ebf…`，存在 128 KiB、1,024 文件和 32 MiB 总量限制，流式读取还需防跨块脱敏遗漏；三端本机 SQLite 3.54.0 已启用 FTS5，但安装 probe 尚未覆盖其他 Python 构建。Kimi 与 CC 已从旧版隔离升级至 A 并暂停调度；Codex 大库、MCP reload、父子模型观测和网络恢复驱动均以默认不执行或待正式 B/core 为当前状态，NPU 8 被 `/qjj_main_k3_mrv2` 的 Worker 占用，未创建容器。
  title: 多端升级准备、校验器约束与验收驱动
- block_id: 06c7f9d2cfcd0dcb920f45c352f3f436a270e53514419f1b39d1fffb34cd4358
  indexed: true
  sha256: e8cf3ebab49de2d251e0d7c5f9fa712fa9e85e4fed6eb77e8926bf715a483de3
  source_range:
    end: 83794781
    part: 1
    start: 68893448
  summary: Kimi 与 CC 均在真实下载失败后保留正式 B 的目标 SHA、`temporary_network`/退避状态和失败次数 1；后续 OS 调度遵守 retry-waiting，未重复安装或调用模型。退避到期后，Kimi 已由第三次真实 OS 调度自动更新到 B 并通过原生读回及两个 MCP 检查，随后卸载专用调度、退出网络开关且全局配置未变；材料称 CC 也完成同一路径，整体后台恢复、清理、依赖 SHA 和零模型调用均通过。下一步只是安装并读回正式 B、等待真实公共经验合入后进行一次 DSV4/检索验收；拟用的自然问题聚焦 Ascend NPU 占用、设备编号、拓扑和启动前证据，尚未启动模型或执行机器检查。
  title: Kimi/CC 后台退避恢复与后续只读检索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f070e16faac169a0aedf678f6a75e1ff28824d75ed4e3066c9d68c0553e36a39
  kind: experience
  material_digest: f38d12b730c30dc143072a44607cbe5a81d37f17dc3a8ab1f307f260211d2583
  revision: a7ff957face88f522a22f6c9445d7573a2fbdfa5d49b92765b6579dd52bcb24f
  schema: mindie-entry/3
  summary: 当前索引覆盖 Kimi、Claude Code 与 Codex 的安装升级、隔离调度、退避重试、网络故障注入、自动恢复、MCP/模型观测、知识库与 DSV4 检索，以及公共 CI 校验器、SQLite/FTS5、NPU 资源和大库验收约束。新增材料表明 Kimi/CC 的后台恢复在材料中被报告为通过，但需保留其来源属性，不将单次 OS 调度、单次原生读回或零模型调用推广为全面发布或长期稳定性证明；同时保留 CI 旧 pin 与大小限制、跨块脱敏风险、其他 Python 的 FTS5 未验证、NPU 资源冲突、MCP reload/父子模型实证尚未运行等限制。后续仍需追踪正式公共经验合入、CC 的真实 DSV4 检索结果、最终配置与普通用户持久安装文档，以及 reporting-enable 全链路、撤销停服确认、旧入口兼容性和检索/文件门槛等未完成事项。
  title: mindie-agent 多端升级恢复、检索验收与未决约束追踪
navigation: 当前索引覆盖 Kimi、Claude Code 与 Codex 的安装升级、隔离调度、退避重试、网络故障注入、自动恢复、MCP/模型观测、知识库与 DSV4 检索，以及公共 CI 校验器、SQLite/FTS5、NPU 资源和大库验收约束。新增材料表明 Kimi/CC 的后台恢复在材料中被报告为通过，但需保留其来源属性，不将单次 OS 调度、单次原生读回或零模型调用推广为全面发布或长期稳定性证明；同时保留 CI 旧 pin 与大小限制、跨块脱敏风险、其他 Python 的 FTS5 未验证、NPU 资源冲突、MCP reload/父子模型实证尚未运行等限制。后续仍需追踪正式公共经验合入、CC 的真实 DSV4 检索结果、最终配置与普通用户持久安装文档，以及 reporting-enable 全链路、撤销停服确认、旧入口兼容性和检索/文件门槛等未完成事项。
schema: mindie-material-task/1
status: complete
task_id: f070e16faac169a0aedf678f6a75e1ff28824d75ed4e3066c9d68c0553e36a39
---

# mindie-agent 多端升级恢复、检索验收与未决约束追踪

当前索引覆盖 Kimi、Claude Code 与 Codex 的安装升级、隔离调度、退避重试、网络故障注入、自动恢复、MCP/模型观测、知识库与 DSV4 检索，以及公共 CI 校验器、SQLite/FTS5、NPU 资源和大库验收约束。新增材料表明 Kimi/CC 的后台恢复在材料中被报告为通过，但需保留其来源属性，不将单次 OS 调度、单次原生读回或零模型调用推广为全面发布或长期稳定性证明；同时保留 CI 旧 pin 与大小限制、跨块脱敏风险、其他 Python 的 FTS5 未验证、NPU 资源冲突、MCP reload/父子模型实证尚未运行等限制。后续仍需追踪正式公共经验合入、CC 的真实 DSV4 检索结果、最终配置与普通用户持久安装文档，以及 reporting-enable 全链路、撤销停服确认、旧入口兼容性和检索/文件门槛等未完成事项。

## Materials

- [多端插件适配与原生验收状态](blocks/853abafe8009d20df94faf841feacb68f4a348d97a9bde1f8292776003dc913c.md): 用户要求并行推进前两个修复，单独建立 Kimi 与 Claude Code（mindie-agent-cc）适配边界，并继承旧 vaws 的九条设计原则；Kimi 原生验收允许使用 K3，所有测试须在实机完成。记录显示 Claude Code 已完成隔离安装、远端调用、任务/fork 授权隔离、升级回滚、默认审批及 macOS 定时触发验收，但贡献采集整理、PR 闭环、领域经验质量、Windows 和生产升级仍未完整验收；Kimi 曾因旧启动文件不识别新配置参数导致调度失败，修正入口后首轮执行成功。知识库下库、withdrawn 精确引用和多端调度已有实测证据，但占位源同步失败、OAuth-only 未验收、Bot 审阅合入及反馈闭环等限制仍需保留，不能将报告中的成功扩大为完整闭环。
- [授权与服务生命周期锁顺序检查](blocks/69406b46ed0638ba6d9020129861de625a55738dbba739f95339d1a2eec92fdc.md): 当前任务要求只读核对授权、冻结、停服和启动之间的锁顺序。重点风险包括更新或回滚后恢复服务发生死锁、误认旧进程，以及恢复流程意外扩大授权范围；该块尚未提供检查结果或验证结论。
- [服务交接、进程组取消与远端错误边界](blocks/72fc381cb84a3c27e54648675313a6d1059278f695fe6397dda5b9886570e177.md): 源码审核与真实本机验收覆盖了更新停服/恢复、Kimi 与 Claude Code 进程组取消、远端错误输出和状态分类；曾发现异步停服、旧 helper、恢复配置、进程残留、授权字段泄露及 MCP 取消阻塞等风险，部分已修复或实测通过，部分仍未完成。真实 CC 升级、服务恢复、Stop 整理器执行及共享远端输出窄修已有证据，但整理器命令行采样缺失、MCP 取消接线和若干发布/rebase 工作仍不能视为完成。受控进程实验确认适配器独立进程组会留下内部子进程，Claude Code 最小修复验证通过；后续整理器退出码分类、状态入口和 NPU 显式开关继续按不泄露敏感信息、不新增重试原则验收。
- [状态诊断、依赖锁定与故障上报接入](blocks/923f63657921780c5db0abfe24c026085bafd7a15a578e6a3c2fb6f039ab125d.md): 受控进程验证确认整理器五类失败可被 core 准确分类并记录耗时，成功结果正常返回，且未泄露 stderr/敏感内容或遗留子进程；共享状态 API、MCP 取消等并行事项仍明确未完成。Codex 状态入口、远端异常安全分类与任务范围隔离已通过离线/真实 CLI 检查，136 项集成检查和编译检查通过；安装器已锁定官方仓库精确 SHA，旧解释器会在写配置前拒绝，但这些结果不等于完整发布闭环。当前新工作转向 Kimi/Claude Code 默认关闭、用户显式启用的故障上报薄接入，同时需收敛定时器无限追加日志和 Kimi 原始网页输出，并保留正在进行的本地实现调用及未验收状态。
- [原生包刷新与 reporting 命令验收](blocks/62d2b37957962825d7b11482714f9f62ac0f1c905e20cae91db344c73bc1736b.md): 完成了旧打包器升级兼容和原生包刷新：隔离 Claude 环境中出现三个 reporting 命令，两个 MCP 连接成功；最终 `/mindie-agent:reporting-status` 经 UserPromptExpansion 实际送达，2.53 秒返回“未配置上报”，无工具调用、授权或上传。过程中保留了验收脚本缺少 MCP SDK、旧包缺少 reporting matcher、同版本缓存不会刷新等失败和限制；PR #7 已更新推送，仍等待 root 合入。
- [三端卸载边界与安装文档缺口审查](blocks/5c538179e6d94b97674580467df3dd91704f5017c8e40df77d672b903afea8bc.md): 最终 HEAD `e9259e09941835cea002c24e95c1318231c0d5c4` 的 88 项组件检查和 CI 通过，原生更新刷新九个命令，唯一 DSV4 `reporting-status` 调用用时 2.526 秒且零重试；配置已恢复、进程已清理。后续只读审查发现原生 `reporting-enable` 的完整跨端接线证据不足，Kimi/Claude 取消调度可能吞掉 `launchctl bootout` 失败并误报 `unscheduled`，普通用户持久安装说明也需补齐。清理旧 Grok/Bot 授权入口的实施曾因自动审批拒绝删除五个旧源码和测试文件而未完成，该拒绝及文件范围已保留，不能视为清理成功。
- [多端安装发布与回滚验收记录](blocks/58512a0326c53e1825a9420ca408d324b7c2eef028d1b15b58c8346c7d2d9bbe.md): 记录了旧文件删除被拒后由工具自行换命令、五个文件恢复，以及文档任务因旧提示路径和超时未交付；新版上报队列保留已发布 Issue 回执和“写入结果未知”记录。静态审核发现三项回滚/授权缺陷：新服务描述可能阻断旧进程回滚、记录失败后版本指针可能未恢复、撤销授权与启动之间存在竞态；这些仅为发现的问题，未改文件。Kimi 持久安装、卸载和三端官方依赖检查有实机或 CI 证据，但 Claude Code 验收曾因观察脚本计入自身进程而失败，后续修正后仅验证关闭状态，原生 ON→OFF 仍未验；DSV4 单样本整理保留公开 API 与数值但遗漏外层 python3 行，不能推出保密保证。跨宿主检索复用找到了 Kimi 经验并正确引用多数参数，但将 dtype 顺序表述过强，该局限被保留。
- [隔离网络开关与 CC 验收准备](blocks/b6091750f20677bbf368f0256ea95e82d8934c2083c62c207191b39ade4b1e21.md): 验收准备强调区分组件故障注入与真实宿主证据，并指出日志超过 64 KiB 不等于累计经验正文超过 64 KiB，也不能用填充文本制造公共经验；另提醒仍需核对 10,000 条检索和 128 KiB 文件门槛。已实现并验证仅作用于指定隔离进程的 HTTPS CONNECT 网络开关：断连后恢复，公开 GitHub 请求正常；全局代理未修改。Claude Code 两阶段升级驱动仍在补齐，候选尚未执行模型验收，投稿方式拟采用本地脱敏后直接公共 PR，敏感测试内容不应进入公共库。
- [多端升级准备、校验器约束与验收驱动](blocks/1be2487217c2f52b4b6883e15088daaca604e4b5ae62f076832f655f8f1de63b.md): 网络探针区分了真实 `ls-remote` 解析成功与 `fetch` 被阻断，放通后同一 SHA 下载成功；Kimi、CC 的驱动完成语法或静态检查，但尚未调用模型、注册调度或执行 B 验收。审查发现公共 CI 仍固定校验器 `40063ebf…`，存在 128 KiB、1,024 文件和 32 MiB 总量限制，流式读取还需防跨块脱敏遗漏；三端本机 SQLite 3.54.0 已启用 FTS5，但安装 probe 尚未覆盖其他 Python 构建。Kimi 与 CC 已从旧版隔离升级至 A 并暂停调度；Codex 大库、MCP reload、父子模型观测和网络恢复驱动均以默认不执行或待正式 B/core 为当前状态，NPU 8 被 `/qjj_main_k3_mrv2` 的 Worker 占用，未创建容器。
- [Kimi/CC 后台退避恢复与后续只读检索](blocks/06c7f9d2cfcd0dcb920f45c352f3f436a270e53514419f1b39d1fffb34cd4358.md): Kimi 与 CC 均在真实下载失败后保留正式 B 的目标 SHA、`temporary_network`/退避状态和失败次数 1；后续 OS 调度遵守 retry-waiting，未重复安装或调用模型。退避到期后，Kimi 已由第三次真实 OS 调度自动更新到 B 并通过原生读回及两个 MCP 检查，随后卸载专用调度、退出网络开关且全局配置未变；材料称 CC 也完成同一路径，整体后台恢复、清理、依赖 SHA 和零模型调用均通过。下一步只是安装并读回正式 B、等待真实公共经验合入后进行一次 DSV4/检索验收；拟用的自然问题聚焦 Ascend NPU 占用、设备编号、拓扑和启动前证据，尚未启动模型或执行机器检查。

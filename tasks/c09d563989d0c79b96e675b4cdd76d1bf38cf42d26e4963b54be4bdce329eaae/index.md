---
blocks:
- block_id: c4e5289a976beed212d33394b3d9937d27d962127e8154dce7b60c27eed3e113
  indexed: true
  sha256: 327a79c1f584667bca42174ee6730fe659ce0215f732c5d43a955406ea177834
  source_range:
    end: 16779573
    part: 0
    start: 0
  summary: 围绕 SSH、资源分配、NPU 解析和 wheel/CI 检查推进了一轮破坏性跨仓迁移，已报告 SSH 主链路、基础资源检查及部分安装验证通过，但候选分支尚未合并发布，完整回归和正式依赖锁定仍待完成。设计随后从“会话关闭即删容器”修正为用户容器长期保留、任务借用 runtime、执行租约和业务端口随任务回收；容器按用户固定命名，例如 `vaws-maoxx241`，同容器任务通过环境目录、进程和租约隔离。后续又暂停实现，转向先定义 remote-dev、coordinator、知识库和工作区的职责边界，并记录了旧 session 路径删除、生命周期、环境选择和请求状态等尚未完全落地的事项。
  title: 跨仓主体迁移与容器复用生命周期
- block_id: ccb04739bde0ed578b9c7f542611ac6f9c43f9ba2de36e16d4fad2362e154d86
  indexed: true
  sha256: 273274a55fb7b0d5dae2539eb4d2c05e24d6173c0cb020ab8e318960643e815c
  source_range:
    end: 16779573
    part: 1
    start: 0
  summary: 进一步确认 torch/torch_npu 可通过独立虚拟环境按进程隔离，但仍需满足 CANN、Python 与宿主驱动兼容；CANN 应使用明确版本目录，避免切换共享 `latest` 或覆盖正在使用的安装。架构讨论将 remote-dev 定义为独立的远端操作映射层，只需 IP、端口等连接信息，coordinator 负责用户容器、环境、代码隔离、执行记录、资源分配和常驻调度，vaws-top 保持机器/NPU 观测职责，知识库主要补充经验、官方资料、原理和指南并标注来源与可信度。已选择本地常驻协调、固定配方自动准备环境和客户端关闭后远端任务继续运行（本机休眠暂停调度）的方向；源码审查指出自动选机、多机分配、后台驱动、稳定任务工作区及知识内容契约仍存在 gap，尚未作为功能验收通过。
  title: 用户容器复用与四组件职责对齐
- block_id: db94a3103869c6bfb87a3279ef49ea0d76e3a4afc075595e3307a313316e34a1
  indexed: true
  sha256: 418d2be04bef294c728ace76bd3744e30ecdf68013f5c814f420a4e1b207ab6c
  source_range:
    end: 16779573
    part: 2
    start: 0
  summary: 架构审查提出三项修正：远端进程控制统一归 remote-dev，任务工作区与 NPU/业务端口等执行资源分开生命周期，coordinator 必须独立常驻推进排队、状态核对和回收；对应 gap 包括跨平台/具名客户端验证、配方环境准备、多机资源分配和知识来源分级。随后否定将请求标识、解释器选择、排队恢复等重状态逻辑塞入 scaffold/skills，明确 scaffold 只是工作区中承载材料、安装配置和业务 skills 的称呼，不是独立基础设施组件。用户同意让 Grok 进行破坏性整体迁移，当前已启动 remote-dev、coordinator 和工作区三个实现包，但仍处于迁移阶段，知识组件初始化失败修复、真实调用链检查和集成验证尚未完成。
  title: 移除 scaffold 概念并按边界破坏性实现
- block_id: 60c4a27845baa2fec403ec65509f27eb6b5a51bda9433b929d8ec4da9786a520
  indexed: true
  sha256: 4aa7760323bfcf43ccd8f2d6f4130d32bfdf0a5a9e1943b99ab507f20b367b72
  source_range:
    end: 33560708
    part: 0
    start: 16779573
  summary: remote-dev 0.5.0、knowledge 0.2.0 和 coordinator 0.3.0 已分别通过记录中的发布、CI 或安装验收；coordinator 已覆盖异步提交、取消、自动清理、资源归还和 remote-dev 传输，知识包也修复了 reference 同步、重复条目和 MCP/stdio 链路问题。迁移期间仍发现并修正了多项问题，包括旧 session 调用、环境隔离、解释器路径含空格、取消状态覆盖、测试进程扫描范围和 Linux `/proc` 启动时序；其中部分验证仅使用模拟资源或独立 CPU 进程，不能推及真实 NPU/模型服务。当前工作区依赖已锁定到三个正式版本，但完整业务测试仍有收集错误后暴露的失败，profiling、parity 及其他测试迁移尚未形成最终整体通过结论。
  title: 四组件迁移与发布验收进展
- block_id: 2249fdbd4eb0fd2a29dd5f8f32ccb5d6f0d75dcde806825ede144a71b859d1aa
  indexed: true
  sha256: 105bd473c9ccfd96f63356607e2f43cc4762bec5fc7feb196cdeaac67c20664c
  source_range:
    end: 36457290
    part: 0
    start: 33560708
  summary: '记录称 workspace 测试从早期失败修复到主套件 535 项通过、1 项跳过，并处理了旧白名单、stdio Unix socket 隔离、异步 finish 以及跨任务执行归属回归；coordinator 0.3.1 据称已使跨任务查询、停止和目标访问被拒绝，同任务会话仍可用。记录还称 remote-dev 0.5.0、coordinator 0.3.1、knowledge 0.2.0 已发布，workspace PR #133 已合并，主工作区和客户端配置已更新；这些结果属于该记录中的验收与 CI 报告。真实 NPU 模型端到端流程尚未验证，客户端还需重载 MCP 才采用新配置；记录中的本地验收路径已以 <redacted:local-path:6ed031daf350> 保留。'
  title: 组件重构发布与跨任务隔离回归记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c09d563989d0c79b96e675b4cdd76d1bf38cf42d26e4963b54be4bdce329eaae
  kind: experience
  material_digest: 2d53dfd147b7523fc42e0be7cfb3c7615b8a6fedc6cce2f808ef1ba8d64ff1d7
  revision: a52b6842e232066e97a38182f5cc1adf9fe4fc7e73e35bea5466bc03901955e8
  schema: mindie-entry/3
  summary: '已有记录报告 remote-dev 0.5.0、coordinator 0.3.1、knowledge 0.2.0 发布，workspace PR #133 合并，主套件报告为 535 项通过、1 项跳过；coordinator 的跨任务访问回归、异步 finish、准备阶段取消和清理问题也据称已修复。此前的环境隔离、旧接口、测试夹具与泄漏白名单问题应作为已处理记录保留，但这些 CI/验收结果不等于完整业务成功。当前主线应先用正式版本验证真实 NPU 端到端流程（环境准备、服务启动、请求、结果采集、资源释放），同时完成 MCP 客户端重载，并继续保留任何未验证、失败或后续修正。'
  title: 基础组件已发布，跨组件真实 NPU 验证仍待完成
navigation: '已有记录报告 remote-dev 0.5.0、coordinator 0.3.1、knowledge 0.2.0 发布，workspace PR #133 合并，主套件报告为 535 项通过、1 项跳过；coordinator 的跨任务访问回归、异步 finish、准备阶段取消和清理问题也据称已修复。此前的环境隔离、旧接口、测试夹具与泄漏白名单问题应作为已处理记录保留，但这些 CI/验收结果不等于完整业务成功。当前主线应先用正式版本验证真实 NPU 端到端流程（环境准备、服务启动、请求、结果采集、资源释放），同时完成 MCP 客户端重载，并继续保留任何未验证、失败或后续修正。'
schema: mindie-material-task/1
status: complete
task_id: c09d563989d0c79b96e675b4cdd76d1bf38cf42d26e4963b54be4bdce329eaae
---

# 基础组件已发布，跨组件真实 NPU 验证仍待完成

已有记录报告 remote-dev 0.5.0、coordinator 0.3.1、knowledge 0.2.0 发布，workspace PR #133 合并，主套件报告为 535 项通过、1 项跳过；coordinator 的跨任务访问回归、异步 finish、准备阶段取消和清理问题也据称已修复。此前的环境隔离、旧接口、测试夹具与泄漏白名单问题应作为已处理记录保留，但这些 CI/验收结果不等于完整业务成功。当前主线应先用正式版本验证真实 NPU 端到端流程（环境准备、服务启动、请求、结果采集、资源释放），同时完成 MCP 客户端重载，并继续保留任何未验证、失败或后续修正。

## Materials

- [跨仓主体迁移与容器复用生命周期](blocks/c4e5289a976beed212d33394b3d9937d27d962127e8154dce7b60c27eed3e113.md): 围绕 SSH、资源分配、NPU 解析和 wheel/CI 检查推进了一轮破坏性跨仓迁移，已报告 SSH 主链路、基础资源检查及部分安装验证通过，但候选分支尚未合并发布，完整回归和正式依赖锁定仍待完成。设计随后从“会话关闭即删容器”修正为用户容器长期保留、任务借用 runtime、执行租约和业务端口随任务回收；容器按用户固定命名，例如 `vaws-maoxx241`，同容器任务通过环境目录、进程和租约隔离。后续又暂停实现，转向先定义 remote-dev、coordinator、知识库和工作区的职责边界，并记录了旧 session 路径删除、生命周期、环境选择和请求状态等尚未完全落地的事项。
- [用户容器复用与四组件职责对齐](blocks/ccb04739bde0ed578b9c7f542611ac6f9c43f9ba2de36e16d4fad2362e154d86.md): 进一步确认 torch/torch_npu 可通过独立虚拟环境按进程隔离，但仍需满足 CANN、Python 与宿主驱动兼容；CANN 应使用明确版本目录，避免切换共享 `latest` 或覆盖正在使用的安装。架构讨论将 remote-dev 定义为独立的远端操作映射层，只需 IP、端口等连接信息，coordinator 负责用户容器、环境、代码隔离、执行记录、资源分配和常驻调度，vaws-top 保持机器/NPU 观测职责，知识库主要补充经验、官方资料、原理和指南并标注来源与可信度。已选择本地常驻协调、固定配方自动准备环境和客户端关闭后远端任务继续运行（本机休眠暂停调度）的方向；源码审查指出自动选机、多机分配、后台驱动、稳定任务工作区及知识内容契约仍存在 gap，尚未作为功能验收通过。
- [移除 scaffold 概念并按边界破坏性实现](blocks/db94a3103869c6bfb87a3279ef49ea0d76e3a4afc075595e3307a313316e34a1.md): 架构审查提出三项修正：远端进程控制统一归 remote-dev，任务工作区与 NPU/业务端口等执行资源分开生命周期，coordinator 必须独立常驻推进排队、状态核对和回收；对应 gap 包括跨平台/具名客户端验证、配方环境准备、多机资源分配和知识来源分级。随后否定将请求标识、解释器选择、排队恢复等重状态逻辑塞入 scaffold/skills，明确 scaffold 只是工作区中承载材料、安装配置和业务 skills 的称呼，不是独立基础设施组件。用户同意让 Grok 进行破坏性整体迁移，当前已启动 remote-dev、coordinator 和工作区三个实现包，但仍处于迁移阶段，知识组件初始化失败修复、真实调用链检查和集成验证尚未完成。
- [四组件迁移与发布验收进展](blocks/60c4a27845baa2fec403ec65509f27eb6b5a51bda9433b929d8ec4da9786a520.md): remote-dev 0.5.0、knowledge 0.2.0 和 coordinator 0.3.0 已分别通过记录中的发布、CI 或安装验收；coordinator 已覆盖异步提交、取消、自动清理、资源归还和 remote-dev 传输，知识包也修复了 reference 同步、重复条目和 MCP/stdio 链路问题。迁移期间仍发现并修正了多项问题，包括旧 session 调用、环境隔离、解释器路径含空格、取消状态覆盖、测试进程扫描范围和 Linux `/proc` 启动时序；其中部分验证仅使用模拟资源或独立 CPU 进程，不能推及真实 NPU/模型服务。当前工作区依赖已锁定到三个正式版本，但完整业务测试仍有收集错误后暴露的失败，profiling、parity 及其他测试迁移尚未形成最终整体通过结论。
- [组件重构发布与跨任务隔离回归记录](blocks/2249fdbd4eb0fd2a29dd5f8f32ccb5d6f0d75dcde806825ede144a71b859d1aa.md): 记录称 workspace 测试从早期失败修复到主套件 535 项通过、1 项跳过，并处理了旧白名单、stdio Unix socket 隔离、异步 finish 以及跨任务执行归属回归；coordinator 0.3.1 据称已使跨任务查询、停止和目标访问被拒绝，同任务会话仍可用。记录还称 remote-dev 0.5.0、coordinator 0.3.1、knowledge 0.2.0 已发布，workspace PR #133 已合并，主工作区和客户端配置已更新；这些结果属于该记录中的验收与 CI 报告。真实 NPU 模型端到端流程尚未验证，客户端还需重载 MCP 才采用新配置；记录中的本地验收路径已以 <redacted:local-path:6ed031daf350> 保留。

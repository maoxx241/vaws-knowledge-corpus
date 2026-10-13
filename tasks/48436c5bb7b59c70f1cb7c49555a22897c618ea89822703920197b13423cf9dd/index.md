---
blocks:
- block_id: d9028c8644eb5a4e9bbe285f390bff76abcd11ef8addd7ab95d9ddbd8bc11ef7
  indexed: true
  sha256: 39c57bb2d2623bef4f7d50c13fb23196da356e1e991bae4899ee435411dcf02e
  source_range:
    end: 17405668
    part: 0
    start: 0
  summary: 调研比较了 OSV、Basic Memory、Haystack、RAGFlow、WeKnora、OpenViking 等方案，并发现现有查询函数对中文 token 支持不足，中文相同摘要查询得分为 0。需求随后明确为：VAWS 初始化时创建 VAWS 与知识库 fork，Agent 在 session 结束后提交经脱敏的经验 PR，Grok 按知识类型审核、去重并自动合入，客户端周期性同步权威知识到本地。审核通过不等于结论已验证，需保留文档支持、作者实测、独立复现和待验证状态。
  title: 知识库需求从组件选型转向 Git 闭环架构
- block_id: 32fc3672b6bac498fc75f85c6fbc110ffd51400c5860e64fb0d04346ca48cf4d
  indexed: true
  sha256: b3ebf4702fcc688cd6cdb528c0f7145314249d67e5e0c7b60d675b553662dd1a
  source_range:
    end: 17405668
    part: 1
    start: 0
  summary: 设计稿尚未实施，后续将选型重点转为复用成熟项目，减少自研存储、检索、去重和更新逻辑。需求收敛为 Markdown 作为权威内容、Git 管理版本、16GB 内存且无独显的本地部署，不考虑 PDF/OCR 和原始材料存储，知识老化暂缓。Basic Memory 与 QMD 的小样本测试显示 QMD 在该语料上命中前三为 40/40、暖内存约 1.52GiB，但其文件直管 Git 的路径更契合；该结论后来因未充分验证 OpenViking 而被撤回。
  title: Markdown/Git 与本地检索方案比较
- block_id: aae380cb174992ac1f846565185aad9fc392d8815ef8ddde5264d51d25ca3f9d
  indexed: true
  sha256: 498224f8e20951e945a3281075c60924362d6ead71f1f255d4087f3ad81890ab
  source_range:
    end: 17405668
    part: 2
    start: 0
  summary: 候选范围扩大到 42 个仓库，重点路线为 ReMe、OpenViking 和 ReMe+QMD；初步验证显示 OpenViking 可在 16GB 环境运行，且 Markdown 导出、Git clone 后重建索引可行，但原生移动操作可能触发后台 LLM 请求。CPU 大规模测试中，ReMe 5 万条建库约 14.3 分钟、查询峰值约 1.35GiB，默认混合搜索中位延迟约 107ms，FAISS 后端约 6.6ms；前三命中率从 1 万条的 88.5% 降至 5 万条的 76.5%，说明规模增长会影响召回质量。增量 100 条新增、50 条修改、50 条删除仅重算 150 条 embedding，原生更新约 6.2 秒；QMD 因 CPU embedding 会话 30 分钟上限中途退出，需支持断点续建，且其 1 万条测试尚未完成。
  title: 候选扩展与 CPU 大规模检索测试进展
- block_id: edfc51d46513a749f4c8f0ff3ac871069912652cd65e610bf4a266e2c40fc3bc
  indexed: true
  sha256: 56a06084cdab6c57f8b5d0c974da30439dbc888825b4aefa84552b34e13e1379
  source_range:
    end: 17405668
    part: 3
    start: 0
  summary: QMD 已从中断位置继续处理约 2,000 条，暂未出现新的 embedding 错误。仍待完成 QMD 的 1 万条检索与增量测试，以及 OpenViking 的 1 万条正文处理对照；此前 5 万条其他方案结果已保留。
  title: QMD 断点续建仍在进行
- block_id: 1830c19dfcae9d8e163b6267a7729a16117668bb6b03674e2687ae9f21c9e23e
  indexed: true
  sha256: 6e530ee3aaef04d748157b64a9ab9acf8d6de2d90dc7d44be97457558c6ae466
  source_range:
    end: 34217899
    part: 0
    start: 17405668
  summary: 本批记录了纯 CPU 下三套方案的索引、查询、内存、磁盘和增量更新结果：QMD 在 1 万条数据上前三命中率 97.5%，但累计建库约 55 分钟；ReMe + FAISS 查询和更新成本更低；OpenViking 正文对照与 ReMe 纯向量命中率均为 86.5%，5 万条查询中位约 11.1ms。OpenViking 的磁盘、安装和增量成本高于 ReMe，但资源仍被认为可接受，并支持通过 OVPack 复用预计算向量；1 千条导入测试未重算文档 embedding，但 Mac 到 Windows 的分发尚未验证。平台结论后来收窄为 ARM64 Mac 与 x86-64 Windows，Windows/PowerShell 实机闭环、真实知识整理和公共贡献往返仍未完成；方案设计倾向 OpenViking 作为更完整底座，但本轮没有正式代码接入。
  title: OpenViking、ReMe 与 QMD 的实测及方案比较
- block_id: e4308e6a591f683c3433df8f206cc7d59a2dbfd2c8e607f728bd0d20af387640
  indexed: true
  sha256: 761a5111de32282d14f0947394c70017ffa8158ceb62842e5683bee5a49a51e7
  source_range:
    end: 34217899
    part: 1
    start: 17405668
  summary: 讨论认为 OpenViking 基本覆盖当前所需的知识存储、增删改、向量检索和上下文管理能力，但 ReMe + FAISS 在 Markdown/Git 直连、查询及增量成本上仍有优势；“能力覆盖”不等于接口、可靠性和整理质量已逐项验证。拟将 VAWS 收敛为 OpenViking 存储检索、知识格式与条件管理、公共 Git/PR 审核、Grok 去重及 OVPack 分发的协作架构，知识捕获放宽为标题、正文和脱敏检查，所有内容仅作参考而非公理。开发流程计划允许先实验后引用证据、减少确认和重复记录，并让本地未审核经验与公共知识一起按相关性和环境匹配返回；这些仍是设计或 planned 状态。第一批实现已交给 Grok，重点为流程减负和 OpenViking 本地最小链路；Kimi Code 负责统一 remote-dev 内核，兼容 Cursor、Codex、Claude Code、Kimi Code 和 Grok 的使用习惯，相关实现尚未独立验收。
  title: VAWS 简化原则与 OpenViking 接入架构
- block_id: b0cf11a8df577ec6517b41525de869a6aa7b4a9055c0e8b6eebd3e77c4f1c6dd
  indexed: true
  sha256: a8a91559f9e1cc6d58efc701d4199b857da3dceb5b7bc25ea9873d0bb1f23a08
  source_range:
    end: 34217899
    part: 2
    start: 17405668
  summary: 后续方向是先精简验证、记录和输出流程，再接入 OpenViking，并通过真实 vllm-ascend 开发任务判断是否减少额外调用、文档阅读、等待和重复实验；资源归属、公开脱敏和实际执行结果仍由组件检查。架构与实施 spec 已整理，但功能批次尚未完成，计划并行推进知识贡献与审核、OVPack 发布同步、remote-dev 增强及平台验收。已明确本地经验和公共知识一起返回，审核状态只描述公共内容的处理状态，不作为本地经验的准入门槛或降权依据；所有知识都只是参考，最终判断依赖 Agent 结合当前代码和运行结果。此前拟纳入 Intel Mac 的范围已修正为仅考虑 ARM Mac 和 x86 Windows。
  title: 并行实施计划与知识可信度边界
- block_id: f64c4ce6b32cc956e782bc53e54e42544fe6667b01928a8962f1dcafd7824d5a
  indexed: true
  sha256: d5c9c71a21f959093c8d54cce0ee5bc64bde763fac28cfaaa5fec9ed2ecbacb0
  source_range:
    end: 51211370
    part: 0
    start: 34217899
  summary: 四个 Agent 均已提交本地交付，四组测试通过；第一组 Grok 先前发现的中文标题覆盖、`dry_run` 删除已有条目、Markdown 索引同步缺口等问题已修复，两个真实 OpenViking 用例及 OVPack 构建、导入、切换和重启用例也通过。独立检查仍复现三处问题：贡献审核只审第一个 Markdown 可能放行整个 PR，remote-dev 在大段输入加 EOF 时截断且中文分页乱码，分发锁超过半小时后可能允许第二个同步进入；这些修复已交回对应 Agent，Windows、真实远端和公共发布流程仍未验证。
  title: 四 Agent 交付后的独立验收与待修问题
- block_id: 49af68b49c6f0f383e6b570ade71a53ae3cf5270678fce3372eb12b30dd10a20
  indexed: true
  sha256: b3872c2d069af643716d9af366e5ebbe954fe927097ee45e5f6f7bb66914c394
  source_range:
    end: 67992698
    part: 0
    start: 51211370
  summary: '四个 PR（remote-dev #5、vaws-knowledge #19、vaws-coordinator #10、workspace #134）已合入 main；153–156 的 144 项远程检查、五个客户端 MCP 调用、OpenViking 写入/查询/重启回读，以及 153 上单卡 NPU 矩阵计算均被报告为通过，CPU 参考误差为 0。联调过程中曾出现 Python/ensurepip、SOC_VERSION、构建产物误识别、workspace 绑定和 `ASCEND_OPP_PATH` 缺失等问题，均据记录修正；但完整模型服务、精度/性能、Windows 原生验收、公网贡献发布仍未验证，且主工作区后来仍安装 `vaws-knowledge 0.2.0`，尚未正式启用合并版本。'
  title: 153–156 联调合入与 OpenViking/NPU 验证状态
- block_id: 930a1194cb77e152b328532909fc5797685cd79aa3ee5347675220aa92d87896
  indexed: true
  sha256: 4459fd478b203e005012e04e98cc8d482f3bba06ae9076d4350c3dbc0621bdbd
  source_range:
    end: 67992698
    part: 1
    start: 51211370
  summary: 当前建议先完成主目录正式启用，再选真实开发任务贯穿查知识、改代码、NPU 验证、保存经验和后续复用；Windows 单独排期。用户要求优先推进公共知识贡献和发布更新，并暂不接入 Grok bot；计划涉及 fork 分支推送、GitHub Release 上传/下载、本地检索与 OVPack 共用鉴权，以及 MCP 存活期间的后台提交和同步。该材料只记录了准备补齐这些接口和人工审核方向，尚未证明真实 GitHub/xAI 流程、自动捕获/脱敏/PR、Release 分发、重复冲突处理或后台同步已经完成。
  title: 公共知识贡献与 OVPack 发布同步的后续接线
- block_id: 3289d0090cc143deb408596f50c9a0ebb41ddbe8d0d4c1566e8fde01db1f1f33
  indexed: true
  sha256: 3c25d7d8991802820f8be5d9194ffde4cd574c0f98829ead1bec8d30464c1380
  source_range:
    end: 85124732
    part: 0
    start: 67992698
  summary: 记录了公共知识库、Release 自动同步、客户端接线及相关 PR/CI 的报告性结果，同时明确 Grok bot、原生客户端信任和主工作区启用仍未完成。随后审计将范围收窄到 VAWS，指出 Claude frontmatter、旧入口、职责冲突、知识流程阻塞和重复确认等问题，并建议撤销 machine-management、session-management、remote-code-parity 三个独立入口，精简 repo-init 与 npu-fleet-monitor，将知识整理迁入知识包。后续改动报告称 PD 分组、parity 调用、客户端投影和知识包安装链路已接回或验证，但工作区 CI 曾因旧版本断言失败，最终 profiling 分析测试状态在该块结尾仍未明确；这些均应视为有限范围的报告，不能提升为全面成功。
  title: VAWS 架构与 Skill 审计取舍
- block_id: 44cfc24fee6766b1f11d0ffccf6a37807eb0e6403b937e97039d21a314adcc90
  indexed: true
  sha256: f60bd569799056a72038bcfcc5d93b72b3abbf0693dd6a9eb44105d054fe5e9b
  source_range:
    end: 85124732
    part: 1
    start: 67992698
  summary: '清理前确认 PD 仍依赖 `session_group.py` 分组文件、远端适配器仍调用 `parity_sync.py`，因此要求先把能力接回业务输入和包接口再删除旧目录；随后报告称六项取舍已落地，并创建知识包 #21 与工作区 #136 草稿 PR。用户要求 NPU 验收后直接合并，当前执行聚焦 PD 配置、托管生命周期和源码准备链路；源码发布快照在真实容器中据报告通过，未改写工作目录，也未触发安装或重编译。双角色 TP2 服务的真实启动、推理、统一停止及资源释放仍在进行中，且该验收不覆盖 KV 连接器性能或传输正确性。'
  title: VAWS Skill 清理后的 NPU 验收
- block_id: b19bedb53ddc2f627776fc523b2afb599925b155a9bb2866b18224972baecd91
  indexed: true
  sha256: f781c449421267db059532a7a2f886d305dc0f8bdda9962c7f3cec3a840fac36
  source_range:
    end: 100629699
    part: 0
    start: 85124732
  summary: A3 上两组 TP=2 服务经历了算子编译、端口分配、源码导入、CLI 参数、状态查询和资源释放等验证；期间发现未声明端口池、任务目录遮蔽 vLLM、旧参数 `--disable-log-requests`、运行状态误报 `launch_pending` 及异步释放显示不当等问题，并分别进行了修复或回归测试。最终记录报告两组服务真实推理成功、4 张 NPU 和两个端口释放、Run Manifest 为 `passed`，三个 PR 的 CI 全绿并已合并；但该记录也明确不包含 KV 传输和性能测试。实机操作仍较不顺手，主要表现为准备阶段黑盒、安装版本与进程加载版本混淆、状态记录不同步、默认返回过大，以及 SSH/HTTP 代理故障容易伪装成业务故障。
  title: 真实 NPU 验证暴露并修复多项生命周期问题
- block_id: d50add1a356dd41e9a8aceab208bad5c69b083c38c163410a76c78d78f0b6b79
  indexed: true
  sha256: 776bb6c26c3c97f95035d57b926c118f718f5fa02a5c22954c6e69c6073b9c0a
  source_range:
    end: 100629699
    part: 1
    start: 85124732
  summary: 针对准备进度、日志回读、版本漂移、状态推进、连接诊断和冗余输出的优化已实施；修复了日志文件先引用后创建、重建环境的 checkout 冲突、预检遗漏绑定源码导入路径等问题，并删除了旧 checkout 辅助逻辑及重复响应层。过期 CLI 参数在分配 NPU 前被预检拒绝且记录为 failed；正确参数下，一次 `start` 后由 `status` 推进 manifest，经 starting/running/stopping 到资源释放完成，报告称两组 TP=2 短推理成功且四张 NPU、两个端口和进程组均已释放。三个 PR 被报告为合并，状态输出从约 32.3 KB 降至 3.9 KB；仍未测试 KV 传输与性能，且这些结论属于该批实机记录中的报告性结果。
  title: 优化设计落地并完成单次启动生命周期验收
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 48436c5bb7b59c70f1cb7c49555a22897c618ea89822703920197b13423cf9dd
  kind: experience
  material_digest: 5556d103bcf83d85b56622ef8179ac2ea1c13350a6eca3396ee009e759ed94d4
  revision: 2d043c4314947de6cb2963c056145b42e284854a7946a9c073c63f183a249c0d
  schema: mindie-entry/3
  summary: 当前任务已从合并前的真实 NPU 验收推进到一套已落地的生命周期与可观测性优化：准备阶段提供心跳、步骤和日志引用，CLI 参数在 NPU 分配前预检，安装版本与进程加载版本可对照，单次 `start` 后由 `status` 自动推进 manifest，并区分进程运行、HTTP 就绪、停止中和资源释放完成。新增记录报告 A3 两组 TP=2 服务完成短推理，过期参数、恢复复用冲突、导入路径不一致及代理/连接问题均有失败证据或修复记录，最终资源释放和 CI/PR 合并被报告为完成；但这些仍是可检索的、可能出错的运行记录，不应扩展为全面验证。KV 传输和性能测试明确未覆盖，源码与编译目录保留；后续若核查，应优先确认实际 main 提交、合并后的代码树、manifest/资源证据与修复后的实机状态是否一致。
  title: VAWS 生命周期优化后的真实 NPU 验收与合并状态
navigation: 当前任务已从合并前的真实 NPU 验收推进到一套已落地的生命周期与可观测性优化：准备阶段提供心跳、步骤和日志引用，CLI 参数在 NPU 分配前预检，安装版本与进程加载版本可对照，单次 `start` 后由 `status` 自动推进 manifest，并区分进程运行、HTTP 就绪、停止中和资源释放完成。新增记录报告 A3 两组 TP=2 服务完成短推理，过期参数、恢复复用冲突、导入路径不一致及代理/连接问题均有失败证据或修复记录，最终资源释放和 CI/PR 合并被报告为完成；但这些仍是可检索的、可能出错的运行记录，不应扩展为全面验证。KV 传输和性能测试明确未覆盖，源码与编译目录保留；后续若核查，应优先确认实际 main 提交、合并后的代码树、manifest/资源证据与修复后的实机状态是否一致。
schema: mindie-material-task/1
status: complete
task_id: 48436c5bb7b59c70f1cb7c49555a22897c618ea89822703920197b13423cf9dd
---

# VAWS 生命周期优化后的真实 NPU 验收与合并状态

当前任务已从合并前的真实 NPU 验收推进到一套已落地的生命周期与可观测性优化：准备阶段提供心跳、步骤和日志引用，CLI 参数在 NPU 分配前预检，安装版本与进程加载版本可对照，单次 `start` 后由 `status` 自动推进 manifest，并区分进程运行、HTTP 就绪、停止中和资源释放完成。新增记录报告 A3 两组 TP=2 服务完成短推理，过期参数、恢复复用冲突、导入路径不一致及代理/连接问题均有失败证据或修复记录，最终资源释放和 CI/PR 合并被报告为完成；但这些仍是可检索的、可能出错的运行记录，不应扩展为全面验证。KV 传输和性能测试明确未覆盖，源码与编译目录保留；后续若核查，应优先确认实际 main 提交、合并后的代码树、manifest/资源证据与修复后的实机状态是否一致。

## Materials

- [知识库需求从组件选型转向 Git 闭环架构](blocks/d9028c8644eb5a4e9bbe285f390bff76abcd11ef8addd7ab95d9ddbd8bc11ef7.md): 调研比较了 OSV、Basic Memory、Haystack、RAGFlow、WeKnora、OpenViking 等方案，并发现现有查询函数对中文 token 支持不足，中文相同摘要查询得分为 0。需求随后明确为：VAWS 初始化时创建 VAWS 与知识库 fork，Agent 在 session 结束后提交经脱敏的经验 PR，Grok 按知识类型审核、去重并自动合入，客户端周期性同步权威知识到本地。审核通过不等于结论已验证，需保留文档支持、作者实测、独立复现和待验证状态。
- [Markdown/Git 与本地检索方案比较](blocks/32fc3672b6bac498fc75f85c6fbc110ffd51400c5860e64fb0d04346ca48cf4d.md): 设计稿尚未实施，后续将选型重点转为复用成熟项目，减少自研存储、检索、去重和更新逻辑。需求收敛为 Markdown 作为权威内容、Git 管理版本、16GB 内存且无独显的本地部署，不考虑 PDF/OCR 和原始材料存储，知识老化暂缓。Basic Memory 与 QMD 的小样本测试显示 QMD 在该语料上命中前三为 40/40、暖内存约 1.52GiB，但其文件直管 Git 的路径更契合；该结论后来因未充分验证 OpenViking 而被撤回。
- [候选扩展与 CPU 大规模检索测试进展](blocks/aae380cb174992ac1f846565185aad9fc392d8815ef8ddde5264d51d25ca3f9d.md): 候选范围扩大到 42 个仓库，重点路线为 ReMe、OpenViking 和 ReMe+QMD；初步验证显示 OpenViking 可在 16GB 环境运行，且 Markdown 导出、Git clone 后重建索引可行，但原生移动操作可能触发后台 LLM 请求。CPU 大规模测试中，ReMe 5 万条建库约 14.3 分钟、查询峰值约 1.35GiB，默认混合搜索中位延迟约 107ms，FAISS 后端约 6.6ms；前三命中率从 1 万条的 88.5% 降至 5 万条的 76.5%，说明规模增长会影响召回质量。增量 100 条新增、50 条修改、50 条删除仅重算 150 条 embedding，原生更新约 6.2 秒；QMD 因 CPU embedding 会话 30 分钟上限中途退出，需支持断点续建，且其 1 万条测试尚未完成。
- [QMD 断点续建仍在进行](blocks/edfc51d46513a749f4c8f0ff3ac871069912652cd65e610bf4a266e2c40fc3bc.md): QMD 已从中断位置继续处理约 2,000 条，暂未出现新的 embedding 错误。仍待完成 QMD 的 1 万条检索与增量测试，以及 OpenViking 的 1 万条正文处理对照；此前 5 万条其他方案结果已保留。
- [OpenViking、ReMe 与 QMD 的实测及方案比较](blocks/1830c19dfcae9d8e163b6267a7729a16117668bb6b03674e2687ae9f21c9e23e.md): 本批记录了纯 CPU 下三套方案的索引、查询、内存、磁盘和增量更新结果：QMD 在 1 万条数据上前三命中率 97.5%，但累计建库约 55 分钟；ReMe + FAISS 查询和更新成本更低；OpenViking 正文对照与 ReMe 纯向量命中率均为 86.5%，5 万条查询中位约 11.1ms。OpenViking 的磁盘、安装和增量成本高于 ReMe，但资源仍被认为可接受，并支持通过 OVPack 复用预计算向量；1 千条导入测试未重算文档 embedding，但 Mac 到 Windows 的分发尚未验证。平台结论后来收窄为 ARM64 Mac 与 x86-64 Windows，Windows/PowerShell 实机闭环、真实知识整理和公共贡献往返仍未完成；方案设计倾向 OpenViking 作为更完整底座，但本轮没有正式代码接入。
- [VAWS 简化原则与 OpenViking 接入架构](blocks/e4308e6a591f683c3433df8f206cc7d59a2dbfd2c8e607f728bd0d20af387640.md): 讨论认为 OpenViking 基本覆盖当前所需的知识存储、增删改、向量检索和上下文管理能力，但 ReMe + FAISS 在 Markdown/Git 直连、查询及增量成本上仍有优势；“能力覆盖”不等于接口、可靠性和整理质量已逐项验证。拟将 VAWS 收敛为 OpenViking 存储检索、知识格式与条件管理、公共 Git/PR 审核、Grok 去重及 OVPack 分发的协作架构，知识捕获放宽为标题、正文和脱敏检查，所有内容仅作参考而非公理。开发流程计划允许先实验后引用证据、减少确认和重复记录，并让本地未审核经验与公共知识一起按相关性和环境匹配返回；这些仍是设计或 planned 状态。第一批实现已交给 Grok，重点为流程减负和 OpenViking 本地最小链路；Kimi Code 负责统一 remote-dev 内核，兼容 Cursor、Codex、Claude Code、Kimi Code 和 Grok 的使用习惯，相关实现尚未独立验收。
- [并行实施计划与知识可信度边界](blocks/b0cf11a8df577ec6517b41525de869a6aa7b4a9055c0e8b6eebd3e77c4f1c6dd.md): 后续方向是先精简验证、记录和输出流程，再接入 OpenViking，并通过真实 vllm-ascend 开发任务判断是否减少额外调用、文档阅读、等待和重复实验；资源归属、公开脱敏和实际执行结果仍由组件检查。架构与实施 spec 已整理，但功能批次尚未完成，计划并行推进知识贡献与审核、OVPack 发布同步、remote-dev 增强及平台验收。已明确本地经验和公共知识一起返回，审核状态只描述公共内容的处理状态，不作为本地经验的准入门槛或降权依据；所有知识都只是参考，最终判断依赖 Agent 结合当前代码和运行结果。此前拟纳入 Intel Mac 的范围已修正为仅考虑 ARM Mac 和 x86 Windows。
- [四 Agent 交付后的独立验收与待修问题](blocks/f64c4ce6b32cc956e782bc53e54e42544fe6667b01928a8962f1dcafd7824d5a.md): 四个 Agent 均已提交本地交付，四组测试通过；第一组 Grok 先前发现的中文标题覆盖、`dry_run` 删除已有条目、Markdown 索引同步缺口等问题已修复，两个真实 OpenViking 用例及 OVPack 构建、导入、切换和重启用例也通过。独立检查仍复现三处问题：贡献审核只审第一个 Markdown 可能放行整个 PR，remote-dev 在大段输入加 EOF 时截断且中文分页乱码，分发锁超过半小时后可能允许第二个同步进入；这些修复已交回对应 Agent，Windows、真实远端和公共发布流程仍未验证。
- [153–156 联调合入与 OpenViking/NPU 验证状态](blocks/49af68b49c6f0f383e6b570ade71a53ae3cf5270678fce3372eb12b30dd10a20.md): 四个 PR（remote-dev #5、vaws-knowledge #19、vaws-coordinator #10、workspace #134）已合入 main；153–156 的 144 项远程检查、五个客户端 MCP 调用、OpenViking 写入/查询/重启回读，以及 153 上单卡 NPU 矩阵计算均被报告为通过，CPU 参考误差为 0。联调过程中曾出现 Python/ensurepip、SOC_VERSION、构建产物误识别、workspace 绑定和 `ASCEND_OPP_PATH` 缺失等问题，均据记录修正；但完整模型服务、精度/性能、Windows 原生验收、公网贡献发布仍未验证，且主工作区后来仍安装 `vaws-knowledge 0.2.0`，尚未正式启用合并版本。
- [公共知识贡献与 OVPack 发布同步的后续接线](blocks/930a1194cb77e152b328532909fc5797685cd79aa3ee5347675220aa92d87896.md): 当前建议先完成主目录正式启用，再选真实开发任务贯穿查知识、改代码、NPU 验证、保存经验和后续复用；Windows 单独排期。用户要求优先推进公共知识贡献和发布更新，并暂不接入 Grok bot；计划涉及 fork 分支推送、GitHub Release 上传/下载、本地检索与 OVPack 共用鉴权，以及 MCP 存活期间的后台提交和同步。该材料只记录了准备补齐这些接口和人工审核方向，尚未证明真实 GitHub/xAI 流程、自动捕获/脱敏/PR、Release 分发、重复冲突处理或后台同步已经完成。
- [VAWS 架构与 Skill 审计取舍](blocks/3289d0090cc143deb408596f50c9a0ebb41ddbe8d0d4c1566e8fde01db1f1f33.md): 记录了公共知识库、Release 自动同步、客户端接线及相关 PR/CI 的报告性结果，同时明确 Grok bot、原生客户端信任和主工作区启用仍未完成。随后审计将范围收窄到 VAWS，指出 Claude frontmatter、旧入口、职责冲突、知识流程阻塞和重复确认等问题，并建议撤销 machine-management、session-management、remote-code-parity 三个独立入口，精简 repo-init 与 npu-fleet-monitor，将知识整理迁入知识包。后续改动报告称 PD 分组、parity 调用、客户端投影和知识包安装链路已接回或验证，但工作区 CI 曾因旧版本断言失败，最终 profiling 分析测试状态在该块结尾仍未明确；这些均应视为有限范围的报告，不能提升为全面成功。
- [VAWS Skill 清理后的 NPU 验收](blocks/44cfc24fee6766b1f11d0ffccf6a37807eb0e6403b937e97039d21a314adcc90.md): 清理前确认 PD 仍依赖 `session_group.py` 分组文件、远端适配器仍调用 `parity_sync.py`，因此要求先把能力接回业务输入和包接口再删除旧目录；随后报告称六项取舍已落地，并创建知识包 #21 与工作区 #136 草稿 PR。用户要求 NPU 验收后直接合并，当前执行聚焦 PD 配置、托管生命周期和源码准备链路；源码发布快照在真实容器中据报告通过，未改写工作目录，也未触发安装或重编译。双角色 TP2 服务的真实启动、推理、统一停止及资源释放仍在进行中，且该验收不覆盖 KV 连接器性能或传输正确性。
- [真实 NPU 验证暴露并修复多项生命周期问题](blocks/b19bedb53ddc2f627776fc523b2afb599925b155a9bb2866b18224972baecd91.md): A3 上两组 TP=2 服务经历了算子编译、端口分配、源码导入、CLI 参数、状态查询和资源释放等验证；期间发现未声明端口池、任务目录遮蔽 vLLM、旧参数 `--disable-log-requests`、运行状态误报 `launch_pending` 及异步释放显示不当等问题，并分别进行了修复或回归测试。最终记录报告两组服务真实推理成功、4 张 NPU 和两个端口释放、Run Manifest 为 `passed`，三个 PR 的 CI 全绿并已合并；但该记录也明确不包含 KV 传输和性能测试。实机操作仍较不顺手，主要表现为准备阶段黑盒、安装版本与进程加载版本混淆、状态记录不同步、默认返回过大，以及 SSH/HTTP 代理故障容易伪装成业务故障。
- [优化设计落地并完成单次启动生命周期验收](blocks/d50add1a356dd41e9a8aceab208bad5c69b083c38c163410a76c78d78f0b6b79.md): 针对准备进度、日志回读、版本漂移、状态推进、连接诊断和冗余输出的优化已实施；修复了日志文件先引用后创建、重建环境的 checkout 冲突、预检遗漏绑定源码导入路径等问题，并删除了旧 checkout 辅助逻辑及重复响应层。过期 CLI 参数在分配 NPU 前被预检拒绝且记录为 failed；正确参数下，一次 `start` 后由 `status` 推进 manifest，经 starting/running/stopping 到资源释放完成，报告称两组 TP=2 短推理成功且四张 NPU、两个端口和进程组均已释放。三个 PR 被报告为合并，状态输出从约 32.3 KB 降至 3.9 KB；仍未测试 KV 传输与性能，且这些结论属于该批实机记录中的报告性结果。

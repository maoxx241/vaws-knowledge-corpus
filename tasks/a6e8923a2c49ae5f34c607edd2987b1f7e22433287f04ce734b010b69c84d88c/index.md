---
blocks:
- block_id: e20eb95bfd2eee1245764bb0c2df8c6263fbbc9237d423c367bcbfe03ee10026
  indexed: true
  sha256: 4e2a8df4ab77fab33aba412d2b94328dd1060708f6627b808d84b7539163d5dc
  source_range:
    end: 5033757
    part: 0
    start: 0
  summary: 任务基于合并后的全特性源码，在四台隔离容器中启用 FlashComm1、KDA、SiTU、工具调用和推理解析，目标是完成 A3 算子构建、服务启动及 GPQA-Diamond。过程中曾因 zsh 引号、缺少 catlass 子模块、被 `--exclude=build` 排除的 CMake 补丁目录、AppleDouble 文件以及抽象 SoC 名称 `ascend910_93` 导致失败；其中最新已定位到 CMake 扩展不支持该 SoC 名称，并改用 `ascend910_9391` 重新构建。历史及当前结果均未证明构建、服务或 GPQA 已成功，GPQA 仍未启动。
  title: 四机全特性构建与持续跟踪背景
- block_id: 1f224251509d4b17cb43099bfbf29ac44f843c6f1699d4b4dd2a1034a27705d8
  indexed: true
  sha256: 30222521401141c1da7fe3c27ed463292361d919dfb49ae76951265f4a1250d3
  source_range:
    end: 5033757
    part: 1
    start: 0
  summary: 输出质量异常需至少两条独立证据支持，包括复读、乱码、空响应、结构损坏或明显无语义；完整 198 题准确率低于 90% 也属于异常，completed 达到 30 后需按连续检查或理论上无法达标的规则介入。触发后应保留原始结果、SHA、参数和 manifest，精确停止本次 GPQA/服务，停用 `kimi-k3-gpqa`，并在隔离 worktree 中对合并基线及 PR20/16/14/10 做可重复 smoke 二分，再交接给动态确认的 PR 原任务。只有完整 GPQA 达到 198/198、准确率至少 90%、且无 request errors 或 missing/unparsed predictions，才允许在指定 `xyx_vllm` 容器中一次性启动 `quant.sh`；目前这些条件尚未满足。
  title: 异常判定、二分修复与后续量化条件
- block_id: b51d36ec49ac4b4be5673981abf328abfd24b30074eca4554c1efe746182f840
  indexed: true
  sha256: 5db869c9cf62765c9f12efb67e442e5796bed3e048eb6dd53682c60988b44a77
  source_range:
    end: 5033757
    part: 2
    start: 0
  summary: 每次 heartbeat 都必须通知并逐台报告 153/154/155/156 的构建状态、可见步骤、日志时间、失败情况和有证据支持的 ETA；启动后还需报告权重加载、ACLGraph、健康码和服务可用性，GPQA 阶段则报告 completed/198、correct、wrong、request errors、unparsed、准确率及新增样本质量。此前四机对象文件曾增长但不单调，最终构建阶段一致因 `SOC_VERSION ascend910_93 does not support` 失败；失败日志已保留，隔离目录已切换为 `ascend910_9391` 并开始第四轮干净构建。最新记录仅说明新构建已稳定运行约 4 分钟，尚无成功标记、manifest 核验、服务启动或 GPQA 结果。
  title: Heartbeat 详细汇报与最新重建状态
- block_id: 2d30184a075e177b8342fd6a3a55d75cbcc538aec6023038ece2deb24042e029
  indexed: true
  sha256: f8c6174191510bb7fc19b9af8e69205d4322afdb9ff0cfc711af1ad9f6908952
  source_range:
    end: 5033757
    part: 3
    start: 0
  summary: 153–156 四个 rank 均保持 running，Ninja 进程和日志持续更新，临时对象数量经历增长及阶段切换后的回收，未见即时失败或 `.failed` 标记。构建仍未生成最终 `output/build_out`，GPQA 保持 0/198，正确、错误、请求错误和未解析均为 0；剩余时间仅为基于历史和当前速度的估计，不能视为完成确认。
  title: 第四轮四机构建持续推进
- block_id: 87f64e36c590e0c863c45777aaf20b5c69bd28ef720c4a820ce8cd69358aee1b
  indexed: true
  sha256: 37dd30306a21b5422f4de81a43ce50f8612fff16d8a186cfc5ac6e15e492b0c6
  source_range:
    end: 5033757
    part: 4
    start: 0
  summary: 新增规则要求对构建、安装、manifest、启动、请求崩溃及可稳定复现的复读/乱码/空响应等明显失败，从首次错误证据沿调用链和相关 PR diff 正向定位，不得用逐 PR 回退替代分析。只有在确认 GPQA 精度低于 90% 且没有可直接归因的明显运行时或输出损坏证据时，才可在隔离环境按 PR20、PR16、PR14、PR10 边界进行可重复二分；候选归属仍需标明证据和不确定性。
  title: 异常处理改为正向定位优先
- block_id: 5a83481d9cf15120ffb519640de4d3f7b2e825ace950ebb660f4e8a888e4e3a4
  indexed: true
  sha256: abe415b92c94abdd2914f6548f78530cd38077134322aafd88f79fcb645e05fd
  source_range:
    end: 5033757
    part: 5
    start: 0
  summary: 该块重复确认明显失败必须保留原始请求/响应、首个错误、stack trace、rank 日志、SHA、参数、manifest 和二进制，并精确停止本次 GPQA 与对应服务后交接责任 PR；不能猜测责任归属。精度异常才进入固定边界的隔离二分，修复返回后仅合入目标修复并重建复验，精度问题必须完整重跑 198 题恢复至至少 90%。
  title: 异常分流规则再次确认
- block_id: 2b6090e84421798d220a5a2e363371bdab6a96adcd2f4c4aa54a8e4e850b6fe8
  indexed: true
  sha256: 28d64389926be5e40eecdf3200f02f5ef17845ff70a32f9f83a725392fabd466
  source_range:
    end: 5033757
    part: 6
    start: 0
  summary: 明确失败（构建、安装、manifest、启动、健康检查、请求错误或稳定输出损坏）必须从首次证据正向定位，不得进行 PR 二分；只有无明显故障且 GPQA 精度确认低于 90% 时才按 PR20、PR16、PR14、PR10 的隔离边界二分。要求保留原始请求响应、stack trace、rank 日志、SHA、启动参数、manifest 和二进制，并将责任组件、证据和修复要求交给对应 PR agent。修复后目标任务仅合入该修复并隔离重建复验，精度问题必须完整重跑 198 题且恢复至 accuracy >= 90%。
  title: 异常分流与 PR 交接规则
- block_id: 68fa69b55ea751e4897283e367f5e9ea9f717235d63832c170a8718016334657
  indexed: true
  sha256: ab390e45f9dd8da7b0fb2bb7b7e087df030908646a39aa610be9a3c40e720912
  source_range:
    end: 5033757
    part: 7
    start: 0
  summary: 四机算子构建、manifest 引用核验和服务拉起曾报告通过，但后续启动在 rank1 的 `profile_run/determine_available_memory` 暴露首次明确错误：A3 A8W4 `aclnnGroupedMatmulWeightNz` 收到 `INT8×INT4→INT32`，返回 161002；调用链定位到 `moe_mlp.py:440` 的 SiTU raw-GMM 路径，并归属于 PR16 commit `cab9422940`。服务已精确停止、证据固化，GPQA 保持 0/198，未进行 PR 二分；随后 PR16 回传修复 commit `543f73cd9b01a908966343f757e652da2100049e`，将 SiTU GMM 输出改为 BF16、调整 DequantSituQuant 契约，并报告隔离单算子、真实 routed/shared TP16、TP8 服务 smoke 均通过，但 GitHub checks 仍 pending，修复尚未在当前 allfeatures 分支复验。
  title: K3 启动失败与 PR16 修复回传
- block_id: ccaf7f7f7bd5d95e8e8de5dee78c67fc837ae89639add9a25a4187538fd726ba
  indexed: true
  sha256: 1a741598ed3abc5b029859da09c70871098ce9cd74ee2a5a8e4a5ac0755f7ede
  source_range:
    end: 5033757
    part: 8
    start: 0
  summary: 当前任务范围限定为指定的 k3-allfeatures-153..156、allfeatures 日志和结果；修复后应仅 cherry-pick PR16 修复，重建受影响的 `dequant_situ_quant` 与 Torch binding，重启四机并先复测语义 smoke，再运行完整 GPQA。只有 198/198、accuracy >= 90%、无 request errors 且无 missing/unparsed predictions 时才可启动量化；此前启动失败意味着服务、GPQA 和量化均未达成。所有结论仍属易错检索参考，需保留失败尝试、未完成状态、隔离验证与后续更正，不能把报告中的 smoke 或计划提升为当前任务成功。
  title: 后续复验范围与量化门槛
- block_id: 9e4212688abd1c5146f9335e4ce3a53d2e9dc84b8c7e3e6b3427cc5d32b4335c
  indexed: true
  sha256: 6189f13b16387461a1e676021e7cb992aea2fe6e3d39695f29f96b7540fa78ab
  source_range:
    end: 5033757
    part: 9
    start: 0
  summary: 四机首次修复构建和启动准备曾通过，但 retry3 在 profile_run 暴露新的 `aclnnDequantSituQuant` 注册问题：运行时仅有 INT32 + FLOAT optional 签名，缺少 BF16 + 全部 optional=None。随后确认根因是 editable build 复用 `csrc/build`，PR16 提交 `9c04ee75...` 将清理范围扩展为 `build output build_out`；当前 clean retry 正在进行，尚未证明整网验证成功。
  title: PR16 构建失效与 retry3 定位
- block_id: 081a45a6832c0780d514af9ca36dfb6294eb2e710495407c08ea917f639355f9
  indexed: true
  sha256: 92ca8aad5fb7f5c5b8f180a9f7c116ad92904542391a53b34d496bb1c849a4c9
  source_range:
    end: 5033757
    part: 10
    start: 0
  summary: 构建失效修复已合入本地分支 `ffb3f86ee`，四台脚本和 UT 已同步；全量 47-op clean build 最终四台均生成 `.success`，manifest、Torch binding、kernel library SHA 一致。已安装包的 BF16 + optional=None DequantSituQuant NPU 测试通过 3 个 shape，随后 TP16×DP4×EP64 服务健康至 HTTP 200，但两条独立 temperature=0 语义 smoke 都出现空内容和 `The!!!!...` 复读，GPQA 保持 0/198，服务已停止。正向定位指向 PR10 的 recurrent KDA decode 替换路径，而 PR16 的 CI 红灯被判断为 PR20 无关 KDA 测试格式问题。
  title: PR16 clean build 与整网 smoke
- block_id: be4f917d6f7d3fa1827de380fc692441a100df0289eacc9518f44f7db1cc8029
  indexed: true
  sha256: 223e992b62e5a34f92cf211f933f42f53a148d433053ec8ff57464b2827bed9e
  source_range:
    end: 5033757
    part: 11
    start: 0
  summary: PR10 修复 `f9a038e1...` 仅应用于 combined 隔离分支，将 recurrent KDA schema 从返回 alias state 改为 mutable input state 与单一非 alias output；隔离验证报告 compiled/eager output、state 差异为 0，并通过多步 A3 用例，但尚未单独证明 `The!!!!` 已消失。四台 binding 重建最终成功并一致为新 SHA，manifest/kernel 未变；服务 retry5 已启动并完成 16/16 worker 与 EP64 映射，当前停在全局 HCCL barrier 后、尚未进入权重加载。
  title: PR10 alias schema 修复与 retry5
- block_id: 309f0fe37bef86e02a5b151a5626ee22a438a1d2c77b2114d6c25fe4fc12b737
  indexed: true
  sha256: 00e01ea09d050b3d55a1faca24adda1a1865c2faf8beb2d53e6d8fc9ed142014
  source_range:
    end: 5033757
    part: 12
    start: 0
  summary: retry5 在 EP 映射后进入全局 HCCL barrier，约 45 秒没有新日志，但四台每台 17/17 相关进程仍活跃，没有退出、zombie 或错误。当前不能判定服务卡死或验证成功；若继续无进展，应检查 worker 状态、CPU 和 HCCL 日志，暂不盲目重启。
  title: retry5 HCCL barrier 等待
- block_id: 88390d00ab1bc259c4a96178f779864d93e4af672d79535af495453b77a4b078
  indexed: true
  sha256: c1fc14f424c1cf3332a45a36af2b5dc3bdfd49d1f716c07fc3b988b0fd07f84c
  source_range:
    end: 10211094
    part: 0
    start: 5033757
  summary: PR10 alias-return 修复后的四机复验虽完成权重加载、torch.compile、ACLGraph 2/2 和 health=200，但两条 temperature=0 smoke 仍返回 HTTP 200、content=null 及 `The!!!!...`，GPQA 保持 0/198，服务已精确停止。随后应用 graph-safe 诊断补丁 8f7eb60c，并固定所有构建使用 `MAX_JOBS=256/-j256`；首次诊断启动因 `dt_bias` 从 `[768]` 拷贝到 `[6,128]` 触发 `aclnnInplaceCopy 161002`，修复提交 5f1b3f45 将 buffer 改为 flat 后，第二轮成功健康并复现异常。由于 worker 没有 `LOCAL_RANK/RANK` 环境变量，初次采样未生成 `.pt`；follow-up 1ead7af3 改为按 PID 保存每个 worker 的两步 trace，但第三轮启动期间又切换为单机渐层/逐层真实权重定位，四机已停止。
  title: PR10 复验失败与诊断补丁启动
- block_id: ccfc2367adfd1dea99e97f9895db22ca665f040142f21852cda8471b8661e534
  indexed: true
  sha256: 993b1d69a60f0a195206d7bfd5911cd68d7f98b50411aa015c17cf2420577769
  source_range:
    end: 10211094
    part: 1
    start: 5033757
  summary: 诊断补丁阶段确认真实 recurrent KDA state 使用 FP32 binary；BF16 state 的 manifest 缺失并非运行时故障。修正 `dt_bias` shape 后四机成功完成 352/352 权重加载、编译和 ACLGraph 2/2，但 smoke 仍出现 `The!!!!`，且 trace 导出因 worker 缺少 rank 环境而得到 0 个文件。随后单机五层真实权重定位证明 eager/TP16 下 KDA 与 Triton 对齐、跨步 state continuity 正常，失败限定在 FULL_DECODE_ONLY graph padding：容量 16/32 而真实 token 为 1、`cu_seqlens=[0,1,1,...]`，kernel 的 `ValidateCuSeqlens` 提前返回，导致 empty_like 输出未写入和 active state 未更新；红测记录 49,583 个 state mismatch、最大绝对误差 0.5641283989，证据已更新到 Draft PR10 head `9792e108`，未修改 kernel。
  title: 诊断采样失败与 KDA 图填充根因
- block_id: a06e8e06fd5c77b225c5533983fdbb687110d265556ec9fb858104b6365e89af
  indexed: true
  sha256: 2e66340f6b05a69a0522ef20be22d068f1304d149b60c7d406560b208f86e195
  source_range:
    end: 10211094
    part: 2
    start: 5033757
  summary: PR14 验证原计划从最新 `ci-new` `f72b35e6a` 叠加 PR14 head `33deb2609`，但 155 的逻辑 NPU 8–15 被量化任务占用，因此四机验证暂停让量化继续，后改为仅在 153 做五层 TP16 单机验证。153 的隔离 editable build 固定使用 `MAX_JOBS=256/-j256`，长尾 Compressor/SparseFlashAttention OPC 持续生成 kernel，曾记录约 1243 个完成边、约 61 个待执行步骤且无 `FAILED/error/traceback`，但截至该块末尾编译仍未完成、服务尚未启动。第三轮四机诊断已按精确 PID/PGID 停止，8077 清空、GPQA 为 0/198；当前策略是不再自行重启四机，等待单机渐层/逐层真实权重诊断。
  title: PR14 单机验证准备与中止状态
- block_id: 68469341b381029d7cf5061ca284f5c8e665ce8fffda62ad0d1e6ff53974c837
  indexed: true
  sha256: d78c518458946a40deefdcb4579c280f94bfe94fd3a0da2849d5b7a0e478e80e
  source_range:
    end: 10211094
    part: 3
    start: 5033757
  summary: PR14 + latest ci-new 的 153 单机五层服务经过多次配置、tokenizer 和裁剪元数据问题后，以 `hf_overrides.text_config.num_hidden_layers=5` 成功启动，health=200；但五层权重不具备精度或语义判断价值，三类请求出现 `content=None`、空 content、无 tool call 和 `finish_reason=length`，未被认定为 PR14 失败。155 上量化随后完成 93/93 层合并，347/347 个 safetensors 及 index/config/tokenizer 核验存在；153 服务被精确停止并留下无执行能力的 zombie 条目。之后收到 PR10 clean kernel 验证结果（15 passed），并开始 latest ci-new + PR16 + PR10 的隔离组合构建；首次构建因缺少 Catlass 子模块提前失败，补齐后以 256 并行继续，目标为 MMMU-Pro、BFCL 工具调用和完整 GPQA。
  title: PR14 单机链路通过、量化完成与后续组合验证
- block_id: 8b2efcebb5e222ce95c85aff9b520807c0beff61b924fb48034ba568d9b72e3e
  indexed: true
  sha256: 71823400eb90f53e35e01f3e1e50157eee385e0efc45341d55afde66a933d1ad
  source_range:
    end: 10211094
    part: 4
    start: 5033757
  summary: 组合验证固定使用 latest ci-new `433bf0330`、PR16 `bdd426c4d` 和 PR10 `8a7f42d8a`，形成提交 `17a7a4d5b`，范围限定在新建的 `k3-pr16-pr10-153..156` 隔离环境；构建要求 `MAX_JOBS=256`、`OPS_CPU_NUMBER=256`、`CMAKE_BUILD_PARALLEL_LEVEL=256` 和显式 `-j256`，禁止复用旧 recurrent KDA kernel。早期 clean build 曾因 Git archive 缺少 `csrc/third_party/catlass` 而在编译前停止，补齐锁定子模块 `41bf90da` 后构建持续推进，期间无失败标记但尚未产出最终包；计划先核验 recurrent KDA/DequantSituQuant，再启动 TP16×DP4×EP64 服务。评测计划是官方 MMMU-Pro vision 前 5 条、BFCL `simple_python` 前 5 条，随后 GPQA-Diamond 198 题，要求分别保留原始请求/响应和结构化评分。
  title: 组合分支 clean build 进行中及验证范围
- block_id: 2d10bbbd059ed04f9b773499255b38a3e7b59b502a4b0a942980270305f57ef1
  indexed: true
  sha256: 4297bf557f44b175bbd52dd426019d72a01a92bcea2822ec99d621f7b57a31f7
  source_range:
    end: 10211094
    part: 5
    start: 5033757
  summary: 干净构建于 03:45:21 UTC 成功结束；manifest 注册 `RecurrentKda` 与 `DequantSituQuant`，368/368 引用存在，PR10 graph-padding 为 `1 passed`，PR16 DequantSituQuant/真实 WeightNz SiTU 定向测试为 `18 passed`，且 recurrent kernel SHA 为 `5f09a9af…`、`5ec956be…`。验证产物已按哈希同步到 154–156，随后四机以 TP16×DP4×EP64、FlashComm1、vision_chunk、FULL_DECODE_ONLY 和 prefix cache off 启动；四个 rank 均存活，先后完成 worker 初始化并进入完整权重加载。最新证据显示 Rank 0 加载到 79/352 shard、health 仍为 `000`，尚未进入 ACLGraph 捕获或评测，因此 MMMU-Pro、BFCL 和 GPQA 尚无结果；服务确认指向用户提供的经脱敏 multicard 权重路径。
  title: 算子门禁通过，四机完整权重服务启动中
- block_id: 900b2862a0625e607364f91fcc66b31d774596139bc42d3bd426fb82768ac433
  indexed: true
  sha256: 1f449db9e8a9eabb257fd547d3d0f294df88255407d0816aea799e572a8fc686
  source_range:
    end: 10211094
    part: 6
    start: 5033757
  summary: 隔离四机 TP16×DP4×EP64 服务已完成 multicard 权重加载、torch.compile、KV-cache profile 和 FULL_DECODE_ONLY ACLGraph `[16,32]` 捕获，rank0 health=200；此前 Rank 2 延迟加载但未见退出、OOM、traceback、缺失 binary 或运行时 ERROR。官方 MMMU-Pro standard vision split 前 5 条已由 runner PID `182108` 启动，初始进度为 `0/5`，尚无可报告的正确数、准确率或最终成功结论。后续顺序仍为 BFCL simple_python 前 5 条，再视两组 smoke 结果运行 GPQA-Diamond 198 题；每分钟需报告进度、错误、未解析和准确率。
  title: 四机服务 health=200，MMMU-Pro 评测已启动
- block_id: 7b13b850c7c4a83f25c3ef1c476ba9704fa7bc54d01a1f11f6f3532a6bb9d0dd
  indexed: true
  sha256: f28792e1adf32bc5a0e3fa497c5f35b8398a3b54e20a1e043799668bc3a2f75e
  source_range:
    end: 13431833
    part: 0
    start: 10211094
  summary: MMMU-Pro 从容器无法访问 Hugging Face 的首次失败改用本地 parquet/data URL 重跑，最终 5 条完成、4 正确 1 错误，准确率 80%，errors=0、unparsed=0；期间四机 health=200。无工具请求多次出现 tool-parser fallback ERROR，随后 BFCL 5 条均 HTTP 200 但没有 tool_calls，其中第 5 条生成了正确工具协议文本却留在 content；完整 GPQA 因结构异常暂停，并将证据交给 Tool Use 任务只读分析。
  title: MMMU-Pro完成并暴露Tool Parser回退
- block_id: c340cfa41124907f69cafa8d846450384a4d905c73c110f8cb08f3fee002320d
  indexed: true
  sha256: eee0e1c924df6b43ba5c840c82668c3d8390e1bc637710e8a8fa7a5832660c48
  source_range:
    end: 13431833
    part: 1
    start: 10211094
  summary: BFCL 已完成 5/5，passed=0、failed=5、request errors=0、unparsed tool_calls=5；前 4 条表现为模型直接作答，第 5 条则是正确调用协议未被 parser 抽取并泄露到 content。独立 GPQA 两条 smoke 均 HTTP 成功且生成大量 token，但 API content/output 为空，prediction=None，最终 unparsed=2、可计分准确率为 0%；完整 GPQA 保持未启动。
  title: BFCL未解析与GPQA两条Smoke失败
- block_id: 140e0c253735b58aac1bb33b10f8c0ce174cc630a538662c0fa4f10d6404d65d
  indexed: true
  sha256: 4754cd52e80aa22c31684965fda98bf0a43f49e06a55f3aae31f721f9e22e073
  source_range:
    end: 13431833
    part: 2
    start: 10211094
  summary: 只读分析将共同主因定位为 K3 detokenization 默认 spaces_between_special_tokens=True：连续 XTML marker 被改成带空格形式，严格 reasoning/tool parser 因此无法匹配，导致内容落入 reasoning、content 为空及工具调用不抽取。另有无 tools 请求被 adjust_request 将 tool_choice 从 none 改为 auto，触发 serving.py:1237 fallback ERROR；建议在 K3 parser adjust_request 中设置 skip_special_tokens=False、spaces_between_special_tokens=False，并保留原始 none 语义，但这些仍是待实施和验证的修复建议。临时请求级绕过可显式设置 spaces_between_special_tokens=false，必要时 tool_choice=null；正式默认行为尚未验收。
  title: K3 marker空格与tool_choice控制流根因
- block_id: a903dd02f031ce6b10b8c7fb0052ce7fee5cee642de522c6f5402cc6775d7e4f
  indexed: true
  sha256: 62d3f234e965ca2d70ae9876e17c497cb5e38d7a5ef8ed7428bda316ef83b4e1
  source_range:
    end: 13431833
    part: 3
    start: 10211094
  summary: 四机服务持续 health=200、rank 存活且无新增 ERROR，但 MMMU-Pro 为 4/5、BFCL 为 0/5 且 5 条未抽取，GPQA smoke 为 2/2 未解析，完整 GPQA 未启动。Tool Use 审计围绕 tokenize=False 的二次编码、EncodeSegment.allow_special 信任边界、特殊 token 解码、多模态 placeholder 和无 tools 控制流展开，期间没有形成可应用修复 commit，也未修改或重启服务。
  title: 四机健康但评测与解析仍阻塞
- block_id: 89b93bc9a140ff8114cf8cb247868ab8a6f9bcca2e1d3eb8e688064b5570661a
  indexed: true
  sha256: 202ae1b67bf8a7fdb162340fe99d227155d9f7b81baacdb2628385133c18be66
  source_range:
    end: 13431833
    part: 4
    start: 10211094
  summary: 只读审计确认 renderer 使用 tokenize=False 拼接完整字符串后再由 vLLM 编码，会丢失 EncodeSegment.allow_special 边界，使用户伪造 XTML marker 可能成为真实控制 token；同时 detokenizer 插入特殊 token 间空格和 none→auto→ERROR fallback 被列为独立 P0/控制流缺陷。历史实现对照指向模型级 ParserEngine、每 choice 独立状态、marker holdback、显式 flush 及增量 arguments，但尚未产出补丁或 commit，完整 GPQA 继续暂停。
  title: 确认 K3 renderer 信任边界为 P0
- block_id: c82578092a2eb9b08d4b9e1c086bc27dbb9524e86c93299700d57aefca9f0798
  indexed: true
  sha256: 69621dac56ec09d59f13264777f269edc3a8ff59a61a5e63da5fb28cbe0737a3
  source_range:
    end: 13431833
    part: 5
    start: 10211094
  summary: 后续快照确认四机仍 health=200、无新增错误，MMMU-Pro 维持 5/5 中 4 对、BFCL 0/5、GPQA smoke 0/2 且 unparsed=2；Tool Use 审计停止扩大范围，但未完成补丁实现或验证。新增只读审计结论标记 MUST FIX：公共 K3 adjustment/to_sampling_params 路径需同时设置 skip_special_tokens=False 与 spaces_between_special_tokens=False，并要求 content_format='openai'、结构化 image parts、可信 image_prompts、强制 tokenize=True 且 sync/async 共用实现；这些建议尚未在服务上操作、重启、请求或验收。
  title: 审计收敛但正式修复仍未验证
- block_id: dccc5138b6b5e805e81f39e1ee5973eda45f1354cbf7346a5f62fb9c27c8cd8f
  indexed: true
  sha256: aedc0dc4cc058f2f9845c3010d6b2d07ae71fae4fe7775a3ddb7f86f384a3fae
  source_range:
    end: 13431833
    part: 6
    start: 10211094
  summary: 审计确认 none→auto 会导致 `cannot determine...` ERROR，none→None 只是利用 Parser 的 auto sentinel 偶然消错，不能作为正式修复；required/named 遇未知工具或非法参数、非法 JSON、缺少 tools end 等路径也可能产生空成功或 stream/nonstream 不一致。拟在 v0.23 ParserManager 的 K3-local 专用入口实现统一 XTML 状态机，覆盖 reasoning、content、tool calls、增量参数和显式 flush，并严格验证真实 tokenizer/detokenizer、chunk 边界、多模态、tool_choice、response_format 与 reasoning 过滤。GPQA 证据仍是 HTTP 200 但 output 为空、missing=2；MMMU-Pro/BFCL 结构门禁及完整 GPQA 均未达标。
  title: K3 Tool Use 审计与统一 XTML 状态机方案
- block_id: d74c6d90b58d7e848827e4b00a29b1dc99c8d8d7632c0ec23d77fc4a6a5e53c1
  indexed: true
  sha256: 32a08ce0a547dcf4147d0601773f3697d4b779638d5c65d6e1369abccc8b56aa
  source_range:
    end: 13431833
    part: 7
    start: 10211094
  summary: Tool Use 两路审计维持 MUST FIX，P0 包括复合 XTML marker 被 detokenizer 插入空格，以及 renderer `tokenize=False` 二次编码丢失 `EncodeSegment.allow_special` 信任边界；另有 none→auto→ERROR、required/named 空成功和语法校验问题。修复限定在独立分支与 worktree，当前 combined 源码和四机服务未修改；需先取得 commit、diff、验证证据和应用步骤，再精确应用、必要时按 `-j256` 重建并重启。05:21 已报告第一轮本地纯状态机 smoke 通过，但官方 K3 tokenizer 与 vLLM 0.23 真实 incremental detokenizer 验证尚未完成。
  title: 隔离 Tool Use 修复推进，四机验证继续暂停
- block_id: dd9b05559ffbfc14e94fda9ad93fdff0644d38701a84adcb2830fdc3cfac7543
  indexed: true
  sha256: ad7bfbc026112ef9b5588496bfa737803315a911370a90159ad7aa301634a421
  source_range:
    end: 13431833
    part: 8
    start: 10211094
  summary: 收到正式修复后必须先复核 diff，仅应用到 combined 隔离源码；Python-only 可免 native rebuild，若触发编译需保持 MAX_JOBS=256、OPS_CPU_NUMBER=256、CMAKE_BUILD_PARALLEL_LEVEL=256 并显式 `-j256`，随后按 exact PID/PGID 精确重启四机。默认请求需依次验证 no-tools reasoning/content、BFCL case5、两条 GPQA，再重跑 MMMU-Pro 5 与 BFCL 5，要求无 marker 泄漏、无 `cannot determine`、正确 tool call、GPQA output 非空且 missing=0。只有结构门禁全部通过后才可运行 GPQA-Diamond 198 题；此前完整 GPQA 仍暂停，目标 `>=90%`、`errors=0`、`unparsed=0` 尚无证据。
  title: 修复后的分阶段门禁与完整 GPQA 前置条件
- block_id: 102cf65ed5a53f8c5b3f30dd59b179f62b92c630900db8070a8bc54d1f2c6ec2
  indexed: true
  sha256: 17ffacee2c5f0e7725039812d1f8906532aabc5b9655d06dbde92185e40dde1e
  source_range:
    end: 13431833
    part: 9
    start: 10211094
  summary: 四台 k3-pr16-pr10 服务持续存活且 rank0 health=200、无新增 ERROR；累计 MMMU-Pro 4/5、BFCL 0/5、GPQA smoke 0/2 且 unparsed=2，完整 GPQA 仍为 0/198。Tool Use 真实 tokenizer/detokenizer 验证期间曾因 tiktoken.model 下载 401 而访问 lala-153-k3，创建临时目录并运行一次已结束 pytest；该范围违规已停止，遗留目录未删除且未再访问。之后已切换至本地权重、官方 tokenization_kimi.py/encoding_k3.py、tiktoken.model、vLLM 0.23.0 detokenizer_utils.py 的合规路径，但早期命令尚无明确通过/失败结论。
  title: 四机健康但 Tool Use 曾越界
- block_id: 921c8f3675c442740d57754877020f5bd57f3b69b5df94c604bd74c6ef9cb9f2
  indexed: true
  sha256: 42e4da37fc45afeb66b701774f4cb70c7ba7e9fe50acc31ca2c4f240f12cfe5b
  source_range:
    end: 13431833
    part: 10
    start: 10211094
  summary: 05:30–05:41 期间四机状态和累计评测结果保持不变：4/4 exact serve、health=200、无新增 ERROR，且没有评测 runner。Tool Use 未发布新的实现或测试结论；本地真实 tokenizer/detokenizer 命令先后被报告为执行中、已完成但等待摘要，不能据此判定门禁通过。远端容器测试保持停用，lala 遗留目录未处理。
  title: 本地验证等待汇总
- block_id: c493d2d4fe5a5905047151077561f7ce1d02c5a66a0cf68445b1b07760a2df58
  indexed: true
  sha256: a99d4bc28129095dc2e9033ccc8235e9789ac51016ed608ad4bf91c4531324d1
  source_range:
    end: 13431833
    part: 11
    start: 10211094
  summary: 审计要求修复复合 XTML marker 插空格、renderer tokenize=False 丢失 EncodeSegment.allow_special 信任边界，以及 none→auto→ERROR、required/named 空成功、stream/nonstream 不一致、非法语法和 response_format 等问题；完整 GPQA 必须暂停，不能用请求级 workaround 冒充修复。必须先取得独立分支的修复 commit、验证证据和应用步骤，再复核并仅应用到 combined 隔离源码，按需精确重启四机并复验 no-tools、BFCL case5、两条 GPQA、MMMU-Pro 5 和 BFCL 5，结构门禁通过后才可运行 GPQA-Diamond 198题。后续委托消息声称隔离分支修复已完成并提交且未访问四机 API 或容器，但未提供 commit、测试结果或正式验收证据。
  title: 正式修复与验收门禁
- block_id: 21f4275efd13a7c097f16af666ac147d29c09393895b16111496827c7c793dd8
  indexed: true
  sha256: c3cf357750c5fda81ef81363f3eddd2a34a1e300d33398f2f7c98d198fa6200f
  source_range:
    end: 13431833
    part: 12
    start: 10211094
  summary: 记录声称分支 `agent/kimi-k3-tool-use` 的 commit `36fe4a442` 完成 K3 XTML 状态机、严格 tool_choice/参数错误处理、tokenizer-owned renderer、图像输入及 vLLM 0.23 `auto` typed key 修复；隔离环境报告 64 passed，pre-commit、py_compile 和 diff check 通过，但宿主机 pytest 因缺少 `torch` 无法启动。修复被 cherry-pick 为 combined `2f7b38ee9` 并同步到四机，旧服务按 exact PGID 停止、新服务启动；截至该块末尾仍在 EngineCore/模型初始化阶段，health 尚未就绪，且未执行四机 live GPQA/MMMU/BFCL。
  title: Tool Use 修复提交与四机重启监控
- block_id: 7bcc0dc023409c325ca20c4e8f3cc3ce95d2da4c9b6665b8c51ed4c268323219
  indexed: true
  sha256: 580093da432fda008ca79c41406444ecbe8f87cf05f9c55bc022fe636c053015
  source_range:
    end: 13431833
    part: 13
    start: 10211094
  summary: 四个 exact serve 进程 `209763`、`171564`、`157088`、`156857` 持续存活，服务从 EngineCore 初始化推进到 352 个 safetensors shard 加载完成、每 rank 分配 `180,645 tokens` KV cache，并开始 `FULL_DECODE_ONLY` 两档 ACLGraph capture。多次日志检查报告 `ERROR/Traceback/Exception=0`，但 `18079/health` 在该阶段仍为 `000`，评测和解析门禁尚未开始；建图期间出现等待通信及共享内存广播信息，尚不足以证明无问题或最终成功。
  title: 四机权重加载与建图推进
- block_id: fed75c10d6b0c1f9a695b1ce4c07fab6b7841a7a5144d54117a6bbd73669035f
  indexed: true
  sha256: 964b903bf63ff331782857f8b2d6b74c90e153667c8d36dcd0d6f43a6fd9c80d
  source_range:
    end: 13431833
    part: 14
    start: 10211094
  summary: 后续记录声称四个 DP rank 已完成两档 ACLGraph capture，且在 153 主机本地及容器内 `/health` 返回 200；管理网直连 `18079` 超时被归因于外部访问路径，但这一解释尚未形成独立验证结论。计划按默认请求依次检查 no-tools thinking、BFCL case5、两条 GPQA parse smoke，再决定是否重跑 MMMU-Pro/BFCL 和完整 GPQA；截至块末尾仅表示已开始第一条 no-tools 请求并保存原始请求/响应，尚无响应结果或结构门禁结论。
  title: 建图完成并开始默认解析门禁
- block_id: 932757d26a26402c9ef493d875f4555af479b39b1ef9037d0c0dd45e0e623125
  indexed: true
  sha256: 1e55386b978a11fb5c3f04a86e96fd925f3938d2cad6e9067671bff4008cc1f6
  source_range:
    end: 16686806
    part: 0
    start: 13431833
  summary: 四机服务据报告 health=200、进程存活且无新增运行时异常；no-tools 与 BFCL case5 解析门禁此前通过。旧 GPQA runner 因使用被禁止的 `chat_template_kwargs.thinking` 连续返回 HTTP 400，随后依据只读契约核对改为顶层 `reasoning_effort=max`，删除该模板参数并通过 `py_compile`；两题 typed smoke 当时仍在生成，完整 GPQA 保持 0/198。
  title: GPQA typed 契约修正后复验启动
- block_id: 77fde564747322581294fc435d5aab66d6566c611474cebce9f9f6af2840a376
  indexed: true
  sha256: 3631fe38fc67f2342b4a4f9007b77363ccda5dcada2b026ab1e0be1bea16dac9
  source_range:
    end: 16686806
    part: 1
    start: 13431833
  summary: typed GPQA 两题复验从 0/2 逐步完成为 2/2，报告称均 HTTP 200、`finish_reason=stop`、`Answer:` 可解析、errors=0、missing/unparsed=0、准确率 100%，且使用 `reasoning_effort=max`；这些是运行记录中的门禁结果。随后启动官方 MMMU-Pro 前 5 条复验，首条 `test_History_1` 报告 prediction/answer 均为 B，reasoning/content 分栏正常且无 XTML 泄漏，进度达到 1/5，BFCL 5 条及完整 GPQA 仍未开始。
  title: typed GPQA smoke 通过并启动 MMMU-Pro 复验
- block_id: d9bea642ad02db2a0469bbdb0012d81ab874e76f9aac3bfcb4e3e55fa1149d98
  indexed: true
  sha256: 3d53e880244d5541aa0785ee17a08dc5dac9825195e64dbee84eca503fd31e5b
  source_range:
    end: 16686806
    part: 2
    start: 13431833
  summary: MMMU-Pro 首轮最终报告为 5/5：4 条正确，`test_Art_113` 因 runner 硬编码 `timeout=180` 触发 `TimeoutError('timed out')`，因此首轮 errors=1、unparsed=1；记录称服务端仍以约 8.8 tokens/s 生成并正常释放请求，health=200 且无日志异常，但这不能把该 case 视为成功。已准备超时改为 900 秒的独立 runner，并对同一 case 使用相同图片、问题、采样配置和 `reasoning_effort=max` 定向复验；当时进度为 0/1，BFCL 与完整 GPQA 仍待后续。
  title: MMMU-Pro 暴露客户端超时并进行定向复验
- block_id: 14dd3f8574a5ea9ac470aa632d66c5f76377064959824b05c7445eddeaf18d97
  indexed: true
  sha256: ff406db29f536bd6c53edd223b216a4d9f8b5c97bc269b225283977922a33b8c
  source_range:
    end: 16686806
    part: 3
    start: 13431833
  summary: 记录 combined commit 2f7b38ee9 的 K3 parser/renderer/chat-params 与 Tool Use 修复、四机文件 SHA/py_compile、服务重启约束及 health/日志检查；本机 pytest 因缺 torch 未启动，不应视为失败。MMMU-Pro 为 4/5（80%，1 个正常答案错误），BFCL 前 5 条协议结构均通过但严格 tool-call 仅 1/5；完整 GPQA 从 0/198 启动，随后出现 68/198、64 对 4 错、errors=0、unparsed=0 等增量结果，仍未完成。后续核对显示 reasoning_effort=max 实际生效，但 runner 未保存 reasoning 字段；当前使用 top_p=0.95 与 Kimi 官方博客的 top_p=1.0 不一致，且官方专属后处理规则尚未完全公开。
  title: 四机修复验证与 GPQA 运行记录
- block_id: 629c05f4dc835ba108bd22d5c5c1dcebdfe2cf0e5ea05a0fc37a7a5401bc4aa1
  indexed: true
  sha256: 7aa86d58b9bdbc7c9abc31f05858e12305aa1376d9703bf20eb80e0ca6461c0e
  source_range:
    end: 16686806
    part: 4
    start: 13431833
  summary: 该块复述同一版本、四机部署和隔离范围，并记录 GPQA 从 16/198（15 对 1 错）推进到 32/198（29 对 3 错），期间 request errors 与 unparsed 均为 0、服务 health=200、四份日志无 ERROR/Traceback/Exception。用户质疑 think 未开启后，记录称请求明确使用 reasoning_effort=max，错题更像模型知识/推理判断错误而非 parser 或服务异常；temperature=1.0 但当前采样为官方配置之外的 top_p=0.95。后续进一步要求查 Kimi K3 官方博客参数，并计划在本轮完成后用仅公开参数独立重测。
  title: GPQA 增量精度与服务健康
- block_id: d4b7762f985c1439ce13d856c6a9e609b51f53d0181e868e1828f4e02cbe1d9a
  indexed: true
  sha256: b266ec8f82daf28142cf92eee9be7175c2867ebb65bcc41654d1054ef7926a41
  source_range:
    end: 16686806
    part: 5
    start: 13431833
  summary: 记录将 Kimi K3 官方博客与 lm-eval 默认口径区分开：公开参数为 reasoning_effort=max、temperature=1.0、top_p=1.0，而 GPQA 专属 prompt、选项排列、抽取 regex、stop/retry 规则及其他采样参数未被确认公开。当前 runner 曾用 top_p=0.95、top_k=20、min_p=0、presence_penalty=1.5、max_tokens=65536，并以 re.search 抽取第一个 Answer；对已完成样本的只读重扫未发现首末答案差异，因此尚无证据表明已有错题由 regex 造成。随后结果推进至 73/198、69 对 4 错、94.52%，仍在运行；计划先归档该轮，再建立独立目录，仅配置三个已知官方生成参数重跑 198 题，当前尚未报告完成结果。
  title: 官方参数口径与后续重测计划
- block_id: aaba1295357003c13b6c31acc898d69e38eb1f6e70aa41836e0d3deb05ada964
  indexed: true
  sha256: 85e49ae404e6adf28f33701116ea4f1fdc9ce577a56171677049be80006038c6
  source_range:
    end: 16686806
    part: 6
    start: 13431833
  summary: 完整 GPQA 记录从 74/198（70 对 4 错）推进到 114/198（106 对 8 错，92.98%），期间出现 id=91、106、113 等模型答案错误，但 errors=0、unparsed=0。四个精确 rank、health=200、runner 和服务日志持续正常；部分 ETA 只是按均速粗估，最终 summary 尚未生成，不能视为完成验收。记录还显示 reasoning 输出可达数千至 12,677 tokens，通常 finish_reason=stop 且输出非空。
  title: GPQA 中段进度与运行状态
- block_id: 7b76664f843d1c918db7918223a393dfa959023a567351728249825f17a0595f
  indexed: true
  sha256: fbde18754fddc5a32f2a0c22935cba1ab61e1e15e2a60798aedd3fd5cebe7279
  source_range:
    end: 16686806
    part: 7
    start: 13431833
  summary: 旧服务按 exact PGID 终止并确认 exact serve=0、端口 18079 未监听；新服务使用 multicard TP16×DP4×EP64、FULL_DECODE_ONLY [16,32]、prefix cache off 及指定 DP RPC 端口，四个 rank 进入 EngineCore 初始化，初始阶段尚未确认 weight loading、ACLGraph 或 health。修复版本为 combined commit 2f7b38ee9，记录称 Python 文件同步、SHA、py_compile、Tool Use 隔离测试和 pre-commit 已通过，但本机 pytest 因缺 torch 无法启动。后续门禁要求 health=200 且日志无异常，再依次检查 no-tools thinking、BFCL solve_quadratic_equation、GPQA parse smoke、MMMU-Pro/BFCL 前五；完整 GPQA 及官方参数复测计划在材料中仍未完成验证。
  title: 四机修复服务与验证门禁
- block_id: 87e6150fe1eb601271f41c42c56df350bc693a83bb1ae713299817767bef0201
  indexed: true
  sha256: b268ccb096460fa4089119abaddf65969ce999ee8af42d25ec7669a2c5644da6
  source_range:
    end: 16686806
    part: 8
    start: 13431833
  summary: GPQA 从 114/198（106 对 8 错）推进至 131/198（122 对 9 错，93.13%），新增批次包含 118、129 等正常停止但回答错误的题目，errors=0、unparsed=0。一次 rank0 只读 SSH 返回为空，短超时重试后恢复，精确 PID、health=200、日志异常计数均正常，因此只能记录为瞬时采样问题，不能删除该不确定性。runner 约运行 40 分钟，剩余 67 题；“约 21–25 分钟完成”的估计仍属粗略预测，不能替代最终结果。
  title: GPQA 后段增量与瞬时 SSH 抖动
- block_id: 5881317b0acc400312451587656bdd59c038a27d28f6668e182e476902b33107
  indexed: true
  sha256: 50cf15ca138803feb708a2810d28845bbbff9945111dfabd14c0f1b09cc68972
  source_range:
    end: 16686806
    part: 9
    start: 13431833
  summary: 记录 combined commit 2f7b38ee9 的 K3 parser/renderer/chat-params 修复及隔离环境、四机 py_compile 检查通过；本机 pytest 因缺少 torch 未启动，不能视为失败或完整验证。旧服务按精确 PGID 终止后，新服务以 multicard TP16×DP4×EP64、FULL_DECODE_ONLY [16,32]、端口 18079 等配置启动，初始阶段尚未完成 weight loading/health，日志阶段与 ETA 均存在不确定性。旧 GPQA runner 2876823 停止并保留 139 条结果；随后启动官方博客参数新轮次 runner 371907，reported 请求参数为 reasoning_effort=max、temperature=1.0、top_p=1.0，早期进度从 11/198、16/198 增至 41/198，期间 errors=0、unparsed=0，但这些增量检查不是最终验收。
  title: 修复服务与官方参数轮次启动记录
- block_id: d813eb5ac94176da2ca244f3255f631c3a6409ca97eccd6211b27ff6d1a371f7
  indexed: true
  sha256: 77432be83cc0d64ba4950027759e49cf3489cc70edfd2520978bdd560d8d4636
  source_range:
    end: 16686806
    part: 10
    start: 13431833
  summary: 正式轮次目录和脚本保持不变，runner PID/PGID reported 为 371907/371907，concurrency32、timeout7200；请求体据报告仅含 model/messages 及 reasoning_effort=max、temperature=1.0、top_p=1.0，明确排除 max_tokens、top_k、min_p、penalties、stream 等未披露字段。后续采样从 48/198（46 对、2 错）推进至 71/198（67 对、4 错），其中记录了 id=75、id=8 等正常停止但答错的题，未见 request errors 或 unparsed。四 rank、health=200 和日志异常计数被多次报告为正常，但仍属于进行中的、可回溯的运行观察，不是最终成功结论。
  title: 官方博客参数 GPQA 中段进度
- block_id: a8f14ef0b70fdd16d1d2ef49879418a40aea62b1c8482cb4eb1eac98fa79dff0
  indexed: true
  sha256: 33038dfde5c183b732cddd5cb7fbe4b4cfda38c3b3fb738a625414a953279145
  source_range:
    end: 16686806
    part: 11
    start: 13431833
  summary: 截至该块，官方博客参数 GPQA 轮次 reported 为 79/198：74 对、5 错、request errors=0、unparsed=0，滚动精度 93.67%；新增批次含 id=106 的错误以及 id=47 的长推理正确结果。runner 371907/371907、四个服务进程、health=200 与四份日志均被报告正常，剩余 119 题，尚未生成或核验最终 summary。此前旧轮次 2876823 的 139 条结果和 run.log 仍保留；目标 accuracy>=90%、errors=0、unparsed=0 只是验收条件，当前状态不能升级为已完成。
  title: GPQA 后段未完成监控状态
- block_id: e698664beb6ebfec00dc6e35f67b259ac74da3aec70dee00d42b6ef4ec517ddc
  indexed: true
  sha256: eeef08aee10f8c185662fd0356fa779da2d4c06c63f0affe75eb3c5b48300bd1
  source_range:
    end: 16686806
    part: 12
    start: 13431833
  summary: 报告记录新 runner 371907/371907 在四 rank 服务（TP16×DP4×EP64、API 18079、DP RPC 14589）上使用 reasoning_effort=max、temperature=1.0、top_p=1.0，进度从 82/198、85/198 到 89/198，累计 84 对、5 错、request errors=0、unparsed=0；这些是逐次观测报告，尚未完成 summary 与 results 一致性核验，不能视为最终成功。旧轮次按精确 PGID 停止并保留 139 条结果；要求持续检查精确进程、health=200、日志异常和输出质量，禁止 broad kill，且此前本机 pytest 因缺 torch 未启动。
  title: 官方参数 GPQA 轮次推进至 89/198（待核验）
- block_id: ee664f4b8a6102c1a823199f127cd99154d0d6249a7c8c5f8e350d6dcf572207
  indexed: true
  sha256: 3e0f26500a016024d301c258e1673e90e1627f9bf07c801eb5fe9936f6498419
  source_range:
    end: 20617548
    part: 0
    start: 16686806
  summary: 报告显示 Kimi 官方博客参数 GPQA-Diamond 进行至 111/198，101 对、10 错，request errors=0、unparsed=0，滚动准确率 90.99%；最近 6 题为 5 对 1 错，错题含 id=80（D→B）。runner 371907/371907、四个精确服务进程和 health=200 均被报告为正常，日志未见新增异常；这些是运行中检查结果，并非最终一致性核验。
  title: 官方博客参数 GPQA 111/198，门槛余量收紧
- block_id: c39d18f7587043fae1abc34f1fad2fe4a385106f794576c91be251b8f93c72ee
  indexed: true
  sha256: 61a66d7f8b5f9612cb261745c14957803d4924be3361d69ef86c4f1c37363542
  source_range:
    end: 20617548
    part: 1
    start: 16686806
  summary: 报告进度从 117/198（107 对、10 错）继续到 151/198（137 对、14 错），request errors=0、unparsed=0，准确率在 90.73% 至 91.45% 间波动；期间记录了 id=102（B→A）和 id=157（C→B）等正常停止但答错的题。四机服务、health=200、runner 和日志均被报告正常，部分阶段明确称 summary.json 尚未生成；剩余题量与“最多还能错”是按当时报告计算的门槛估计，不是最终结果。
  title: GPQA 131/198 至 151/198，准确率接近门槛
- block_id: 23f21c70d863b72f3d9ebb3ccd08722d663520899dc17eccbea45664c0195e2c
  indexed: true
  sha256: 540a88472f575db6ed8ad4570dcb8ad8335d4c2c913bab07852bf31febf5b40a
  source_range:
    end: 20617548
    part: 2
    start: 16686806
  summary: 本块报告进度由 157/198（143 对、14 错，91.08%）增至 161/198（147 对、14 错，91.30%），request errors=0、unparsed=0；最近 6 题及随后 4 题均报告为正常 stop、输出非空且可解析。runner 已运行约 50m35s，四个精确服务 PID/PGID 存活、health=200，服务日志异常计数仍为 0；任务尚有 37 题，且未提供最终 summary 与 results 一致性核验。
  title: GPQA 157/198 至 161/198，仍在运行
- block_id: f0c8aa34d7b452fcf46412b0df88d0c5ee0a6844d81b8fadf20a4f46fc0b50d6
  indexed: true
  sha256: 28254c60b7e40a4135629d96c741960f0928b6c5a9b7d540ab731a6786cd925a
  source_range:
    end: 20617548
    part: 3
    start: 16686806
  summary: '正式 GPQA 轮次从 161/198 推进至 178/198，报告 164 对、14 错、errors=0、unparsed=0，准确率 92.13%；最近新增题目均报告为正常 stop、输出非空且可解析，但 runner 仍存活，最终 summary 与 results 一致性尚未确认。期间记录 PR #24 已合入 releases/v0.23.0_ci_new，squash commit 为 7a74db55cbbcdf56bd7efa07c2901e4e92f0bc89，包含请求兼容性修复及 tokenizer-native chat_template_kwargs、response_format/schema、tools/tool_choice:null 等处理；四机服务和当前 GPQA 未重启或变更。'
  title: GPQA 运行推进至 178/198，PR24 已合入但未应用
- block_id: 87d45a7e3ed2704600df58df7353f392fe0592ded18a9c9c13a07dcafe785f86
  indexed: true
  sha256: 4cb9875ae5ddb793bcd021e1533214b946bcad1913180c0c0781feaf9b0f29cd
  source_range:
    end: 20617548
    part: 4
    start: 16686806
  summary: 该段报告 GPQA 从 181/198 推进至 191/198，累计最高记录为 175 对、16 错、errors=0、unparsed=0，准确率 91.62%；id76 曾记录 D→A，id130 记录 B→C，均为答案错误而非请求或解析错误。四个精确服务 PID/PGID、rank0 health=200、runner 371907/371907 多次被报告存活，日志未见新增 ERROR/Traceback/Exception；但 runner 尚未结束，summary.json 尚未生成或尚未完成最终核对。
  title: GPQA 由 181 推进至 191/198，仍在途且出现新增错题
- block_id: 340314af65fe6c4b0a86b425fc9e92804562a24556d01e21769f429d86d01ac1
  indexed: true
  sha256: 8311b0b162ac3b2fda6400030f2945f9634418a0cc871d1d3f7dd3ddeb125d69
  source_range:
    end: 20617548
    part: 5
    start: 16686806
  summary: 后续报告将进度从 189/198 推进至 193/198，累计 177 对、16 错、errors=0、unparsed=0，准确率 91.71%；新增题目包括 id183、id127、id180、id15、id186，均被报告为正常 stop、输出非空且可解析。runner 仍存活约 72 分钟，四机服务及 health=200 正常、日志无新增异常；剩余 5 题，尚未看到最终 summary 与 198 条 results 的一致性验证，因此不能视为最终成功。
  title: GPQA 推进至 193/198，最终五题仍未完成
- block_id: ab5e68dc4fa8b873c7056e33adfd1192cf6b8c0b4507e4ae48bd1843da6431fe
  indexed: true
  sha256: ba5ce34c6b4e6241e131c639e16100201a1425bae496491a019379e00a0773ce
  source_range:
    end: 20617548
    part: 6
    start: 16686806
  summary: 正式 GPQA 轮次从 193/198 推进到 195/198，报告 178 对、17 错、errors=0、unparsed=0，准确率降至 91.28%；id90 答对、id88 出现 D→A 错误，剩余 3 题仍在途。多次报告四个精确服务 PID/PGID 存活、rank0 health=200、日志无 ERROR/Traceback/Exception，但 runner、summary.json 与 results 的最终一致性尚未确认。后续用户检查复读/乱码时指出非流式请求无法读取隐藏推理；id147 的可见正文扫描未发现 XTML、替换字符、控制字符、重复行或重复 8-gram，id145、id185 仍未完成。
  title: GPQA长尾监控：194至195题，尾题仍在途
- block_id: ce8e807f00d4d9e8756a76417cc26757bd9e335d5bded460fce26fa45cbb3d32
  indexed: true
  sha256: cd76f95aa09b8961753b10e75bc2df26743a480203b4f82c38f2f4ef7db9073b
  source_range:
    end: 20617548
    part: 7
    start: 16686806
  summary: 监控要求持续核对 198 题进度、精确服务健康、runner 存活、日志异常、请求错误、unparsed、准确率及最终 summary/results 一致性；相关阶段从 195/198 继续停留到 196/198，178 对、18 错、errors=0、unparsed=0、准确率 90.82%，仅剩 id145、id185。已完成的 id147 报告为 32,143 completion tokens、可见正文 1,258 字符，复读/乱码扫描暂未发现异常；但非流式接口仅保存 message.content，未保存 message.reasoning，因此无法排除隐藏推理内部复读。该阶段仍有长尾请求在途，最终门槛、summary 生成及完整结果核验均未确认。
  title: GPQA尾题状态与非流式输出可见性限制
- block_id: 9bcbfd56faa8c5f61e6080c278aa8d331a79748fc11708d1834c0200bdada14a
  indexed: true
  sha256: 2b9de1717c6fb8a4960a4345dbd951cf207231f3365629ec4f654d8772e281e7
  source_range:
    end: 20617548
    part: 8
    start: 16686806
  summary: 用户随后要求 kill 当前服务和测试，只用 ci-new 搭配 PR10 测试 GPQA；此前正式轮次仍停在 196/198，id145、id185 未返回，且没有最终复读/乱码结论。助手表示将按记录的精确 PGID 终止 runner 371907 及四个服务组，并保留日志和结果、避免 broad kill；随后报告 runner 进程组已停止且无组内残留，并开始向四个服务 PGID 发送 TERM。新 ci-new+PR10 构建和 GPQA 验证只是计划，服务终止后的完整确认、构建结果及新测试结果尚未提供。
  title: 用户要求终止旧轮次并改测 ci-new+PR10
- block_id: 231c68e53a252784384d38e55102bfc6074ae10719521ce1bddc11597e8cd574
  indexed: true
  sha256: cee2037015a23cd0afdfbacb99b56069f840c74c38508ccc3eb86caf639d765d
  source_range:
    end: 20617548
    part: 9
    start: 16686806
  summary: 四机 ci-new+PR10 服务完成 clean 构建、二进制/manifest/schema 及仓内 A3 定向测试门禁；外部 fla_npu PTA 脚本因依赖缺失未启动，不计作内核失败。服务已健康（health=200），四 rank 精确 PGID 存活且日志异常为0；非流式两题结构 smoke 为2/2正确，随后用户取消了流式方案，正式测试改用原 SHA=d1d3d8…dfe235a 的非流式脚本，以 concurrency=32 启动198题。正式轮当前43/198完成，42对1错、errors=0、unparsed=0、准确率97.67%；id=17预测D而标准C且finish_reason=stop，属于答错而非解析错误。已完成输出暂未发现替换字符、NUL、XTML或启发式复读证据，但测试仍在进行，不能据此宣称完整198题成功；早期监控读取异常已定位为splitlines误把输出中的非LF分隔符当记录，实际15条JSONL均有效。
  title: ci-new+PR10服务通过门禁，正式GPQA进行至43/198
- block_id: b16fc59fa58b1d1855645c3e5a53c019ec76f649ed03c2a70db929fef46e91f8
  indexed: true
  sha256: fed7be2b972a6e4220c821bddc51f55e8f744541b60830ba8c9eaffe86f33e27
  source_range:
    end: 27907394
    part: 0
    start: 20617548
  summary: ci-new+PR10 冻结快照的 GPQA 监控从 46/198 推进到 187/198，期间出现 1 次真实 parser 500（closing message marker 缺失）并经重试恢复，另有瞬时 health/SSH/进程探测假阴性；结果文件持续 errors=0、unparsed=0，但该轮未完成，不能视为最终成功。与旧 PR16+PR10 版本的阶段性同题比较一度显示准确率更高但延迟更慢，且旧轮最终仅 196/198，结论受样本阶段、随机温度和未完成状态限制。随后按精确 PGID 停止旧服务，并纠正 A3 规格：DequantSituQuant 同时服务共享专家和路由专家，路由输入为 BF16、不执行 dequant；A5 独立使用 SituMxQuant，TP16 实机采样范围也被限定为仅 prefill/decode 真实数据。
  title: PR10阶段结果与算子契约修正
- block_id: 6e305f391c5f3b3b30e8d0cb393c249e95044104880319f444f4062f447a97e2
  indexed: true
  sha256: 8efef909a2e1f09ae71e0c435f69131409ed3a553452738fd2a2a37e8ab57016
  source_range:
    end: 27907394
    part: 1
    start: 20617548
  summary: 旧 GPQA 轮次继续到 192/198、181 对 11 错，6 个长尾请求仍以非零吞吐生成；runner 曾被一次性探测误报为消失，后确认仍存活，因此没有把该轮判为成功或故障终止。随后用户要求停止服务并转入算子工作，四个服务组和 runner 按精确 PGID 发送 TERM，服务最终仅剩僵尸成员、API=000，结果与日志保留在 192/198。为验证 ci-new+PR16，固定了 ci-new `27887632…` 与 PR16 `df26e35…`；PR16 首次干净构建受 AppleDouble `._*` 文件污染失败，第二次又因 Torch adapter 缺少 `weight_scale_value`/`activation_scale_value` 链接失败，补回两行未提交热修复后构建成功。
  title: PR16切换前的服务停止与版本固定
- block_id: 16ccb71fb34ba72d0862deac91b0455fe928240625b11564705262fbea0e45d6
  indexed: true
  sha256: 7c4a976acc5ffa29f3e580ed9ecb3e35b108e11fc7e88b3ac88195a96c5d1d3a
  source_range:
    end: 27907394
    part: 2
    start: 20617548
  summary: ci-new+PR16 运行时完成构建并在四台机器一致部署；按用户更正重新打开 Prefix Cache 和 FlashComm1，四 rank 日志与 EngineCore 配置均确认启用，服务经历权重加载、编译和两档 ACLGraph 捕获后达到 health=200。新的 TP16×DP4×EP64、FULL_DECODE_ONLY `[16,32]` 服务启动 GPQA-Diamond 198 题，runner 为 `848342/848342`；截至材料末尾完成 55/198，54 对、1 错、errors=0、unparsed=0、准确率 98.18%，四机 health、PID/PGID 和日志异常计数均正常。该轮仍在进行，长推理造成吞吐波动；当前数字是阶段性结果，不能升级为最终准确率或完整门禁通过。
  title: Prefix Cache与FlashComm开启后的PR16 GPQA
- block_id: 7aa24b05161a3ed5675c7a3ffdf36a13f2bc4e37e079b1f68f7b077af686d597
  indexed: true
  sha256: 201cd1ea7f20c8212e9f3685e855a82d02d362601bbfcdad3fdaba74536e4e36
  source_range:
    end: 27907394
    part: 3
    start: 20617548
  summary: ci-new+PR16 的 GPQA 从约55/198推进到141/198；最新为135对、6错、errors=0、unparsed=0、95.74%，新增错题包括167（当前D、标准A），此前76与 recurrent_kda-only 共同答错。与 recurrent_kda-only 的实时交集显示，102、113由错变对，75由对变错，17、30、76、106稳定错；这些均为正常可解析的 stop 结果，不能归因于 parser。期间一次监控命令发生 shell 引号错误、代理返回502，后确认502来自本机HTTP代理而非服务，绕过代理后 health=200；四机进程、日志异常计数和runner持续正常，但测试尚未完成，不能据此宣称最终精度或门禁通过。
  title: ci-new+PR16 GPQA推进至141题及错题对比
- block_id: 02266baa7e11c70abb3f219d0fc7c8055fb274af6293d184ade93166cc741221
  indexed: true
  sha256: 880081f9d35ccce5d960edc7efc26eae9f35b7b4a8a542717218de4b62dbb5a4
  source_range:
    end: 31400654
    part: 0
    start: 27907394
  summary: GPQA 旧轮次推进至196/198：185对、11错，errors=0、unparsed=0；最后题目71、147因超长生成仍未返回，结果不是完整198题终值。此前曾误将latency与7200秒HTTP读取超时等同，后续更正为latency包含semaphore排队；runner只保存content和usage，无法审计未落盘的reasoning。PR16已通过隔离worktree提交两个optional scale Tensor物化修复，fast-forward head为65233bec…，但新CI状态仍未完成验证。
  title: 旧轮次收尾与PR16最小修复
- block_id: f3140216462138d71efee396ce0292b5e49750718dddae7fc61b158944ce8681
  indexed: true
  sha256: 72b824a5ad6cefc36ed1d47d0eb9866cc5ecf8c945e1a7f5fcc7c61896a6cfdc
  source_range:
    end: 31400654
    part: 1
    start: 27907394
  summary: 旧服务按精确PGID停止并保留196行结果；随后以相同TP16×DP4×EP64、EP64、Prefix Cache和FlashComm配置启动[REDACTED_SECRET]，经历64-rank初始化、347个权重分片、torch.compile与ACLGraph capture后health=200。新轮次正式GPQA已启动，脚本SHA为d1d3d8c…fe235a、concurrency=32、timeout=7200；早期结果持续无errors/unparsed和输出质量异常。用户中断的旧轮次196/198快照、启动参数及后处理规则已发布到PR16评论[#issuecomment-5061566817](https://github.com/vllm-ascend/vllm-ascend-kimi-k3/pull/16#issuecomment-5061566817)，该评论记录的是阶段结果而非最终成绩。
  title: 切换新权重并发布阶段性记录
- block_id: 5621f5abbf289f54e344d96ad5f95725d57ad8db8aacdf4e3155de201a5cfd10
  indexed: true
  sha256: add7bc7d0ce2cc4205163572dd5e1617baed5d202ab46652b79de5a2a82b2184
  source_range:
    end: 31400654
    part: 2
    start: 27907394
  summary: 新权重轮次推进至190/198：178对、12错，errors=0、unparsed=0，准确率93.68%；剩余8题仍在生成，未形成完整198题结论。与旧权重的已完成同题比较出现多处答案变化：ID81、89、113、102为旧对新错，ID157为旧错新对，另有75、76、88等错题选项变化，累计差异记录到8题、净差约−3题；这些是正常stop后的推理结论差异，不是解析或乱码故障。尾题因未设置max_tokens可能生成很长，但Engine吞吐、KV使用和health=200持续显示仍在推进，不能据阶段快照断言最终失败或成功。
  title: 新权重精度差异与长尾生成
- block_id: 8920383946ac4b7cff05a7856bbaabc7f2d131fa1a50ce031d7ee7b11e82c467
  indexed: true
  sha256: 0ac2ffd574be668b86c23ccc0fffb93c4708f12b64488a2f328454a344a1aeeb
  source_range:
    end: 34876098
    part: 0
    start: 31400654
  summary: 上一轮新权重GPQA最终完成198/198，182对、16错、91.92%，errors=0、unparsed=0，全部结果为finish_reason=stop；ID 71首次请求曾因约7200秒read timeout取消，重试后答对。共同196题对比显示旧权重185对、新权重180对，净少5题；全量输出扫描未见乱码、XTML、控制字符或明显复读。随后转向ci-new分支与multicard-0724权重，确认四机权重一致并创建隔离构建目录；启动脚本首次传输路径有误，但未影响源码或进程。
  title: 上一轮新权重GPQA完成及ci-new启动准备
- block_id: 835697d257ff93e9035dd1e511cf101c09edef4f8cfbc16025fa9b032dc21fc1
  indexed: true
  sha256: ace008cdcd65f599c8280812c5717b9ab2c056a29e3926ffe0f72e45529c4374
  source_range:
    end: 34876098
    part: 1
    start: 31400654
  summary: 最新ci-new提交17cb94b9e的原生构建以退出码0完成；第一次向154容器的流式同步因SSH连接重置中止，随后改用带校验和的归档方式，四机扩展、内核so、manifest和头文件哈希一致。四机服务按TP16×DP4×EP64、Prefix Cache、FlashComm1和FULL_DECODE_ONLY配置启动，经历权重加载、torch.compile及NPUGraph capture后，容器内health=200且模型ID为kimi-k3-ci-new-multicards-0724。GPQA正式运行198题、并发32、timeout 7200；截至113/198为107对、6错、errors=0、unparsed=0，期间一次SSH状态查询变慢但独立检查确认runner与服务正常。
  title: ci-new构建与multicard-0724服务启动
- block_id: 49d07c9db57140583648f9c3e9709563ebac149c1ea47a132c4a488aca9c1efb
  indexed: true
  sha256: a68342bda82172a2b6c624dce1d8ec658df11214da57be0a776ad5c7a38e3131
  source_range:
    end: 34876098
    part: 2
    start: 31400654
  summary: ci-new轮次持续推进至196/198：182对、14错、errors=0、unparsed=0、accuracy 92.86%，剩余ID为145和147，health=200且runner仍在运行。此前ID185已正确返回，finish_reason=stop，耗时6353秒、completion 28,619 tokens，文本未见替换字符、NUL、XTML或复读；长尾请求仍可能继续生成。过程中的阶段性统计包括194/198时180对14错，以及ID121完成后195/198；这些不是最终完整结果，ID145和ID147的最终答案、质量和全量复核仍未确认。
  title: ci-new GPQA进入最后长尾
- block_id: 4f6453f98fdaa9844f6de5191a9f66bf19c4f2f956924672be2d2ccd21e35bd0
  indexed: true
  sha256: fceb32f68d1c2a1bd69a4671e42f53b86447e65ae03e126b546d55086e083700
  source_range:
    end: 39818594
    part: 0
    start: 34876098
  summary: 此前ci-new GPQA已完成198/198，182对、16错，accuracy 91.9192%，errors=0、unparsed=0；独立审计和四机服务门禁通过。切换到93_4_ci_new后，首次四机启动约16分钟时退出，首因是复用.so和csrc/build时遗漏运行时_vllm_ascend/_cann_ops_custom，导致libopapi.so中的aclnnAddRmsNormBias不可用；补齐包后尚未完成真实NPU探针，桌面SSH认证一度被拒绝。
  title: ci-new完成并切换93_4，首次启动因运行时包缺失失败
- block_id: c7618228dfc6dd0a76ca33ff500a3d2ee8f084e30b1f1fcd1e6a2e89dba866d2
  indexed: true
  sha256: fc1a68744e770c719f16ab6f1a2615df7dbaacd556b94049e452bd9cb6c9a7f4
  source_range:
    end: 39818594
    part: 1
    start: 34876098
  summary: 真实 NPU 探针报告 `enable_custom_op=True` 下 `npu_add_rms_norm_bias` 在 NPU0 返回有限的 BF16 `(1,16)` 与 FP32 `rstd (1,1)`；四个 DP rank 随后完成 2 个 FULL_DECODE_ONLY 图捕获，日志新增 `ERROR/Traceback/Exception=0`，rank0 曾报告 `health=200`。正式 runner 首次因容器内缺少旧数据集路径触发 `FileNotFoundError` 并退出；数据集重建的第一次审计又因 Python `splitlines()` 错把 U+2028 当作 JSONL 边界而出现 `JSONDecodeError`。改用严格换行分割后，报告称 198 条输入逐字段匹配并启动正式轮次，但该块没有提供完整 GPQA 结果或最终比较。
  title: 93_4_ci_new 启动与数据集重建记录
- block_id: fc7751bac2c48d37cc3246eea70e4a7d1fc4e76f4a3de0a60964a743924e0b70
  indexed: true
  sha256: afbed0c1829c57e215b5a28f36c598b629803b03c598fe1542ec9f3ac0c30f61
  source_range:
    end: 39818594
    part: 2
    start: 34876098
  summary: 多次 heartbeat 要求持续监控 `93_4_ci_new` 的 GPQA-Diamond 198 题运行，并与此前 `ci-new` 的 `182/198`、`91.9191919%`、`errors=0`、`missing=0` 结果比较。指定环境为四机 TP16×DP4×EP64，prefix cache 与 FlashComm1 开启，FULL_DECODE_ONLY `[16,32]`，API `18079`、RPC `14589`，并要求仅检查精确 PID/PGID、health、日志错误、runner、summary 和结果质量。该块只有用户侧监控要求，没有新的 completed/correct/wrong/errors/unparsed/accuracy 数值，也没有证明轮次完成或服务状态最终保持不变。
  title: 93_4_ci_new 长时间监控指令
- block_id: 7a6f8250129e962968e40b8ca7c30b48cf05cb2f7f0d06a52af6bdab559a71b4
  indexed: true
  sha256: 47b51489feba678251f27d84598fb117867af0a25b5fd5ab36b985586d985797
  source_range:
    end: 39818594
    part: 3
    start: 34876098
  summary: 后续 heartbeat 延续同一 198 题 GPQA-Diamond 任务、四机精确进程范围和与 `ci-new` 基线 `182/198（91.9191919%，errors0，missing0）` 的逐题比较要求。监控条件仍包括 TP16×DP4×EP64、prefix cache ON、FlashComm1 ON、FULL_DECODE_ONLY `[16,32]`、API `18079`、RPC `14589`，以及检查 ERROR/Traceback/Exception、复读乱码和最终 fixed/regressed/unchanged 集合。该块仍未给出实际运行统计、最终审计、质量扫描或完成结论，因此不能把持续监控要求视为验证成功。
  title: 93_4_ci_new 后续监控范围
- block_id: 5921ce90563809b8473a3d274da23f94d7bc9d7a933b54b54ec8ca5c909f104c
  indexed: true
  sha256: 66c37737315234cd60413550d7194f581d070d6fef3abe7e9317f0cb64a199cd
  source_range:
    end: 39818594
    part: 4
    start: 34876098
  summary: 本批记录了最新 `releases/v0.23.0_ci_new` 提交 `d954213e9d872c7f04d86c543de63a1864ed2ed7` 的准备过程：prefix cache 与 FlashComm1 显式开启，`enable_shared_expert_dp=false`，并以 `MAX_JOBS=256`、`OPS_CPU_NUMBER=256`、`CMAKE_BUILD_PARALLEL_LEVEL=256`、`USE_NINJA=1` 编译。A3 自定义算子阶段完成 8/8 Compressor，日志报告 `build_aclnn.sh executed successfully!`；但后续 editable wheel 的 host CMake 阶段失败，首个错误是使用了不支持的 `SOC_VERSION=ascend910_93`，当前 CANN 要求 `ascend910_9391`。记录提出在不重编已成功算子的情况下单独修正并编译 host extension，但尚无完整安装、四机启动、GPQA 结果或功能验证证据。
  title: 最新 ci-new 构建：算子成功但 host 扩展因 SoC 名称失败
- block_id: fc5b1696f1761969a58985ee3ad61c7eee8677d924c82f5904a40dbdcf97cafe
  indexed: true
  sha256: 5fd35ecb14e17a2b744442420a9da5fa0f15e7d2f754abc449b652e208558b11
  source_range:
    end: 39818594
    part: 5
    start: 34876098
  summary: 两处纯Python修复后的第三次启动中，四个精确launcher仍存活，日志暂无叶子异常；此前health=000被判断为初始化阶段而非已就绪。进度显示四台串行创建本地worker，global rank从9/25/41/57推进到14/30/46/62，8077尚未ready，尚未开始351 shards权重加载；因此PR功能验证和完整GPQA精度仍未完成。记录还保留了SSH用户选错、远端检查中断等方法性问题，以及此前同名临时文件操作曾短暂清空153/154运行时文件、后已恢复的纠正。
  title: 'PR #12950第三次启动进入worker初始化'
- block_id: e46d5d3f56816f623360683c1b4e03e51e2f73252ccc1b33fafb582d1e44eb59
  indexed: true
  sha256: 050a53d9980de562657a48e8dc23e5fc9c267f34f7e6d0ea639088881c325109
  source_range:
    end: 49024040
    part: 0
    start: 39818594
  summary: 定位并修复了 vLLM 0.25.1 中 SiTU activation 桥接、Kimi K3 KDA OOT 注册、A_log loader、NFS safetensors mmap/prefetch、dense SiTU 及 shared-expert 输入契约等多个阻断点；真实环境定向测试从 6/6、27/27、28/28 等通过。四机多轮 TP16×DP4×EP64 启动中曾出现 Gloo mesh reset、UntypedStorage shape 错误、3584/7168 输入维度错误和错误启动拓扑，相关失败与修复过程均保留。后续修复已通过权重加载、AOT 编译和 initial profiling，但随后又暴露 K3 identity RoPE/decode cache、parser 参数透传及请求层 API 兼容问题，不能仅据中间阶段判定端到端成功。
  title: SiTU、权重加载与共享专家链路修复
- block_id: 8a037cf4b29b387a03433ff7bbbf9584289d0d200bee6d64f30e7694f2fa7000
  indexed: true
  sha256: 199de5a197c3a165ae4538e5438b691a28ceb16d390039dea1777a570460a31f
  source_range:
    end: 49024040
    part: 1
    start: 39818594
  summary: 清理重复启动进程后，干净四机服务完成 64-rank Gloo、EP 映射及 352 个权重分片加载；诊断确认 shared path 错把 routed latent `[32,3584]` 传给需要原始 hidden K=7168 的量化权重。修复 `_forward_impl` 丢弃 `shared_experts_input` 的接线问题后，真实环境定向测试达到 28 passed，AOT 编译和 initial profiling 完成，未再出现 3584/7168 shape 错误。随后仍出现 decode identity RoPE 的 128/512 reshape 问题及 `NoneType` RoPE cache 问题，后者定位为 decode graph-capture 未复用 K3 的 identity 分支；这些结果证明了局部修复，不等同于最终任务成功。
  title: 共享专家输入契约修复与实机编译验证
- block_id: 5a4ed7596425037128cd5d191dd7331592a0c28e6432cf98cdfeb7735ae19ea1
  indexed: true
  sha256: 2ef8b913b4f9f9fe65116c7f132bfbfab9c3df3038c7c90c35bbf15f50dd7b17
  source_range:
    end: 49024040
    part: 2
    start: 39818594
  summary: 修正 no-RoPE MLA decode 返回整页 cache、prefill 返回当前 token 的契约后，四机完成 7/7 ACLGraph 捕获并达到 health=200；parser 的 `is_harmony` 透传和非流式 `mm_token_counts` 兼容问题也分别通过 4/4、70/70 测试，相关修复测试合计报告为 176 项通过，另有 MLA A2 78/78。真实功能门禁报告 no-tools、BFCL tool-call 抽取和 SSE 流式重组均通过，四 rank 无异常；本机缺少 `gitleaks`/`wget` 的检查失败、旧 tokenizer fixture SHA 不匹配以及若干启动/SSH 传输问题均被保留为环境或流程限制。完整 GPQA-Diamond 已以 198 条数据、并发 32 和指定参数启动，记录了脱敏目录、PID/PGID 及数据集/runner SHA，但该批材料尚未给出 GPQA 最终准确率或 PR 最终单提交/DCO 结论。
  title: 图捕获、API 兼容修复与GPQA启动
- block_id: 17ea1beb65555d8422c11c65ad3e4eb8fdf55e7a3e6cd516d6ce1d029beaf268
  indexed: true
  sha256: 8ed998e333c5dc8c7b7f253d8788a81d59b78949298c2a8495cf711eca707045
  source_range:
    end: 49024040
    part: 3
    start: 39818594
  summary: GPQA在至少48/198时为47正确、1错误、0 errors/0 unparsed，唯一错题与既有基线相同；随后runner已不在进程表中，71条未完成结果和日志保留，因此不能视为完整精度结果。远端已推送单提交`5b642437d`，但后续远端分支被更新为`2eadb7e03`，并已回退此前为vLLM 0.23.0加入的12个兼容文件增量；远端网络问题通过本地传输和SHA校验规避。四机源码已对齐精确commit`fe784ff22e630a31fd798f392b01e0a75c18f047`，安装版本显示为`0.1.dev1+gfe784ff22.empty`；原计划以256并发编译vLLM Ascend尚未实际开始，首个wrapper因`nounset`与Ascend `set_env.sh`不兼容而提前退出。
  title: GPQA暂停并切换vLLM基线，Ascend编译尚未启动
- block_id: 90a5ba7ebb1b5a3d13203abd8fe32269ddbc1bfa07a19512fad39ec9be0a05f1
  indexed: true
  sha256: e6039385187537f71757663ef2d7c4271037a664385f3ef3e71cca187ae24dca
  source_range:
    end: 61862142
    part: 0
    start: 49024040
  summary: vLLM 已对齐精确提交 `fe784ff22e630a31fd798f392b01e0a75c18f047`，四机 256 并行编译和 189 项定向测试曾通过；运行初期发现显存不足、meta tensor `.to()`、以及其他 Kubernetes 服务争用 NPU 等问题。meta tensor 修复为纯 Python，未重编译 native 算子；后来确认 PR 已包含 `_move_module_to_device` 相关修改。单机 5 层验证期间继续暴露旧容器依赖、`acl` 路径、SiTU/FusedMoE、A_log loader 等兼容问题，且曾在旧本地树上定位，不能直接等同于当前 PR 验证。
  title: 四机编译、运行定位与单机验证准备
- block_id: de40d2b7796a6bfc99d61795632b54176bd5e2c0070fce1c026bc3e9fe2c9cb9
  indexed: true
  sha256: cad30ada903375508dce268f7c1511f161cc49cab6427e896e880a1cd4ac718b
  source_range:
    end: 61862142
    part: 1
    start: 49024040
  summary: '`fdf3cad` 被确认不是本轮提交；PR 后续 head 更新为 `4a1857bed6b385cb58a7b7eabf91d462345050cb`。在 exact vLLM `fe784ff22`、单机 TP8、5 层 INT4 环境中，PR head 成功加载 19/19 权重、完成 KV cache/profile、warmup，并生成 2 token `[128820, 96476]`；这是功能成功证据，但此前旧树上的 `situ_beta` 和 A_log 错误不应冒充当前 head 结果。期间曾精确停止 `mindie-motor/vllm-u0` 的 4 副本以释放资源，未停止集群控制面或其他 workload。'
  title: PR head 单机五层实机验证成功
- block_id: 81f377321609d7ea2f2da23f25e7fe1effff921e29bbaddedb2184cc84e9f816
  indexed: true
  sha256: 614eb825520f3e1e6741a440ad532dda50a22e414083f197fe15bab5a2c004a5
  source_range:
    end: 61862142
    part: 2
    start: 49024040
  summary: PR `4a1857b` 配套 vLLM `fe784ff` 的四机 TP16×DP4×EP64 完整权重服务曾成功启动，health=200，8 路并发数学请求全部 HTTP 200 且答案正确，未复现截图中的 KDA 16↔256 广播错误；服务随后按要求被直接终止。之后发现 PR 与最新 base `81d3450` 冲突，且单机 5 层重新验证时先排除环境、路径和权重挂载问题，最终确认 FC1 已开启但临时 runner 漏传 `enable_expert_parallel=True`；该修正尚未形成最终提交或新的 FC1 成功证据。
  title: 四机完整权重门禁通过，后续冲突与FC1待修
- block_id: 96794d9d9f9a32ca3794d189f588f343b6442c8f6e09ff9fac520bce42856f66
  indexed: true
  sha256: de82f26a94142dc83442a073fae2fe0f9c51c9689155498448355d60241774d1
  source_range:
    end: 61862142
    part: 3
    start: 49024040
  summary: '单机真实权重验证曾通过，Kimi KDA 的 `g_proj` 通用 SequenceColumnParallelOp 误匹配导致的 `8 vs 64`/TP16 gate shape 错误已在修复快照中消失；四机也完成 64 rank 初始化、`torch.compile` profile 和 `[16,32]` 图捕获并达到 health=200。但四机首个在线 prefill 在 `sample_tokens` RPC 等待 5 分钟后超时，rank0 API 进程退出，因此四机 serving 功能尚未验证成功。PR #12950 的 CI 失败已定位为重新引入 `QuantType.NONE` MegaMoE fallback 及相关 K3 SiTU/shared-expert 逻辑，另有 `globalBS=256` 与 `epWorldSize=2` 不匹配；CI 修复和在线门禁仍未完成。'
  title: 四机 FC1 修复后通过图捕获，但在线首请求超时；PR CI 根因已定位
- block_id: eb70d6c38bca59c6a9e50861e12f95d569a7504e92ef179cce94ce2eabd9a2dc
  indexed: true
  sha256: 6995c7698e6696942762ffa834859930b8e2edeaeb60c5119ef0b13e8c09c68a
  source_range:
    end: 61862142
    part: 4
    start: 49024040
  summary: 修正 `max_num_batched_tokens` 后，进程 `120463/120463` 存活并完成 TP8/HCCL worker 初始化及 8 个 rank 的权重加载。FC1/KDA 原始 shape 异常未再出现，当前仍在逐卡 CPU/IRQ 绑定阶段；尚未提供 health=200、在线请求返回或完整 serving 成功证据。
  title: 单机在线门禁已完成权重加载，尚未到 health 或推理结论
- block_id: fb9991d73d191e7796b9ff273a6ec4448f68037bcd410f7599d76dd7463b83af
  indexed: true
  sha256: 04788d46af9d9366125ea0d9340817e5e5796474000c9676b79bca6908b9d263
  source_range:
    end: 73985256
    part: 0
    start: 61862142
  summary: 单机 5 层 eager 与 FULL_DECODE_ONLY 均返回 HTTP 200，但完整四机 FC1 ON 在 eager 和图模式首请求均于约 300 秒触发 `sample_tokens` RPC timeout；FC1 OFF 对照同请求约 60.70 秒返回正确答案 391。trace 将阻塞点定位到首个 MoE 层 `before_mlp`，根因指向 FlashComm1 后 DP token 数 7/1/1/1 与 MC2/EP64 通信契约不一致；后续手工 padding 未能修复，AllGather/AllToAll 绕路又产生复读、乱码或控制 token 错误，因此这些尝试均未验证成功。
  title: 四机 FC1 首请求超时定位
- block_id: b4127c3f28662034d5be9ab3b4370292fab7bc0c6faed07288962f35f8986a7e
  indexed: true
  sha256: d4078d013e171215c76bf7b71ab76fad93024fd5b16a815d26880c57b113fbce
  source_range:
    end: 73985256
    part: 1
    start: 61862142
  summary: 单机 TP8/EP8 的多轮 smoke 可完成 prefill/decode，但四机 v7 的 MC2 padding 仍在约 300 秒后 `sample_tokens` 超时；v8 的 Kimi 专用 AllGather fallback 虽解除死锁，却返回 `content=null`、`of of...` 复读并截断。v9 证明零初始化 DP padding 不是主因，单机 FC1 ON/OFF 也产生不同 token，逐层 A/B 将首个分叉锁定在 layer1 SiTU W4A8 MoE；AllToAll 又出现乱码和 `Unexpected K3 control token inside text content`，v11 uniform MC2 仍卡在首个 forward。
  title: MC2 padding 与 AllGather 回退均未通过
- block_id: 88f8bae1abac6238c842b20726417129c9d9191726488ab09a1c4ee2ce9a7459
  indexed: true
  sha256: 688864d5d5db645794feb259699e410882198a9792b096bf3acc2709f3c5ebf9
  source_range:
    end: 73985256
    part: 2
    start: 61862142
  summary: v10 实机确认 AllToAll 能越过死锁但输出质量不合格；v11/v12 的 uniform MC2 与静态 profile 仍不足以证明真实 7/1/1/1 请求可用，v13 还出现 TERM 后残留 worker 占用显存，随后通过精确 PGID 清理并在 v14 重启。v14 在线阶段日志最终发现同一 batch 中 DP0 选择 AllToAll，而 DP1–3 选择 MC2（`mask/global_bs` 不同），导致不同 collective 互等；修复为依据跨 rank 最大 token 数统一通信方法并共同 padding/裁剪，v15 已启动且初始化无异常，但尚无首请求结果，不能视为修复成功。CI 方面，shared-expert-DP 参数扩展和 `QuantType.NONE` 误入 generic FusedMC2 两类回归已定位并收窄/恢复，最终提交与 CI 仍未完成。
  title: 在线通信选择不一致，v15 进入验证
- block_id: e2427fff82e6a055fdd1031d0d2b0caddbdf0e1082a1d0e76ea995787b8c506a
  indexed: true
  sha256: f8c1cdd194614f362b9406370907b61fb7c0c979369123d85fdabada8716c564
  source_range:
    end: 73985256
    part: 3
    start: 61862142
  summary: 四机完整权重、health=200 和启动阶段检查已多次完成；此前在线 2+2 请求仍出现 DP0 走 AllToAll、DP1–3 走 MC2，证据显示非主 rank 只收到本地 token 向量 `[16,16,16,16]`，而 DP0 收到 `[96,16,16,16]`，请求因此卡住。针对 Kimi 的专属判断已撤销，当前修复改为按具体 `vllm_config` 独立判断 `is_moe_model()`，避免 target/draft/headless 初始化顺序污染进程级缓存，适用于所有 MoE；v22 尚未证明在线请求成功。最新状态是四 rank 日志无 ERROR/Traceback/Exception、health=200，1-token 请求正在执行，仍需核对全局 token 向量和 collective 选择；FULL_DECODE_ONLY、临时诊断清理、PR CI 和最终提交也未完成。
  title: v22 通用 MoE 判定修复已启动，在线一致性仍待确认
- block_id: ff23f9ea41576b929fdca93846dac2efe7c591d37190f11379428f39aae9d75d
  indexed: true
  sha256: 67d8f8b89f08c085f397cf067b9efb0738c7ca1eb6bd1b299913d740919b8436
  source_range:
    end: 75563507
    part: 0
    start: 73985256
  summary: 四机 FC1 首请求挂起被定位为通用 MoE/DP metadata 分裂现象；此前 EP 优先判定、padding、dispatch 和通信选择改动均属无效诊断并已撤回，不能作为最终修复。当前修法限定为 KDA 入口统一 AllGather，g_proj 保持普通 ColumnParallel，不单独触发 AllGather；单机 5 层已通过初始化、prefill/decode 并生成 `[79451, 46956]`，未再出现 KDA `16/256` shape mismatch。四机文件同步和进程启动已完成，但 rank0 子进程出现 `APIServer pid=... Traceback`，主根进程仍存活；尚未证明服务 ready、请求成功或跨 DP metadata 一致，CI 也未单独修复。
  title: KDA 最小 FC1 修法单机通过，四机启动仍待核实
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a6e8923a2c49ae5f34c607edd2987b1f7e22433287f04ce734b010b69c84d88c
  kind: experience
  material_digest: c272dcdc2458c8fddec137e59a431d5f201fdb8a1907fa58ac8f73fdc03f649e
  revision: 0f8cea51646d164f7c351e32a4ba67b734a67b83c55ba3b839a0db8bb56b2d3d
  schema: mindie-entry/3
  summary: 此前记录的 NPU 探针、health=200、图捕获和输入匹配不能代表四机功能或 GPQA 成功；旧数据集路径与 JSONDecodeError、FlashComm1/KDA 通信及跨 DP 问题仍应独立检索。新增材料确认最新 ci-new 已将 prefix cache、FlashComm1 与共享专家 DP 显式解耦，并完成 256 并发 A3 算子编译，但 host extension 因 `SOC_VERSION=ascend910_93` 与 CANN 要求 `ascend910_9391` 不匹配而失败；后续修正尚未形成可验证的服务或精度结果。
  title: ci-new 最新构建已定位 host CMake 阻塞，GPQA 仍未验证
navigation: 此前记录的 NPU 探针、health=200、图捕获和输入匹配不能代表四机功能或 GPQA 成功；旧数据集路径与 JSONDecodeError、FlashComm1/KDA 通信及跨 DP 问题仍应独立检索。新增材料确认最新 ci-new 已将 prefix cache、FlashComm1 与共享专家 DP 显式解耦，并完成 256 并发 A3 算子编译，但 host extension 因 `SOC_VERSION=ascend910_93` 与 CANN 要求 `ascend910_9391` 不匹配而失败；后续修正尚未形成可验证的服务或精度结果。
schema: mindie-material-task/1
status: complete
task_id: a6e8923a2c49ae5f34c607edd2987b1f7e22433287f04ce734b010b69c84d88c
---

# ci-new 最新构建已定位 host CMake 阻塞，GPQA 仍未验证

此前记录的 NPU 探针、health=200、图捕获和输入匹配不能代表四机功能或 GPQA 成功；旧数据集路径与 JSONDecodeError、FlashComm1/KDA 通信及跨 DP 问题仍应独立检索。新增材料确认最新 ci-new 已将 prefix cache、FlashComm1 与共享专家 DP 显式解耦，并完成 256 并发 A3 算子编译，但 host extension 因 `SOC_VERSION=ascend910_93` 与 CANN 要求 `ascend910_9391` 不匹配而失败；后续修正尚未形成可验证的服务或精度结果。

## Materials

- [四机全特性构建与持续跟踪背景](blocks/e20eb95bfd2eee1245764bb0c2df8c6263fbbc9237d423c367bcbfe03ee10026.md): 任务基于合并后的全特性源码，在四台隔离容器中启用 FlashComm1、KDA、SiTU、工具调用和推理解析，目标是完成 A3 算子构建、服务启动及 GPQA-Diamond。过程中曾因 zsh 引号、缺少 catlass 子模块、被 `--exclude=build` 排除的 CMake 补丁目录、AppleDouble 文件以及抽象 SoC 名称 `ascend910_93` 导致失败；其中最新已定位到 CMake 扩展不支持该 SoC 名称，并改用 `ascend910_9391` 重新构建。历史及当前结果均未证明构建、服务或 GPQA 已成功，GPQA 仍未启动。
- [异常判定、二分修复与后续量化条件](blocks/1f224251509d4b17cb43099bfbf29ac44f843c6f1699d4b4dd2a1034a27705d8.md): 输出质量异常需至少两条独立证据支持，包括复读、乱码、空响应、结构损坏或明显无语义；完整 198 题准确率低于 90% 也属于异常，completed 达到 30 后需按连续检查或理论上无法达标的规则介入。触发后应保留原始结果、SHA、参数和 manifest，精确停止本次 GPQA/服务，停用 `kimi-k3-gpqa`，并在隔离 worktree 中对合并基线及 PR20/16/14/10 做可重复 smoke 二分，再交接给动态确认的 PR 原任务。只有完整 GPQA 达到 198/198、准确率至少 90%、且无 request errors 或 missing/unparsed predictions，才允许在指定 `xyx_vllm` 容器中一次性启动 `quant.sh`；目前这些条件尚未满足。
- [Heartbeat 详细汇报与最新重建状态](blocks/b51d36ec49ac4b4be5673981abf328abfd24b30074eca4554c1efe746182f840.md): 每次 heartbeat 都必须通知并逐台报告 153/154/155/156 的构建状态、可见步骤、日志时间、失败情况和有证据支持的 ETA；启动后还需报告权重加载、ACLGraph、健康码和服务可用性，GPQA 阶段则报告 completed/198、correct、wrong、request errors、unparsed、准确率及新增样本质量。此前四机对象文件曾增长但不单调，最终构建阶段一致因 `SOC_VERSION ascend910_93 does not support` 失败；失败日志已保留，隔离目录已切换为 `ascend910_9391` 并开始第四轮干净构建。最新记录仅说明新构建已稳定运行约 4 分钟，尚无成功标记、manifest 核验、服务启动或 GPQA 结果。
- [第四轮四机构建持续推进](blocks/2d30184a075e177b8342fd6a3a55d75cbcc538aec6023038ece2deb24042e029.md): 153–156 四个 rank 均保持 running，Ninja 进程和日志持续更新，临时对象数量经历增长及阶段切换后的回收，未见即时失败或 `.failed` 标记。构建仍未生成最终 `output/build_out`，GPQA 保持 0/198，正确、错误、请求错误和未解析均为 0；剩余时间仅为基于历史和当前速度的估计，不能视为完成确认。
- [异常处理改为正向定位优先](blocks/87f64e36c590e0c863c45777aaf20b5c69bd28ef720c4a820ce8cd69358aee1b.md): 新增规则要求对构建、安装、manifest、启动、请求崩溃及可稳定复现的复读/乱码/空响应等明显失败，从首次错误证据沿调用链和相关 PR diff 正向定位，不得用逐 PR 回退替代分析。只有在确认 GPQA 精度低于 90% 且没有可直接归因的明显运行时或输出损坏证据时，才可在隔离环境按 PR20、PR16、PR14、PR10 边界进行可重复二分；候选归属仍需标明证据和不确定性。
- [异常分流规则再次确认](blocks/5a83481d9cf15120ffb519640de4d3f7b2e825ace950ebb660f4e8a888e4e3a4.md): 该块重复确认明显失败必须保留原始请求/响应、首个错误、stack trace、rank 日志、SHA、参数、manifest 和二进制，并精确停止本次 GPQA 与对应服务后交接责任 PR；不能猜测责任归属。精度异常才进入固定边界的隔离二分，修复返回后仅合入目标修复并重建复验，精度问题必须完整重跑 198 题恢复至至少 90%。
- [异常分流与 PR 交接规则](blocks/2b6090e84421798d220a5a2e363371bdab6a96adcd2f4c4aa54a8e4e850b6fe8.md): 明确失败（构建、安装、manifest、启动、健康检查、请求错误或稳定输出损坏）必须从首次证据正向定位，不得进行 PR 二分；只有无明显故障且 GPQA 精度确认低于 90% 时才按 PR20、PR16、PR14、PR10 的隔离边界二分。要求保留原始请求响应、stack trace、rank 日志、SHA、启动参数、manifest 和二进制，并将责任组件、证据和修复要求交给对应 PR agent。修复后目标任务仅合入该修复并隔离重建复验，精度问题必须完整重跑 198 题且恢复至 accuracy >= 90%。
- [K3 启动失败与 PR16 修复回传](blocks/68fa69b55ea751e4897283e367f5e9ea9f717235d63832c170a8718016334657.md): 四机算子构建、manifest 引用核验和服务拉起曾报告通过，但后续启动在 rank1 的 `profile_run/determine_available_memory` 暴露首次明确错误：A3 A8W4 `aclnnGroupedMatmulWeightNz` 收到 `INT8×INT4→INT32`，返回 161002；调用链定位到 `moe_mlp.py:440` 的 SiTU raw-GMM 路径，并归属于 PR16 commit `cab9422940`。服务已精确停止、证据固化，GPQA 保持 0/198，未进行 PR 二分；随后 PR16 回传修复 commit `543f73cd9b01a908966343f757e652da2100049e`，将 SiTU GMM 输出改为 BF16、调整 DequantSituQuant 契约，并报告隔离单算子、真实 routed/shared TP16、TP8 服务 smoke 均通过，但 GitHub checks 仍 pending，修复尚未在当前 allfeatures 分支复验。
- [后续复验范围与量化门槛](blocks/ccaf7f7f7bd5d95e8e8de5dee78c67fc837ae89639add9a25a4187538fd726ba.md): 当前任务范围限定为指定的 k3-allfeatures-153..156、allfeatures 日志和结果；修复后应仅 cherry-pick PR16 修复，重建受影响的 `dequant_situ_quant` 与 Torch binding，重启四机并先复测语义 smoke，再运行完整 GPQA。只有 198/198、accuracy >= 90%、无 request errors 且无 missing/unparsed predictions 时才可启动量化；此前启动失败意味着服务、GPQA 和量化均未达成。所有结论仍属易错检索参考，需保留失败尝试、未完成状态、隔离验证与后续更正，不能把报告中的 smoke 或计划提升为当前任务成功。
- [PR16 构建失效与 retry3 定位](blocks/9e4212688abd1c5146f9335e4ce3a53d2e9dc84b8c7e3e6b3427cc5d32b4335c.md): 四机首次修复构建和启动准备曾通过，但 retry3 在 profile_run 暴露新的 `aclnnDequantSituQuant` 注册问题：运行时仅有 INT32 + FLOAT optional 签名，缺少 BF16 + 全部 optional=None。随后确认根因是 editable build 复用 `csrc/build`，PR16 提交 `9c04ee75...` 将清理范围扩展为 `build output build_out`；当前 clean retry 正在进行，尚未证明整网验证成功。
- [PR16 clean build 与整网 smoke](blocks/081a45a6832c0780d514af9ca36dfb6294eb2e710495407c08ea917f639355f9.md): 构建失效修复已合入本地分支 `ffb3f86ee`，四台脚本和 UT 已同步；全量 47-op clean build 最终四台均生成 `.success`，manifest、Torch binding、kernel library SHA 一致。已安装包的 BF16 + optional=None DequantSituQuant NPU 测试通过 3 个 shape，随后 TP16×DP4×EP64 服务健康至 HTTP 200，但两条独立 temperature=0 语义 smoke 都出现空内容和 `The!!!!...` 复读，GPQA 保持 0/198，服务已停止。正向定位指向 PR10 的 recurrent KDA decode 替换路径，而 PR16 的 CI 红灯被判断为 PR20 无关 KDA 测试格式问题。
- [PR10 alias schema 修复与 retry5](blocks/be4f917d6f7d3fa1827de380fc692441a100df0289eacc9518f44f7db1cc8029.md): PR10 修复 `f9a038e1...` 仅应用于 combined 隔离分支，将 recurrent KDA schema 从返回 alias state 改为 mutable input state 与单一非 alias output；隔离验证报告 compiled/eager output、state 差异为 0，并通过多步 A3 用例，但尚未单独证明 `The!!!!` 已消失。四台 binding 重建最终成功并一致为新 SHA，manifest/kernel 未变；服务 retry5 已启动并完成 16/16 worker 与 EP64 映射，当前停在全局 HCCL barrier 后、尚未进入权重加载。
- [retry5 HCCL barrier 等待](blocks/309f0fe37bef86e02a5b151a5626ee22a438a1d2c77b2114d6c25fe4fc12b737.md): retry5 在 EP 映射后进入全局 HCCL barrier，约 45 秒没有新日志，但四台每台 17/17 相关进程仍活跃，没有退出、zombie 或错误。当前不能判定服务卡死或验证成功；若继续无进展，应检查 worker 状态、CPU 和 HCCL 日志，暂不盲目重启。
- [PR10 复验失败与诊断补丁启动](blocks/88390d00ab1bc259c4a96178f779864d93e4af672d79535af495453b77a4b078.md): PR10 alias-return 修复后的四机复验虽完成权重加载、torch.compile、ACLGraph 2/2 和 health=200，但两条 temperature=0 smoke 仍返回 HTTP 200、content=null 及 `The!!!!...`，GPQA 保持 0/198，服务已精确停止。随后应用 graph-safe 诊断补丁 8f7eb60c，并固定所有构建使用 `MAX_JOBS=256/-j256`；首次诊断启动因 `dt_bias` 从 `[768]` 拷贝到 `[6,128]` 触发 `aclnnInplaceCopy 161002`，修复提交 5f1b3f45 将 buffer 改为 flat 后，第二轮成功健康并复现异常。由于 worker 没有 `LOCAL_RANK/RANK` 环境变量，初次采样未生成 `.pt`；follow-up 1ead7af3 改为按 PID 保存每个 worker 的两步 trace，但第三轮启动期间又切换为单机渐层/逐层真实权重定位，四机已停止。
- [诊断采样失败与 KDA 图填充根因](blocks/ccfc2367adfd1dea99e97f9895db22ca665f040142f21852cda8471b8661e534.md): 诊断补丁阶段确认真实 recurrent KDA state 使用 FP32 binary；BF16 state 的 manifest 缺失并非运行时故障。修正 `dt_bias` shape 后四机成功完成 352/352 权重加载、编译和 ACLGraph 2/2，但 smoke 仍出现 `The!!!!`，且 trace 导出因 worker 缺少 rank 环境而得到 0 个文件。随后单机五层真实权重定位证明 eager/TP16 下 KDA 与 Triton 对齐、跨步 state continuity 正常，失败限定在 FULL_DECODE_ONLY graph padding：容量 16/32 而真实 token 为 1、`cu_seqlens=[0,1,1,...]`，kernel 的 `ValidateCuSeqlens` 提前返回，导致 empty_like 输出未写入和 active state 未更新；红测记录 49,583 个 state mismatch、最大绝对误差 0.5641283989，证据已更新到 Draft PR10 head `9792e108`，未修改 kernel。
- [PR14 单机验证准备与中止状态](blocks/a06e8e06fd5c77b225c5533983fdbb687110d265556ec9fb858104b6365e89af.md): PR14 验证原计划从最新 `ci-new` `f72b35e6a` 叠加 PR14 head `33deb2609`，但 155 的逻辑 NPU 8–15 被量化任务占用，因此四机验证暂停让量化继续，后改为仅在 153 做五层 TP16 单机验证。153 的隔离 editable build 固定使用 `MAX_JOBS=256/-j256`，长尾 Compressor/SparseFlashAttention OPC 持续生成 kernel，曾记录约 1243 个完成边、约 61 个待执行步骤且无 `FAILED/error/traceback`，但截至该块末尾编译仍未完成、服务尚未启动。第三轮四机诊断已按精确 PID/PGID 停止，8077 清空、GPQA 为 0/198；当前策略是不再自行重启四机，等待单机渐层/逐层真实权重诊断。
- [PR14 单机链路通过、量化完成与后续组合验证](blocks/68469341b381029d7cf5061ca284f5c8e665ce8fffda62ad0d1e6ff53974c837.md): PR14 + latest ci-new 的 153 单机五层服务经过多次配置、tokenizer 和裁剪元数据问题后，以 `hf_overrides.text_config.num_hidden_layers=5` 成功启动，health=200；但五层权重不具备精度或语义判断价值，三类请求出现 `content=None`、空 content、无 tool call 和 `finish_reason=length`，未被认定为 PR14 失败。155 上量化随后完成 93/93 层合并，347/347 个 safetensors 及 index/config/tokenizer 核验存在；153 服务被精确停止并留下无执行能力的 zombie 条目。之后收到 PR10 clean kernel 验证结果（15 passed），并开始 latest ci-new + PR16 + PR10 的隔离组合构建；首次构建因缺少 Catlass 子模块提前失败，补齐后以 256 并行继续，目标为 MMMU-Pro、BFCL 工具调用和完整 GPQA。
- [组合分支 clean build 进行中及验证范围](blocks/8b2efcebb5e222ce95c85aff9b520807c0beff61b924fb48034ba568d9b72e3e.md): 组合验证固定使用 latest ci-new `433bf0330`、PR16 `bdd426c4d` 和 PR10 `8a7f42d8a`，形成提交 `17a7a4d5b`，范围限定在新建的 `k3-pr16-pr10-153..156` 隔离环境；构建要求 `MAX_JOBS=256`、`OPS_CPU_NUMBER=256`、`CMAKE_BUILD_PARALLEL_LEVEL=256` 和显式 `-j256`，禁止复用旧 recurrent KDA kernel。早期 clean build 曾因 Git archive 缺少 `csrc/third_party/catlass` 而在编译前停止，补齐锁定子模块 `41bf90da` 后构建持续推进，期间无失败标记但尚未产出最终包；计划先核验 recurrent KDA/DequantSituQuant，再启动 TP16×DP4×EP64 服务。评测计划是官方 MMMU-Pro vision 前 5 条、BFCL `simple_python` 前 5 条，随后 GPQA-Diamond 198 题，要求分别保留原始请求/响应和结构化评分。
- [算子门禁通过，四机完整权重服务启动中](blocks/2d10bbbd059ed04f9b773499255b38a3e7b59b502a4b0a942980270305f57ef1.md): 干净构建于 03:45:21 UTC 成功结束；manifest 注册 `RecurrentKda` 与 `DequantSituQuant`，368/368 引用存在，PR10 graph-padding 为 `1 passed`，PR16 DequantSituQuant/真实 WeightNz SiTU 定向测试为 `18 passed`，且 recurrent kernel SHA 为 `5f09a9af…`、`5ec956be…`。验证产物已按哈希同步到 154–156，随后四机以 TP16×DP4×EP64、FlashComm1、vision_chunk、FULL_DECODE_ONLY 和 prefix cache off 启动；四个 rank 均存活，先后完成 worker 初始化并进入完整权重加载。最新证据显示 Rank 0 加载到 79/352 shard、health 仍为 `000`，尚未进入 ACLGraph 捕获或评测，因此 MMMU-Pro、BFCL 和 GPQA 尚无结果；服务确认指向用户提供的经脱敏 multicard 权重路径。
- [四机服务 health=200，MMMU-Pro 评测已启动](blocks/900b2862a0625e607364f91fcc66b31d774596139bc42d3bd426fb82768ac433.md): 隔离四机 TP16×DP4×EP64 服务已完成 multicard 权重加载、torch.compile、KV-cache profile 和 FULL_DECODE_ONLY ACLGraph `[16,32]` 捕获，rank0 health=200；此前 Rank 2 延迟加载但未见退出、OOM、traceback、缺失 binary 或运行时 ERROR。官方 MMMU-Pro standard vision split 前 5 条已由 runner PID `182108` 启动，初始进度为 `0/5`，尚无可报告的正确数、准确率或最终成功结论。后续顺序仍为 BFCL simple_python 前 5 条，再视两组 smoke 结果运行 GPQA-Diamond 198 题；每分钟需报告进度、错误、未解析和准确率。
- [MMMU-Pro完成并暴露Tool Parser回退](blocks/7b13b850c7c4a83f25c3ef1c476ba9704fa7bc54d01a1f11f6f3532a6bb9d0dd.md): MMMU-Pro 从容器无法访问 Hugging Face 的首次失败改用本地 parquet/data URL 重跑，最终 5 条完成、4 正确 1 错误，准确率 80%，errors=0、unparsed=0；期间四机 health=200。无工具请求多次出现 tool-parser fallback ERROR，随后 BFCL 5 条均 HTTP 200 但没有 tool_calls，其中第 5 条生成了正确工具协议文本却留在 content；完整 GPQA 因结构异常暂停，并将证据交给 Tool Use 任务只读分析。
- [BFCL未解析与GPQA两条Smoke失败](blocks/c340cfa41124907f69cafa8d846450384a4d905c73c110f8cb08f3fee002320d.md): BFCL 已完成 5/5，passed=0、failed=5、request errors=0、unparsed tool_calls=5；前 4 条表现为模型直接作答，第 5 条则是正确调用协议未被 parser 抽取并泄露到 content。独立 GPQA 两条 smoke 均 HTTP 成功且生成大量 token，但 API content/output 为空，prediction=None，最终 unparsed=2、可计分准确率为 0%；完整 GPQA 保持未启动。
- [K3 marker空格与tool_choice控制流根因](blocks/140e0c253735b58aac1bb33b10f8c0ce174cc630a538662c0fa4f10d6404d65d.md): 只读分析将共同主因定位为 K3 detokenization 默认 spaces_between_special_tokens=True：连续 XTML marker 被改成带空格形式，严格 reasoning/tool parser 因此无法匹配，导致内容落入 reasoning、content 为空及工具调用不抽取。另有无 tools 请求被 adjust_request 将 tool_choice 从 none 改为 auto，触发 serving.py:1237 fallback ERROR；建议在 K3 parser adjust_request 中设置 skip_special_tokens=False、spaces_between_special_tokens=False，并保留原始 none 语义，但这些仍是待实施和验证的修复建议。临时请求级绕过可显式设置 spaces_between_special_tokens=false，必要时 tool_choice=null；正式默认行为尚未验收。
- [四机健康但评测与解析仍阻塞](blocks/a903dd02f031ce6b10b8c7fb0052ce7fee5cee642de522c6f5402cc6775d7e4f.md): 四机服务持续 health=200、rank 存活且无新增 ERROR，但 MMMU-Pro 为 4/5、BFCL 为 0/5 且 5 条未抽取，GPQA smoke 为 2/2 未解析，完整 GPQA 未启动。Tool Use 审计围绕 tokenize=False 的二次编码、EncodeSegment.allow_special 信任边界、特殊 token 解码、多模态 placeholder 和无 tools 控制流展开，期间没有形成可应用修复 commit，也未修改或重启服务。
- [确认 K3 renderer 信任边界为 P0](blocks/89b93bc9a140ff8114cf8cb247868ab8a6f9bcca2e1d3eb8e688064b5570661a.md): 只读审计确认 renderer 使用 tokenize=False 拼接完整字符串后再由 vLLM 编码，会丢失 EncodeSegment.allow_special 边界，使用户伪造 XTML marker 可能成为真实控制 token；同时 detokenizer 插入特殊 token 间空格和 none→auto→ERROR fallback 被列为独立 P0/控制流缺陷。历史实现对照指向模型级 ParserEngine、每 choice 独立状态、marker holdback、显式 flush 及增量 arguments，但尚未产出补丁或 commit，完整 GPQA 继续暂停。
- [审计收敛但正式修复仍未验证](blocks/c82578092a2eb9b08d4b9e1c086bc27dbb9524e86c93299700d57aefca9f0798.md): 后续快照确认四机仍 health=200、无新增错误，MMMU-Pro 维持 5/5 中 4 对、BFCL 0/5、GPQA smoke 0/2 且 unparsed=2；Tool Use 审计停止扩大范围，但未完成补丁实现或验证。新增只读审计结论标记 MUST FIX：公共 K3 adjustment/to_sampling_params 路径需同时设置 skip_special_tokens=False 与 spaces_between_special_tokens=False，并要求 content_format='openai'、结构化 image parts、可信 image_prompts、强制 tokenize=True 且 sync/async 共用实现；这些建议尚未在服务上操作、重启、请求或验收。
- [K3 Tool Use 审计与统一 XTML 状态机方案](blocks/dccc5138b6b5e805e81f39e1ee5973eda45f1354cbf7346a5f62fb9c27c8cd8f.md): 审计确认 none→auto 会导致 `cannot determine...` ERROR，none→None 只是利用 Parser 的 auto sentinel 偶然消错，不能作为正式修复；required/named 遇未知工具或非法参数、非法 JSON、缺少 tools end 等路径也可能产生空成功或 stream/nonstream 不一致。拟在 v0.23 ParserManager 的 K3-local 专用入口实现统一 XTML 状态机，覆盖 reasoning、content、tool calls、增量参数和显式 flush，并严格验证真实 tokenizer/detokenizer、chunk 边界、多模态、tool_choice、response_format 与 reasoning 过滤。GPQA 证据仍是 HTTP 200 但 output 为空、missing=2；MMMU-Pro/BFCL 结构门禁及完整 GPQA 均未达标。
- [隔离 Tool Use 修复推进，四机验证继续暂停](blocks/d74c6d90b58d7e848827e4b00a29b1dc99c8d8d7632c0ec23d77fc4a6a5e53c1.md): Tool Use 两路审计维持 MUST FIX，P0 包括复合 XTML marker 被 detokenizer 插入空格，以及 renderer `tokenize=False` 二次编码丢失 `EncodeSegment.allow_special` 信任边界；另有 none→auto→ERROR、required/named 空成功和语法校验问题。修复限定在独立分支与 worktree，当前 combined 源码和四机服务未修改；需先取得 commit、diff、验证证据和应用步骤，再精确应用、必要时按 `-j256` 重建并重启。05:21 已报告第一轮本地纯状态机 smoke 通过，但官方 K3 tokenizer 与 vLLM 0.23 真实 incremental detokenizer 验证尚未完成。
- [修复后的分阶段门禁与完整 GPQA 前置条件](blocks/dd9b05559ffbfc14e94fda9ad93fdff0644d38701a84adcb2830fdc3cfac7543.md): 收到正式修复后必须先复核 diff，仅应用到 combined 隔离源码；Python-only 可免 native rebuild，若触发编译需保持 MAX_JOBS=256、OPS_CPU_NUMBER=256、CMAKE_BUILD_PARALLEL_LEVEL=256 并显式 `-j256`，随后按 exact PID/PGID 精确重启四机。默认请求需依次验证 no-tools reasoning/content、BFCL case5、两条 GPQA，再重跑 MMMU-Pro 5 与 BFCL 5，要求无 marker 泄漏、无 `cannot determine`、正确 tool call、GPQA output 非空且 missing=0。只有结构门禁全部通过后才可运行 GPQA-Diamond 198 题；此前完整 GPQA 仍暂停，目标 `>=90%`、`errors=0`、`unparsed=0` 尚无证据。
- [四机健康但 Tool Use 曾越界](blocks/102cf65ed5a53f8c5b3f30dd59b179f62b92c630900db8070a8bc54d1f2c6ec2.md): 四台 k3-pr16-pr10 服务持续存活且 rank0 health=200、无新增 ERROR；累计 MMMU-Pro 4/5、BFCL 0/5、GPQA smoke 0/2 且 unparsed=2，完整 GPQA 仍为 0/198。Tool Use 真实 tokenizer/detokenizer 验证期间曾因 tiktoken.model 下载 401 而访问 lala-153-k3，创建临时目录并运行一次已结束 pytest；该范围违规已停止，遗留目录未删除且未再访问。之后已切换至本地权重、官方 tokenization_kimi.py/encoding_k3.py、tiktoken.model、vLLM 0.23.0 detokenizer_utils.py 的合规路径，但早期命令尚无明确通过/失败结论。
- [本地验证等待汇总](blocks/921c8f3675c442740d57754877020f5bd57f3b69b5df94c604bd74c6ef9cb9f2.md): 05:30–05:41 期间四机状态和累计评测结果保持不变：4/4 exact serve、health=200、无新增 ERROR，且没有评测 runner。Tool Use 未发布新的实现或测试结论；本地真实 tokenizer/detokenizer 命令先后被报告为执行中、已完成但等待摘要，不能据此判定门禁通过。远端容器测试保持停用，lala 遗留目录未处理。
- [正式修复与验收门禁](blocks/c493d2d4fe5a5905047151077561f7ce1d02c5a66a0cf68445b1b07760a2df58.md): 审计要求修复复合 XTML marker 插空格、renderer tokenize=False 丢失 EncodeSegment.allow_special 信任边界，以及 none→auto→ERROR、required/named 空成功、stream/nonstream 不一致、非法语法和 response_format 等问题；完整 GPQA 必须暂停，不能用请求级 workaround 冒充修复。必须先取得独立分支的修复 commit、验证证据和应用步骤，再复核并仅应用到 combined 隔离源码，按需精确重启四机并复验 no-tools、BFCL case5、两条 GPQA、MMMU-Pro 5 和 BFCL 5，结构门禁通过后才可运行 GPQA-Diamond 198题。后续委托消息声称隔离分支修复已完成并提交且未访问四机 API 或容器，但未提供 commit、测试结果或正式验收证据。
- [Tool Use 修复提交与四机重启监控](blocks/21f4275efd13a7c097f16af666ac147d29c09393895b16111496827c7c793dd8.md): 记录声称分支 `agent/kimi-k3-tool-use` 的 commit `36fe4a442` 完成 K3 XTML 状态机、严格 tool_choice/参数错误处理、tokenizer-owned renderer、图像输入及 vLLM 0.23 `auto` typed key 修复；隔离环境报告 64 passed，pre-commit、py_compile 和 diff check 通过，但宿主机 pytest 因缺少 `torch` 无法启动。修复被 cherry-pick 为 combined `2f7b38ee9` 并同步到四机，旧服务按 exact PGID 停止、新服务启动；截至该块末尾仍在 EngineCore/模型初始化阶段，health 尚未就绪，且未执行四机 live GPQA/MMMU/BFCL。
- [四机权重加载与建图推进](blocks/7bcc0dc023409c325ca20c4e8f3cc3ce95d2da4c9b6665b8c51ed4c268323219.md): 四个 exact serve 进程 `209763`、`171564`、`157088`、`156857` 持续存活，服务从 EngineCore 初始化推进到 352 个 safetensors shard 加载完成、每 rank 分配 `180,645 tokens` KV cache，并开始 `FULL_DECODE_ONLY` 两档 ACLGraph capture。多次日志检查报告 `ERROR/Traceback/Exception=0`，但 `18079/health` 在该阶段仍为 `000`，评测和解析门禁尚未开始；建图期间出现等待通信及共享内存广播信息，尚不足以证明无问题或最终成功。
- [建图完成并开始默认解析门禁](blocks/fed75c10d6b0c1f9a695b1ce4c07fab6b7841a7a5144d54117a6bbd73669035f.md): 后续记录声称四个 DP rank 已完成两档 ACLGraph capture，且在 153 主机本地及容器内 `/health` 返回 200；管理网直连 `18079` 超时被归因于外部访问路径，但这一解释尚未形成独立验证结论。计划按默认请求依次检查 no-tools thinking、BFCL case5、两条 GPQA parse smoke，再决定是否重跑 MMMU-Pro/BFCL 和完整 GPQA；截至块末尾仅表示已开始第一条 no-tools 请求并保存原始请求/响应，尚无响应结果或结构门禁结论。
- [GPQA typed 契约修正后复验启动](blocks/932757d26a26402c9ef493d875f4555af479b39b1ef9037d0c0dd45e0e623125.md): 四机服务据报告 health=200、进程存活且无新增运行时异常；no-tools 与 BFCL case5 解析门禁此前通过。旧 GPQA runner 因使用被禁止的 `chat_template_kwargs.thinking` 连续返回 HTTP 400，随后依据只读契约核对改为顶层 `reasoning_effort=max`，删除该模板参数并通过 `py_compile`；两题 typed smoke 当时仍在生成，完整 GPQA 保持 0/198。
- [typed GPQA smoke 通过并启动 MMMU-Pro 复验](blocks/77fde564747322581294fc435d5aab66d6566c611474cebce9f9f6af2840a376.md): typed GPQA 两题复验从 0/2 逐步完成为 2/2，报告称均 HTTP 200、`finish_reason=stop`、`Answer:` 可解析、errors=0、missing/unparsed=0、准确率 100%，且使用 `reasoning_effort=max`；这些是运行记录中的门禁结果。随后启动官方 MMMU-Pro 前 5 条复验，首条 `test_History_1` 报告 prediction/answer 均为 B，reasoning/content 分栏正常且无 XTML 泄漏，进度达到 1/5，BFCL 5 条及完整 GPQA 仍未开始。
- [MMMU-Pro 暴露客户端超时并进行定向复验](blocks/d9bea642ad02db2a0469bbdb0012d81ab874e76f9aac3bfcb4e3e55fa1149d98.md): MMMU-Pro 首轮最终报告为 5/5：4 条正确，`test_Art_113` 因 runner 硬编码 `timeout=180` 触发 `TimeoutError('timed out')`，因此首轮 errors=1、unparsed=1；记录称服务端仍以约 8.8 tokens/s 生成并正常释放请求，health=200 且无日志异常，但这不能把该 case 视为成功。已准备超时改为 900 秒的独立 runner，并对同一 case 使用相同图片、问题、采样配置和 `reasoning_effort=max` 定向复验；当时进度为 0/1，BFCL 与完整 GPQA 仍待后续。
- [四机修复验证与 GPQA 运行记录](blocks/14dd3f8574a5ea9ac470aa632d66c5f76377064959824b05c7445eddeaf18d97.md): 记录 combined commit 2f7b38ee9 的 K3 parser/renderer/chat-params 与 Tool Use 修复、四机文件 SHA/py_compile、服务重启约束及 health/日志检查；本机 pytest 因缺 torch 未启动，不应视为失败。MMMU-Pro 为 4/5（80%，1 个正常答案错误），BFCL 前 5 条协议结构均通过但严格 tool-call 仅 1/5；完整 GPQA 从 0/198 启动，随后出现 68/198、64 对 4 错、errors=0、unparsed=0 等增量结果，仍未完成。后续核对显示 reasoning_effort=max 实际生效，但 runner 未保存 reasoning 字段；当前使用 top_p=0.95 与 Kimi 官方博客的 top_p=1.0 不一致，且官方专属后处理规则尚未完全公开。
- [GPQA 增量精度与服务健康](blocks/629c05f4dc835ba108bd22d5c5c1dcebdfe2cf0e5ea05a0fc37a7a5401bc4aa1.md): 该块复述同一版本、四机部署和隔离范围，并记录 GPQA 从 16/198（15 对 1 错）推进到 32/198（29 对 3 错），期间 request errors 与 unparsed 均为 0、服务 health=200、四份日志无 ERROR/Traceback/Exception。用户质疑 think 未开启后，记录称请求明确使用 reasoning_effort=max，错题更像模型知识/推理判断错误而非 parser 或服务异常；temperature=1.0 但当前采样为官方配置之外的 top_p=0.95。后续进一步要求查 Kimi K3 官方博客参数，并计划在本轮完成后用仅公开参数独立重测。
- [官方参数口径与后续重测计划](blocks/d4b7762f985c1439ce13d856c6a9e609b51f53d0181e868e1828f4e02cbe1d9a.md): 记录将 Kimi K3 官方博客与 lm-eval 默认口径区分开：公开参数为 reasoning_effort=max、temperature=1.0、top_p=1.0，而 GPQA 专属 prompt、选项排列、抽取 regex、stop/retry 规则及其他采样参数未被确认公开。当前 runner 曾用 top_p=0.95、top_k=20、min_p=0、presence_penalty=1.5、max_tokens=65536，并以 re.search 抽取第一个 Answer；对已完成样本的只读重扫未发现首末答案差异，因此尚无证据表明已有错题由 regex 造成。随后结果推进至 73/198、69 对 4 错、94.52%，仍在运行；计划先归档该轮，再建立独立目录，仅配置三个已知官方生成参数重跑 198 题，当前尚未报告完成结果。
- [GPQA 中段进度与运行状态](blocks/aaba1295357003c13b6c31acc898d69e38eb1f6e70aa41836e0d3deb05ada964.md): 完整 GPQA 记录从 74/198（70 对 4 错）推进到 114/198（106 对 8 错，92.98%），期间出现 id=91、106、113 等模型答案错误，但 errors=0、unparsed=0。四个精确 rank、health=200、runner 和服务日志持续正常；部分 ETA 只是按均速粗估，最终 summary 尚未生成，不能视为完成验收。记录还显示 reasoning 输出可达数千至 12,677 tokens，通常 finish_reason=stop 且输出非空。
- [四机修复服务与验证门禁](blocks/7b76664f843d1c918db7918223a393dfa959023a567351728249825f17a0595f.md): 旧服务按 exact PGID 终止并确认 exact serve=0、端口 18079 未监听；新服务使用 multicard TP16×DP4×EP64、FULL_DECODE_ONLY [16,32]、prefix cache off 及指定 DP RPC 端口，四个 rank 进入 EngineCore 初始化，初始阶段尚未确认 weight loading、ACLGraph 或 health。修复版本为 combined commit 2f7b38ee9，记录称 Python 文件同步、SHA、py_compile、Tool Use 隔离测试和 pre-commit 已通过，但本机 pytest 因缺 torch 无法启动。后续门禁要求 health=200 且日志无异常，再依次检查 no-tools thinking、BFCL solve_quadratic_equation、GPQA parse smoke、MMMU-Pro/BFCL 前五；完整 GPQA 及官方参数复测计划在材料中仍未完成验证。
- [GPQA 后段增量与瞬时 SSH 抖动](blocks/87e6150fe1eb601271f41c42c56df350bc693a83bb1ae713299817767bef0201.md): GPQA 从 114/198（106 对 8 错）推进至 131/198（122 对 9 错，93.13%），新增批次包含 118、129 等正常停止但回答错误的题目，errors=0、unparsed=0。一次 rank0 只读 SSH 返回为空，短超时重试后恢复，精确 PID、health=200、日志异常计数均正常，因此只能记录为瞬时采样问题，不能删除该不确定性。runner 约运行 40 分钟，剩余 67 题；“约 21–25 分钟完成”的估计仍属粗略预测，不能替代最终结果。
- [修复服务与官方参数轮次启动记录](blocks/5881317b0acc400312451587656bdd59c038a27d28f6668e182e476902b33107.md): 记录 combined commit 2f7b38ee9 的 K3 parser/renderer/chat-params 修复及隔离环境、四机 py_compile 检查通过；本机 pytest 因缺少 torch 未启动，不能视为失败或完整验证。旧服务按精确 PGID 终止后，新服务以 multicard TP16×DP4×EP64、FULL_DECODE_ONLY [16,32]、端口 18079 等配置启动，初始阶段尚未完成 weight loading/health，日志阶段与 ETA 均存在不确定性。旧 GPQA runner 2876823 停止并保留 139 条结果；随后启动官方博客参数新轮次 runner 371907，reported 请求参数为 reasoning_effort=max、temperature=1.0、top_p=1.0，早期进度从 11/198、16/198 增至 41/198，期间 errors=0、unparsed=0，但这些增量检查不是最终验收。
- [官方博客参数 GPQA 中段进度](blocks/d813eb5ac94176da2ca244f3255f631c3a6409ca97eccd6211b27ff6d1a371f7.md): 正式轮次目录和脚本保持不变，runner PID/PGID reported 为 371907/371907，concurrency32、timeout7200；请求体据报告仅含 model/messages 及 reasoning_effort=max、temperature=1.0、top_p=1.0，明确排除 max_tokens、top_k、min_p、penalties、stream 等未披露字段。后续采样从 48/198（46 对、2 错）推进至 71/198（67 对、4 错），其中记录了 id=75、id=8 等正常停止但答错的题，未见 request errors 或 unparsed。四 rank、health=200 和日志异常计数被多次报告为正常，但仍属于进行中的、可回溯的运行观察，不是最终成功结论。
- [GPQA 后段未完成监控状态](blocks/a8f14ef0b70fdd16d1d2ef49879418a40aea62b1c8482cb4eb1eac98fa79dff0.md): 截至该块，官方博客参数 GPQA 轮次 reported 为 79/198：74 对、5 错、request errors=0、unparsed=0，滚动精度 93.67%；新增批次含 id=106 的错误以及 id=47 的长推理正确结果。runner 371907/371907、四个服务进程、health=200 与四份日志均被报告正常，剩余 119 题，尚未生成或核验最终 summary。此前旧轮次 2876823 的 139 条结果和 run.log 仍保留；目标 accuracy>=90%、errors=0、unparsed=0 只是验收条件，当前状态不能升级为已完成。
- [官方参数 GPQA 轮次推进至 89/198（待核验）](blocks/e698664beb6ebfec00dc6e35f67b259ac74da3aec70dee00d42b6ef4ec517ddc.md): 报告记录新 runner 371907/371907 在四 rank 服务（TP16×DP4×EP64、API 18079、DP RPC 14589）上使用 reasoning_effort=max、temperature=1.0、top_p=1.0，进度从 82/198、85/198 到 89/198，累计 84 对、5 错、request errors=0、unparsed=0；这些是逐次观测报告，尚未完成 summary 与 results 一致性核验，不能视为最终成功。旧轮次按精确 PGID 停止并保留 139 条结果；要求持续检查精确进程、health=200、日志异常和输出质量，禁止 broad kill，且此前本机 pytest 因缺 torch 未启动。
- [官方博客参数 GPQA 111/198，门槛余量收紧](blocks/ee664f4b8a6102c1a823199f127cd99154d0d6249a7c8c5f8e350d6dcf572207.md): 报告显示 Kimi 官方博客参数 GPQA-Diamond 进行至 111/198，101 对、10 错，request errors=0、unparsed=0，滚动准确率 90.99%；最近 6 题为 5 对 1 错，错题含 id=80（D→B）。runner 371907/371907、四个精确服务进程和 health=200 均被报告为正常，日志未见新增异常；这些是运行中检查结果，并非最终一致性核验。
- [GPQA 131/198 至 151/198，准确率接近门槛](blocks/c39d18f7587043fae1abc34f1fad2fe4a385106f794576c91be251b8f93c72ee.md): 报告进度从 117/198（107 对、10 错）继续到 151/198（137 对、14 错），request errors=0、unparsed=0，准确率在 90.73% 至 91.45% 间波动；期间记录了 id=102（B→A）和 id=157（C→B）等正常停止但答错的题。四机服务、health=200、runner 和日志均被报告正常，部分阶段明确称 summary.json 尚未生成；剩余题量与“最多还能错”是按当时报告计算的门槛估计，不是最终结果。
- [GPQA 157/198 至 161/198，仍在运行](blocks/23f21c70d863b72f3d9ebb3ccd08722d663520899dc17eccbea45664c0195e2c.md): 本块报告进度由 157/198（143 对、14 错，91.08%）增至 161/198（147 对、14 错，91.30%），request errors=0、unparsed=0；最近 6 题及随后 4 题均报告为正常 stop、输出非空且可解析。runner 已运行约 50m35s，四个精确服务 PID/PGID 存活、health=200，服务日志异常计数仍为 0；任务尚有 37 题，且未提供最终 summary 与 results 一致性核验。
- [GPQA 运行推进至 178/198，PR24 已合入但未应用](blocks/f0c8aa34d7b452fcf46412b0df88d0c5ee0a6844d81b8fadf20a4f46fc0b50d6.md): 正式 GPQA 轮次从 161/198 推进至 178/198，报告 164 对、14 错、errors=0、unparsed=0，准确率 92.13%；最近新增题目均报告为正常 stop、输出非空且可解析，但 runner 仍存活，最终 summary 与 results 一致性尚未确认。期间记录 PR #24 已合入 releases/v0.23.0_ci_new，squash commit 为 7a74db55cbbcdf56bd7efa07c2901e4e92f0bc89，包含请求兼容性修复及 tokenizer-native chat_template_kwargs、response_format/schema、tools/tool_choice:null 等处理；四机服务和当前 GPQA 未重启或变更。
- [GPQA 由 181 推进至 191/198，仍在途且出现新增错题](blocks/87d45a7e3ed2704600df58df7353f392fe0592ded18a9c9c13a07dcafe785f86.md): 该段报告 GPQA 从 181/198 推进至 191/198，累计最高记录为 175 对、16 错、errors=0、unparsed=0，准确率 91.62%；id76 曾记录 D→A，id130 记录 B→C，均为答案错误而非请求或解析错误。四个精确服务 PID/PGID、rank0 health=200、runner 371907/371907 多次被报告存活，日志未见新增 ERROR/Traceback/Exception；但 runner 尚未结束，summary.json 尚未生成或尚未完成最终核对。
- [GPQA 推进至 193/198，最终五题仍未完成](blocks/340314af65fe6c4b0a86b425fc9e92804562a24556d01e21769f429d86d01ac1.md): 后续报告将进度从 189/198 推进至 193/198，累计 177 对、16 错、errors=0、unparsed=0，准确率 91.71%；新增题目包括 id183、id127、id180、id15、id186，均被报告为正常 stop、输出非空且可解析。runner 仍存活约 72 分钟，四机服务及 health=200 正常、日志无新增异常；剩余 5 题，尚未看到最终 summary 与 198 条 results 的一致性验证，因此不能视为最终成功。
- [GPQA长尾监控：194至195题，尾题仍在途](blocks/ab5e68dc4fa8b873c7056e33adfd1192cf6b8c0b4507e4ae48bd1843da6431fe.md): 正式 GPQA 轮次从 193/198 推进到 195/198，报告 178 对、17 错、errors=0、unparsed=0，准确率降至 91.28%；id90 答对、id88 出现 D→A 错误，剩余 3 题仍在途。多次报告四个精确服务 PID/PGID 存活、rank0 health=200、日志无 ERROR/Traceback/Exception，但 runner、summary.json 与 results 的最终一致性尚未确认。后续用户检查复读/乱码时指出非流式请求无法读取隐藏推理；id147 的可见正文扫描未发现 XTML、替换字符、控制字符、重复行或重复 8-gram，id145、id185 仍未完成。
- [GPQA尾题状态与非流式输出可见性限制](blocks/ce8e807f00d4d9e8756a76417cc26757bd9e335d5bded460fce26fa45cbb3d32.md): 监控要求持续核对 198 题进度、精确服务健康、runner 存活、日志异常、请求错误、unparsed、准确率及最终 summary/results 一致性；相关阶段从 195/198 继续停留到 196/198，178 对、18 错、errors=0、unparsed=0、准确率 90.82%，仅剩 id145、id185。已完成的 id147 报告为 32,143 completion tokens、可见正文 1,258 字符，复读/乱码扫描暂未发现异常；但非流式接口仅保存 message.content，未保存 message.reasoning，因此无法排除隐藏推理内部复读。该阶段仍有长尾请求在途，最终门槛、summary 生成及完整结果核验均未确认。
- [用户要求终止旧轮次并改测 ci-new+PR10](blocks/9bcbfd56faa8c5f61e6080c278aa8d331a79748fc11708d1834c0200bdada14a.md): 用户随后要求 kill 当前服务和测试，只用 ci-new 搭配 PR10 测试 GPQA；此前正式轮次仍停在 196/198，id145、id185 未返回，且没有最终复读/乱码结论。助手表示将按记录的精确 PGID 终止 runner 371907 及四个服务组，并保留日志和结果、避免 broad kill；随后报告 runner 进程组已停止且无组内残留，并开始向四个服务 PGID 发送 TERM。新 ci-new+PR10 构建和 GPQA 验证只是计划，服务终止后的完整确认、构建结果及新测试结果尚未提供。
- [ci-new+PR10服务通过门禁，正式GPQA进行至43/198](blocks/231c68e53a252784384d38e55102bfc6074ae10719521ce1bddc11597e8cd574.md): 四机 ci-new+PR10 服务完成 clean 构建、二进制/manifest/schema 及仓内 A3 定向测试门禁；外部 fla_npu PTA 脚本因依赖缺失未启动，不计作内核失败。服务已健康（health=200），四 rank 精确 PGID 存活且日志异常为0；非流式两题结构 smoke 为2/2正确，随后用户取消了流式方案，正式测试改用原 SHA=d1d3d8…dfe235a 的非流式脚本，以 concurrency=32 启动198题。正式轮当前43/198完成，42对1错、errors=0、unparsed=0、准确率97.67%；id=17预测D而标准C且finish_reason=stop，属于答错而非解析错误。已完成输出暂未发现替换字符、NUL、XTML或启发式复读证据，但测试仍在进行，不能据此宣称完整198题成功；早期监控读取异常已定位为splitlines误把输出中的非LF分隔符当记录，实际15条JSONL均有效。
- [PR10阶段结果与算子契约修正](blocks/b16fc59fa58b1d1855645c3e5a53c019ec76f649ed03c2a70db929fef46e91f8.md): ci-new+PR10 冻结快照的 GPQA 监控从 46/198 推进到 187/198，期间出现 1 次真实 parser 500（closing message marker 缺失）并经重试恢复，另有瞬时 health/SSH/进程探测假阴性；结果文件持续 errors=0、unparsed=0，但该轮未完成，不能视为最终成功。与旧 PR16+PR10 版本的阶段性同题比较一度显示准确率更高但延迟更慢，且旧轮最终仅 196/198，结论受样本阶段、随机温度和未完成状态限制。随后按精确 PGID 停止旧服务，并纠正 A3 规格：DequantSituQuant 同时服务共享专家和路由专家，路由输入为 BF16、不执行 dequant；A5 独立使用 SituMxQuant，TP16 实机采样范围也被限定为仅 prefill/decode 真实数据。
- [PR16切换前的服务停止与版本固定](blocks/6e305f391c5f3b3b30e8d0cb393c249e95044104880319f444f4062f447a97e2.md): 旧 GPQA 轮次继续到 192/198、181 对 11 错，6 个长尾请求仍以非零吞吐生成；runner 曾被一次性探测误报为消失，后确认仍存活，因此没有把该轮判为成功或故障终止。随后用户要求停止服务并转入算子工作，四个服务组和 runner 按精确 PGID 发送 TERM，服务最终仅剩僵尸成员、API=000，结果与日志保留在 192/198。为验证 ci-new+PR16，固定了 ci-new `27887632…` 与 PR16 `df26e35…`；PR16 首次干净构建受 AppleDouble `._*` 文件污染失败，第二次又因 Torch adapter 缺少 `weight_scale_value`/`activation_scale_value` 链接失败，补回两行未提交热修复后构建成功。
- [Prefix Cache与FlashComm开启后的PR16 GPQA](blocks/16ccb71fb34ba72d0862deac91b0455fe928240625b11564705262fbea0e45d6.md): ci-new+PR16 运行时完成构建并在四台机器一致部署；按用户更正重新打开 Prefix Cache 和 FlashComm1，四 rank 日志与 EngineCore 配置均确认启用，服务经历权重加载、编译和两档 ACLGraph 捕获后达到 health=200。新的 TP16×DP4×EP64、FULL_DECODE_ONLY `[16,32]` 服务启动 GPQA-Diamond 198 题，runner 为 `848342/848342`；截至材料末尾完成 55/198，54 对、1 错、errors=0、unparsed=0、准确率 98.18%，四机 health、PID/PGID 和日志异常计数均正常。该轮仍在进行，长推理造成吞吐波动；当前数字是阶段性结果，不能升级为最终准确率或完整门禁通过。
- [ci-new+PR16 GPQA推进至141题及错题对比](blocks/7aa24b05161a3ed5675c7a3ffdf36a13f2bc4e37e079b1f68f7b077af686d597.md): ci-new+PR16 的 GPQA 从约55/198推进到141/198；最新为135对、6错、errors=0、unparsed=0、95.74%，新增错题包括167（当前D、标准A），此前76与 recurrent_kda-only 共同答错。与 recurrent_kda-only 的实时交集显示，102、113由错变对，75由对变错，17、30、76、106稳定错；这些均为正常可解析的 stop 结果，不能归因于 parser。期间一次监控命令发生 shell 引号错误、代理返回502，后确认502来自本机HTTP代理而非服务，绕过代理后 health=200；四机进程、日志异常计数和runner持续正常，但测试尚未完成，不能据此宣称最终精度或门禁通过。
- [旧轮次收尾与PR16最小修复](blocks/02266baa7e11c70abb3f219d0fc7c8055fb274af6293d184ade93166cc741221.md): GPQA 旧轮次推进至196/198：185对、11错，errors=0、unparsed=0；最后题目71、147因超长生成仍未返回，结果不是完整198题终值。此前曾误将latency与7200秒HTTP读取超时等同，后续更正为latency包含semaphore排队；runner只保存content和usage，无法审计未落盘的reasoning。PR16已通过隔离worktree提交两个optional scale Tensor物化修复，fast-forward head为65233bec…，但新CI状态仍未完成验证。
- [切换新权重并发布阶段性记录](blocks/f3140216462138d71efee396ce0292b5e49750718dddae7fc61b158944ce8681.md): 旧服务按精确PGID停止并保留196行结果；随后以相同TP16×DP4×EP64、EP64、Prefix Cache和FlashComm配置启动[REDACTED_SECRET]，经历64-rank初始化、347个权重分片、torch.compile与ACLGraph capture后health=200。新轮次正式GPQA已启动，脚本SHA为d1d3d8c…fe235a、concurrency=32、timeout=7200；早期结果持续无errors/unparsed和输出质量异常。用户中断的旧轮次196/198快照、启动参数及后处理规则已发布到PR16评论[#issuecomment-5061566817](https://github.com/vllm-ascend/vllm-ascend-kimi-k3/pull/16#issuecomment-5061566817)，该评论记录的是阶段结果而非最终成绩。
- [新权重精度差异与长尾生成](blocks/5621f5abbf289f54e344d96ad5f95725d57ad8db8aacdf4e3155de201a5cfd10.md): 新权重轮次推进至190/198：178对、12错，errors=0、unparsed=0，准确率93.68%；剩余8题仍在生成，未形成完整198题结论。与旧权重的已完成同题比较出现多处答案变化：ID81、89、113、102为旧对新错，ID157为旧错新对，另有75、76、88等错题选项变化，累计差异记录到8题、净差约−3题；这些是正常stop后的推理结论差异，不是解析或乱码故障。尾题因未设置max_tokens可能生成很长，但Engine吞吐、KV使用和health=200持续显示仍在推进，不能据阶段快照断言最终失败或成功。
- [上一轮新权重GPQA完成及ci-new启动准备](blocks/8920383946ac4b7cff05a7856bbaabc7f2d131fa1a50ce031d7ee7b11e82c467.md): 上一轮新权重GPQA最终完成198/198，182对、16错、91.92%，errors=0、unparsed=0，全部结果为finish_reason=stop；ID 71首次请求曾因约7200秒read timeout取消，重试后答对。共同196题对比显示旧权重185对、新权重180对，净少5题；全量输出扫描未见乱码、XTML、控制字符或明显复读。随后转向ci-new分支与multicard-0724权重，确认四机权重一致并创建隔离构建目录；启动脚本首次传输路径有误，但未影响源码或进程。
- [ci-new构建与multicard-0724服务启动](blocks/835697d257ff93e9035dd1e511cf101c09edef4f8cfbc16025fa9b032dc21fc1.md): 最新ci-new提交17cb94b9e的原生构建以退出码0完成；第一次向154容器的流式同步因SSH连接重置中止，随后改用带校验和的归档方式，四机扩展、内核so、manifest和头文件哈希一致。四机服务按TP16×DP4×EP64、Prefix Cache、FlashComm1和FULL_DECODE_ONLY配置启动，经历权重加载、torch.compile及NPUGraph capture后，容器内health=200且模型ID为kimi-k3-ci-new-multicards-0724。GPQA正式运行198题、并发32、timeout 7200；截至113/198为107对、6错、errors=0、unparsed=0，期间一次SSH状态查询变慢但独立检查确认runner与服务正常。
- [ci-new GPQA进入最后长尾](blocks/49d07c9db57140583648f9c3e9709563ebac149c1ea47a132c4a488aca9c1efb.md): ci-new轮次持续推进至196/198：182对、14错、errors=0、unparsed=0、accuracy 92.86%，剩余ID为145和147，health=200且runner仍在运行。此前ID185已正确返回，finish_reason=stop，耗时6353秒、completion 28,619 tokens，文本未见替换字符、NUL、XTML或复读；长尾请求仍可能继续生成。过程中的阶段性统计包括194/198时180对14错，以及ID121完成后195/198；这些不是最终完整结果，ID145和ID147的最终答案、质量和全量复核仍未确认。
- [ci-new完成并切换93_4，首次启动因运行时包缺失失败](blocks/4f6453f98fdaa9844f6de5191a9f66bf19c4f2f956924672be2d2ccd21e35bd0.md): 此前ci-new GPQA已完成198/198，182对、16错，accuracy 91.9192%，errors=0、unparsed=0；独立审计和四机服务门禁通过。切换到93_4_ci_new后，首次四机启动约16分钟时退出，首因是复用.so和csrc/build时遗漏运行时_vllm_ascend/_cann_ops_custom，导致libopapi.so中的aclnnAddRmsNormBias不可用；补齐包后尚未完成真实NPU探针，桌面SSH认证一度被拒绝。
- [93_4_ci_new 启动与数据集重建记录](blocks/c7618228dfc6dd0a76ca33ff500a3d2ee8f084e30b1f1fcd1e6a2e89dba866d2.md): 真实 NPU 探针报告 `enable_custom_op=True` 下 `npu_add_rms_norm_bias` 在 NPU0 返回有限的 BF16 `(1,16)` 与 FP32 `rstd (1,1)`；四个 DP rank 随后完成 2 个 FULL_DECODE_ONLY 图捕获，日志新增 `ERROR/Traceback/Exception=0`，rank0 曾报告 `health=200`。正式 runner 首次因容器内缺少旧数据集路径触发 `FileNotFoundError` 并退出；数据集重建的第一次审计又因 Python `splitlines()` 错把 U+2028 当作 JSONL 边界而出现 `JSONDecodeError`。改用严格换行分割后，报告称 198 条输入逐字段匹配并启动正式轮次，但该块没有提供完整 GPQA 结果或最终比较。
- [93_4_ci_new 长时间监控指令](blocks/fc7751bac2c48d37cc3246eea70e4a7d1fc4e76f4a3de0a60964a743924e0b70.md): 多次 heartbeat 要求持续监控 `93_4_ci_new` 的 GPQA-Diamond 198 题运行，并与此前 `ci-new` 的 `182/198`、`91.9191919%`、`errors=0`、`missing=0` 结果比较。指定环境为四机 TP16×DP4×EP64，prefix cache 与 FlashComm1 开启，FULL_DECODE_ONLY `[16,32]`，API `18079`、RPC `14589`，并要求仅检查精确 PID/PGID、health、日志错误、runner、summary 和结果质量。该块只有用户侧监控要求，没有新的 completed/correct/wrong/errors/unparsed/accuracy 数值，也没有证明轮次完成或服务状态最终保持不变。
- [93_4_ci_new 后续监控范围](blocks/7a6f8250129e962968e40b8ca7c30b48cf05cb2f7f0d06a52af6bdab559a71b4.md): 后续 heartbeat 延续同一 198 题 GPQA-Diamond 任务、四机精确进程范围和与 `ci-new` 基线 `182/198（91.9191919%，errors0，missing0）` 的逐题比较要求。监控条件仍包括 TP16×DP4×EP64、prefix cache ON、FlashComm1 ON、FULL_DECODE_ONLY `[16,32]`、API `18079`、RPC `14589`，以及检查 ERROR/Traceback/Exception、复读乱码和最终 fixed/regressed/unchanged 集合。该块仍未给出实际运行统计、最终审计、质量扫描或完成结论，因此不能把持续监控要求视为验证成功。
- [最新 ci-new 构建：算子成功但 host 扩展因 SoC 名称失败](blocks/5921ce90563809b8473a3d274da23f94d7bc9d7a933b54b54ec8ca5c909f104c.md): 本批记录了最新 `releases/v0.23.0_ci_new` 提交 `d954213e9d872c7f04d86c543de63a1864ed2ed7` 的准备过程：prefix cache 与 FlashComm1 显式开启，`enable_shared_expert_dp=false`，并以 `MAX_JOBS=256`、`OPS_CPU_NUMBER=256`、`CMAKE_BUILD_PARALLEL_LEVEL=256`、`USE_NINJA=1` 编译。A3 自定义算子阶段完成 8/8 Compressor，日志报告 `build_aclnn.sh executed successfully!`；但后续 editable wheel 的 host CMake 阶段失败，首个错误是使用了不支持的 `SOC_VERSION=ascend910_93`，当前 CANN 要求 `ascend910_9391`。记录提出在不重编已成功算子的情况下单独修正并编译 host extension，但尚无完整安装、四机启动、GPQA 结果或功能验证证据。
- [PR #12950第三次启动进入worker初始化](blocks/fc5b1696f1761969a58985ee3ad61c7eee8677d924c82f5904a40dbdcf97cafe.md): 两处纯Python修复后的第三次启动中，四个精确launcher仍存活，日志暂无叶子异常；此前health=000被判断为初始化阶段而非已就绪。进度显示四台串行创建本地worker，global rank从9/25/41/57推进到14/30/46/62，8077尚未ready，尚未开始351 shards权重加载；因此PR功能验证和完整GPQA精度仍未完成。记录还保留了SSH用户选错、远端检查中断等方法性问题，以及此前同名临时文件操作曾短暂清空153/154运行时文件、后已恢复的纠正。
- [SiTU、权重加载与共享专家链路修复](blocks/e46d5d3f56816f623360683c1b4e03e51e2f73252ccc1b33fafb582d1e44eb59.md): 定位并修复了 vLLM 0.25.1 中 SiTU activation 桥接、Kimi K3 KDA OOT 注册、A_log loader、NFS safetensors mmap/prefetch、dense SiTU 及 shared-expert 输入契约等多个阻断点；真实环境定向测试从 6/6、27/27、28/28 等通过。四机多轮 TP16×DP4×EP64 启动中曾出现 Gloo mesh reset、UntypedStorage shape 错误、3584/7168 输入维度错误和错误启动拓扑，相关失败与修复过程均保留。后续修复已通过权重加载、AOT 编译和 initial profiling，但随后又暴露 K3 identity RoPE/decode cache、parser 参数透传及请求层 API 兼容问题，不能仅据中间阶段判定端到端成功。
- [共享专家输入契约修复与实机编译验证](blocks/8a037cf4b29b387a03433ff7bbbf9584289d0d200bee6d64f30e7694f2fa7000.md): 清理重复启动进程后，干净四机服务完成 64-rank Gloo、EP 映射及 352 个权重分片加载；诊断确认 shared path 错把 routed latent `[32,3584]` 传给需要原始 hidden K=7168 的量化权重。修复 `_forward_impl` 丢弃 `shared_experts_input` 的接线问题后，真实环境定向测试达到 28 passed，AOT 编译和 initial profiling 完成，未再出现 3584/7168 shape 错误。随后仍出现 decode identity RoPE 的 128/512 reshape 问题及 `NoneType` RoPE cache 问题，后者定位为 decode graph-capture 未复用 K3 的 identity 分支；这些结果证明了局部修复，不等同于最终任务成功。
- [图捕获、API 兼容修复与GPQA启动](blocks/5a4ed7596425037128cd5d191dd7331592a0c28e6432cf98cdfeb7735ae19ea1.md): 修正 no-RoPE MLA decode 返回整页 cache、prefill 返回当前 token 的契约后，四机完成 7/7 ACLGraph 捕获并达到 health=200；parser 的 `is_harmony` 透传和非流式 `mm_token_counts` 兼容问题也分别通过 4/4、70/70 测试，相关修复测试合计报告为 176 项通过，另有 MLA A2 78/78。真实功能门禁报告 no-tools、BFCL tool-call 抽取和 SSE 流式重组均通过，四 rank 无异常；本机缺少 `gitleaks`/`wget` 的检查失败、旧 tokenizer fixture SHA 不匹配以及若干启动/SSH 传输问题均被保留为环境或流程限制。完整 GPQA-Diamond 已以 198 条数据、并发 32 和指定参数启动，记录了脱敏目录、PID/PGID 及数据集/runner SHA，但该批材料尚未给出 GPQA 最终准确率或 PR 最终单提交/DCO 结论。
- [GPQA暂停并切换vLLM基线，Ascend编译尚未启动](blocks/17ea1beb65555d8422c11c65ad3e4eb8fdf55e7a3e6cd516d6ce1d029beaf268.md): GPQA在至少48/198时为47正确、1错误、0 errors/0 unparsed，唯一错题与既有基线相同；随后runner已不在进程表中，71条未完成结果和日志保留，因此不能视为完整精度结果。远端已推送单提交`5b642437d`，但后续远端分支被更新为`2eadb7e03`，并已回退此前为vLLM 0.23.0加入的12个兼容文件增量；远端网络问题通过本地传输和SHA校验规避。四机源码已对齐精确commit`fe784ff22e630a31fd798f392b01e0a75c18f047`，安装版本显示为`0.1.dev1+gfe784ff22.empty`；原计划以256并发编译vLLM Ascend尚未实际开始，首个wrapper因`nounset`与Ascend `set_env.sh`不兼容而提前退出。
- [四机编译、运行定位与单机验证准备](blocks/90a5ba7ebb1b5a3d13203abd8fe32269ddbc1bfa07a19512fad39ec9be0a05f1.md): vLLM 已对齐精确提交 `fe784ff22e630a31fd798f392b01e0a75c18f047`，四机 256 并行编译和 189 项定向测试曾通过；运行初期发现显存不足、meta tensor `.to()`、以及其他 Kubernetes 服务争用 NPU 等问题。meta tensor 修复为纯 Python，未重编译 native 算子；后来确认 PR 已包含 `_move_module_to_device` 相关修改。单机 5 层验证期间继续暴露旧容器依赖、`acl` 路径、SiTU/FusedMoE、A_log loader 等兼容问题，且曾在旧本地树上定位，不能直接等同于当前 PR 验证。
- [PR head 单机五层实机验证成功](blocks/de40d2b7796a6bfc99d61795632b54176bd5e2c0070fce1c026bc3e9fe2c9cb9.md): `fdf3cad` 被确认不是本轮提交；PR 后续 head 更新为 `4a1857bed6b385cb58a7b7eabf91d462345050cb`。在 exact vLLM `fe784ff22`、单机 TP8、5 层 INT4 环境中，PR head 成功加载 19/19 权重、完成 KV cache/profile、warmup，并生成 2 token `[128820, 96476]`；这是功能成功证据，但此前旧树上的 `situ_beta` 和 A_log 错误不应冒充当前 head 结果。期间曾精确停止 `mindie-motor/vllm-u0` 的 4 副本以释放资源，未停止集群控制面或其他 workload。
- [四机完整权重门禁通过，后续冲突与FC1待修](blocks/81f377321609d7ea2f2da23f25e7fe1effff921e29bbaddedb2184cc84e9f816.md): PR `4a1857b` 配套 vLLM `fe784ff` 的四机 TP16×DP4×EP64 完整权重服务曾成功启动，health=200，8 路并发数学请求全部 HTTP 200 且答案正确，未复现截图中的 KDA 16↔256 广播错误；服务随后按要求被直接终止。之后发现 PR 与最新 base `81d3450` 冲突，且单机 5 层重新验证时先排除环境、路径和权重挂载问题，最终确认 FC1 已开启但临时 runner 漏传 `enable_expert_parallel=True`；该修正尚未形成最终提交或新的 FC1 成功证据。
- [四机 FC1 修复后通过图捕获，但在线首请求超时；PR CI 根因已定位](blocks/96794d9d9f9a32ca3794d189f588f343b6442c8f6e09ff9fac520bce42856f66.md): 单机真实权重验证曾通过，Kimi KDA 的 `g_proj` 通用 SequenceColumnParallelOp 误匹配导致的 `8 vs 64`/TP16 gate shape 错误已在修复快照中消失；四机也完成 64 rank 初始化、`torch.compile` profile 和 `[16,32]` 图捕获并达到 health=200。但四机首个在线 prefill 在 `sample_tokens` RPC 等待 5 分钟后超时，rank0 API 进程退出，因此四机 serving 功能尚未验证成功。PR #12950 的 CI 失败已定位为重新引入 `QuantType.NONE` MegaMoE fallback 及相关 K3 SiTU/shared-expert 逻辑，另有 `globalBS=256` 与 `epWorldSize=2` 不匹配；CI 修复和在线门禁仍未完成。
- [单机在线门禁已完成权重加载，尚未到 health 或推理结论](blocks/eb70d6c38bca59c6a9e50861e12f95d569a7504e92ef179cce94ce2eabd9a2dc.md): 修正 `max_num_batched_tokens` 后，进程 `120463/120463` 存活并完成 TP8/HCCL worker 初始化及 8 个 rank 的权重加载。FC1/KDA 原始 shape 异常未再出现，当前仍在逐卡 CPU/IRQ 绑定阶段；尚未提供 health=200、在线请求返回或完整 serving 成功证据。
- [四机 FC1 首请求超时定位](blocks/fb9991d73d191e7796b9ff273a6ec4448f68037bcd410f7599d76dd7463b83af.md): 单机 5 层 eager 与 FULL_DECODE_ONLY 均返回 HTTP 200，但完整四机 FC1 ON 在 eager 和图模式首请求均于约 300 秒触发 `sample_tokens` RPC timeout；FC1 OFF 对照同请求约 60.70 秒返回正确答案 391。trace 将阻塞点定位到首个 MoE 层 `before_mlp`，根因指向 FlashComm1 后 DP token 数 7/1/1/1 与 MC2/EP64 通信契约不一致；后续手工 padding 未能修复，AllGather/AllToAll 绕路又产生复读、乱码或控制 token 错误，因此这些尝试均未验证成功。
- [MC2 padding 与 AllGather 回退均未通过](blocks/b4127c3f28662034d5be9ab3b4370292fab7bc0c6faed07288962f35f8986a7e.md): 单机 TP8/EP8 的多轮 smoke 可完成 prefill/decode，但四机 v7 的 MC2 padding 仍在约 300 秒后 `sample_tokens` 超时；v8 的 Kimi 专用 AllGather fallback 虽解除死锁，却返回 `content=null`、`of of...` 复读并截断。v9 证明零初始化 DP padding 不是主因，单机 FC1 ON/OFF 也产生不同 token，逐层 A/B 将首个分叉锁定在 layer1 SiTU W4A8 MoE；AllToAll 又出现乱码和 `Unexpected K3 control token inside text content`，v11 uniform MC2 仍卡在首个 forward。
- [在线通信选择不一致，v15 进入验证](blocks/88f8bae1abac6238c842b20726417129c9d9191726488ab09a1c4ee2ce9a7459.md): v10 实机确认 AllToAll 能越过死锁但输出质量不合格；v11/v12 的 uniform MC2 与静态 profile 仍不足以证明真实 7/1/1/1 请求可用，v13 还出现 TERM 后残留 worker 占用显存，随后通过精确 PGID 清理并在 v14 重启。v14 在线阶段日志最终发现同一 batch 中 DP0 选择 AllToAll，而 DP1–3 选择 MC2（`mask/global_bs` 不同），导致不同 collective 互等；修复为依据跨 rank 最大 token 数统一通信方法并共同 padding/裁剪，v15 已启动且初始化无异常，但尚无首请求结果，不能视为修复成功。CI 方面，shared-expert-DP 参数扩展和 `QuantType.NONE` 误入 generic FusedMC2 两类回归已定位并收窄/恢复，最终提交与 CI 仍未完成。
- [v22 通用 MoE 判定修复已启动，在线一致性仍待确认](blocks/e2427fff82e6a055fdd1031d0d2b0caddbdf0e1082a1d0e76ea995787b8c506a.md): 四机完整权重、health=200 和启动阶段检查已多次完成；此前在线 2+2 请求仍出现 DP0 走 AllToAll、DP1–3 走 MC2，证据显示非主 rank 只收到本地 token 向量 `[16,16,16,16]`，而 DP0 收到 `[96,16,16,16]`，请求因此卡住。针对 Kimi 的专属判断已撤销，当前修复改为按具体 `vllm_config` 独立判断 `is_moe_model()`，避免 target/draft/headless 初始化顺序污染进程级缓存，适用于所有 MoE；v22 尚未证明在线请求成功。最新状态是四 rank 日志无 ERROR/Traceback/Exception、health=200，1-token 请求正在执行，仍需核对全局 token 向量和 collective 选择；FULL_DECODE_ONLY、临时诊断清理、PR CI 和最终提交也未完成。
- [KDA 最小 FC1 修法单机通过，四机启动仍待核实](blocks/ff23f9ea41576b929fdca93846dac2efe7c591d37190f11379428f39aae9d75d.md): 四机 FC1 首请求挂起被定位为通用 MoE/DP metadata 分裂现象；此前 EP 优先判定、padding、dispatch 和通信选择改动均属无效诊断并已撤回，不能作为最终修复。当前修法限定为 KDA 入口统一 AllGather，g_proj 保持普通 ColumnParallel，不单独触发 AllGather；单机 5 层已通过初始化、prefill/decode 并生成 `[79451, 46956]`，未再出现 KDA `16/256` shape mismatch。四机文件同步和进程启动已完成，但 rank0 子进程出现 `APIServer pid=... Traceback`，主根进程仍存活；尚未证明服务 ready、请求成功或跨 DP metadata 一致，CI 也未单独修复。

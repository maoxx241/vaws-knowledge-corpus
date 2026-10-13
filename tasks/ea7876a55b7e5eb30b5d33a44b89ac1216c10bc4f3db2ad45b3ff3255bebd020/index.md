---
blocks:
- block_id: 0e6ec148c857ae4057bb4b8c0447934672bf95d69ad3d812046567da110c12aa
  indexed: true
  sha256: 0a49fd6f530594b949d8808d1f5c227c648a17f102f7351ec976d30d1c88f256
  source_range:
    end: 16809914
    part: 0
    start: 0
  summary: 定位显示，原始问题中已直接捕获 draft layer 94/block 1969 与 target layer 23/block 5224 的物理 backing collision，随后 target KV 首次出现 NaN；这比后续修复提交更像根因。当前 KDA/GDN 不能使用首轴非连续 view，若从历史修复中选择最大嫌疑，则是可能不完整回迁 GDN/FULL graph replay metadata 的 `af51098fa`/`637d49d04`，但尚未完成因果验证。上游对 stale block-table、多个 full-attention group 裁剪、Mamba prefix 边界和 zeroer 生命周期已有相关修复，建议分别 A/B 验证，避免把假设当成成功。
  title: dspark+prefix cache 精度问题定位与系统化方案
- block_id: 2a786ab3aa84b1d152c252a32cedd2f2cb43928f48ee32bcd1afc9dd121275ec
  indexed: true
  sha256: dbebec4e7ca95a15f0f3bc48ce567f7474d2303133d3d285c3ff12011682b1fc
  source_range:
    end: 16809914
    part: 1
    start: 0
  summary: 方案将 GQA 从独立物理页改为沿用已验证的 MLA+Mamba 公共布局，通过 4:1 parent-page/subslot packing 保持首轴连续；1M 上下文新增浪费估算约为 0.30334 GiB/TP rank，原有 padding 合计约 0.88935 GiB/TP rank。仍需修复 FULL replay stale row、多 full-attention group 共同命中边界、Mamba committed boundary admission，以及 non-uniform zeroer/H2D buffer 生命周期；419c315 只作为历史对照，不能宣称已解决精度问题。当前 exact writer/victim 验证尚未完成，建议先以 `6fbbcf215`、`a5836312d`、`8472b0161` 和 capability-based layout 分别做受控 A/B。
  title: 4:1 GQA padding、生命周期契约与验证顺序
- block_id: 1cee7e74db6378eed6cd7078418545f5b820dae389daef22e3789d7a74b9b62b
  indexed: true
  sha256: e0062029de54b10b8f585fb92f686affc7b1a45a8b789ceef57078473ca3ef79
  source_range:
    end: 16809914
    part: 2
    start: 0
  summary: 独立 GQA 页和 runtime fallback 被明确废弃；当前唯一合法路径是从实际 rank-local page bytes 推导 packing factor，TP16/8/4/2 当前均为 4，但不把 4 固化为生产常量。H2D 初步根因是 side-stream 状态更新完成后缺少到下一轮默认流/H2D 缓冲复用的 GPU event 依赖，修复应采用 GPU event 链而非 CPU synchronize；随后逐项落地 1/2/3/5 并保持可二分。候选测试曾达到 59/59 通过并完成四节点初始化和图捕获，但首个 smoke 暴露 GPU-only align 仍无条件调用旧 CPU `do_mamba_copy_block` 的变量未定义错误；该公共调用随后被移除，仅保留真正的 legacy 分支，r2 重启验证尚未给出精度结论。
  title: 放弃独立页并优先修异步调度与精度闭环
- block_id: f1fbfb80ecfc4ab165d252c1b9889084f276bc21678918f2a228c200fc0d1431
  indexed: true
  sha256: 391b24366eb1eb7701f886a2674afb9fa39812cbb3ed584bb4f8668387ea1f12
  source_range:
    end: 16809914
    part: 3
    start: 0
  summary: r2 完成权重、KV 规划、capture 和首个 forward；固定 129,792-token prompt 在 c2 物理压力与 reset 后三连均返回 token 2354，历史 `2354 → 0 → 0` 未复现，但 GQA、H2D、布局等仍是强相关证据而非单变量因果证明。随后四 DP 短请求并发压测出现 `No available shared memory broadcast block found in 60 seconds`，running 停滞、generation throughput 为 0，尽管 `/health` 为 200；现场与 GPU-only async Mamba 的跨流 event 回边相符，已停止压测客户端并提出默认流修复。Fable5/Cursor session `e2f41d14-9af5-4285-a8ff-f20ea452983b` 接受该方向，并额外调整 packed-GQA 的结构化 gating；相关修复曾报告 57 个测试通过，但尚需结合后续高并发结果审查。
  title: r2精度复现消失后，短压暴露并发死锁
- block_id: b0da6a531b320a750cefcf1f85bc2fc923fe659e3c429d50f685ba28331b6048
  indexed: true
  sha256: 3235a3ca1c3ab74f91de789c9482433b2a437a416f694ddfc8deb377cdee1f0b
  source_range:
    end: 16809914
    part: 4
    start: 0
  summary: Fable5 改动补充了两个泛化边界：无待 packing dense group 时不应因 Mamba layout 差异报错，`page_size_padded` 未实际增大时不应触发 packing；真实依赖环境相关测试报告为 62 passed。r3 四 DP 的 4/16 并发短请求通过且 `None=0`，但 64 并发复现 `running=5/6/5/16`、generation counter 停止和 shared-memory timeout，最终表现为 `RPC call to sample_tokens timed out`，因此默认流修复未被证明充分。随后 eager（ACLGraph disabled）对照在预热正常后也于 c64 停滞，阶段探针未进入，AICore 为 100% 而图模式为 0%，当前仍无法判定是极慢执行、HCCL/通信自旋、`execute_model`/调度入口还是 RPC/共享内存路径；服务已停止且无残留进程。
  title: r3低并发通过但64并发仍死锁，eager对照未决
- block_id: 0af47544c87a123f2a5afe4bcaa50e8969893596f49c535299393b796993f7ed
  indexed: true
  sha256: 7ae2a1434aa037c8182dfdee7874b1f96809351df624e81e8b037d6a2a5fdddf
  source_range:
    end: 31621711
    part: 0
    start: 16809914
  summary: R6在取消探针门槛后，以每DP串行请求再并发压测复现卡死；冷启动c64曾通过，但串行状态复用后冻结并最终触发`sample_tokens` RPC超时。初步证据曾指向DP波次/target-draft相位错位，后续R8又发现同一collective ordinal上存在真实batch与`execute_dummy_batch`语义切换，因此dummy/real调用序列和异步工作完成时序仍属待确认原因。Fable/Cursor完成了从基线到PR的只读审计，但没有修改代码或证明单一根因。
  title: R6定位DP波次错位与dummy/real序列
- block_id: 64187f0c20b2996d0b06b3fdf399d1886416e0b64c3d9e625e58b193fa56ef27
  indexed: true
  sha256: c913d22bd62a74f904bd71c87fc54449979c05fb67727d936cbf9f2d56884b17
  source_range:
    end: 31621711
    part: 1
    start: 16809914
  summary: R9回退`FULL replay`的capture builder后仍在ordinal 105附近卡死，R10引入上游式zeroer并发buffer ring后仍在target forward停滞，R11将GPU-only Mamba align及相关路径恢复为legacy后也在ordinal 107复现。64个worker均停在layer 1的MoE`token_dispatcher._preprocess`/`repeat_interleave`，未见跨layer分叉，提示前一层到MoE之间存在未完成的默认流设备工作，但尚未证明具体代码。随后R12继续回退Mamba公共helper，R13则回退`e32ece64...`的异步metadata H2D并保留成熟padding；R13仍发生serial1→c64停滞，因此这些改动均不是已证实的必要根因。
  title: 排除capture、zeroer与GPU-Mamba单变量回退
- block_id: 82dc3d9d160d847ffa7235aa2921076b1e9423f7cce161291f02c898fb7d0fd7
  indexed: true
  sha256: 1e1245086db01820590eac851cabe0f3e05ca18246d486644f9e9cfa2f7f1ddb
  source_range:
    end: 31621711
    part: 2
    start: 16809914
  summary: R13以`0d72522f6`为基线，移除`e32`及后续拷贝、Mamba和capture相关改动，仅保留成熟GQA padding/packing及必要消费逻辑；启动、编译、图捕获和单请求均正常，但serial1/DP后c64仍出现running数与generation token不再增长。该结果使异步metadata H2D不是卡死必要条件，并把范围进一步收敛到padding KV的运行时消费链；但目前只能说实现可能违反block-ID/layout契约，不能断言公共parent pool+padding方案本身错误。主要待查项包括physical block命名空间、子块/交错ID展开次数、zeroer页步长与释放后H2D生命周期；此前关于二次展开的判断仍是未验证假设。
  title: R13将嫌疑收敛到padding KV消费链
- block_id: e10565b1e0b4a441e54eaa3fdc0eb8c3f842d511c7ec13a6f9b4c95fff952b74
  indexed: true
  sha256: 09ab3ab64895bd9eff9cac3493204b6094dedc7590dcb29964b016a781a79234
  source_range:
    end: 31621711
    part: 3
    start: 16809914
  summary: 交错 KV 已改为全局连续 `[K][V]`，GQA block size 从 384 提升到 1536；四节点启动、graph 捕获和短请求 smoke 均通过，但全 DP c64 仍使服务卡死，随后单请求也无法恢复。现场证据显示请求进入 scheduler 并生成 `new_block_ids_to_zero`，最终卡在 TP worker 的异步 `execute_model → sample_tokens`，因此 API、DP wave 和模型执行前的 input queue 不是当前主嫌。当前判断是改 KV 后新增了首批并发 block 清零路径，而 Ascend zeroer 仍复用单个 nonblocking H2D buffer，可能导致异步 ID 覆盖或 stream 卡住；这一因果链尚未完成验证。
  title: R14连续K/V布局仍在并发zeroer路径卡死
- block_id: e1c7de926ba142da4564f4e8a1b3451e61abf04256c9507727fd16f6652e4f2d
  indexed: true
  sha256: 1837983fa73f527eea974b563763b800ca1c70095828fd0b275b1705084a6796
  source_range:
    end: 31621711
    part: 4
    start: 16809914
  summary: 'R10 的 ring 实验不能直接否定 zeroer race：它使用旧 packed/interleaved 改动，并在 `new_block_ids_to_zero=null` 的后续 decode 阶段卡住，与 R14 首批新 block 清零路径不同。R15 已将 zeroer 改为按 page size 分组、使用静态 kernel，并按 `max_concurrent_batches` 轮转 H2D 源/目标 buffer；远端 CPU 单测 51 passed，四节点源码和启动器哈希一致。vLLM commit `1cd75b3dd`/PR #48085 提供了相同单缓冲 nonblocking H2D 覆盖问题的上游修复依据，但 R15 当时仍在初始化，尚无通过或失败结果；若该 A/B 仍失败，promoted parent→kernel block 的地址映射仍需保留为主嫌。'
  title: R15采用上游并发zeroer模型验证因果链
- block_id: f634a333d91d00ff7616c88af4f268dfb06a0effa0279a98afb1fe8c85574af1
  indexed: true
  sha256: c5c627f2f3681206e46357ef2829c1072ca8afdc08c27f3872ea69c567ed0e57
  source_range:
    end: 48400279
    part: 0
    start: 31621711
  summary: R15 的 zeroer ring、n_blocks=7、多 segment 和连续异步提交均在独立 NPU 实验中通过，因此不能再把 zeroer 视为主因；R15/R16 仍在并发 c64 卡死，最终分别表现为第二 speculative batch 或 `sample_tokens` 超时。R16 修正为完整 `[conv][K][V]`、K/V 各 393216 B 后仍复现，R16/R18 将范围收敛到 mixed prefill/decode 的 target forward、block-table/slot mapping 或异构 block-size 处理；R18 观察到不同 DP 卡在不同 attention/MoE 波前。后续修正方向是撤销错误的 GQA `384→3072` promotion，保持 scheduler/coordinator/block-table 语义为 384，并仅修复物理页规划与连续视图；历史精度和数学语义压测尚未开始。
  title: R15/R16：zeroer 排除后定位到 target forward 与 KV 布局/调度耦合
- block_id: 74c2b26d6903cc2efbefc93ed2c63e8a178388e61b009f1f6f9bc6e565e41ef5
  indexed: true
  sha256: a264645fa3575602a0a5fe0fedd49c9e4efefbac7230ed50224748d3b6e66a75
  source_range:
    end: 48400279
    part: 1
    start: 31621711
  summary: '`--enforce-eager` 已复现同样的 `sample_tokens` 超时，说明 ACLGraph capture/replay 和 Torch compile 不是必要条件；R18 的同步探针显示 attention 前同步完成，但 DP0/1 进入 layer1 MoE、DP2/3 卡在 layer0 attention，符合跨 DP collective 顺序错位。另确认 DSpark 路径未初始化 zeroer，故当前卡死不能归因于 zero kernel；更强的解释是把物理连续性错误编码为 scheduler 可见的 3072 block，导致短请求分配膨胀并改变执行波前。用户指出 scheduler block 定义不应由本 PR 修改后，方案改为保持 spec、hash、coordinator、BlockPool 和 block table 为 384，仅沿既有多-spec 对齐链路扩展公共物理页并修正 worker reshape。'
  title: R17/R18：诊断探针确认 collective 波前错位，回归方案收紧为 384 block
- block_id: 518f2fd3a2fa9309bd5e7b3405a8b5aff7ce2829253c9b90294c86369e962594
  indexed: true
  sha256: 4a9a4f76d1987c23f482b86f2ceb76d0b78ded5b0bac304bb4f288308d122e5c
  source_range:
    end: 48400279
    part: 2
    start: 31621711
  summary: 已回退 Mamba metadata hook，删除 R18 探针；针对性检查包括规划 7 项、字节级布局 1 项和 zeroer 3 项均通过，但完整 UT 被中断，R19 尚未同步四节点，也未看到真实启动日志或重跑 c64，因此不能宣告修复。补充分析确认 K3 共有 24 个 MLA 层、69 个 KDA/Mamba 层和 5 个 draft GQA 层，按最小 layer 数 5 分为 5 个 MLA group、1 个 GQA group、14 个 KDA group；TP16、384 tokens、1 个本地 KV head、64 dim、BF16 计算出单层 GQA K 为 `384×1×64×2=49152 B`，V 同值。后续需完成 384/586752 运行时核对、c64、历史精度 case，以及覆盖 prefix cache 开关、长短序列、数学题和采信率/乱码/`None` 检查的语义压测；此前把公共页简单画成追加 `[GQA K][GQA V]` 的说法已被标记为可能不正确，正确地址映射仍需基于现有公共 backing 的 lane 复用规则进一步核实。
  title: 当前 R19 方案与旧 KV group/物理布局、验证边界
- block_id: 703ff10823a4771a1bf95ba579e8796b591c618c0549f58036dc86800d3114ec
  indexed: true
  sha256: 6ffafbb99e8a1a676c8a1c052ab20e0c781db352302ee22a11ca55e0fef6e1f7
  source_range:
    end: 48400279
    part: 3
    start: 31621711
  summary: 材料先提出在 MLA-K/Mamba-SSM lane 内复用 GQA K/V，随后根据用户纠正确认该方案违反原有三条 SoA lane 契约：GQA K 应对齐 T2，V 应对齐 T3，不能交错塞入第二个 tensor，也不能把 scheduler block_size 从 384 改为 1536。当前拟采用 `[conv][K/SSM][V]` 三 lane、页大小 832,512 B、GQA 每个公共页 8 个 384-token 子槽，并让整页句柄与子槽句柄互斥；材料声称代码路径和 ownership 已开始修改，但单测、四节点服务启动、压力测试及精度验证仍未完成，不能视为实现成功。
  title: 三 lane GQA K/V 对齐方案已落地，验证尚未完成
- block_id: 9ca628e6e150a13249ef3dc398a298e3b6c20418c61eb5f3eed961c2d98abce6
  indexed: true
  sha256: 92d893d402fad79e3d0cf55ce38448c78e889e284e870621ba6bf73213bf80a4
  source_range:
    end: 68349459
    part: 0
    start: 48400279
  summary: 四节点候选启动和 API 短请求曾通过，但 64 并发在 `execute_seq=41` 重现卡死，随后触发 `sample_tokens` 超时与 EngineCore/API 链路断开；失败批次的 allocator 映射未见明显踩踏，定位重点转向 zero kernel。单卡真实 NPU 微测确认新 zero kernel 使用 Triton `continue` 会触发 `UnsupportedLanguageConstruct`，改为静态分组后该微测 64 轮完成且目标区残留为 0，但完整服务回归尚未报告。后续材料提出以白板定义的 4-page×3-lane、TP8/TP16 兼容布局作为实现依据，并强调 page、连续地址、slab 容量及 block-id 映射仍需逐项核对；白板是否足以支撑 allocator、reshape、zero/copy、CoW、prefix-cache、swap 和 block table 的完整实现尚未得出结论。
  title: KV 布局卡死定位与 TP8/TP16 白板核对
- block_id: a57e20c04bbd3d1a8b126f2cb224c7743891023cc159ec28568e020e3ce9d6cb
  indexed: true
  sha256: 58225788ba81d5f95442b9e8275299fce480d439b98d43f6c5ae74a575708948
  source_range:
    end: 85138557
    part: 0
    start: 68349459
  summary: 材料将白板布局解释为四个连续 page plane：P0 为 Mamba conv，P1 为 Mamba recurrent/MLA K-nope，P2 为 MLA K-rope/GQA K，P3 为 GQA V；TP16 页面字节数为 46,080/393,216/49,152/49,152，TP8 全部翻倍，scheduler block size 为 384。后续更正明确应使用 group_size=24、T0…T23，并由 scheduler 管理 MLA 24 层、Mamba 24+24+21 层、GQA 5 层；GQA 的 P2/P3 仅物化 5 个实际 lane，其余为 PAD。材料称方案已可实现，并计划删除 group_size=5、GQA 独立页、共享大页、parent/subslot、首轴非连续 view 及特殊 scheduler 分支，但这些仍属于计划或设计判断，不是已验证成功；另提到将使用一份被标记为 [REDACTED_SECRET] 的单机剪裁权重测试启动、短请求和并发卡死，尚无测试结果。
  title: 白板布局契约与 group_size=24 方案
- block_id: 3f877dcad781945b81e5ba71a72ca767dcd7cf12192b9c09db6b32bb3b93985d
  indexed: true
  sha256: 42b5ab27f8998ae6d45a0287624c180d57de7a0e83aedd53547708ed12797e5f
  source_range:
    end: 101928549
    part: 0
    start: 85138557
  summary: 在统一版本的四机环境中，四平面 SoA、24 个公共 parent 与 5 个 GQA-V P3 可完成启动、KV 规划、图捕获及低并发请求；DP0–DP3 短请求语义冒烟和 c16 曾通过。首次 c32 压力下四个 EngineCore 最终因 `sample_tokens` RPC 超时并退出，服务端连接被重置，128K 采信率测试因此未启动。后续发现真实运行时曾错误构造 20 组而非预期 5 组，随后修正 grouping；精确匹配的历史 SGLang 128K 结果为 c1 81.95%、c2 80.28%，但当前 vLLM 尚无有效可比结果。
  title: 四平面布局启动通过但 c32 卡死
- block_id: 0630eed1dd2f95772ece8239dfbc6fe8c4b86ecfbf8b0ad271bb39596cb00495
  indexed: true
  sha256: a0fa2dbbbc59ddac49e524af1bccf93dae6e2939dbb5e2e5c3fcd1509b86df82
  source_range:
    end: 101928549
    part: 1
    start: 85138557
  summary: 将真实分组修正为 `24/24/24/21/5` 后，r3 仍在 c32 出现 running 数停滞、吞吐归零和 shared-memory broadcast 警告，四个 DP 最终因 `sample_tokens` RPC 超时自退出。曾怀疑 block-major partial-prefix CoW 与 plane-major 布局不兼容并修复为逐 plane copy，但 r4 的 1-token、无 prefix hit 工作负载不会执行 CoW，却复现相同卡点，因此该修复不是根因。分析随后排除了 DSpark 路径上的 Eagle3 zero metadata 初始化和 CoW，未验证的嫌疑收敛到 5-group/29 tensor 的契约、view/reshape 地址、block-table 及 forward 路径。
  title: 5-group 与 CoW 修复仍未消除 c32 停滞
- block_id: b8292356fcc6bc33906ee875bf3b6ce7c074f8f5e228b5bb55065fe11adf80b2
  indexed: true
  sha256: 93b7a195292ccfd665fabb7eb0408a2c524983d52072863cf1edc62830ca0535
  source_range:
    end: 101928549
    part: 2
    start: 85138557
  summary: 材料确认正确基线应是库上 v0.26.0 的 20 个逻辑 group、5 块共享 raw Tensor、原有 BlockPool/申请释放/block table 生命周期；29 个独立 Tensor、重分组及 zeroer/copy-ring 改动均应删除。按此最小方案仅将统一 page 从 `488448` 扩至 `537600`、新增 P3 并调整 view 后，启动和短请求可通过，但相同 1-token c32 仍停滞，说明 29-Tensor 拓扑不是充分根因。zeroer 虽仍是潜在生命周期风险，但已被多轮排除，当前应通过可撤销诊断探针正向记录各 TP rank 的 worker、NPU/collective、异步 D2H 与返回队列阶段，不能再把假设或计划当作验证成功。
  title: 回归 0.26 原有拓扑后仍卡死
- block_id: 8e4fa72fe9b13618fcac896685150d6365c541d41465f4697371c02fb0fd59c4
  indexed: true
  sha256: 1192b8b42c4afe7eb61bff4473773594480ce282b1fea43a7a7fd2d20b2de41a
  source_range:
    end: 110260348
    part: 0
    start: 101928549
  summary: R6 诊断服务已在四个 DP、64 个 TP worker 上完成初始化并返回 health=200；首批无 prefix-hit 的 c32 请求中，第一轮设备事件完整完成，第二轮所有 TP rank 按相同顺序停在第 0 个 KDA 层的首条 TP `dist.all_gather_into_tensor`，设备流自该层起未完成，尚不能据此断言 KDA 本身是根因。此前后台 NPU event 探针可能扰动 HCCL/stream 时序，已计划移除，仅保留 P3 的 5 个生产文件并改用主执行链 event 与 host-only 元数据；白板轴向也得到修正：三类 page 使用四条连续 lane，GQA page 为 `L2 + L3`，真正与 target 共享的是 L2（MLA K-rope/GQA-K），L3 仅为 GQA-V，因此后续应核对 L2 的 shape/stride/storage_offset/data_ptr、block ownership 和跨 step 异步复用，而非笼统给“P3”加 barrier。
  title: R6 首批 c32 将卡点收敛至跨 step 的 L2 复用与 TP all-gather 等待
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ea7876a55b7e5eb30b5d33a44b89ac1216c10bc4f3db2ad45b3ff3255bebd020
  kind: experience
  material_digest: 23f4be52dc03182a07675fc922833b5cb3fc4c4c619b4d78738fa3836180e388
  revision: 3e7a5a532a07dcc1646675f4f8f13540bbda82c994468a28a4cf178ac88cd53d
  schema: mindie-entry/3
  summary: 可信基线仍是库上 v0.26.0 的 20-group、5 块共享 raw Tensor、原有 BlockPool/申请释放/block table 生命周期；此前 29 个独立 Tensor、重分组、CoW 修复及 zeroer 改动均未验证。最小扩页并新增 P3 的实现可启动并完成图捕获，但无 prefix-hit、无 CoW 的 1-token c32 仍在第二调度 step 的第 0 个 KDA 层首条 TP `all_gather` 处全体等待，尚未证明根因是 KDA、ACLGraph、forward/collective 契约或共享 L2 生命周期；zeroer 已反复排除为主变量。当前布局理解已更正为四条连续 lane、三类逻辑 page：GQA page 是 `L2 + L3`，其中 L2 与 target 的 MLA K-rope 重叠，L3 只有 GQA-V；应保留失败与探针扰动现场，对齐两轮 scheduler/request/copy 元数据，并以主执行流上的 L2 生产者完成事件到下一轮消费者的 handoff 实验验证跨 step 复用因果。
  title: 当前任务导航：以 v0.26 原有拓扑基线定位 L2 跨 step 复用导致的 TP 等待
navigation: 可信基线仍是库上 v0.26.0 的 20-group、5 块共享 raw Tensor、原有 BlockPool/申请释放/block table 生命周期；此前 29 个独立 Tensor、重分组、CoW 修复及 zeroer 改动均未验证。最小扩页并新增 P3 的实现可启动并完成图捕获，但无 prefix-hit、无 CoW 的 1-token c32 仍在第二调度 step 的第 0 个 KDA 层首条 TP `all_gather` 处全体等待，尚未证明根因是 KDA、ACLGraph、forward/collective 契约或共享 L2 生命周期；zeroer 已反复排除为主变量。当前布局理解已更正为四条连续 lane、三类逻辑 page：GQA page 是 `L2 + L3`，其中 L2 与 target 的 MLA K-rope 重叠，L3 只有 GQA-V；应保留失败与探针扰动现场，对齐两轮 scheduler/request/copy 元数据，并以主执行流上的 L2 生产者完成事件到下一轮消费者的 handoff 实验验证跨 step 复用因果。
schema: mindie-material-task/1
status: complete
task_id: ea7876a55b7e5eb30b5d33a44b89ac1216c10bc4f3db2ad45b3ff3255bebd020
---

# 当前任务导航：以 v0.26 原有拓扑基线定位 L2 跨 step 复用导致的 TP 等待

可信基线仍是库上 v0.26.0 的 20-group、5 块共享 raw Tensor、原有 BlockPool/申请释放/block table 生命周期；此前 29 个独立 Tensor、重分组、CoW 修复及 zeroer 改动均未验证。最小扩页并新增 P3 的实现可启动并完成图捕获，但无 prefix-hit、无 CoW 的 1-token c32 仍在第二调度 step 的第 0 个 KDA 层首条 TP `all_gather` 处全体等待，尚未证明根因是 KDA、ACLGraph、forward/collective 契约或共享 L2 生命周期；zeroer 已反复排除为主变量。当前布局理解已更正为四条连续 lane、三类逻辑 page：GQA page 是 `L2 + L3`，其中 L2 与 target 的 MLA K-rope 重叠，L3 只有 GQA-V；应保留失败与探针扰动现场，对齐两轮 scheduler/request/copy 元数据，并以主执行流上的 L2 生产者完成事件到下一轮消费者的 handoff 实验验证跨 step 复用因果。

## Materials

- [dspark+prefix cache 精度问题定位与系统化方案](blocks/0e6ec148c857ae4057bb4b8c0447934672bf95d69ad3d812046567da110c12aa.md): 定位显示，原始问题中已直接捕获 draft layer 94/block 1969 与 target layer 23/block 5224 的物理 backing collision，随后 target KV 首次出现 NaN；这比后续修复提交更像根因。当前 KDA/GDN 不能使用首轴非连续 view，若从历史修复中选择最大嫌疑，则是可能不完整回迁 GDN/FULL graph replay metadata 的 `af51098fa`/`637d49d04`，但尚未完成因果验证。上游对 stale block-table、多个 full-attention group 裁剪、Mamba prefix 边界和 zeroer 生命周期已有相关修复，建议分别 A/B 验证，避免把假设当成成功。
- [4:1 GQA padding、生命周期契约与验证顺序](blocks/2a786ab3aa84b1d152c252a32cedd2f2cb43928f48ee32bcd1afc9dd121275ec.md): 方案将 GQA 从独立物理页改为沿用已验证的 MLA+Mamba 公共布局，通过 4:1 parent-page/subslot packing 保持首轴连续；1M 上下文新增浪费估算约为 0.30334 GiB/TP rank，原有 padding 合计约 0.88935 GiB/TP rank。仍需修复 FULL replay stale row、多 full-attention group 共同命中边界、Mamba committed boundary admission，以及 non-uniform zeroer/H2D buffer 生命周期；419c315 只作为历史对照，不能宣称已解决精度问题。当前 exact writer/victim 验证尚未完成，建议先以 `6fbbcf215`、`a5836312d`、`8472b0161` 和 capability-based layout 分别做受控 A/B。
- [放弃独立页并优先修异步调度与精度闭环](blocks/1cee7e74db6378eed6cd7078418545f5b820dae389daef22e3789d7a74b9b62b.md): 独立 GQA 页和 runtime fallback 被明确废弃；当前唯一合法路径是从实际 rank-local page bytes 推导 packing factor，TP16/8/4/2 当前均为 4，但不把 4 固化为生产常量。H2D 初步根因是 side-stream 状态更新完成后缺少到下一轮默认流/H2D 缓冲复用的 GPU event 依赖，修复应采用 GPU event 链而非 CPU synchronize；随后逐项落地 1/2/3/5 并保持可二分。候选测试曾达到 59/59 通过并完成四节点初始化和图捕获，但首个 smoke 暴露 GPU-only align 仍无条件调用旧 CPU `do_mamba_copy_block` 的变量未定义错误；该公共调用随后被移除，仅保留真正的 legacy 分支，r2 重启验证尚未给出精度结论。
- [r2精度复现消失后，短压暴露并发死锁](blocks/f1fbfb80ecfc4ab165d252c1b9889084f276bc21678918f2a228c200fc0d1431.md): r2 完成权重、KV 规划、capture 和首个 forward；固定 129,792-token prompt 在 c2 物理压力与 reset 后三连均返回 token 2354，历史 `2354 → 0 → 0` 未复现，但 GQA、H2D、布局等仍是强相关证据而非单变量因果证明。随后四 DP 短请求并发压测出现 `No available shared memory broadcast block found in 60 seconds`，running 停滞、generation throughput 为 0，尽管 `/health` 为 200；现场与 GPU-only async Mamba 的跨流 event 回边相符，已停止压测客户端并提出默认流修复。Fable5/Cursor session `e2f41d14-9af5-4285-a8ff-f20ea452983b` 接受该方向，并额外调整 packed-GQA 的结构化 gating；相关修复曾报告 57 个测试通过，但尚需结合后续高并发结果审查。
- [r3低并发通过但64并发仍死锁，eager对照未决](blocks/b0da6a531b320a750cefcf1f85bc2fc923fe659e3c429d50f685ba28331b6048.md): Fable5 改动补充了两个泛化边界：无待 packing dense group 时不应因 Mamba layout 差异报错，`page_size_padded` 未实际增大时不应触发 packing；真实依赖环境相关测试报告为 62 passed。r3 四 DP 的 4/16 并发短请求通过且 `None=0`，但 64 并发复现 `running=5/6/5/16`、generation counter 停止和 shared-memory timeout，最终表现为 `RPC call to sample_tokens timed out`，因此默认流修复未被证明充分。随后 eager（ACLGraph disabled）对照在预热正常后也于 c64 停滞，阶段探针未进入，AICore 为 100% 而图模式为 0%，当前仍无法判定是极慢执行、HCCL/通信自旋、`execute_model`/调度入口还是 RPC/共享内存路径；服务已停止且无残留进程。
- [R6定位DP波次错位与dummy/real序列](blocks/0af47544c87a123f2a5afe4bcaa50e8969893596f49c535299393b796993f7ed.md): R6在取消探针门槛后，以每DP串行请求再并发压测复现卡死；冷启动c64曾通过，但串行状态复用后冻结并最终触发`sample_tokens` RPC超时。初步证据曾指向DP波次/target-draft相位错位，后续R8又发现同一collective ordinal上存在真实batch与`execute_dummy_batch`语义切换，因此dummy/real调用序列和异步工作完成时序仍属待确认原因。Fable/Cursor完成了从基线到PR的只读审计，但没有修改代码或证明单一根因。
- [排除capture、zeroer与GPU-Mamba单变量回退](blocks/64187f0c20b2996d0b06b3fdf399d1886416e0b64c3d9e625e58b193fa56ef27.md): R9回退`FULL replay`的capture builder后仍在ordinal 105附近卡死，R10引入上游式zeroer并发buffer ring后仍在target forward停滞，R11将GPU-only Mamba align及相关路径恢复为legacy后也在ordinal 107复现。64个worker均停在layer 1的MoE`token_dispatcher._preprocess`/`repeat_interleave`，未见跨layer分叉，提示前一层到MoE之间存在未完成的默认流设备工作，但尚未证明具体代码。随后R12继续回退Mamba公共helper，R13则回退`e32ece64...`的异步metadata H2D并保留成熟padding；R13仍发生serial1→c64停滞，因此这些改动均不是已证实的必要根因。
- [R13将嫌疑收敛到padding KV消费链](blocks/82dc3d9d160d847ffa7235aa2921076b1e9423f7cce161291f02c898fb7d0fd7.md): R13以`0d72522f6`为基线，移除`e32`及后续拷贝、Mamba和capture相关改动，仅保留成熟GQA padding/packing及必要消费逻辑；启动、编译、图捕获和单请求均正常，但serial1/DP后c64仍出现running数与generation token不再增长。该结果使异步metadata H2D不是卡死必要条件，并把范围进一步收敛到padding KV的运行时消费链；但目前只能说实现可能违反block-ID/layout契约，不能断言公共parent pool+padding方案本身错误。主要待查项包括physical block命名空间、子块/交错ID展开次数、zeroer页步长与释放后H2D生命周期；此前关于二次展开的判断仍是未验证假设。
- [R14连续K/V布局仍在并发zeroer路径卡死](blocks/e10565b1e0b4a441e54eaa3fdc0eb8c3f842d511c7ec13a6f9b4c95fff952b74.md): 交错 KV 已改为全局连续 `[K][V]`，GQA block size 从 384 提升到 1536；四节点启动、graph 捕获和短请求 smoke 均通过，但全 DP c64 仍使服务卡死，随后单请求也无法恢复。现场证据显示请求进入 scheduler 并生成 `new_block_ids_to_zero`，最终卡在 TP worker 的异步 `execute_model → sample_tokens`，因此 API、DP wave 和模型执行前的 input queue 不是当前主嫌。当前判断是改 KV 后新增了首批并发 block 清零路径，而 Ascend zeroer 仍复用单个 nonblocking H2D buffer，可能导致异步 ID 覆盖或 stream 卡住；这一因果链尚未完成验证。
- [R15采用上游并发zeroer模型验证因果链](blocks/e1c7de926ba142da4564f4e8a1b3451e61abf04256c9507727fd16f6652e4f2d.md): R10 的 ring 实验不能直接否定 zeroer race：它使用旧 packed/interleaved 改动，并在 `new_block_ids_to_zero=null` 的后续 decode 阶段卡住，与 R14 首批新 block 清零路径不同。R15 已将 zeroer 改为按 page size 分组、使用静态 kernel，并按 `max_concurrent_batches` 轮转 H2D 源/目标 buffer；远端 CPU 单测 51 passed，四节点源码和启动器哈希一致。vLLM commit `1cd75b3dd`/PR #48085 提供了相同单缓冲 nonblocking H2D 覆盖问题的上游修复依据，但 R15 当时仍在初始化，尚无通过或失败结果；若该 A/B 仍失败，promoted parent→kernel block 的地址映射仍需保留为主嫌。
- [R15/R16：zeroer 排除后定位到 target forward 与 KV 布局/调度耦合](blocks/f634a333d91d00ff7616c88af4f268dfb06a0effa0279a98afb1fe8c85574af1.md): R15 的 zeroer ring、n_blocks=7、多 segment 和连续异步提交均在独立 NPU 实验中通过，因此不能再把 zeroer 视为主因；R15/R16 仍在并发 c64 卡死，最终分别表现为第二 speculative batch 或 `sample_tokens` 超时。R16 修正为完整 `[conv][K][V]`、K/V 各 393216 B 后仍复现，R16/R18 将范围收敛到 mixed prefill/decode 的 target forward、block-table/slot mapping 或异构 block-size 处理；R18 观察到不同 DP 卡在不同 attention/MoE 波前。后续修正方向是撤销错误的 GQA `384→3072` promotion，保持 scheduler/coordinator/block-table 语义为 384，并仅修复物理页规划与连续视图；历史精度和数学语义压测尚未开始。
- [R17/R18：诊断探针确认 collective 波前错位，回归方案收紧为 384 block](blocks/74c2b26d6903cc2efbefc93ed2c63e8a178388e61b009f1f6f9bc6e565e41ef5.md): `--enforce-eager` 已复现同样的 `sample_tokens` 超时，说明 ACLGraph capture/replay 和 Torch compile 不是必要条件；R18 的同步探针显示 attention 前同步完成，但 DP0/1 进入 layer1 MoE、DP2/3 卡在 layer0 attention，符合跨 DP collective 顺序错位。另确认 DSpark 路径未初始化 zeroer，故当前卡死不能归因于 zero kernel；更强的解释是把物理连续性错误编码为 scheduler 可见的 3072 block，导致短请求分配膨胀并改变执行波前。用户指出 scheduler block 定义不应由本 PR 修改后，方案改为保持 spec、hash、coordinator、BlockPool 和 block table 为 384，仅沿既有多-spec 对齐链路扩展公共物理页并修正 worker reshape。
- [当前 R19 方案与旧 KV group/物理布局、验证边界](blocks/518f2fd3a2fa9309bd5e7b3405a8b5aff7ce2829253c9b90294c86369e962594.md): 已回退 Mamba metadata hook，删除 R18 探针；针对性检查包括规划 7 项、字节级布局 1 项和 zeroer 3 项均通过，但完整 UT 被中断，R19 尚未同步四节点，也未看到真实启动日志或重跑 c64，因此不能宣告修复。补充分析确认 K3 共有 24 个 MLA 层、69 个 KDA/Mamba 层和 5 个 draft GQA 层，按最小 layer 数 5 分为 5 个 MLA group、1 个 GQA group、14 个 KDA group；TP16、384 tokens、1 个本地 KV head、64 dim、BF16 计算出单层 GQA K 为 `384×1×64×2=49152 B`，V 同值。后续需完成 384/586752 运行时核对、c64、历史精度 case，以及覆盖 prefix cache 开关、长短序列、数学题和采信率/乱码/`None` 检查的语义压测；此前把公共页简单画成追加 `[GQA K][GQA V]` 的说法已被标记为可能不正确，正确地址映射仍需基于现有公共 backing 的 lane 复用规则进一步核实。
- [三 lane GQA K/V 对齐方案已落地，验证尚未完成](blocks/703ff10823a4771a1bf95ba579e8796b591c618c0549f58036dc86800d3114ec.md): 材料先提出在 MLA-K/Mamba-SSM lane 内复用 GQA K/V，随后根据用户纠正确认该方案违反原有三条 SoA lane 契约：GQA K 应对齐 T2，V 应对齐 T3，不能交错塞入第二个 tensor，也不能把 scheduler block_size 从 384 改为 1536。当前拟采用 `[conv][K/SSM][V]` 三 lane、页大小 832,512 B、GQA 每个公共页 8 个 384-token 子槽，并让整页句柄与子槽句柄互斥；材料声称代码路径和 ownership 已开始修改，但单测、四节点服务启动、压力测试及精度验证仍未完成，不能视为实现成功。
- [KV 布局卡死定位与 TP8/TP16 白板核对](blocks/9ca628e6e150a13249ef3dc398a298e3b6c20418c61eb5f3eed961c2d98abce6.md): 四节点候选启动和 API 短请求曾通过，但 64 并发在 `execute_seq=41` 重现卡死，随后触发 `sample_tokens` 超时与 EngineCore/API 链路断开；失败批次的 allocator 映射未见明显踩踏，定位重点转向 zero kernel。单卡真实 NPU 微测确认新 zero kernel 使用 Triton `continue` 会触发 `UnsupportedLanguageConstruct`，改为静态分组后该微测 64 轮完成且目标区残留为 0，但完整服务回归尚未报告。后续材料提出以白板定义的 4-page×3-lane、TP8/TP16 兼容布局作为实现依据，并强调 page、连续地址、slab 容量及 block-id 映射仍需逐项核对；白板是否足以支撑 allocator、reshape、zero/copy、CoW、prefix-cache、swap 和 block table 的完整实现尚未得出结论。
- [白板布局契约与 group_size=24 方案](blocks/a57e20c04bbd3d1a8b126f2cb224c7743891023cc159ec28568e020e3ce9d6cb.md): 材料将白板布局解释为四个连续 page plane：P0 为 Mamba conv，P1 为 Mamba recurrent/MLA K-nope，P2 为 MLA K-rope/GQA K，P3 为 GQA V；TP16 页面字节数为 46,080/393,216/49,152/49,152，TP8 全部翻倍，scheduler block size 为 384。后续更正明确应使用 group_size=24、T0…T23，并由 scheduler 管理 MLA 24 层、Mamba 24+24+21 层、GQA 5 层；GQA 的 P2/P3 仅物化 5 个实际 lane，其余为 PAD。材料称方案已可实现，并计划删除 group_size=5、GQA 独立页、共享大页、parent/subslot、首轴非连续 view 及特殊 scheduler 分支，但这些仍属于计划或设计判断，不是已验证成功；另提到将使用一份被标记为 [REDACTED_SECRET] 的单机剪裁权重测试启动、短请求和并发卡死，尚无测试结果。
- [四平面布局启动通过但 c32 卡死](blocks/3f877dcad781945b81e5ba71a72ca767dcd7cf12192b9c09db6b32bb3b93985d.md): 在统一版本的四机环境中，四平面 SoA、24 个公共 parent 与 5 个 GQA-V P3 可完成启动、KV 规划、图捕获及低并发请求；DP0–DP3 短请求语义冒烟和 c16 曾通过。首次 c32 压力下四个 EngineCore 最终因 `sample_tokens` RPC 超时并退出，服务端连接被重置，128K 采信率测试因此未启动。后续发现真实运行时曾错误构造 20 组而非预期 5 组，随后修正 grouping；精确匹配的历史 SGLang 128K 结果为 c1 81.95%、c2 80.28%，但当前 vLLM 尚无有效可比结果。
- [5-group 与 CoW 修复仍未消除 c32 停滞](blocks/0630eed1dd2f95772ece8239dfbc6fe8c4b86ecfbf8b0ad271bb39596cb00495.md): 将真实分组修正为 `24/24/24/21/5` 后，r3 仍在 c32 出现 running 数停滞、吞吐归零和 shared-memory broadcast 警告，四个 DP 最终因 `sample_tokens` RPC 超时自退出。曾怀疑 block-major partial-prefix CoW 与 plane-major 布局不兼容并修复为逐 plane copy，但 r4 的 1-token、无 prefix hit 工作负载不会执行 CoW，却复现相同卡点，因此该修复不是根因。分析随后排除了 DSpark 路径上的 Eagle3 zero metadata 初始化和 CoW，未验证的嫌疑收敛到 5-group/29 tensor 的契约、view/reshape 地址、block-table 及 forward 路径。
- [回归 0.26 原有拓扑后仍卡死](blocks/b8292356fcc6bc33906ee875bf3b6ce7c074f8f5e228b5bb55065fe11adf80b2.md): 材料确认正确基线应是库上 v0.26.0 的 20 个逻辑 group、5 块共享 raw Tensor、原有 BlockPool/申请释放/block table 生命周期；29 个独立 Tensor、重分组及 zeroer/copy-ring 改动均应删除。按此最小方案仅将统一 page 从 `488448` 扩至 `537600`、新增 P3 并调整 view 后，启动和短请求可通过，但相同 1-token c32 仍停滞，说明 29-Tensor 拓扑不是充分根因。zeroer 虽仍是潜在生命周期风险，但已被多轮排除，当前应通过可撤销诊断探针正向记录各 TP rank 的 worker、NPU/collective、异步 D2H 与返回队列阶段，不能再把假设或计划当作验证成功。
- [R6 首批 c32 将卡点收敛至跨 step 的 L2 复用与 TP all-gather 等待](blocks/8e4fa72fe9b13618fcac896685150d6365c541d41465f4697371c02fb0fd59c4.md): R6 诊断服务已在四个 DP、64 个 TP worker 上完成初始化并返回 health=200；首批无 prefix-hit 的 c32 请求中，第一轮设备事件完整完成，第二轮所有 TP rank 按相同顺序停在第 0 个 KDA 层的首条 TP `dist.all_gather_into_tensor`，设备流自该层起未完成，尚不能据此断言 KDA 本身是根因。此前后台 NPU event 探针可能扰动 HCCL/stream 时序，已计划移除，仅保留 P3 的 5 个生产文件并改用主执行链 event 与 host-only 元数据；白板轴向也得到修正：三类 page 使用四条连续 lane，GQA page 为 `L2 + L3`，真正与 target 共享的是 L2（MLA K-rope/GQA-K），L3 仅为 GQA-V，因此后续应核对 L2 的 shape/stride/storage_offset/data_ptr、block ownership 和跨 step 异步复用，而非笼统给“P3”加 barrier。

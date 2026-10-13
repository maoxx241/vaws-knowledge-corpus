---
blocks:
- block_id: fb4ae558d9b910a1dadf6107d976f154826d65124f248b5a1f94ac596db2e3b2
  indexed: true
  sha256: 5b5a12547745d40d28410222bfa5e8eca1c3f16071731802d836ceafd7fe72b1
  source_range:
    end: 10507692
    part: 0
    start: 0
  summary: '在纯 0.26.0rc、无 DSpark 的最小复现中，只有 385-token（384+1）失败：state copy 138/138 一致，但 decode graph 后 [1,7168] hidden 全为 NaN，输出连续 token 0；386-token 正常。证据将根因定位为 GDN metadata 与 decode graph 的 stateful 单 token prefill 分类契约缺失，主仓 #11161 后未带入私仓 #12255；恢复分类后 385 两次均精确恢复，且 64-token 延续正常。对 128K/1K DSpark 场景，完整 #12255 修复了乱码、NaN 和假 100% 采信，但连续采信率仍从 60.27% 降至 39.50%，说明还有状态复用或调度问题未解决；对 #12027 的直接归因也未被 trace 证实。'
  title: '0.26.0rc 385-token NaN 根因定位与 #12255 修复'
- block_id: 6ebc5eb6af6788a2df2b819fd6af9e05f939f58d9c5e136f0a89a05ae512efdc
  indexed: true
  sha256: 9e5ed5485fff0ba5a70c8971e2f8348705240376ae210e7c7bb485e3c0f0b165
  source_range:
    end: 10507692
    part: 1
    start: 0
  summary: '完整 #12255 修复后，128K/1K c1 从坏基线的“100% 采信但全 !”恢复为 76.49% 正常输出；c2 首轮为 60.27%，第二轮降至 39.50%，且输出 hash 变化。加入 #12027 最终保留的 builder 语义后，三轮为 45.60%、45.70%、46.07%，但四个 DP 的相关 trace 均为 0、PCP=1，不能据此宣称 #12027 是直接修复；当前更像 prefix-cache 后 recurrent state 或 slot/block ownership 漂移。'
  title: '128K/1K 采信率与 PR #12027 的未决关系'
- block_id: 46f3b08bdfbf8856343da810990d64cca2c4a89e16179568a727ef6c1019b9eb
  indexed: true
  sha256: 255affe957af8beeff9b5ddcc191e687c3e195b2a6d01dd7dafda45336d55de1
  source_range:
    end: 10507692
    part: 2
    start: 0
  summary: '在 PR #13277 上完成 rebase、CI 修复，并移植 #12255 与 #12027 的最终有效语义；最终远端 SHA 为 af51098fa，静态检查、format、compileall 和 native exact-SHA build 通过，focused pytest 仍受本机缺少 vllm_ascend._build_info 阻塞，gitleaks 也未运行。四节点 980 个 native 文件和 import manifest 已校验一致，旧 r26 已切换到 port 18117 的新服务，64-rank 初始化、345/345 权重加载和 GQA draft 加载均推进正常。当前仍处于 exact torch-compile 与 FULL_DECODE graph capture 阶段，尚未完成 health、四 rank 错误审计或 128K×2×1K DSpark 压测，因此“库上有可用且无问题的 PR”尚未被验证。'
  title: '#13277 集成修复与 DSpark 四节点压测进行中'
- block_id: 53f71d6cca3e2ea4e9ed910ffbec9424f9cfc217e0b81b411ba8162c023c08aa
  indexed: true
  sha256: 741477499688030d8ec64160c317c2ac6b50b47b4838626de5dbbd13eae2f7a8
  source_range:
    end: 10507692
    part: 3
    start: 0
  summary: '四 rank 服务完成 graph capture、health=200、fatal=0；补入 #13407 后，8K×1K 在同权重同 prompt 下采信率从约 0.3% 恢复至 75.9259%，输出哈希与历史通过版本一致，确认该回归源于遗漏 #13407。128K 首次 full prefill 仍正常，但第二次 cache-hit 继续退化为重复 `!`，说明 KDA/Mamba cache lifecycle 仍有未解决的状态问题；此前的 KDA tail clear 未覆盖真实命中路径。后续又发现历史通过组合还包含 #13327（`ec56b6b2`）的 warmup、rejection sampler `do_not_specialize` 和 worker warmup 改动；old-native A/B 与补入 #13327 的严格反事实验证仍未完成，当前 PR 不可宣称可用。'
  title: '#13277：#13407 修复 8K 采信率，但 128K cache-hit 仍失败'
- block_id: 7549a6fbb53e3b54eef01d562da0a7b5352eb9a2f25b41db9b3204630a87450c
  indexed: true
  sha256: 858ca5695c83b83099fdc40bed445fe70a9641bccfdc4a983c44751d5b8562ab
  source_range:
    end: 18232549
    part: 0
    start: 10507692
  summary: 旧 native 已排除：8K 结果一致，但 128K 第一次正常、第二次高命中稳定输出 token id 0（`!`），与当前 native 相同。#13327 的 warmup 已在四个 DP rank 上真实执行且失败数为 0，但 8K/1K 通过后 128K 仍失败，因此该候选未推送。干净服务对照显示问题依赖先执行 8K/1K；`spec_decode_metadata=None` 是普通 sampler 的正常分支标志，首个已知异常是 sampler 收到的 raw target logits 全 NaN。
  title: 从 token 0 追查到 sampler 前的 NaN
- block_id: 78c64958b6baea2622c182ce87b396a7440162d7955499bfa88ef7add565feae
  indexed: true
  sha256: 13b1a99b4812bb263cae561838a6ee75228434a513d304bd3c795b5ad7854663
  source_range:
    end: 18232549
    part: 1
    start: 10507692
  summary: '探针确认第二次 128K 的 raw logits 为 `(1,163840)` 且 163840 个值全部为 NaN；processed logits 仅将两个禁用 token 改为 `-inf`，最终 argmax=0，故 sampler 和 `None` 均被排除。只清零新分配 Mamba/KDA recurrent-state block 的候选 `998347889` 未修复 request1，仍输出 `!`，所以不能视为根因或成功修复。运行时显示 zeroer 实际消费了 block ID，但 Mamba meta 只覆盖少数 segment；另有 KV `offset/block_stride` 未应用、漏合 #12027、以及 copy/event 时序等候选，均仍需证据区分。'
  title: 全 NaN logits 与 Mamba/KDA 状态候选
- block_id: cb03c9d47241e0f8b012fe66388c7eb86c7b9526e5391ff97be893520f602a42
  indexed: true
  sha256: 81e7b1b0d4d44a2e8ef761413a042962f2d47473904b11e90d37a1cdbcaffaba
  source_range:
    end: 18232549
    part: 2
    start: 10507692
  summary: combined DFX 服务按精确 PGID 重启，request0 正常；request1 在启用逐步 `.item()` 探针后输出 `The` 而非 `!`，说明同步探针改变了异步时序，不能据此宣称修复。设备侧链路显示 zeroer 并非未执行：三步实际消费 113、17、15 个 block ID，buffer 按 0→1→0 轮转，但 Mamba meta 只覆盖两类 page 各 5 个 segment，而约 69 个 KDA/Mamba layer segment 需要清零。随后提出的布局别名、#12027 判定遗漏、copy event 覆盖和多 KV-group 命中长度截断等均被保留为候选；当前无 async-scheduling/DBO，#12027 反事实候选已通过 8K/1K，但 128K request0→request1 尚未完成。
  title: 同步探针遮蔽故障并暴露 zeroer 覆盖不足
- block_id: 1a760e164544bc872933a796f2d02aab00acc01b6e5790993285598b5533d2ba
  indexed: true
  sha256: 2901f789a255f327340abed6917bd626b7e7b38a18fb50639ee54ceefabc24e4
  source_range:
    end: 18232549
    part: 3
    start: 10507692
  summary: '补齐完整 #12027 hunk 的独立候选未破坏 8K→1K：prompt hash 为 `4825bf16…`，输出 token hash 为 `a37736b…`，采信率 `861/1134 = 75.9259%`，与 golden 逐 token 一致。128K request0→request1 仍在进行，尚未证明该候选能消除第二次 cache-hit 的全 NaN logits 或 token 0；不得将短场景通过视为根因已修复。'
  title: '完整 #12027 反事实通过短场景，长场景待定'
- block_id: 05407b2c91e512111d26a14aeaab8a7f2de6c93e9107c6df83db69e0b384e459
  indexed: true
  sha256: d6db802ab7002eea9b284d95d8701e3aad19b349b1bd21a8d349e9f6273b3b0e
  source_range:
    end: 25206135
    part: 0
    start: 18232549
  summary: '补全 #12027 后，128K 第二次请求仍复现全 NaN logits；`spec_decode_metadata=None` 被确认是正常普通 sampler 分支，`!` 是全 NaN 行被 `argmax` 映射到 token 0 的表象。DFX dump 中 876 条记录均 finite，138 组 Mamba copy 的 source/destination 一致，但探针可能通过改变时序遮蔽竞态；随后 `non_blocking=False` 的阻塞 metadata staging 候选在最小反例、8K/1K 和 128K c1/c2 中报告通过。该结果只能证明同步敏感性，不能单独证明具体依赖根因；期间还发现并报告了错误引入的 `model_runner` 改动、缺失 `gitleaks` 以及未验证的最终根因边界。'
  title: NaN 复现与阻塞拷贝诊断
- block_id: b3e33f4a599545dfe011d431eb7a459cff9208e7d5ecbc868627a2a09f2b6543
  indexed: true
  sha256: 5f1be439c90ca622b0334e8195355ffea8c2ab456a99d7f8f3985c5a2c24daee
  source_range:
    end: 25206135
    part: 1
    start: 18232549
  summary: '材料确认 #12283 回退了 #12027 的 `model_runner` dispatch 改动，#12255 改在 GDN metadata builder 中处理 stateful one-token prefill，因此当前 `a4b00791e` 被指出不应留在 #13277。`non_blocking=False` 会同步当前 stream，可能同时掩盖 state 写入、metadata H2D、raw-pointer memcpy、consumer 读取或 pinned buffer 生命周期竞态；材料明确将其定位为诊断性探针，而非正向修复。后续建议保持 `model_runner` 不变、保留异步 H2D，并用 buffer ownership、event 或明确 producer→consumer 依赖区分竞态类型；该批材料中的部分 PR/压测通过结论仍属于来源陈述。'
  title: 历史回退与阻塞方案的局限
- block_id: 9972f208d06062ff39edb760a38529bcc42ace72c78a73dae29757e293422d38
  indexed: true
  sha256: cfdac9ef3c2e6746cbcaf2183739035a438acac82e4ea247ec7725de4a8f39ae
  source_range:
    end: 25206135
    part: 2
    start: 18232549
  summary: 按白名单重新收敛后，候选 `2a827f021` 仅增加一个生产文件和一个 UT：把三张 metadata 表的 non-blocking H2D 前移到已有 `synchronize_input_prep()` 保护范围，真实 Mamba state copy 仍在 KV load 后执行；未改 `model_runner`、TASK_QUEUE、KDA 热路径或全局同步。该候选在默认 `TASK_QUEUE_ENABLE=1`、metadata nonblocking、四节点服务上通过了 Linux UT 3 项和 2 次/10240 bytes 的 NPU async pointer smoke；连续三组 128K probe 报告 fresh `The`、warmed `He` 且无 `!`/NaN，但 exact 8K/1K 在材料结尾仍在运行，尚无该批完整长场景结论。
  title: 非阻塞一文件候选与连续验证
- block_id: ce7f9e119cb985fa85bc9ab8a727acc9bdd55bf5cb9f8f68b724c251718fa185
  indexed: true
  sha256: 176d716a7627bac610ecc849d0743f0f61d9f73348f88a66483c9a3c4028c87e
  source_range:
    end: 25206135
    part: 3
    start: 18232549
  summary: '候选 `2a827f021` 在 128K c2 首轮出现输出 hash 漂移，唯一 bounded repeat 降至 `34.5952%` 并触发 `literal_none`，虽 prefix hit 仍为 `99.6094%`、fatal/NaN/Inf 为 0，因此不能视为通过；其失败代码未推送。PR #13277 已精确收敛到 `d37a76b43`，保留已确认提交，head、DCO 和当前 CI 检查回读正常。为区分异步 raw Mamba state copy 的 stream 依赖，独立 DFX commit `656c58db8` 仅加入 copy 前后两次 `torch.npu.synchronize()`，保留 metadata nonblocking 与 `TASK_QUEUE=1`；Linux UT、NPU raw-pointer smoke、四节点标志校验均通过，但服务当时仍处于权重加载初始化阶段，尚无 exact 128K c2 correctness 结果。'
  title: PR 收敛与同步 raw state copy A/B 部署
- block_id: f2130fc618cd194beb2904def3ff128351a60136b6e7578ec703407fdce5c17f
  indexed: true
  sha256: 92080cc84aa07d39eac7eb3d155637f169dadc772cdbf5d2374f22749e137a5d
  source_range:
    end: 29487293
    part: 0
    start: 25206135
  summary: 在 `d37a76b43` 基线上仅将 late metadata H2D 改为 `non_blocking=False`，targeted UT 2/2、真实 NPU raw-pointer smoke（修正首次 `uint64` harness 错误为实际 `int64` 后）通过，但 exact 128K c2 首轮仍未命中 golden。结果为 `1560/3409/487`、采信率 `45.7612%`、prefix `98.7305%`，两条输出 hash 均漂移，未执行 repeat。由此仅能排除 metadata H2D blocking 作为充分/唯一修复；此前 raw state-copy 前后同步也失败，copy 的 index、地址、alias/覆盖和 recurrent state divergence 仍未定论。
  title: clean late-H2D 同步 A/B 仍失败
- block_id: eb9581c6d673c2442b6b677ff7aa226fd2b0ee1cd8dcb39da24c6af199256b29
  indexed: true
  sha256: 422aa4bc2a02d445da21bce310c0eaf9bf7b86e1019f350e4480090321f828e9
  source_range:
    end: 29487293
    part: 1
    start: 25206135
  summary: DP2 路由审计显示只有 engine 2 计数变化，engine 0/1/3 保持为 0；四 rank 的 `bad=0`、`naninf=0`、leader PID=PGID 存活且 `health=200`。数据集、131072×2/1024×2、prompt hash、路由和源码均匹配，结果文件 SHA256 为 `65cce510bf08925d97c149b59eb84f382010c4ff92b5880a2f75d03bf10dc2cb`。记录认为 raw state-copy stream 同步和 late metadata H2D blocking 都不是单点正确修复；纯 `d37` 的高 prefix-hit 重放曾出现采信率由 `45.7612%` 降至 `29.0786%`、控制标记污染，但三轮均未观察到 literal `None`。
  title: 路由与同步候选排除审计
- block_id: 9a10414e446ce38d440b2a32a47f494fc95cb5ddd3ac8cd6f84537d09537bdc7
  indexed: true
  sha256: fcb31604e906939149a9ac6dbaf3cdda65d1fedae7d19547da4be76b19825e2a
  source_range:
    end: 29487293
    part: 2
    start: 25206135
  summary: 模型卡列出的 GSM8K、MATH500、HumanEval、MBPP、MT-Bench、AIME26 及截断至 4096 token 的 OpenPerfectBlend 都没有可直接使用的自然 128K 样本；InfiniteBench `longbook_choice_eng` 被提出为可裁成 131072 token 且问题可判定的候选，但尚未形成验证结果。AIME26 更适合将多条自然结束的 `thinking_effort=max` 正确轨迹按完整 chat 轮次拼成 128K 语义前缀，不能用 `ignore_eos` 强行延长；生成源可采用已验证的 0.23.0 GPQA 记录，当前 `d37` 只作被测对象。已有纯 `d37` bounded r1/r2/r3 证据显示三轮未出现 `None`，但高命中重放稳定出现坏输出、内部指令/`<|close|>`/`<|sep|>` 控制标记和 `29.0786%` 采信率。
  title: 语义长上下文候选与测试设计
- block_id: c536526c6129a817db48497d3fb0b49c55bc5e01cd9194f96ce6c2e632863656
  indexed: true
  sha256: 6f4285e5ea3ccb62e235a6d3526b6dd7f6c28678de07d44b1f3cfa65c680c538
  source_range:
    end: 29487293
    part: 3
    start: 25206135
  summary: '测试矩阵被确定为 vLLM（PR #13277 pure head `d37a76b431…`）与 SGLang 镜像各 6 组：原始 GSM8K 32 题、8K/1K 构造、128K/1K prefix-cache 构造，分别比较单并发与多并发；共同使用 `0726-Kimi-K3-w4a8-int-moe` 非 QuaRot target 和最新 GQA DSpark draft。vLLM GSM8K matched 已完成：c1 为 32/32、采信率 `55.8836%`，c16 为 31/32、`55.2257%`，两边 `None=0`，但 7/32 文本 hash 和 17/32 token 序列变化，说明有轻微并发扰动而非已证实的大幅普遍坍塌。exact 8K c1 曾因缺失 helper 导致结果不纳入统计，失败证据保留；全新目录正式重跑正在进行，已完成 7/16，均 HTTP 200，完整输出与 raw metrics 正在落盘。'
  title: 非 QuaRot 权重下 vLLM 测试进行中
- block_id: b77d15b1d7897f5d52bddf7518e39594c3d527f59cfef295cbccd27fa255526d
  indexed: true
  sha256: a7d9f5e2543ee5cc6c00feabafcb85c8b90b1b7b95bda22c215a5478903f16fe
  source_range:
    end: 38548255
    part: 0
    start: 29487293
  summary: 已报告非旋转权重下 vLLM 在 128K prefix-on、c2 出现两条 1024 个 `!` 的异常，而 SGLang c2 正常；但此前 vLLM 与 SGLang 的 128K 输入 token ID/hash 并未完全对齐，跨后端绝对值只能作参考。随后将主要嫌疑收敛为长序列、prefix reuse 与并发状态复用/异步拷贝交互，并提出非旋转 8K prefix on/off、c1/c16 的四格对照及逐步缩小 dump 条件。
  title: 非旋转与 QuaRot 的 vLLM/SGLang 长序列对照及后续边界实验设计
- block_id: ff2d5f86919d3cdfe86a663b0455a27db3e757436f217ebc7c347afed816d91d
  indexed: true
  sha256: 4729edce0145148bc4c68b4517109f95b9519980684cbba9750ebb79d9ab0f18
  source_range:
    end: 38548255
    part: 1
    start: 29487293
  summary: QuaRot GSM8K 接近非旋转 vLLM，但 8K/1K 采信率仅 30.86%/33.35%、平均接收长度 3.160/3.334，并出现 `None` 语义词及控制/内部指令续写；这些请求虽 HTTP 200 且长度完整，不能视为成功。QuaRot 128K c1 为 42.97%/4.008，c2 与非旋转 vLLM 一样变成全 `!`，表面 100%/AL 8.0 是坏状态伪高值；SGLang 128K c2 正常，但其 tokenizer 输入 hash 与 vLLM 不同，因此严格归因仍限于 QuaRot 与非旋转 vLLM 的同 token 对比。
  title: QuaRot 六组结果显示长输出退化与共同的 128K 并发崩坏
- block_id: 73695d553189d56d0d1eeb233e0f533847f382a63a34f6c3db9f3fcf3fca2bef
  indexed: true
  sha256: a9e6ae53486f9ba78bddc00fdb4166b7ddd0cdb8428b6c39597ce350da5e4c11
  source_range:
    end: 38548255
    part: 2
    start: 29487293
  summary: 非旋转 prefix-on 的 `8064+128` 与 `8064+384+128` 两组均在 c1/c16 复现 7680 或 8064 cached tokens、全 `!`、100% 采信和 AL=8，说明并发不是必要条件，也不能归因于是否多命中一个完整 block；进一步的实际命中 384、896-token case 也复现全 `!`。为排除重复拼接语义，改用一条固定 revision 的天然数学推导记录，精确 896=768+128；同 artifact 的 prefix-off 已正常回答 `17/21`、cached=0、采信率 63.53%、AL 5.45，而 prefix-on 384 命中对照尚未给出最终结果。
  title: Align prefix 命中边界与自然 896-token 最小复现
- block_id: 6c65cdfd67e69e8108ad9b7ccbdfcbae852b09816f8fd6b3e8558b6b8e46e88e
  indexed: true
  sha256: 75efad013d40457515a91108202e9c7484d8f4362a6cce71f116b2a879896225
  source_range:
    end: 38548255
    part: 3
    start: 29487293
  summary: 同为 896=768+128、实际命中 384 的条件下，Synthetic GSM8K prompt 在 prefix-on 服务中稳定输出 1024 个 `!`，而自然数学 prompt 正常；因此 prefix hit 不是充分条件，prompt 数据或边界状态可能参与触发。当前更合理的待验证方向是 recurrent state/KDA 恢复、block/slot 映射、异步 state copy 或数值/缓冲区污染；连续 token 0 不能单独证明存在 NaN/Inf。已从纯 `d37a76b431` 基线隔离出不改 ModelRunner 的 DFX，准备比较 prefix-on `computed=384/scheduled=512` 与 prefix-off `computed=0/scheduled=896` 的映射、copy metadata、KV/state、逐层中间结果和 logits。
  title: Synthetic 与自然 prompt 的稳定差异及 DFX dump 方案
- block_id: f14fd1156c78b5e3fef42f7a946f17b05db519408105b6664993a8a3ba888a94
  indexed: true
  sha256: 8491a31e44d0a4deb3f70270ec60188a25b2d7116088303c5a4dce59168d18c2
  source_range:
    end: 38548255
    part: 4
    start: 29487293
  summary: 四机 DFX 服务已完成 64-rank 初始化、351 个 target shard、KV/profile 和 graph capture，health=200，KV capacity 为 343,109 tokens；启动阶段未见 traceback、OOM 或 DFX 异常。对 SHA=`7c1217…b7d` 的 896-token Synthetic GSM8K artifact，目标请求准确命中 `computed=384/scheduled=512` 并写出约 1.5 MB manifest 和 330 MB tensor，但带 DFX 的 4/4 请求均未输出 `!`，与纯 `d37` 基线此前 4/4 全 `!` 不同。由于只有首请求 dump/synchronize，观测本身可能改变异步时序，现阶段只能提高竞态/异步 copy 嫌疑，不能宣布数学 NaN 或某个 block/slot 非法已被证实；prefix-off 对照及 dump 内容核查仍未完成。
  title: DFX 观测改变复现结果，竞态嫌疑上升
- block_id: f85999a142224fb7650a93cd698b5a6c9b0be276fd8f7a495ec68df6f029758e
  indexed: true
  sha256: f3c61cc8c99387473bf28277001636ec4432af0e5a6cb3262ea00720916978e2
  source_range:
    end: 51494840
    part: 0
    start: 38548255
  summary: prefix-off 复现确认未进入 `mamba_cache_mode == "align"` 的 `preprocess_mamba`，因此改为仅在 target model forward 中按真实 shape 采集，并过滤 `in_profile_run`、ACLGraph `capturing` 和 dummy/profile forward。384、512 的 dump 全部 finite，slot mapping 512/512 匹配，前三个共同 MLA block 及 69 层 KDA initial state 逐 bit 一致；shape=16 的编译/tracing 重入混合记录被判定为不可直接使用。该批数据可作为正常时序 golden、排除稳定的 mapping/KV/state 错误，但不足以定位只在未观测路径出现的竞态，因此停止扩大 dump。
  title: Forward-only 多 shape DFX 与 golden 排查
- block_id: df132e4c5adfe7a3c29fa9d750b4c0050c9ba7586c0c7994b74b6f4c01901231
  indexed: true
  sha256: f9cc84111e72afe40380dcd5c3db22c21fc1f0c31eaa77eb21cf57b0112e69d9
  source_range:
    end: 51494840
    part: 1
    start: 38548255
  summary: '正向链路发现 `preprocess_mamba` 填充的三张 pinned CPU metadata 表位于 `synchronize_input_prep()` 保护区内，但 `copy_to_gpu(non_blocking=True)` 在保护区退出后才执行，过早 event 允许下一轮重写源 buffer；这可解释 prefix cache 命中时才触发。最小提交 `2a827f021` 将 metadata H2D 纳入保护区、保留 KV load 后 raw state copy，3/3 单测和 NPU smoke 通过；896-token case 三轮 12/12、128K 首 token 2/2 正常，但不能视为所有问题已解决。干净 DP1/DP2 的 128K 单/双并发采信率约 63.47%/63.66%，而已使用 DP0 或 DP3 在长 decode 后的 c2 wave 可稳定出现全 `!`、随后 c1 恢复，表明仍有请求历史相关的当场并发踩踏；随后定位到 0.23.0 的 #12038 等价 guard 已在 0.26，而 #12189 的异步 `block_table` 源缓冲区固化修复似乎未带入当前分支，尚待验证。'
  title: Mamba metadata H2D 生命周期修复及残留并发踩踏
- block_id: 82fa4aaf284882db08ef36b38bb30e6b7267d25b8b110c484ffad67681aeebb6
  indexed: true
  sha256: e4ff297154b7013e1d27dc6da29e433131d6b0ceb05adebb81a5b36364b025cf
  source_range:
    end: 51494840
    part: 2
    start: 38548255
  summary: '在修复后的干净 engine 上，非旋转权重 128K/1K 的 c1-first 与 c2-first 采信率分别为 63.471% 和 63.660%，没有证明单并发或双并发本身导致下降；但同一 engine 经历长前缀复用后，c2 可出现两条全 `!`，且普通 `/reset_prefix_cache` 不能恢复当时状态。进一步在 DP3 复现了“c1/1K 正常→c2 首 token 全 `!`→c1 恢复”，因此更准确是高命中并发 wave 的瞬时状态/缓冲区踩踏，而非已证实的永久污染。对历史修复的核对认为 #12038 的 hybrid hit 防护在 0.26 已有等价逻辑；#12189 关于异步 CPU→NPU `block_table` 源缓冲区被重写的修复只确认进入 0.23.0，当前 0.26 代码仍直接 non-blocking `copy_to_gpu()`，是否为剩余根因尚未验证。'
  title: 非旋转权重 128K 单多并发与满 KV 修复核对
- block_id: 72151e5cbbe46335ad35a500bf1b4169fc9313b6e6e4f3137c5813618546a303
  indexed: true
  sha256: 0c8732edc574e8a62c77679b5a9256791f2c513f5608c632fbc8eac22c0ed850
  source_range:
    end: 51494840
    part: 3
    start: 38548255
  summary: 将 `commit_block_table()` 改为同步 CPU→NPU 拷贝后，c1/1K 的采信率 `0.6347101175` 和平均接收长度 `5.4429708223` 与旧版本一致，但 c2/1-token 仍稳定输出 `!`，后置 c1 恢复为 `" He"`、再 c2 又变为 `!`。这排除了当前现象由该类 block-table H2D 覆盖竞态单独造成的解释，但不能据此关闭其他 metadata 或状态生命周期问题；实验提交为 `3a2cae19b0f638...`，此前归档中曾出现错误展开的 full SHA，已记录为 provenance 笔误。#13277 已推到线性 head `e560c8031`，净 diff 保持不含 MLA preprocess/ModelRunner 改动；CI 后续虽通过部分阶段，但用户要求暂不继续跟进。
  title: 同步 block_table 实验未改变 c2 精度异常
- block_id: 66bd5888d3bbe235482e5fa31e6d59095c3f6659ad945aac44c45c43bb12c1d5
  indexed: true
  sha256: 34061acb2b2212d0d520d16a1da5dc80dfcf7f5875402d59b6188915ce84567f
  source_range:
    end: 51494840
    part: 4
    start: 38548255
  summary: '建议用 `num_gpu_blocks` 限制低可用 KV，固定 token、DP rank 和请求顺序，以短 prompt 放大 prefix cache、Mamba state、异步调度和物理 block 复用问题；该场景仍只是辅助复现器。纯 CPU 分析在 `fe784ff22` 的真实 20-group K3 拓扑中发现，reconciled hit 只有 1 block 时，DSpark 的 group 5 仍保留 4 个共享 block，其中 3 个越过合法边界并被 `allocate_slots()` 绑定给新请求；仅裁剪所有 full-attention group 后，20/20 group 满足 ownership invariant。该结果支持一个确定存在的 core ownership bug，但其与 NPU `!` 现象的因果关系仍需低 KV A/B 验证；上游对应 PR #50457 仍为 OPEN，实验修改尚未提交。'
  title: 定位到 full-attention group 的 cache ownership 踩踏候选
- block_id: 3537cc16109e50db35ee2fc728d5b88aebae015ad50b84df39063ebeaaac1471
  indexed: true
  sha256: bd342b7b13aea0b541de5efef9f62fdf7060009e3b2e9c6c2e4fd3b683c12ca5
  source_range:
    end: 51494840
    part: 5
    start: 38548255
  summary: '采用干净 #13277 的异步 block-table copy，加上 `kv_cache_coordinator.py` 的最小 core 修复，低 KV 服务先后因 draft 上限 4096 与 target 2048 不一致、以及 FULL_DECODE_ONLY 下 capture size `[4]` 不是 TP16 倍数而在模型加载前退出；这些是配置门禁失败，不是修复失败，失败日志均保留。调整为 target/draft 2048、capture `[16]`、`max_num_seqs=16` 和 `num_gpu_blocks_override=512` 的 `r3` 后，四个 rank 已拉齐 16/16 TP workers，进入 HCCL rendezvous 和量化模型初始化，当前无 traceback/error。服务尚未报告 health、请求结果或低 KV 复现结论；日志中的 `v0.23.0` 仅是节点安装包元数据差异，实际 core 源文件哈希一致。'
  title: 低 KV 修复实验服务仍在启动阶段
- block_id: b06de90b8ea7a728aa31e83b698a6d1b610fe2c77ade8dc510ca3f3340cada75
  indexed: true
  sha256: b389b92d1483549874a0720d17d47e0db14ba76e3869ad414bd564ff0e5186f3
  source_range:
    end: 64156799
    part: 0
    start: 51494840
  summary: 低 KV 的 64 条历史压力及 canonical c1/c2 均通过，但 full-KV 的固定 128K 并发 c2 仍复现两条 1024 个 `!`，而 c1 正常；因此“截断所有 FullAttention group”的修复在该拓扑上实际是 no-op，不能归因于本次精度问题。审计显示普通 serving 不经过相关 hybrid finder，剩余候选转向 Mamba align 跨 384-token 边界的 state ownership，以及 prefix-cache block/state 复用；PCP 仅在当前 Kimi/MRV1 路径固定为 `pcp_world_size=1`，仓库整体仍保留通用 PCP 代码。
  title: Full-KV c2 仍失败，问题转向长序列状态污染
- block_id: 552c0f93eb67eaaff0fef1c6fd2ff40fab25cd098763acb75c136c98fd94e2b1
  indexed: true
  sha256: 5f86c716c28b79dde121cc7fc0dfc1e91200fff24a84be52ccd92ee2e0659e4c
  source_range:
    end: 64156799
    part: 1
    start: 51494840
  summary: 固定 128K 单 token c1/c2 dump 的多轮探针先后受到环境变量未传播、scheduler `computed/scheduled` 假设错误、触发入口过窄及混合 CPU/NPU `torch.stack` 落盘异常影响；这些都是诊断代码问题，不能当作模型失败。通过改用 CPU prompt 长度、修复逐项搬运并将触发延后到正式请求第 5/6 个 prefill，最终获得可用异常对照：c2 仍输出 `!`，且 Mamba cache 的 `source_before_copy` 已含 NaN，随后传播到 KDA/MLA 和 logits。
  title: 长序列 DFX 探针反复校正，最终确认需延后抓取
- block_id: 86c0ba44a6afa4d8b1841cfb5391f2d20aa43b4f0dcbce121260058a54a9e925
  indexed: true
  sha256: c4e7390e154b8ef77a0fbe25754f18f0d54b5d593912a4bbfd7c48fbb57002b7
  source_range:
    end: 64156799
    part: 2
    start: 51494840
  summary: '延后 dump 的复现表明第一轮 c2 可正常、后续清 cache 后的长序列请求才稳定退化，污染在 reset 后的 warmup 已出现；layer 0–2 KDA 仍 finite，layer 3 MLA attention output 首次出现 NaN，支持复用的 full-attention KV block 未清零/错误复用这一方向，但尚未完成修复验证。尝试放开 Ascend `worker.py` 中仅限 `eagle3` 的 zeroer 初始化后，启动暴露 `Non-uniform page sizes: 98304 vs 12288`，说明 Kimi 的异构 MLA page size 还需要分组清零；当前 r8 候选同时关注 page-size 分组和 Ascend MLA spec 是否会上报待清零 block，ModelRunner 仍未修改。'
  title: 锁定 KV block 未清零，zeroer 暴露异构 page-size 门禁
- block_id: 5e6bcec0290a1f51d8006cc3570b973b2170df1b9b5fa4c532499b0df76dc7db
  indexed: true
  sha256: 636c7ca697fbd8b38f22cf27781dd1d2b10d0f8c9acec9fca0ff69b0524129e3
  source_range:
    end: 64156799
    part: 3
    start: 51494840
  summary: 统一 hybrid zeroer、按 98304/12288 page size 分组清零，并补齐 AscendMLAAttentionSpec 的新 block ID 上报后，r8 仍在第二轮 c2 稳定复现两条 `!`；延迟到首次消费后的 dump 也未改变结果，未支持普通异步拷贝竞态假设。正常/异常请求的 block table、slot mapping 和 context_lens 一致，失败链路中 layer 3 当前 q/k/v 与 FIA 输出有限，但首个 context chunk 读取的 compressed-KV `kv_c` 已含 NaN，而同位置 `k_pe` 有限，提示坏值更早来自写入或 hybrid cache 覆盖。r9 曾因部署包缺失约 67 MB 的 `custom_transformer` vendor 目录、找不到 `aclnnAddRmsNormBias` 而启动失败；补齐并重试后精确复现，r10 已准备进一步检查 129792-token prefill 的 `kv_c` 写后读回、block NaN 分布及 cache storage 区间，但尚未产生新结论。
  title: 异构 MLA zeroer 后复现并定位 kv_c 污染
- block_id: c336ae1a910386c4865ae54a26f0dfb0209c258e845f4bb921304e13ccb48751
  indexed: true
  sha256: 40068c8bd2c6a1ebbf2444b4e109d588b7f124ed3fc5b870ae9842decd43e054
  source_range:
    end: 78651729
    part: 0
    start: 64156799
  summary: r10–r12 在 64-rank HCCL、KV 初始化和 size-16 graph capture 后，按 c1→c2→marker→c2 复现了两个 `!`；错误客户端曾污染一次历史，随后用 formal SHA `087744…`/`f78cbc…` 的正确包装器重跑。dump 与 KDA 状态摘要未消除现象：match 8 的 layer 3 之后出现局部 NaN，layer 28 KDA 的 `recurrent_state_after_write` 才观察到下游污染；已有证据支持继续检查 MLA/KDA 共享 backing storage，但尚未形成最终修复验证。
  title: r10–r12：确认首坏边界并排除同步假设
- block_id: 3936416417dc7d56cd7bfd50d0b86a61bd67b2fadc98a02b510f20c8578824fb
  indexed: true
  sha256: 517ba46d5e3e97f64087a48525743bb4d159a10a828781a2b600da40b30eecb3
  source_range:
    end: 78651729
    part: 1
    start: 64156799
  summary: r13 的单页哨兵显示目标 page 在 match 7 `post_logits` 仍 finite，而 match 8 layer 0 attention 前已有 448 个 NaN，写坏发生在两个 forward 之间；因此 zeroer、普通 attention/MLP 首次写坏以及“固定 logical block 必然对应目标页”的判断均被削弱或排除。r14 重新分配后发现 DSpark draft 未执行、spec `drafts=0`，旧 page 监控也保持有限；随后设计全链路一次性 dump，r15 因二维 `cache_indices` 触发 ACL `index_select` 错误而失败，属于诊断探针启动问题。
  title: r13–r15：zeroer 与 draft 假设的修正
- block_id: a1e564f007131216b7e45a34a40cf5581811fd6752f4a3a9f2dcaf45434130ac
  indexed: true
  sha256: a6409acc760acd28fd17795beb82b2f8070f2993f99e245f4628ed0c44943662
  source_range:
    end: 78651729
    part: 2
    start: 64156799
  summary: r16 修复了 dummy/图编译路径中的二维 `index_select`，成功通过 64-rank 启动、graph capture 和健康检查；c1/c2 历史 SHA、命中率与输出均和基线一致，最终 c2 仍输出 `!`，所以 dump 未治好现象。全链路记录显示 match 8 layer 23 读取历史 `loaded_kv_c` 时已有 448 个 NaN，涉及 group0 block `5224`/page `15672`，并观察到 draft layer94 的 group5 block `1969` 与其物理地址重合；这是强正向证据，指向不同 KV group 的紧凑 stride 与共享 backing storage 发生地址别名，但仍需修复后的独立验证。材料还指出当前 Ascend 分配链路没有真正支持首轴非连续 view，单改 `page_size_padded` 不够，需让实际物理布局包含 padding 或隔离物理池。
  title: r16：全链路 dump 与物理别名证据
- block_id: 1d5205232d3d515e87c4eb540df52f529f85bc8b5df166408959ef244b346170
  indexed: true
  sha256: 25f2e9498b5d77f038d2c0ff01cde94919893c55f2587528759136b8cdec836c
  source_range:
    end: 78651729
    part: 3
    start: 64156799
  summary: 讨论先拆分 DSpark draft backing 的验证性方案，随后修正为在首次 KV layout planning 时同时纳入 target、Mamba 与 DSpark spec，并按物理布局兼容性分 bucket。隔离测试曾报告 19 passed，r16 拓扑模拟估算由 5689 降至约 4735 blocks，但服务未重启，因此线上问题尚未验证修复。进一步审查认为 K3 特判过于定制化；DSV4 通过既有 MTP 独立 KV pool 规避了同类风险，而长期方向应是通用 physical-layout planner 与 stride-aware consumer。
  title: 从事后拆分转向统一布局规划
- block_id: b1560b9e2b2df3a6c425501750be913e7d3b1cd4f67c3dd208b56dba9654e807
  indexed: true
  sha256: 1b474ffffb89a6e728c9e78ea767eca8ea07d10512166f59909fde81a9e818c4
  source_range:
    end: 78651729
    part: 4
    start: 64156799
  summary: 材料区分了只计入 allocation 的 `page_size_padded=488448` 与 consumer 实际按 padded `block_stride` 寻址；当前 hybrid attention 的 `.view()` 仍近似使用 draft 的 98304B 紧凑步长，故单改 planner padding 不能避免 alias。引用的上游案例包括 packed layout、runtime block stride 与 FlashInfer consumer 不匹配导致 GSM8K 崩溃，且 vLLM-Ascend 正在推进 `offset/block_stride` 对齐；这些外部结论在本材料中作为报告内容保留，未在此批次独立核验。当前决定暂按 DSV4 MTP 方式让 5 个 DSpark draft layer 各自使用连续 backing，并在代码中声明待 FIA 支持首轴非连续且 Ascend allocation/reshape 接通社区 stride 方案后再迁移；目标 UT 报告 19/19，但仍未提交、推送或完成带请求历史的 128K 运行时回归。
  title: padding 契约与 DSV4 式临时隔离
- block_id: 8b1a513e5d4e1768e9cd14423f41fe08ed5bafd582ef9f24f981c5719df753ea
  indexed: true
  sha256: da8c7b7c57478d86e243c334e9ce99c8bdbab3474a96b87d113793c39c509d2b
  source_range:
    end: 84849913
    part: 0
    start: 78651729
  summary: 最初判断将 backing collision 归因于 GQA DSpark，随后将条件收敛为限定 Kimi K3 + DSpark，并比较 target/draft 的 `unpadded_page_size_bytes`；GQA 触发独立 backing，MLA 同构布局走原生共享路径。一次长历史复现曾显示修复有效并将 PR 从 7 个提交整理为 6 个，但后续 A3 压测重新出现 1024 个 `!`，证明此前验证不能视为最终成功。PR 的 6 个提交按反合、`fused_norm_gate`、MLA、QuaRot、异步 metadata、GQA KV 隔离分类，代码 tree 保持不变；CI 中的 PR create action 失败被报告为 runner 的 GraphQL 超时。
  title: GQA/MLA 条件收敛与 PR 提交重组
- block_id: ebd5a1a6c6df2adc98e85372bf564d7d324bca23ce4839352a7e2aa42a14d23c
  indexed: true
  sha256: 39d3633d7f2bcc7094826e942a5af563efb64c84c34b7444ec66cd5613951d3e
  source_range:
    end: 84849913
    part: 1
    start: 78651729
  summary: 在 A3 上连续执行 128K/1K、c2 压力序列后，第二次 prefix warmup 及后续 10 条请求均变成 1024 个 `!`；KV 容量约 285.6K tokens、并发约 2.12x，四节点 worker 和 health 均正常，因此未能归因于容量过载或进程错误。全新服务上连续三次相同 prefix 可以正常，但经过长序列并发与长 decode 后，下一次高命中读取会暴露 token 0；`reset_prefix_cache` 更像暴露/切换观察点，而非根因。A5 提供的短 prompt `who are you` 二次复现未能在冷服务上重现，说明现象依赖既有 KV 历史或容量状态，仍不能宣称两场景已完全等价。
  title: A3/A5 长历史触发 KV 污染复现
- block_id: b2213f002047e9bc4cb818b7bce835018c3a47d36750f3ff2460a340b036ddf5
  indexed: true
  sha256: 151f4db37eb4fc424ff390fd8cbd64392429142f56a94deff96d81321eea3d67
  source_range:
    end: 84849913
    part: 2
    start: 78651729
  summary: 短 prompt 冷服务测试两次均正常；随后建立长请求历史时，正式 128K c2 请求出现 1024 个 `!`、100% 接受率和平均接受长度 8，且第二次 129792-token warmup 也从 `does` 变为 `!`，支持“前序 KV 写坏、后续 prefix read 受害”的方向。现有 dump 只抓到了第一条正常请求的前三个 7680-token prefill chunk：主链和 `raw_target_logits` 均 finite，Mamba 的 `destination_before_copy`、`initial_state_before_clear` 等 NaN 与正常缓冲区现象同时存在，不能据此归因 Mamba。当前 selector 按 chunk 重复命中同一 request，尚未抓到真正坏请求；计划改为按 request ID 计数，目标定位 match17（正常末 chunk）、match18（首个坏请求）和 match19（后续坏请求），该定位结果尚未完成。
  title: Dump 首轮未捕获坏请求，需修正选择器
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6e8e56e03f14cabd74fa97482deafe594561d16aa829fb94866cabfdd965200b
  kind: experience
  material_digest: 7159d9d383de6562d6206201f2c16776f7cee962eb6d642d8d42031a364b8cdc
  revision: 01f856a9b3fdfbd3e814cb7f218d07079822c63deeb2c073c01b5547eb914ede
  schema: mindie-entry/3
  summary: DSV4 式 GQA draft 独立连续 backing 已通过定向 UT、容量规划和部分历史验证，但 A3/A5 后续压力测试再次出现 token 0/`!`，因此修复仍不能视为完成。当前较强线索是长序列并发与长 decode 期间仍有 KV backing、slot/block 或 stride 复用写坏，之后由 prefix-cache 高命中读取暴露；`reset_prefix_cache` 不是已确认根因，短 prompt 二次命中也未在冷服务独立复现。现有 Tensor dump 只覆盖正常 prefill chunk，未捕获坏请求；下一步应修正 selector 按 request ID 计数，获取 match17/18/19 的 block ID、slot mapping、物理 backing 地址及首个异常层，并反查前一轮请求的写入者。MLA 同构布局旁路和按 `unpadded_page_size_bytes` 判断仍是当前实现假设；按 physical layout family 分池、FIA/`offset`/`block_stride` 全链路支持仍属于后续方案，不能当作已验证事实。
  title: 当前任务导航：追踪未完全修复的 hybrid KV backing 踩踏
navigation: DSV4 式 GQA draft 独立连续 backing 已通过定向 UT、容量规划和部分历史验证，但 A3/A5 后续压力测试再次出现 token 0/`!`，因此修复仍不能视为完成。当前较强线索是长序列并发与长 decode 期间仍有 KV backing、slot/block 或 stride 复用写坏，之后由 prefix-cache 高命中读取暴露；`reset_prefix_cache` 不是已确认根因，短 prompt 二次命中也未在冷服务独立复现。现有 Tensor dump 只覆盖正常 prefill chunk，未捕获坏请求；下一步应修正 selector 按 request ID 计数，获取 match17/18/19 的 block ID、slot mapping、物理 backing 地址及首个异常层，并反查前一轮请求的写入者。MLA 同构布局旁路和按 `unpadded_page_size_bytes` 判断仍是当前实现假设；按 physical layout family 分池、FIA/`offset`/`block_stride` 全链路支持仍属于后续方案，不能当作已验证事实。
schema: mindie-material-task/1
status: complete
task_id: 6e8e56e03f14cabd74fa97482deafe594561d16aa829fb94866cabfdd965200b
---

# 当前任务导航：追踪未完全修复的 hybrid KV backing 踩踏

DSV4 式 GQA draft 独立连续 backing 已通过定向 UT、容量规划和部分历史验证，但 A3/A5 后续压力测试再次出现 token 0/`!`，因此修复仍不能视为完成。当前较强线索是长序列并发与长 decode 期间仍有 KV backing、slot/block 或 stride 复用写坏，之后由 prefix-cache 高命中读取暴露；`reset_prefix_cache` 不是已确认根因，短 prompt 二次命中也未在冷服务独立复现。现有 Tensor dump 只覆盖正常 prefill chunk，未捕获坏请求；下一步应修正 selector 按 request ID 计数，获取 match17/18/19 的 block ID、slot mapping、物理 backing 地址及首个异常层，并反查前一轮请求的写入者。MLA 同构布局旁路和按 `unpadded_page_size_bytes` 判断仍是当前实现假设；按 physical layout family 分池、FIA/`offset`/`block_stride` 全链路支持仍属于后续方案，不能当作已验证事实。

## Materials

- [0.26.0rc 385-token NaN 根因定位与 #12255 修复](blocks/fb4ae558d9b910a1dadf6107d976f154826d65124f248b5a1f94ac596db2e3b2.md): 在纯 0.26.0rc、无 DSpark 的最小复现中，只有 385-token（384+1）失败：state copy 138/138 一致，但 decode graph 后 [1,7168] hidden 全为 NaN，输出连续 token 0；386-token 正常。证据将根因定位为 GDN metadata 与 decode graph 的 stateful 单 token prefill 分类契约缺失，主仓 #11161 后未带入私仓 #12255；恢复分类后 385 两次均精确恢复，且 64-token 延续正常。对 128K/1K DSpark 场景，完整 #12255 修复了乱码、NaN 和假 100% 采信，但连续采信率仍从 60.27% 降至 39.50%，说明还有状态复用或调度问题未解决；对 #12027 的直接归因也未被 trace 证实。
- [128K/1K 采信率与 PR #12027 的未决关系](blocks/6ebc5eb6af6788a2df2b819fd6af9e05f939f58d9c5e136f0a89a05ae512efdc.md): 完整 #12255 修复后，128K/1K c1 从坏基线的“100% 采信但全 !”恢复为 76.49% 正常输出；c2 首轮为 60.27%，第二轮降至 39.50%，且输出 hash 变化。加入 #12027 最终保留的 builder 语义后，三轮为 45.60%、45.70%、46.07%，但四个 DP 的相关 trace 均为 0、PCP=1，不能据此宣称 #12027 是直接修复；当前更像 prefix-cache 后 recurrent state 或 slot/block ownership 漂移。
- [#13277 集成修复与 DSpark 四节点压测进行中](blocks/46f3b08bdfbf8856343da810990d64cca2c4a89e16179568a727ef6c1019b9eb.md): 在 PR #13277 上完成 rebase、CI 修复，并移植 #12255 与 #12027 的最终有效语义；最终远端 SHA 为 af51098fa，静态检查、format、compileall 和 native exact-SHA build 通过，focused pytest 仍受本机缺少 vllm_ascend._build_info 阻塞，gitleaks 也未运行。四节点 980 个 native 文件和 import manifest 已校验一致，旧 r26 已切换到 port 18117 的新服务，64-rank 初始化、345/345 权重加载和 GQA draft 加载均推进正常。当前仍处于 exact torch-compile 与 FULL_DECODE graph capture 阶段，尚未完成 health、四 rank 错误审计或 128K×2×1K DSpark 压测，因此“库上有可用且无问题的 PR”尚未被验证。
- [#13277：#13407 修复 8K 采信率，但 128K cache-hit 仍失败](blocks/53f71d6cca3e2ea4e9ed910ffbec9424f9cfc217e0b81b411ba8162c023c08aa.md): 四 rank 服务完成 graph capture、health=200、fatal=0；补入 #13407 后，8K×1K 在同权重同 prompt 下采信率从约 0.3% 恢复至 75.9259%，输出哈希与历史通过版本一致，确认该回归源于遗漏 #13407。128K 首次 full prefill 仍正常，但第二次 cache-hit 继续退化为重复 `!`，说明 KDA/Mamba cache lifecycle 仍有未解决的状态问题；此前的 KDA tail clear 未覆盖真实命中路径。后续又发现历史通过组合还包含 #13327（`ec56b6b2`）的 warmup、rejection sampler `do_not_specialize` 和 worker warmup 改动；old-native A/B 与补入 #13327 的严格反事实验证仍未完成，当前 PR 不可宣称可用。
- [从 token 0 追查到 sampler 前的 NaN](blocks/7549a6fbb53e3b54eef01d562da0a7b5352eb9a2f25b41db9b3204630a87450c.md): 旧 native 已排除：8K 结果一致，但 128K 第一次正常、第二次高命中稳定输出 token id 0（`!`），与当前 native 相同。#13327 的 warmup 已在四个 DP rank 上真实执行且失败数为 0，但 8K/1K 通过后 128K 仍失败，因此该候选未推送。干净服务对照显示问题依赖先执行 8K/1K；`spec_decode_metadata=None` 是普通 sampler 的正常分支标志，首个已知异常是 sampler 收到的 raw target logits 全 NaN。
- [全 NaN logits 与 Mamba/KDA 状态候选](blocks/78c64958b6baea2622c182ce87b396a7440162d7955499bfa88ef7add565feae.md): 探针确认第二次 128K 的 raw logits 为 `(1,163840)` 且 163840 个值全部为 NaN；processed logits 仅将两个禁用 token 改为 `-inf`，最终 argmax=0，故 sampler 和 `None` 均被排除。只清零新分配 Mamba/KDA recurrent-state block 的候选 `998347889` 未修复 request1，仍输出 `!`，所以不能视为根因或成功修复。运行时显示 zeroer 实际消费了 block ID，但 Mamba meta 只覆盖少数 segment；另有 KV `offset/block_stride` 未应用、漏合 #12027、以及 copy/event 时序等候选，均仍需证据区分。
- [同步探针遮蔽故障并暴露 zeroer 覆盖不足](blocks/cb03c9d47241e0f8b012fe66388c7eb86c7b9526e5391ff97be893520f602a42.md): combined DFX 服务按精确 PGID 重启，request0 正常；request1 在启用逐步 `.item()` 探针后输出 `The` 而非 `!`，说明同步探针改变了异步时序，不能据此宣称修复。设备侧链路显示 zeroer 并非未执行：三步实际消费 113、17、15 个 block ID，buffer 按 0→1→0 轮转，但 Mamba meta 只覆盖两类 page 各 5 个 segment，而约 69 个 KDA/Mamba layer segment 需要清零。随后提出的布局别名、#12027 判定遗漏、copy event 覆盖和多 KV-group 命中长度截断等均被保留为候选；当前无 async-scheduling/DBO，#12027 反事实候选已通过 8K/1K，但 128K request0→request1 尚未完成。
- [完整 #12027 反事实通过短场景，长场景待定](blocks/1a760e164544bc872933a796f2d02aab00acc01b6e5790993285598b5533d2ba.md): 补齐完整 #12027 hunk 的独立候选未破坏 8K→1K：prompt hash 为 `4825bf16…`，输出 token hash 为 `a37736b…`，采信率 `861/1134 = 75.9259%`，与 golden 逐 token 一致。128K request0→request1 仍在进行，尚未证明该候选能消除第二次 cache-hit 的全 NaN logits 或 token 0；不得将短场景通过视为根因已修复。
- [NaN 复现与阻塞拷贝诊断](blocks/05407b2c91e512111d26a14aeaab8a7f2de6c93e9107c6df83db69e0b384e459.md): 补全 #12027 后，128K 第二次请求仍复现全 NaN logits；`spec_decode_metadata=None` 被确认是正常普通 sampler 分支，`!` 是全 NaN 行被 `argmax` 映射到 token 0 的表象。DFX dump 中 876 条记录均 finite，138 组 Mamba copy 的 source/destination 一致，但探针可能通过改变时序遮蔽竞态；随后 `non_blocking=False` 的阻塞 metadata staging 候选在最小反例、8K/1K 和 128K c1/c2 中报告通过。该结果只能证明同步敏感性，不能单独证明具体依赖根因；期间还发现并报告了错误引入的 `model_runner` 改动、缺失 `gitleaks` 以及未验证的最终根因边界。
- [历史回退与阻塞方案的局限](blocks/b3e33f4a599545dfe011d431eb7a459cff9208e7d5ecbc868627a2a09f2b6543.md): 材料确认 #12283 回退了 #12027 的 `model_runner` dispatch 改动，#12255 改在 GDN metadata builder 中处理 stateful one-token prefill，因此当前 `a4b00791e` 被指出不应留在 #13277。`non_blocking=False` 会同步当前 stream，可能同时掩盖 state 写入、metadata H2D、raw-pointer memcpy、consumer 读取或 pinned buffer 生命周期竞态；材料明确将其定位为诊断性探针，而非正向修复。后续建议保持 `model_runner` 不变、保留异步 H2D，并用 buffer ownership、event 或明确 producer→consumer 依赖区分竞态类型；该批材料中的部分 PR/压测通过结论仍属于来源陈述。
- [非阻塞一文件候选与连续验证](blocks/9972f208d06062ff39edb760a38529bcc42ace72c78a73dae29757e293422d38.md): 按白名单重新收敛后，候选 `2a827f021` 仅增加一个生产文件和一个 UT：把三张 metadata 表的 non-blocking H2D 前移到已有 `synchronize_input_prep()` 保护范围，真实 Mamba state copy 仍在 KV load 后执行；未改 `model_runner`、TASK_QUEUE、KDA 热路径或全局同步。该候选在默认 `TASK_QUEUE_ENABLE=1`、metadata nonblocking、四节点服务上通过了 Linux UT 3 项和 2 次/10240 bytes 的 NPU async pointer smoke；连续三组 128K probe 报告 fresh `The`、warmed `He` 且无 `!`/NaN，但 exact 8K/1K 在材料结尾仍在运行，尚无该批完整长场景结论。
- [PR 收敛与同步 raw state copy A/B 部署](blocks/ce7f9e119cb985fa85bc9ab8a727acc9bdd55bf5cb9f8f68b724c251718fa185.md): 候选 `2a827f021` 在 128K c2 首轮出现输出 hash 漂移，唯一 bounded repeat 降至 `34.5952%` 并触发 `literal_none`，虽 prefix hit 仍为 `99.6094%`、fatal/NaN/Inf 为 0，因此不能视为通过；其失败代码未推送。PR #13277 已精确收敛到 `d37a76b43`，保留已确认提交，head、DCO 和当前 CI 检查回读正常。为区分异步 raw Mamba state copy 的 stream 依赖，独立 DFX commit `656c58db8` 仅加入 copy 前后两次 `torch.npu.synchronize()`，保留 metadata nonblocking 与 `TASK_QUEUE=1`；Linux UT、NPU raw-pointer smoke、四节点标志校验均通过，但服务当时仍处于权重加载初始化阶段，尚无 exact 128K c2 correctness 结果。
- [clean late-H2D 同步 A/B 仍失败](blocks/f2130fc618cd194beb2904def3ff128351a60136b6e7578ec703407fdce5c17f.md): 在 `d37a76b43` 基线上仅将 late metadata H2D 改为 `non_blocking=False`，targeted UT 2/2、真实 NPU raw-pointer smoke（修正首次 `uint64` harness 错误为实际 `int64` 后）通过，但 exact 128K c2 首轮仍未命中 golden。结果为 `1560/3409/487`、采信率 `45.7612%`、prefix `98.7305%`，两条输出 hash 均漂移，未执行 repeat。由此仅能排除 metadata H2D blocking 作为充分/唯一修复；此前 raw state-copy 前后同步也失败，copy 的 index、地址、alias/覆盖和 recurrent state divergence 仍未定论。
- [路由与同步候选排除审计](blocks/eb9581c6d673c2442b6b677ff7aa226fd2b0ee1cd8dcb39da24c6af199256b29.md): DP2 路由审计显示只有 engine 2 计数变化，engine 0/1/3 保持为 0；四 rank 的 `bad=0`、`naninf=0`、leader PID=PGID 存活且 `health=200`。数据集、131072×2/1024×2、prompt hash、路由和源码均匹配，结果文件 SHA256 为 `65cce510bf08925d97c149b59eb84f382010c4ff92b5880a2f75d03bf10dc2cb`。记录认为 raw state-copy stream 同步和 late metadata H2D blocking 都不是单点正确修复；纯 `d37` 的高 prefix-hit 重放曾出现采信率由 `45.7612%` 降至 `29.0786%`、控制标记污染，但三轮均未观察到 literal `None`。
- [语义长上下文候选与测试设计](blocks/9a10414e446ce38d440b2a32a47f494fc95cb5ddd3ac8cd6f84537d09537bdc7.md): 模型卡列出的 GSM8K、MATH500、HumanEval、MBPP、MT-Bench、AIME26 及截断至 4096 token 的 OpenPerfectBlend 都没有可直接使用的自然 128K 样本；InfiniteBench `longbook_choice_eng` 被提出为可裁成 131072 token 且问题可判定的候选，但尚未形成验证结果。AIME26 更适合将多条自然结束的 `thinking_effort=max` 正确轨迹按完整 chat 轮次拼成 128K 语义前缀，不能用 `ignore_eos` 强行延长；生成源可采用已验证的 0.23.0 GPQA 记录，当前 `d37` 只作被测对象。已有纯 `d37` bounded r1/r2/r3 证据显示三轮未出现 `None`，但高命中重放稳定出现坏输出、内部指令/`<|close|>`/`<|sep|>` 控制标记和 `29.0786%` 采信率。
- [非 QuaRot 权重下 vLLM 测试进行中](blocks/c536526c6129a817db48497d3fb0b49c55bc5e01cd9194f96ce6c2e632863656.md): 测试矩阵被确定为 vLLM（PR #13277 pure head `d37a76b431…`）与 SGLang 镜像各 6 组：原始 GSM8K 32 题、8K/1K 构造、128K/1K prefix-cache 构造，分别比较单并发与多并发；共同使用 `0726-Kimi-K3-w4a8-int-moe` 非 QuaRot target 和最新 GQA DSpark draft。vLLM GSM8K matched 已完成：c1 为 32/32、采信率 `55.8836%`，c16 为 31/32、`55.2257%`，两边 `None=0`，但 7/32 文本 hash 和 17/32 token 序列变化，说明有轻微并发扰动而非已证实的大幅普遍坍塌。exact 8K c1 曾因缺失 helper 导致结果不纳入统计，失败证据保留；全新目录正式重跑正在进行，已完成 7/16，均 HTTP 200，完整输出与 raw metrics 正在落盘。
- [非旋转与 QuaRot 的 vLLM/SGLang 长序列对照及后续边界实验设计](blocks/b77d15b1d7897f5d52bddf7518e39594c3d527f59cfef295cbccd27fa255526d.md): 已报告非旋转权重下 vLLM 在 128K prefix-on、c2 出现两条 1024 个 `!` 的异常，而 SGLang c2 正常；但此前 vLLM 与 SGLang 的 128K 输入 token ID/hash 并未完全对齐，跨后端绝对值只能作参考。随后将主要嫌疑收敛为长序列、prefix reuse 与并发状态复用/异步拷贝交互，并提出非旋转 8K prefix on/off、c1/c16 的四格对照及逐步缩小 dump 条件。
- [QuaRot 六组结果显示长输出退化与共同的 128K 并发崩坏](blocks/ff2d5f86919d3cdfe86a663b0455a27db3e757436f217ebc7c347afed816d91d.md): QuaRot GSM8K 接近非旋转 vLLM，但 8K/1K 采信率仅 30.86%/33.35%、平均接收长度 3.160/3.334，并出现 `None` 语义词及控制/内部指令续写；这些请求虽 HTTP 200 且长度完整，不能视为成功。QuaRot 128K c1 为 42.97%/4.008，c2 与非旋转 vLLM 一样变成全 `!`，表面 100%/AL 8.0 是坏状态伪高值；SGLang 128K c2 正常，但其 tokenizer 输入 hash 与 vLLM 不同，因此严格归因仍限于 QuaRot 与非旋转 vLLM 的同 token 对比。
- [Align prefix 命中边界与自然 896-token 最小复现](blocks/73695d553189d56d0d1eeb233e0f533847f382a63a34f6c3db9f3fcf3fca2bef.md): 非旋转 prefix-on 的 `8064+128` 与 `8064+384+128` 两组均在 c1/c16 复现 7680 或 8064 cached tokens、全 `!`、100% 采信和 AL=8，说明并发不是必要条件，也不能归因于是否多命中一个完整 block；进一步的实际命中 384、896-token case 也复现全 `!`。为排除重复拼接语义，改用一条固定 revision 的天然数学推导记录，精确 896=768+128；同 artifact 的 prefix-off 已正常回答 `17/21`、cached=0、采信率 63.53%、AL 5.45，而 prefix-on 384 命中对照尚未给出最终结果。
- [Synthetic 与自然 prompt 的稳定差异及 DFX dump 方案](blocks/6c65cdfd67e69e8108ad9b7ccbdfcbae852b09816f8fd6b3e8558b6b8e46e88e.md): 同为 896=768+128、实际命中 384 的条件下，Synthetic GSM8K prompt 在 prefix-on 服务中稳定输出 1024 个 `!`，而自然数学 prompt 正常；因此 prefix hit 不是充分条件，prompt 数据或边界状态可能参与触发。当前更合理的待验证方向是 recurrent state/KDA 恢复、block/slot 映射、异步 state copy 或数值/缓冲区污染；连续 token 0 不能单独证明存在 NaN/Inf。已从纯 `d37a76b431` 基线隔离出不改 ModelRunner 的 DFX，准备比较 prefix-on `computed=384/scheduled=512` 与 prefix-off `computed=0/scheduled=896` 的映射、copy metadata、KV/state、逐层中间结果和 logits。
- [DFX 观测改变复现结果，竞态嫌疑上升](blocks/f14fd1156c78b5e3fef42f7a946f17b05db519408105b6664993a8a3ba888a94.md): 四机 DFX 服务已完成 64-rank 初始化、351 个 target shard、KV/profile 和 graph capture，health=200，KV capacity 为 343,109 tokens；启动阶段未见 traceback、OOM 或 DFX 异常。对 SHA=`7c1217…b7d` 的 896-token Synthetic GSM8K artifact，目标请求准确命中 `computed=384/scheduled=512` 并写出约 1.5 MB manifest 和 330 MB tensor，但带 DFX 的 4/4 请求均未输出 `!`，与纯 `d37` 基线此前 4/4 全 `!` 不同。由于只有首请求 dump/synchronize，观测本身可能改变异步时序，现阶段只能提高竞态/异步 copy 嫌疑，不能宣布数学 NaN 或某个 block/slot 非法已被证实；prefix-off 对照及 dump 内容核查仍未完成。
- [Forward-only 多 shape DFX 与 golden 排查](blocks/f85999a142224fb7650a93cd698b5a6c9b0be276fd8f7a495ec68df6f029758e.md): prefix-off 复现确认未进入 `mamba_cache_mode == "align"` 的 `preprocess_mamba`，因此改为仅在 target model forward 中按真实 shape 采集，并过滤 `in_profile_run`、ACLGraph `capturing` 和 dummy/profile forward。384、512 的 dump 全部 finite，slot mapping 512/512 匹配，前三个共同 MLA block 及 69 层 KDA initial state 逐 bit 一致；shape=16 的编译/tracing 重入混合记录被判定为不可直接使用。该批数据可作为正常时序 golden、排除稳定的 mapping/KV/state 错误，但不足以定位只在未观测路径出现的竞态，因此停止扩大 dump。
- [Mamba metadata H2D 生命周期修复及残留并发踩踏](blocks/df132e4c5adfe7a3c29fa9d750b4c0050c9ba7586c0c7994b74b6f4c01901231.md): 正向链路发现 `preprocess_mamba` 填充的三张 pinned CPU metadata 表位于 `synchronize_input_prep()` 保护区内，但 `copy_to_gpu(non_blocking=True)` 在保护区退出后才执行，过早 event 允许下一轮重写源 buffer；这可解释 prefix cache 命中时才触发。最小提交 `2a827f021` 将 metadata H2D 纳入保护区、保留 KV load 后 raw state copy，3/3 单测和 NPU smoke 通过；896-token case 三轮 12/12、128K 首 token 2/2 正常，但不能视为所有问题已解决。干净 DP1/DP2 的 128K 单/双并发采信率约 63.47%/63.66%，而已使用 DP0 或 DP3 在长 decode 后的 c2 wave 可稳定出现全 `!`、随后 c1 恢复，表明仍有请求历史相关的当场并发踩踏；随后定位到 0.23.0 的 #12038 等价 guard 已在 0.26，而 #12189 的异步 `block_table` 源缓冲区固化修复似乎未带入当前分支，尚待验证。
- [非旋转权重 128K 单多并发与满 KV 修复核对](blocks/82fa4aaf284882db08ef36b38bb30e6b7267d25b8b110c484ffad67681aeebb6.md): 在修复后的干净 engine 上，非旋转权重 128K/1K 的 c1-first 与 c2-first 采信率分别为 63.471% 和 63.660%，没有证明单并发或双并发本身导致下降；但同一 engine 经历长前缀复用后，c2 可出现两条全 `!`，且普通 `/reset_prefix_cache` 不能恢复当时状态。进一步在 DP3 复现了“c1/1K 正常→c2 首 token 全 `!`→c1 恢复”，因此更准确是高命中并发 wave 的瞬时状态/缓冲区踩踏，而非已证实的永久污染。对历史修复的核对认为 #12038 的 hybrid hit 防护在 0.26 已有等价逻辑；#12189 关于异步 CPU→NPU `block_table` 源缓冲区被重写的修复只确认进入 0.23.0，当前 0.26 代码仍直接 non-blocking `copy_to_gpu()`，是否为剩余根因尚未验证。
- [同步 block_table 实验未改变 c2 精度异常](blocks/72151e5cbbe46335ad35a500bf1b4169fc9313b6e6e4f3137c5813618546a303.md): 将 `commit_block_table()` 改为同步 CPU→NPU 拷贝后，c1/1K 的采信率 `0.6347101175` 和平均接收长度 `5.4429708223` 与旧版本一致，但 c2/1-token 仍稳定输出 `!`，后置 c1 恢复为 `" He"`、再 c2 又变为 `!`。这排除了当前现象由该类 block-table H2D 覆盖竞态单独造成的解释，但不能据此关闭其他 metadata 或状态生命周期问题；实验提交为 `3a2cae19b0f638...`，此前归档中曾出现错误展开的 full SHA，已记录为 provenance 笔误。#13277 已推到线性 head `e560c8031`，净 diff 保持不含 MLA preprocess/ModelRunner 改动；CI 后续虽通过部分阶段，但用户要求暂不继续跟进。
- [定位到 full-attention group 的 cache ownership 踩踏候选](blocks/66bd5888d3bbe235482e5fa31e6d59095c3f6659ad945aac44c45c43bb12c1d5.md): 建议用 `num_gpu_blocks` 限制低可用 KV，固定 token、DP rank 和请求顺序，以短 prompt 放大 prefix cache、Mamba state、异步调度和物理 block 复用问题；该场景仍只是辅助复现器。纯 CPU 分析在 `fe784ff22` 的真实 20-group K3 拓扑中发现，reconciled hit 只有 1 block 时，DSpark 的 group 5 仍保留 4 个共享 block，其中 3 个越过合法边界并被 `allocate_slots()` 绑定给新请求；仅裁剪所有 full-attention group 后，20/20 group 满足 ownership invariant。该结果支持一个确定存在的 core ownership bug，但其与 NPU `!` 现象的因果关系仍需低 KV A/B 验证；上游对应 PR #50457 仍为 OPEN，实验修改尚未提交。
- [低 KV 修复实验服务仍在启动阶段](blocks/3537cc16109e50db35ee2fc728d5b88aebae015ad50b84df39063ebeaaac1471.md): 采用干净 #13277 的异步 block-table copy，加上 `kv_cache_coordinator.py` 的最小 core 修复，低 KV 服务先后因 draft 上限 4096 与 target 2048 不一致、以及 FULL_DECODE_ONLY 下 capture size `[4]` 不是 TP16 倍数而在模型加载前退出；这些是配置门禁失败，不是修复失败，失败日志均保留。调整为 target/draft 2048、capture `[16]`、`max_num_seqs=16` 和 `num_gpu_blocks_override=512` 的 `r3` 后，四个 rank 已拉齐 16/16 TP workers，进入 HCCL rendezvous 和量化模型初始化，当前无 traceback/error。服务尚未报告 health、请求结果或低 KV 复现结论；日志中的 `v0.23.0` 仅是节点安装包元数据差异，实际 core 源文件哈希一致。
- [Full-KV c2 仍失败，问题转向长序列状态污染](blocks/b06de90b8ea7a728aa31e83b698a6d1b610fe2c77ade8dc510ca3f3340cada75.md): 低 KV 的 64 条历史压力及 canonical c1/c2 均通过，但 full-KV 的固定 128K 并发 c2 仍复现两条 1024 个 `!`，而 c1 正常；因此“截断所有 FullAttention group”的修复在该拓扑上实际是 no-op，不能归因于本次精度问题。审计显示普通 serving 不经过相关 hybrid finder，剩余候选转向 Mamba align 跨 384-token 边界的 state ownership，以及 prefix-cache block/state 复用；PCP 仅在当前 Kimi/MRV1 路径固定为 `pcp_world_size=1`，仓库整体仍保留通用 PCP 代码。
- [长序列 DFX 探针反复校正，最终确认需延后抓取](blocks/552c0f93eb67eaaff0fef1c6fd2ff40fab25cd098763acb75c136c98fd94e2b1.md): 固定 128K 单 token c1/c2 dump 的多轮探针先后受到环境变量未传播、scheduler `computed/scheduled` 假设错误、触发入口过窄及混合 CPU/NPU `torch.stack` 落盘异常影响；这些都是诊断代码问题，不能当作模型失败。通过改用 CPU prompt 长度、修复逐项搬运并将触发延后到正式请求第 5/6 个 prefill，最终获得可用异常对照：c2 仍输出 `!`，且 Mamba cache 的 `source_before_copy` 已含 NaN，随后传播到 KDA/MLA 和 logits。
- [锁定 KV block 未清零，zeroer 暴露异构 page-size 门禁](blocks/86c0ba44a6afa4d8b1841cfb5391f2d20aa43b4f0dcbce121260058a54a9e925.md): 延后 dump 的复现表明第一轮 c2 可正常、后续清 cache 后的长序列请求才稳定退化，污染在 reset 后的 warmup 已出现；layer 0–2 KDA 仍 finite，layer 3 MLA attention output 首次出现 NaN，支持复用的 full-attention KV block 未清零/错误复用这一方向，但尚未完成修复验证。尝试放开 Ascend `worker.py` 中仅限 `eagle3` 的 zeroer 初始化后，启动暴露 `Non-uniform page sizes: 98304 vs 12288`，说明 Kimi 的异构 MLA page size 还需要分组清零；当前 r8 候选同时关注 page-size 分组和 Ascend MLA spec 是否会上报待清零 block，ModelRunner 仍未修改。
- [异构 MLA zeroer 后复现并定位 kv_c 污染](blocks/5e6bcec0290a1f51d8006cc3570b973b2170df1b9b5fa4c532499b0df76dc7db.md): 统一 hybrid zeroer、按 98304/12288 page size 分组清零，并补齐 AscendMLAAttentionSpec 的新 block ID 上报后，r8 仍在第二轮 c2 稳定复现两条 `!`；延迟到首次消费后的 dump 也未改变结果，未支持普通异步拷贝竞态假设。正常/异常请求的 block table、slot mapping 和 context_lens 一致，失败链路中 layer 3 当前 q/k/v 与 FIA 输出有限，但首个 context chunk 读取的 compressed-KV `kv_c` 已含 NaN，而同位置 `k_pe` 有限，提示坏值更早来自写入或 hybrid cache 覆盖。r9 曾因部署包缺失约 67 MB 的 `custom_transformer` vendor 目录、找不到 `aclnnAddRmsNormBias` 而启动失败；补齐并重试后精确复现，r10 已准备进一步检查 129792-token prefill 的 `kv_c` 写后读回、block NaN 分布及 cache storage 区间，但尚未产生新结论。
- [r10–r12：确认首坏边界并排除同步假设](blocks/c336ae1a910386c4865ae54a26f0dfb0209c258e845f4bb921304e13ccb48751.md): r10–r12 在 64-rank HCCL、KV 初始化和 size-16 graph capture 后，按 c1→c2→marker→c2 复现了两个 `!`；错误客户端曾污染一次历史，随后用 formal SHA `087744…`/`f78cbc…` 的正确包装器重跑。dump 与 KDA 状态摘要未消除现象：match 8 的 layer 3 之后出现局部 NaN，layer 28 KDA 的 `recurrent_state_after_write` 才观察到下游污染；已有证据支持继续检查 MLA/KDA 共享 backing storage，但尚未形成最终修复验证。
- [r13–r15：zeroer 与 draft 假设的修正](blocks/3936416417dc7d56cd7bfd50d0b86a61bd67b2fadc98a02b510f20c8578824fb.md): r13 的单页哨兵显示目标 page 在 match 7 `post_logits` 仍 finite，而 match 8 layer 0 attention 前已有 448 个 NaN，写坏发生在两个 forward 之间；因此 zeroer、普通 attention/MLP 首次写坏以及“固定 logical block 必然对应目标页”的判断均被削弱或排除。r14 重新分配后发现 DSpark draft 未执行、spec `drafts=0`，旧 page 监控也保持有限；随后设计全链路一次性 dump，r15 因二维 `cache_indices` 触发 ACL `index_select` 错误而失败，属于诊断探针启动问题。
- [r16：全链路 dump 与物理别名证据](blocks/a1e564f007131216b7e45a34a40cf5581811fd6752f4a3a9f2dcaf45434130ac.md): r16 修复了 dummy/图编译路径中的二维 `index_select`，成功通过 64-rank 启动、graph capture 和健康检查；c1/c2 历史 SHA、命中率与输出均和基线一致，最终 c2 仍输出 `!`，所以 dump 未治好现象。全链路记录显示 match 8 layer 23 读取历史 `loaded_kv_c` 时已有 448 个 NaN，涉及 group0 block `5224`/page `15672`，并观察到 draft layer94 的 group5 block `1969` 与其物理地址重合；这是强正向证据，指向不同 KV group 的紧凑 stride 与共享 backing storage 发生地址别名，但仍需修复后的独立验证。材料还指出当前 Ascend 分配链路没有真正支持首轴非连续 view，单改 `page_size_padded` 不够，需让实际物理布局包含 padding 或隔离物理池。
- [从事后拆分转向统一布局规划](blocks/1d5205232d3d515e87c4eb540df52f529f85bc8b5df166408959ef244b346170.md): 讨论先拆分 DSpark draft backing 的验证性方案，随后修正为在首次 KV layout planning 时同时纳入 target、Mamba 与 DSpark spec，并按物理布局兼容性分 bucket。隔离测试曾报告 19 passed，r16 拓扑模拟估算由 5689 降至约 4735 blocks，但服务未重启，因此线上问题尚未验证修复。进一步审查认为 K3 特判过于定制化；DSV4 通过既有 MTP 独立 KV pool 规避了同类风险，而长期方向应是通用 physical-layout planner 与 stride-aware consumer。
- [padding 契约与 DSV4 式临时隔离](blocks/b1560b9e2b2df3a6c425501750be913e7d3b1cd4f67c3dd208b56dba9654e807.md): 材料区分了只计入 allocation 的 `page_size_padded=488448` 与 consumer 实际按 padded `block_stride` 寻址；当前 hybrid attention 的 `.view()` 仍近似使用 draft 的 98304B 紧凑步长，故单改 planner padding 不能避免 alias。引用的上游案例包括 packed layout、runtime block stride 与 FlashInfer consumer 不匹配导致 GSM8K 崩溃，且 vLLM-Ascend 正在推进 `offset/block_stride` 对齐；这些外部结论在本材料中作为报告内容保留，未在此批次独立核验。当前决定暂按 DSV4 MTP 方式让 5 个 DSpark draft layer 各自使用连续 backing，并在代码中声明待 FIA 支持首轴非连续且 Ascend allocation/reshape 接通社区 stride 方案后再迁移；目标 UT 报告 19/19，但仍未提交、推送或完成带请求历史的 128K 运行时回归。
- [GQA/MLA 条件收敛与 PR 提交重组](blocks/8b1a513e5d4e1768e9cd14423f41fe08ed5bafd582ef9f24f981c5719df753ea.md): 最初判断将 backing collision 归因于 GQA DSpark，随后将条件收敛为限定 Kimi K3 + DSpark，并比较 target/draft 的 `unpadded_page_size_bytes`；GQA 触发独立 backing，MLA 同构布局走原生共享路径。一次长历史复现曾显示修复有效并将 PR 从 7 个提交整理为 6 个，但后续 A3 压测重新出现 1024 个 `!`，证明此前验证不能视为最终成功。PR 的 6 个提交按反合、`fused_norm_gate`、MLA、QuaRot、异步 metadata、GQA KV 隔离分类，代码 tree 保持不变；CI 中的 PR create action 失败被报告为 runner 的 GraphQL 超时。
- [A3/A5 长历史触发 KV 污染复现](blocks/ebd5a1a6c6df2adc98e85372bf564d7d324bca23ce4839352a7e2aa42a14d23c.md): 在 A3 上连续执行 128K/1K、c2 压力序列后，第二次 prefix warmup 及后续 10 条请求均变成 1024 个 `!`；KV 容量约 285.6K tokens、并发约 2.12x，四节点 worker 和 health 均正常，因此未能归因于容量过载或进程错误。全新服务上连续三次相同 prefix 可以正常，但经过长序列并发与长 decode 后，下一次高命中读取会暴露 token 0；`reset_prefix_cache` 更像暴露/切换观察点，而非根因。A5 提供的短 prompt `who are you` 二次复现未能在冷服务上重现，说明现象依赖既有 KV 历史或容量状态，仍不能宣称两场景已完全等价。
- [Dump 首轮未捕获坏请求，需修正选择器](blocks/b2213f002047e9bc4cb818b7bce835018c3a47d36750f3ff2460a340b036ddf5.md): 短 prompt 冷服务测试两次均正常；随后建立长请求历史时，正式 128K c2 请求出现 1024 个 `!`、100% 接受率和平均接受长度 8，且第二次 129792-token warmup 也从 `does` 变为 `!`，支持“前序 KV 写坏、后续 prefix read 受害”的方向。现有 dump 只抓到了第一条正常请求的前三个 7680-token prefill chunk：主链和 `raw_target_logits` 均 finite，Mamba 的 `destination_before_copy`、`initial_state_before_clear` 等 NaN 与正常缓冲区现象同时存在，不能据此归因 Mamba。当前 selector 按 chunk 重复命中同一 request，尚未抓到真正坏请求；计划改为按 request ID 计数，目标定位 match17（正常末 chunk）、match18（首个坏请求）和 match19（后续坏请求），该定位结果尚未完成。

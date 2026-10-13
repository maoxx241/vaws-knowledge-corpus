---
block_id: bc6f92adf83b5e28f737eedcd52b0eb46dd50484f638e4ebfab1fff6fdaa5622
schema: mindie-material-block/1
source_range:
  end: 32665112
  part: 2
  start: 15887542
---

### assistant:commentary
我继续盯 r8 最终候选：当前代码已经补上同一步 COW 目标物理块的防命中，以及 recurrent state 精确边界校验。接下来并行做两件事：等待四机服务完成启动，同时用已校验的 ARM64 gitleaks 对“本次差异本身”做增量扫描；服务就绪后立即跑同一 128K 高命中条件的多次复现，稳定性不过关就不会进入提 PR 阶段。

### assistant:commentary
增量密钥扫描已通过：本次约 206 KB 的 tracked diff 加全部未跟踪候选文件没有发现泄漏。刚才启动状态查询只是本地 shell 对正则里的 `|` 误解析，并非四机服务报错；我已改成逐条只读查询继续检查。

### assistant:commentary
r8 四个父进程与 64 个 worker 都在各自已核验的 PGID 下，日志没有 Traceback/ERROR；目前最慢的 rank 3 已进入 OOT 编译阶段，18110 尚未监听，属于正常冷启动进程。增量 gitleaks、23 个最新 CPU 定向用例和此前 NPU KDA 迁移用例均已通过；我现在按 20 秒间隔守候健康端点，端口一开就先核对模型身份与四 rank 错误日志。

### assistant:commentary
健康端点仍未开放，进程没有退出。我会把等待与阶段判断分开：继续守候端口，同时抽查 rank 0/rank 3 最新日志，确认是在加载/编译/绑核中的哪一步，避免把“活着”误当成“已可服务”。

### assistant:commentary
启动仍在权重装载阶段：rank 0 当前 146/345 shard（42%），rank 3 已完成页缓存预取，四机都没有异常退出。等待期间我先固定最终结果目录，并记录 benchmark 脚本、GSM8K 数据集和两套权重标记文件的校验值；后续每个 case 都会把 prompt token 数与 hash 回读，防止“同名 128K”实际输入不一致。

### assistant:commentary
输入证据已固定：benchmark 脚本 SHA256 为 `68164e1d…`，GSM8K 数据集为 `3730d312…`；主 QuaRot 与 GQA draft 的 `config.json` 分别为 `7430bb3f…`、`410dd228…`。服务仍未监听，我继续按实际 shard 进度而不是只看健康码判断启动。

### assistant:commentary
主模型 shard 已接近/完成装载，日志已进入 DSpark GQA draft 装载，并明确打印了 QuaRot 专用路径：保留 draft 自有、反旋转的 `lm_head`，没有错误地复用已旋转 target `lm_head`。这确认 #13327/#13407 的 QuaRot 兼容逻辑确实在最终运行时生效；接下来还需等 draft、图捕获和绑核完成。

### assistant:commentary
四个 rank 已越过主权重与 draft 权重装载，rank 0/1 已完成首次 profiling/warmup，并直接复用已验证的 AOT 编译缓存；当前在后续内存初始化/图捕获阶段。端口还没开，日志仍无错误，我继续守候。

### assistant:commentary
四机现在都在同一个 `FULL_DECODE_ONLY` 图捕获屏障；`No available shared memory ... 60 seconds` 是编译/图捕获期间的提示，不是错误，但我会用日志时间是否持续前进来区分慢与真挂。父/子进程均保持运行，暂不做任何清理或重启。

### assistant:commentary
我对照了保留的 r7 成功启动日志：同一张 FULL decode 图上次耗时 197 秒，并连续出现 3 次相同的 60 秒等待提示后正常完成。r8 当前形态一致，已略慢于上次但仍在合理范围；若日志停止推进超过既有基线，我会再查 NPU/通信状态，而不是贸然重启。

### assistant:commentary
r8 已在 01:14:24 UTC 返回 `health=200`。现在进入服务级验收：先核对 `/v1/models` 的精确 served name、权重根目录与最大长度，再逐 rank 扫错误日志；这些都通过后才发第一条 8K/1K 实请求。

### assistant:commentary
服务身份核对通过：唯一模型是 `kimi-k3-quarot-dspark-gqa-cow-block-guard`，root 精确指向 `[REDACTED_SECRET]`，`max_model_len=135000`；四个 rank 的错误扫描均为空。现在跑 DP0 的精确 8192-token / 1024-output 单请求基线，预期 prompt SHA 需回到 `4825bf16…`。

### assistant:commentary
8K 基线完全复现：8192 输入、prompt SHA `4825bf16…`、质量通过，输出 SHA `8987d092…`，采信 `861/1134 = 75.9259%`，与 r7 正常基线逐项一致。现在转到从未接收过请求的 DP1，先跑 `--skip-prefix-warmup` 的 128K/1K 冷请求，建立不受 prefix cache 影响的 target/draft 正常值。

### assistant:commentary
128K 冷请求仍在执行，客户端会等完整 1024 token 后一次性汇总，所以暂无中间 JSON；DP1 服务进程仍在，未出现请求错误。这个 case 的关键对照是 `prefix_hit_rate=0` 下的输出/采信，完成后同一 prompt 再跑 warm 请求，只有两者一致才说明缓存恢复状态正确。

### assistant:commentary
128K 冷基线也逐项复现：131072 输入、prompt SHA `3ffdb034…`、命中率 0，输出 SHA `fd1aee0b…`，采信 `784/1680 = 46.6667%`，质量通过。现在在同一 DP1、同一 prompt 上跑 warm 版本；目标是约 99.609% 命中时仍保持完全相同的输出 SHA 与 `784/1680`。

### assistant:commentary
关键单请求对照已通过：warm 命中 `130560/131072 = 99.609375%`，输出文本/token SHA 与冷请求完全相同，采信仍精确为 `784/1680 = 46.6667%`，TTFT 从 52.07 秒降到 0.99 秒。现在进入真正触发过故障的并发路径：在全新的 DP2 上跑 128K/1K、并发 2，然后连续重复同一波至少 4 次，专门验证 COW 同步与同一步 alias 防护。

### assistant:commentary
DP2 的首轮并发 2 仍在生成，两条请求都共享 129792-token 前缀，这是最容易在同一 scheduler step 形成 partial-hash/COW alias 的组合。当前没有客户端超时或服务错误；结果返回后我会逐请求比较两个输出 SHA、质量标记和最终 SSE 采信计数，而不是只看汇总比例。

### assistant:commentary
DP2 首轮并发结果正常且与已知基线一致：两条输出 SHA 为 `fd1aee0…` / `7d50047e…`，质量均通过，采信 `1560/3409 = 45.7612%`，命中 98.7305%。下面开始完全相同的 r2；此时两条完整 prompt 都已缓存，预期命中升到 99.609%，而输出 SHA 必须保持不变。

### assistant:commentary
r2 仍在生成，暂无错误或提前结束。高命中并发下每个 SSE chunk 接受的 token 会变少，因此同样 1024 输出可能比首轮更慢；这正是为什么我保留请求级最终计数，不能把中间 `/server_info` 或瞬时采信率当结论。

### assistant:commentary
r8 没通过关键门槛：第二轮 99.609% 命中时，两条输出 SHA 已漂移，其中一条以 `<|close|>` 开头，采信变为 `1388/4627 = 29.9978%`；这与 r7 后续曾出现的坏状态完全同型。说明 COW 物理块 guard 只消除了“未落盘目标被同一步命中”的一类竞态，真正的 recurrent-state 边界/所有权问题还在。我暂停扩展矩阵，保留服务与证据，转入上游 MambaManager 和 scheduler 调用链的逐步审计。

### assistant:commentary
调用链显示一个更具体的风险：Mamba `align` 会在调度时把“可命中的 recurrent 快照”挂到物理块上，而同一请求后续 decode 仍可能迁移/复用 running-state 块；坏状态只在完整 prompt 被生成过 1024 token 后复用时出现，而独立 shared-prefix warmup 的 98.73% 命中正常。下一步我会同时查当前上游是否已有针对“cached recurrent snapshot 被后续 decode 覆写/复用”的修复，并在本地构造块生命周期测试；若没有，就用运行时块 ID 追踪确认所有权。

### user
进展如何，我看好像并没有实质性的进展


### assistant:commentary
你判断得对：目前还没有达到“可交付 PR”的实质完成状态，所以我没有拿一个未解决的版本去新建 PR。

已经确认的实质结果是：

- #13327、#13407 已完整整合，QuaRot 权重已在四机 TP16×DP4×EP64 跑通；8K/1K 与 128K/1K 冷请求都正常。
- 128K 单请求从 0% 到 99.609% 前缀命中时，输出 SHA 和采信 `784/1680=46.67%` 完全一致。
- 但并发 2 的决定性复现仍失败：首轮正常，第二轮 99.609% 命中后输出漂移，其中一条以 `<|close|>` 开头，采信从 `45.76%` 变成 `29.9978%`。这与之前 0%/100% 跳变属于同一类 recurrent-state 错配，只是没有再退化成全 `!`。
- 前两版修复排除了 partial-hash alias 和同一步 COW 未落盘命中，但没有解决根因。现在已把范围收敛到 vLLM 的 `Mamba align + speculative decode + async scheduling`：完整 prompt 的 recurrent 快照在后续 decode 中存在所有权/写回错位，而不是旋转量化后的 Attention linear 本身算坏。

我接下来不再盲跑矩阵，而是直接给物理 block、running-state index、缓存 hash 和 KDA state checksum 加生命周期追踪，确认是哪一步把已发布快照覆盖，再做快照 COW/提交时机修复。关键复现不过，我不会建 PR。

### assistant:commentary
已经开始做物理证据追踪，不是继续试参数：调度侧会记录 120K 以后每次 Mamba block 的逻辑索引、物理 ID、refcount、hash 状态与 COW；worker 侧在每个 384-token 边界记录 running-state 源/目标索引，并对首个 KDA 层的 conv/recurrent state 做 checksum。这样下一轮可以直接指出“哪个 hash 指向的物理状态在第几步改变”，而不是再靠输出猜根因。

### assistant:commentary
追踪版已部署到四机独立目录，原 r8 代码与失败日志没有覆盖。为了让物理 block ID 从干净缓存池开始可解释，我现在会按已核验的四个 PGID 精确停止 r8（不用任何 broad `pkill`），保留其日志和结果，然后用新端口 18111 启动 r9 追踪服务。

### assistant:commentary
你说得对：目前还没有形成可提交的修复，现有修改在第二轮 128K×2 并发复测时仍会从约 46% 掉到 30%，所以不能算实质完成。

已经排除的是 QuaRot Attention 数学本身；问题缩小到高命中前缀下 Mamba/KDA 状态块的物理复用或写回。现在我停止扩测试矩阵，直接做带物理 block ID、引用计数和状态校验和的四机复现；拿到第一次错误覆盖的位置后就改根因，而不是继续堆保护条件。先确认旧服务完全退出，再启动这轮定点追踪。

### assistant:commentary
旧服务的四个精确 PGID 已全部退出，18110 也已实测拒绝连接；四台机器上的追踪源码哈希一致，没有 AppleDouble 污染。接下来只启动 18111 的诊断实例，并保留全量日志。

### assistant:commentary
启动前校验拦住了一个部署问题：诊断源码目录缺了两条指向已验证原生库的符号链接，因此四机实际没有启动任何服务。这不是模型故障；我正在按上一轮已验证目标补齐链接并重新核对，避免带不完整运行时继续浪费启动时间。

### assistant:commentary
启动日志又暴露出一个必须先核对的差异：DP2 报出的 vLLM 版本是 `v0.23.0`，另外三机是 `v0.1.dev1+gfe784ff22`。这可能只是包元数据，也可能解释为什么此前恰好在 DP2 的并发复测失败；我会立刻对四机实际导入路径和关键调度源码做哈希比对，同时服务继续加载。

### assistant:commentary
版本差异已排除为源码漂移：四机的 Mamba manager、worker 和 async scheduler 关键文件哈希完全一致，DP2 只是已安装 `dist-info` 的版本字符串不同。

代码审计同时确认，最可疑的链路是“部分前缀命中后将 Mamba 状态快照迁移到 CoW block，再由后续请求恢复”。这正好只在第二轮高命中时生效，也能解释为何冷跑和单并发都正常。当前追踪会直接验证 CoW 源/目的 block 的状态校验和是否一致。

### assistant:commentary
我找到了与本问题高度同构的上游缺陷族：`MTP/DSpark + hybrid Mamba + prefix cache` 会在冷并发写入或多轮读取后静默污染输出；上游已有针对通用 CUDA 路径的修复和回归测试，但当前 Ascend 路径仍有自己的 manager/worker 覆盖层。也就是说，这不是 QuaRot 权重本身的随机性，而是该权重把状态恢复错误放大成了采信率跳变。现在正在确认 Ascend 覆盖层具体偏离在哪一个物理状态迁移点。

### assistant:commentary
诊断服务已启动完成，四机模型、DSpark GQA 和 QuaRot 反旋转路径均正常加载，`/health=200`。现在开始同一 DP2 上的 128K×2 两轮复现；本轮日志会把每个请求的逻辑位置、物理 block、CoW 对和状态校验和一一对应起来。

### assistant:commentary
你判断得对：到目前为止还不能算“问题已解决”。现在已有一条关键证据——同一服务第二轮 128K 会从健康输出退化，而且刚加的 Mamba 生命周期埋点完全没有进入，说明此前修补的管理路径可能根本不是运行时实际使用的路径。我先把这个绑定/分发链路查实，再跑第二轮复现；不会把“服务启动成功”当成实质进展。

### assistant:commentary
埋点模块确实被导入了，但没有任何生命周期调用记录；这把范围进一步收窄到“类已加载、实际 coordinator/manager 实例却未走该实现”，而不是部署错源码。我现在保持同一服务和缓存状态跑第二次 128K，同时核对 coordinator 创建出的真实 manager 类型。

### assistant:commentary
第二轮命令刚才没有真正发出：调试运行目录里只有 launcher，基准脚本仍在既有的精确基准目录。我正在从首轮结果元数据回读原始命令并原样重发，避免凭记忆改动 workload。服务与缓存状态未受影响。

### assistant:commentary
第二轮已经完成，结果是健康的：两条 128K/1K 输出均与已知健康基线一致，采信率 46.07%，prefix hit 99.61%，没有再出现 0%/100% 跳变。这说明 r9 至少把 r8 的“第二轮必坏”现象压住了，但一次通过还不够。我会在不重启、保留同一缓存池的条件下连续跑 5 轮，并逐轮比对输出哈希与采信率；同时把 manager 实例类型的启动证据补上。

### assistant:commentary
连续压测第 3 轮已经抓到复现：prefix hit 仍是 99.61%，但两条输出哈希同时偏离，采信率降到 27.41%。这确认“跳变”不是 QuaRot 权重本身随机，而是高命中复用后的状态污染。更关键的是，失败轮仍没有触发 Ascend Mamba manager 的任何埋点；当前最强根因是补丁导入顺序导致 `KVCacheManager` 继续持有 upstream coordinator 的旧函数引用，修复代码从未进入实际调度路径。我正在验证这个旧绑定并直接修它。

### assistant:commentary
已经确认普通启动序列里的 coordinator 函数绑定本身会被正确替换，旧函数引用不是最终根因。更准确的问题在 manager 注册表：vLLM 会在 Ascend 补丁生效前缓存 upstream `MambaManager`，现有代码只改模块属性，无法替换已登记的类；而 Ascend 的显式路由只覆盖部分 coordinator 路径。我将修复点下沉到 manager registry，使任何 coordinator 路径都强制得到 `AscendMambaManager`，并在下一轮启动时打印实际类名作为硬证据。

### assistant:commentary
我也发现刚才为读取 KV 配置调用的调试 RPC 返回了不可序列化的 `torch.dtype`，导致 DP0 的输出线程异常退出；健康检查仍为 200，但该服务已不适合作为后续证据。四个主进程及 PGID 已逐一核对为本次 r9 服务，我现在按这四个精确 PGID 停止它，保留全部日志和结果，然后部署 r10。

### assistant:commentary
r10 的后台启动在 launcher 预检阶段退出了，四机都没有产生 PID 或日志，也没有遗留服务进程；这不是模型启动失败。现在逐项回读预检条件，定位是哪一个部署一致性检查拦截，修正后再启动。


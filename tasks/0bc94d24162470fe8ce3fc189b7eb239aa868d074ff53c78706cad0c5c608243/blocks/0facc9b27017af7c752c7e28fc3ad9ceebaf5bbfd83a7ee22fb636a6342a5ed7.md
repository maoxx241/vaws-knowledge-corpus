---
block_id: 0facc9b27017af7c752c7e28fc3ad9ceebaf5bbfd83a7ee22fb636a6342a5ed7
schema: mindie-material-block/1
source_range:
  end: 237633933
  part: 2
  start: 224914120
---

### assistant:commentary
128K 高命中 c1 也已通过，命中 129408/131072=98.7305%，完整 1024 token，无空值/`!`/乱码。强制 `ignore_eos=True` 跑满 1K 后出现了重复 K3 控制标记，这属于越过正常 EOS 的压力输出；真实自然 GSM8K 使用正常 stop/EOS 时 32/32 且无质量违规。我会在结论里把这两类证据分开，不把强制续写当成正常语义输出。

### assistant:commentary
核心 DSpark 历史矩阵已跑完并通过。全 rank 日志目前 ACLGraph replay 各 1 次，4 份日志均 0 个 ERROR/CRITICAL、0 个 traceback/EngineDead/invalid-GM/HCCL timeout/NaN。为了让“大 PR 全面精度”不只停在文本，我再补一次服务器内发起的真实图片多模态请求和工具调用解析；这两项也是 K3 服务历史验收边界。

### assistant:commentary
多模态与工具调用也通过：图片请求正确识别 Grace Hopper 军装、美国国旗和蓝色背景；工具请求生成 `get_weather({"city":"Paris"})`。当前 health=200，四个 engine 均 running=0、waiting=0；高压过程中 DP0 累计发生 35 次预占/重算，但 c35/c64 全部请求及后继请求都成功，这是预期的容量路径而非精度失败。现在做最终日志与产物一致性审计。

### assistant:final_answer
四机全面回归通过，当前未复现 K3 历史上的精度污染、卡死、invalid-GM、NaN、空/None 输出等问题。

| 场景 | 结果 |
|---|---|
| 8K→1K c16 冷/热 | 32/32；热命中 93.75%，无 `!` 污染 |
| c35 两轮高压 | 70/70；冷 miss、热命中，跨高 state-index 正常 |
| 单 DP c64 超容量 | 64/64；发生预占重算但全部完成，后继四 rank 4/4 |
| 四 DP 镜像 c64 | 64/64 短答案正确 |
| 1-token、42-task 边界 | 两轮 14/14 |
| block_size+1，seq_len=17 | 两轮 32/32 |
| 128K→1K no-hit c2 | 2/2，hits=0，无连续 `!` |
| 128K 高命中 c1/c2 | 3/3，命中率 98.7305%，无 NaN/`!` |
| 自然 GSM8K c16 | 32/32 正确、32/32 可解析、0 质量违规 |
| 多模态、工具调用 | 2/2；图片描述正确，工具参数正确 |

共计 327 条正式验证请求，另有启动 smoke 和前缀 warmup。四份 rank 日志均满足：

- ACLGraph replay 已发生。
- `ERROR/CRITICAL=0`。
- 无 traceback、EngineDead、invalid-GM、HCCL timeout、NaN。
- 最终 health=200，四个 engine 均 running=0、waiting=0。

旧脚本报告的两次 15/16 是误判：完整 HTTP 200/1024-token 输出中出现了普通英文单词 `None`，不是空值、`None` 对象或 exact `None/null`。修正 sentinel 后两轮均为 16/16。

强制 `ignore_eos=True` 跑满 1K 时，个别输出在正常答案之后续写 K3 控制标记；正常 EOS/stop 的自然 GSM8K 为 32/32 且没有该现象，因此没有把强制越过 EOS 的文本当作正常语义精度失败。

验证产物保存在四机容器内：

`/tmp/pr14454-full-gqa-mixed-dc9e8375-prefix-on-r2/`

服务按工作区约束继续保留运行在 153–156，端口 18521。GPQA 仍按之前约定跳过，本轮没有修改或推送 PR 代码。



### user
当前这个方案下gqa和mla的dspark是不是都是4个group只需要build4次来着


### assistant:commentary
我先按当前实际运行的 `dc9e8375` 方案把“KV cache group 数量”和“metadata builder 调用次数”分开核对；这两个概念之前很容易混在一起。我会用 vLLM-Ascend 模型适配流程直接查当前构建路径，不按旧四平面方案的记忆数值回答。

### assistant:commentary
我会重点确认“4 个共分组、每步 build 4 次”是否成立，以及它是不是被误解成“GQA 4 个 + MLA 4 个”。同时会把旧方案里的物理四平面与 metadata group 明确区分开。

### assistant:commentary
从当前提交本身已经看到一个关键差异：它构造的是 4 个 KV cache group，但对应单测明确断言 metadata builder 是 5 个。接下来我再沿实际 runner 的 builder 创建与每步 `build()` 循环核一下，避免把“builder 实例数”和“每步调用次数”混为一谈。

### assistant:final_answer
不完全是。当前 `dc9e8375` 方案下：

- MLA DSpark：`1 个 MLA group + 3 个 Mamba group = 4 groups`，target runner 每步 `build()` 4 次。
- GQA DSpark：也是 4 groups，布局为 `[29, 23, 23, 23]`；但第一个 mixed group 内含 MLA 和 GQA 两种 builder，所以是 `2 + 3 = 5` 次 `build()`，不是 4 次。单测也明确断言了“4 groups、5 builders”：[test_prefix_cache_cp_patches.py](<redacted:local-path:2714eadc934d>:234)。
- 如果只算 DSpark proposer 自己的 draft metadata，两种 DSpark 都只有 1 个 draft attention group，因此各自只 build 1 次：[dspark_proposer.py](<redacted:local-path:d1733279811a>:171)、[llm_base_proposer.py](<redacted:local-path:002efe3ebffa>:2317)。

所以准确结论是：**都是 4 个 KV cache group；整体 metadata build 数 MLA=4、GQA=5；draft proposer 自身都是 1。**



### user
用当前的代码更新一下父pr以及子pr，我理解子pr可能需要更新一下或者拆分的更细一些？你可以判断一下保持每个pr的原子性


### assistant:commentary
我先把当前父 PR、所有子 PR、对应分支和 `dc9e8375` 相对现有提交栈的差异重新拉齐，再按“一个 PR 只改变一个可独立验证的契约”判断是否需要继续拆分。这里会优先保持已有 review 历史；只有确实跨越 scheduler grouping、worker 适配、DSpark proposer 三个独立边界时才新拆。

### assistant:commentary
实时状态确认了：现有 7 个子 PR 仍是原来的特性拆分，而新方案不适合直接塞进模型或 DSpark 子 PR。它改变的是 scheduler 的 KV ownership/grouping，并附带 worker 对 `UniformTypeKVCacheSpecs` 的 Mamba/BlockTable 适配；我正在确认这些改动能否作为一个独立的“GQA DSpark KV grouping”子 PR，而不是扩大 #14597 的通用 hybrid runtime 范围。

### assistant:commentary
原子性结论已经比较清楚：

- `7234bad` 和 `8ace869` 都属于现有 #14601：一个是旧 GQA DSpark 配置归一化，一个是 QuaRot 的共享边界处理，不应新建 PR。
- `dc9e837` 应单独成为一个新的 KV-cache 子 PR。虽然涉及 scheduler、Mamba helper、BlockTable 和 runner，但后面三处都是启用 mixed group 后不可缺少的同一运行时契约；继续拆会产生不能独立工作的半成品 PR。
- #14599 和 #14601 目前与最新 `main` 有真实冲突，我会一并 rebase；其余子 PR 当前都可合并，不做无意义改写。父 PR 最后按新顺序重建并保持“子 PR 合完只剩文档”。



### assistant:commentary
最新 `main` 对应的 vLLM `cdc4824` 已经原生实现了 `UniformTypeKVCacheSpecs.max_num_blocks_per_req()`，所以旧提交里的同名 monkeypatch 现在已经过时。我会删掉它，只保留 upstream 还没有覆盖的两处：Uniform-wrapped Mamba 的识别，以及 K3 GQA/MLA 的四组 ownership 规划；这正好符合你前面要求的“已有实现就复用，不单独重做”。

### user
还有个事情，0.26分支有一个Attention residual的Triton算子需要移到main分支，你看看一起整一下


### assistant:commentary
收到，这个会作为同一轮 PR 原子性整理的一部分处理。我先对照 `releases/v0.26.0rc` 的 Attention Residual Triton 实现和当前 #14600 里的版本；如果确实是同一个算子契约，我会把算子及单测归到 #14598（Attention），#14600 只保留模型调用和注册，避免模型 PR 自带底层算子。

### assistant:commentary
对照完成了：这个算子其实已经随父栈带进来了，但目前错误地放在 #14600（Model）里；而且当前版本比 0.26 原始实现多了真实运行需要的非连续 stride 支持。处理方式是：

- #14598 接收 Triton kernel 和 NPU 数值/非连续视图回归；
- #14600 只保留模型侧调用；
- 不回退到 0.26 只支持连续 buffer 的旧实现。

这样既完成从 0.26 向 main 的迁移，也保留当前四机验证过的 stride 修复。

### assistant:commentary
我继续按已经确定的原子边界处理：先把 Attention Residual 从模型适配 PR 移到 Attention PR，并保留当前修过非连续 stride 的版本；随后完成 DSpark 两个更新、处理冲突的子 PR，最后重建父 PR，确保合入子 PR 后父 PR 只剩文档。会用精确 lease 推送，避免覆盖期间出现的新提交。

### user

# Response annotations:
Each item contains text selected from an earlier Codex response and may include a user comment. Treat items as Annotation 1, Annotation 2, and so on in array order. Use every selection as context and address every comment. For every annotation you address, include its inline directive `:codex-annotation{index="N"}`, where N is its one-based array position (for example, `:codex-annotation{index="1"}`). Do not use unstructured annotation labels.
<response-annotations>
[{"text":"非连续 stride 支持"}]
</response-annotations>

## My request:
这个我记得是你中途debug问题的时候加上的，我理解如果是模型组网引入的这种奇怪的东西是不是这个部分应该回到0.26的版本会更好？


### assistant:commentary
你说得对，这个边界需要重新确认，不能因为调试时遇到过切片就把兼容逻辑自动带进算子 PR。我先沿实际模型调用链确认 `block_residual` 在 0.26 和当前 main 适配中分别怎么构造；如果非连续输入只是后来组网方式造成的，就回退算子到 0.26 的原始契约，并在模型侧恢复同样的数据布局。:codex-annotation{index="1"}

### assistant:commentary
查清了：这个非连续 view 不是 0.26 算子的原始契约，而是我们在 main 适配里先把预分配的 residual buffer 切成 `[:, :num_valid_blocks, :]` 后造成的；但“预分配 buffer + 有效 block 数”本身来自 vLLM 0.27 上游的 NVIDIA/AMD 实现，直接退回 0.26 每层 `torch.cat` 会偏离上游并影响图稳定性/性能。

所以我不保留泛化的 stride 补丁，也不退回旧组网；改成上游同样的明确契约：算子接收完整连续的预分配 buffer 和 `num_valid_blocks`，模型侧不再生成非连续切片。这样 0.26 的核心算子逻辑不变，只适配 0.27 官方 buffer 生命周期。:codex-annotation{index="1"}

### assistant:commentary
Attention Residual 的边界现在已经收敛：算子 PR 不再包含调试期的任意 stride 兼容；模型 PR 也删除了切片输入和 main 已移除的 FlashComm v1 路径。接下来我处理剩余冲突子 PR，然后一起推送，避免父 PR 在中间状态指向不一致的依赖。

### assistant:commentary
四个有代码变化的子 PR 已经用精确 lease 更新，新的 KV 分组子 PR 是 [#14765](https://github.com/vllm-project/vllm-ascend/pull/14765)。我现在重建父分支，不沿用旧父分支的混杂历史：严格按 8 个子 PR 的顺序重放，最后只加部署文档，并用 diff 证明“子 PR 全合入后父 PR 只剩文档”。

### assistant:commentary
父 PR 现在已经不冲突，8 个子 PR也都是可合并状态；不过刷新后 #14599、#14600、#14601 出现了新的失败检查。我先看每个当前 head 的第一条真实失败日志，区分代码问题、基线问题和资源/标签门禁，能在本地修的会继续修，不把红灯留给后续猜测。

### assistant:commentary
CI 里刚才的两条真实代码失败已经修掉：#14599 是 SiTU beta 的 Optional 类型未按上游默认值收窄，现统一采用上游 `SituAndMul` 的 `1.0` 默认；#14600 是堆叠 PR 在依赖 #14598 尚未合入时触发的 mypy `import-untyped`，现只加了精确类型抑制，没有运行时兜底。其余 `ci-gate` 红灯确认只是缺少仓库维护者的 `ready-precise/ready-all` 标签。

### user
需要额外分析一个事情，codex://threads/01a027f7-7bb4-7901-ba4c-2836103ae73c这个session说0.27比0.26多占用了4个多G的显存，这部分是来源于什么地方


### assistant:commentary
这个很关键，我会把“多 4+ GiB”按分配来源拆开，不只看 `npu-smi` 总量：先读取该任务里 0.26/0.27 的精确启动参数、KV 配置和内存日志，再对照当前四分组实现计算权重、ACLGraph、KV cache、Mamba state、draft GQA cache各自的差额。PR 更新这边的代码失败已修完，CI 只剩继续确认与标签门禁。

### assistant:commentary
该任务的历史读取比较慢，我同时先按现有证据建立核算基线：4+ GiB 如果是“每卡”差额，最可疑的不是权重，而是 0.27 的预分配 Attention Residual、GQA draft KV 页、Mamba speculative state，以及 ACLGraph capture pool；其中 residual buffer 只有几十 MiB 量级，不可能单独解释 4 GiB，重点会放在 KV/state 的 page padding 与 graph reserved memory。

### assistant:commentary
我先把这个任务里 0.26/0.27 两边的显存统计、启动参数和 KV capacity 原样抽出来，再按“权重 / 非 KV 峰值 / KV page / ACLGraph”四项做账；不能仅凭最终 `npu-smi` 占用判断，因为两边都可能按 `gpu-memory-utilization` 把剩余空间吃满。另外我会按模型适配调试规范核对实际运行路径。

### assistant:commentary
这个规范要求先确认真实服务路径、配置和特性开关，不能用 dummy 或启动成功代替真实显存证据；我会据此只读核账，不先改代码或重启现有服务。

### assistant:commentary
现在可以确认“4.45 GiB”是同为 `gpu_memory_utilization=0.8` 时的 **KV 可用预算差**（13.76→9.31 GiB），不是四平面 page 本身多分配了 4.45 GiB。下一步要拆开 profiler 的 `weights / activation peak / non-torch / graph`，定位是谁吃掉了这部分。

### assistant:commentary
拆账已经出来了：4.45 GiB 的主体不是 KV，而是 0.27 profiler 里的峰值激活从 `0.70` 涨到 `4.23 GiB`（+3.53）；其次权重 +0.91、图 +0.39，non-torch 不变。现在继续判断这三项里哪些是版本代码，哪些只是两次启动参数不一致。

### assistant:commentary
参数差异确认了：0.27 反而是 `max_num_seqs=16`，0.26 是 32；两边 `max_num_batched_tokens=8192` 一致，所以 +3.53 GiB 不是并发规模造成的。图显存 +0.39 可以直接归因于 capture 从 `[16]` 变成 `[16,128]`；峰值激活需要继续对比同一个 8192-token profile forward 的模型实现。

### user
用153和154直接单机拉起同一份权重对比吧


### assistant:commentary
可以。我按真正可归因的 A/B 来跑：153 用 0.26、154 用当前 0.27，目标权重和 MLA draft 完全相同，并把 `max_num_batched_tokens / max_num_seqs / prefix cache / capture size / warmup / memory util` 全部锁成一致。先确认两台现有服务的精确 PGID、源码 SHA 和权重可见性，再只停止我们这轮四机服务；不会用宽泛 `pkill`。

### assistant:commentary
两台现在都没有活服务，但容器源码已被后续任务恢复过：153 的 `lala-153-k3` 当前是 0.23，154 的容器源码也不是刚才日志对应的 exact head。为了避免“容器名字像 0.26、实际源码不是”的假对比，我会从日志锁定原始 core/plugin SHA，再在两台各建隔离 worktree 拉同参数，不直接拿容器当前 HEAD。

### assistant:commentary
A/B 版本边界也锁定了：0.26 是 core `fe784ff22…` + plugin `be4dfdb69…`，0.27 是 core `6e448d0ea…` + plugin `8ace869d3…`。我会把这四个 exact commit 部署到新的隔离目录；权重两边统一用同一个 QuaRot 代理目录（大权重文件仍指向同一 checkpoint，只补了 0.27 loader 需要的 descriptor），避免“同名但描述符不同”。

### assistant:commentary
两台源码已经精确锁定，但 NPU 并不空：每卡仍有约 58.8 GiB，占用 PID 分别是 153 的 `1128932…` 和 154 的 `3959110…`。之前按进程名扫到的是 zombie，不能据此判断空卡。我先把这些 PID 映射到容器/PGID；只有确认是我们刚才那套 K3 服务才会停。


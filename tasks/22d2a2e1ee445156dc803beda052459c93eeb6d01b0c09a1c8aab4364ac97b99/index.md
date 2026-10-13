---
blocks:
- block_id: 1d8f59e3882c25ae62dfdd532f637c60c6fc001469435abfd343a20c1246fef0
  indexed: true
  sha256: a1fded0516b32fb6f35bd7e12030bfa221a78b4de24d83af23f42d10ba287c8e
  source_range:
    end: 12448812
    part: 0
    start: 0
  summary: 任务目标是基于 ci-new 在四机上实现固定 K=7 的 DSpark 临时版本，并纳入 K3 五层 aux hidden capture、普通 prefix cache、KDA 状态、PD 分离及后续特性叠加验证。资源协调经历了共享专家多流占用、精确 PGID 清理和多次交接等待；期间明确不得抢占四机，部分两机冒烟计划后来取消，四机真实 NPU 验证仍未完成。实现与静态检查已覆盖 core、Ascend proposer、Mooncake P/D 和 prefix-cache 语义，但真实权重加载、DP4、接受率、精度及叠加场景仍属待验证项。
  title: DSpark 临时版范围、资源协调与实现路线
- block_id: e55950694e42f1b0031807979ef13c5a44afcf8e52ffcb972c139e3d8829f581
  indexed: true
  sha256: 9ef40e6b187be6501f0db9dd49b1732840592fdd6643b809483f1bd3c69fd588
  source_range:
    end: 12448812
    part: 1
    start: 0
  summary: 临时 prefix-cache 方案要求 Target MLA、KDA/Mamba 和 DSpark draft FullAttention 共同决定命中边界；任一 draft group 缺块时应回退公共命中，必要时降为 0，并由 Eagle last-block replay 重建尾块 draft KV。K3 的五层 aux capture、KDA verify 长度 8、7-query/8-target slot mapping、DP4/TP16 场景和 cold/warm 一致性均列为必须测试内容；本地实现者报告的 compile、Ruff、diff 检查通过，但 pytest 或真实设备结果不能由这些静态结果替代。后来 core 与 Ascend 侧报告已完成若干 DSpark/PD 改动，仍需保留对真实 checkpoint、NPU 状态和接受率的未完成标记。
  title: K3 prefix cache 固定点与 DSpark 测试要求
- block_id: 88b5fca3a28d303bcaec4e6a5a7923db6e7a0c6d9286a22e1f376da5e5534e92
  indexed: true
  sha256: ccae78f86e7f031aef65231b30140acce378ab9495dfc02c25cf1d17ed22f4da
  source_range:
    end: 12448812
    part: 2
    start: 0
  summary: PD 审计发现 Mooncake `kv_port` 使用 24000/25000 存在与 AscendDirectTransport `20000–35999` 冲突的风险，建议改为 P=36000、D=36100，并在四节点检查端口；验收还必须确认实际 `draft_tokens > 0`、提供 `--expected-text`，不能只依赖 draft 轮次或 external prefix hit。共享专家任务报告 ON 平均 TPOT 103.030ms、OFF 111.721ms，降低 7.78%，但这属于其性能 A/B，不是 DSpark 验证；当时仅剩 GPQA1，四机资源尚未明确交接。用户随后要求立即开始 256 并发算子编译，尽管此前为避免干扰 GPQA1 曾暂缓，该动作的实际执行结果未在材料中给出。
  title: Mooncake PD 启动审计与最终硬件验收条件
- block_id: c6d801d676046cfda90bdcd0340f63714f3dcdfafb4401129378e1791bd46309
  indexed: true
  sha256: 5cc94d66e95bc4cc65e68c3660878e5cf2ddecfe88c4484031097fdebdb41dac
  source_range:
    end: 12448812
    part: 3
    start: 0
  summary: 四机隔离构建首轮因复制目录中的 CMakeCache 路径缓存污染失败，修正后改用完整跟踪源码并以 `MAX_JOBS=256` 重编；源码集成最终强调不含 PR49 `attn_res.py`、不启用共享专家多流，并保留 FlashComm/shared-expert-DP。此前静态与 mock 测试曾报告通过，但尚未等同于真实 NPU 验收；用户要求先用完整权重验证功能、prefix cache、接受率和精度，再用 5 层权重验证 P/D KV 传输。交接时确认四机资源已清空，但后续完整权重服务启动仍需经过首条 `2+2=4` 质量门。
  title: 四机编译与验收口径收敛
- block_id: a8c5690c0695189f91c7f30aed0607ddcee3a7117b65a06eabf893bf57ef29a6
  indexed: true
  sha256: 456447b0b0a331feefb4b63ae66d523b5e16cb2037dd7fd1bf8736cc709ba82f
  source_range:
    end: 12448812
    part: 4
    start: 0
  summary: 四机算子构建均以 exit code 0 完成，产物包含 `dequant_situ_quant` 和 `recurrent_kda`；随后纯 ci-new + DSpark 的完整权重启动首轮因缺少 `specforge`、`DSparkDraftModel` 未归一化为 `Qwen3DSparkModel` 而失败。配置归一化修复后又在约 312/352 权重加载处触发 `KimiK3Config` 缺少 `image_token_index`，API 未监听且未发送推理请求；诊断确认这是 proposer 多模态 token 字段桥接缺口，K3 实际字段为 `media_placeholder_token_id`，并非 DSpark 算法或权重问题。后续提出将 K3 纳入 Kimi K2.5 同一兼容分支，相关本地回归报告为 94 passed，但真实四机验收结果仍未形成。
  title: 完整权重启动失败与配置修复
- block_id: 204a9f439d71b06d36165721d035cbe14b8626369827aabe5da85f85a9142f54
  indexed: true
  sha256: b456c3382700b0e97f1d93d9e5be3e25b2d5b18ea88dc72cb1b527e26aa3ca0d
  source_range:
    end: 12448812
    part: 5
    start: 0
  summary: '`media_placeholder_token_id` 问题源于 DSpark 启动时触发的通用 spec-decode proposer 多模态装载路径：K3 继承 `SupportsMultiModal`，原逻辑只特判 Kimi K2.5，错误回退读取不存在的 `image_token_index`；建议的最窄修复是仅在 proposer 中桥接现有字段，不向 `KimiK3Config` 增加静态别名。第三轮启动审计曾确认 64/64 worker、K=7、prefix cache ON、`multistream_overlap_shared_expert=false`，但材料没有给出完整权重推理验收已完成的证据。用户明确要求先完成 full-weight 的质量门、prefix cache、DSpark 接受率和 GPQA smoke，确认资源精确释放后，才启动使用完整 tokenizer、`8192` 上下文配置的 5 层 aggregate，再进入 P/D proxy 与 Mooncake KV 传输验证。'
  title: 完整权重优先，随后再做五层 P/D
- block_id: 02a7a61ceccd802075b3cf8d2f5575fe37dbeb0a8fa934a5043a30543f520028
  indexed: true
  sha256: ce37a507981ab841e2e3d524cb518241cf9d71d26afc6aff1dd79e62cc852dfc
  source_range:
    end: 12448812
    part: 6
    start: 0
  summary: 规划的验收命令要求检查 proxy health、cold/warm P/D 输出一致、5 层 aggregate、D 端 N−1 KV 命中、warm local_hits、draft/accepted tokens，以及无 `Skipping incomplete DSpark remote KV`；5 层 `target_layer_ids=[-1,0,1,2,3]` 仅用于 KV/功能 smoke，不能推出精度结论，且断言失败时 validator 可能先抛异常再写 JSON。r3 未发送推理请求，四机一致的首因是外层多模态 K3 wrapper 未暴露 `SupportsEagle3`/aux hidden-state 转发，`aux_hidden_state_outputs` 请求因此在 draft model 加载阶段失败，后续 TBE 和 `IndexError` 属于连带噪声；此前的 `media_placeholder_token_id` 与 `image_token_index` 是另一独立兼容缺口。提交 `f1611bdad` 的 Python-only 修复据报告已通过 K3 单测 30/30、DSpark proposer 12/12 和 Ruff，并完成远端接口静态确认，但第四轮完整权重启动及后续质量门仍只是进行中，尚无真实 NPU 成功证据。
  title: K3 DSpark：r3 失败根因与 outer wrapper 修复，第四轮待验收
- block_id: 08f5f3e2d5fc62720dba9edbc43c7c0084df6df386c2dfd2c39cce71f427876d
  indexed: true
  sha256: aec93e4cbf05da127d9254ebac11357631ad03ef4c846c76854538cb5719be4e
  source_range:
    end: 22390342
    part: 0
    start: 12448812
  summary: 完整权重、DSpark draft、HCCL_BUFFSIZE=512、四机ACLGraph捕获和health=200均已观察到，但首条2+2返回“4”后连续“!”；因此prefix cache、接收率、GPQA和P/D验证均未完成。随后创建了vllm-ascend-kimi-k3#54草稿PR，明确ci-new单PR、core改动以插件patch承载、不包含PR49 Triton；此前一次跨fork创建失败，后续同仓分支创建成功。全eager对照返回干净的“4”，使问题范围收敛到target FULL_DECODE_ONLY/ACLGraph路径，但这仍不是最终根因证明。
  title: r5启动成功但首条FULL图质量门失败
- block_id: 630b66b2c1f090597c59412785512db7d3305d5363034b50cf24f0532d33f876
  indexed: true
  sha256: 7a76f9b0ab76105e69a6768ff9056368439675a1437ef7a7b797f6a95dc90ef1
  source_range:
    end: 22390342
    part: 1
    start: 12448812
  summary: 审计排除了“vocab-parallel本地argmax”这一误判，并确认A3/A5不运行310P；dummy row的num_accepted_tokens由1改0与当前A3/A5 zero-length kernel行为近似等价，不能视为乱码根因。关键候选是check_gdn_layer只检查layer_types而漏掉Kimi K3的is_linear_attn=True，导致FIA/MLA的[0,8,16]边界错误地复用于GDN/KDA，而非应为[0,8,8]；该判断起初仍标为候选。单图size=16修复后的r8质量门中，四个DP首轮均精确返回“4”，并捕获到MLA[0,8,16]、GDN[0,8,8]、actual_seq_lengths=[0,8,0]；但复杂异构短请求32题仅28题输出干净，4题出现close token后连续“!”，说明仍有未定位的结束/填充或replay问题。
  title: ACLGraph输入审计发现GDN检测与padding缺口
- block_id: bf13934bf18bc2eae74f95b09998eea8a0cbbfb09b157cdb80cfe435702e1c70
  indexed: true
  sha256: be169588ddc7008196f5584c2b3581fc427b4d6247623b94294d94a8bb222f72
  source_range:
    end: 22390342
    part: 2
    start: 12448812
  summary: 静态复核确认Kimi K3未被check_gdn_layer识别会绕过已有GDN graph双边界修复，并同时影响verify边界与DSpark proposer的slot_mapping；这解释了普通ci-new精度正常而DSpark+FULL图暴露问题，但不能单独证明全部异常已解决。r8在完整权重、prefix cache开启、target FULL_DECODE_ONLY、draft eager、单图size=16和无PR49条件下启动成功，四路2+2首轮均返回精确“4”，drafted 28、accepted 24，但后续32个复杂短请求出现4个带“<|close|>”后连续“!”的脏输出。当前应保留该部分通过与失败证据，暂停prefix/GPQA，不把单轮通过或28/32干净结果升级为完整精度验收。
  title: r8修复后简单门禁通过但复杂批次仍异常
- block_id: e8691316a5f2fbd8218fd19dee312b4323882814330811a4e80b66839d6bbd14
  indexed: true
  sha256: f2feb245569d9376ca9c7902c56de3aafae1f06b5becf3cec3dd2e0400cc0c4b
  source_range:
    end: 22390342
    part: 3
    start: 12448812
  summary: prefix cache 已验证 cold 0 hit、warm 3072/3727 hit、reset 后 0 hit，PR 提交为 Draft 且未混入无效 accepted-token 改动；GDN `_dummy_run` 接线 UT 的静态检查通过，但本地 pytest 受缺少 `partial_json_parser` 等依赖阻塞。r9 的 4/32 尾部 `!` 复现表明，FULL graph dummy replay 在 fresh metadata 的 spec 字段为 `None` 时未覆盖 capture 遗留缓冲，旧 reset 修复了空闲 rank 路径但未解决 hot-rank 问题；随后静态审计将重点转向 `global_stream` 上一轮 Mamba/KDA state postprocess 与下一轮 `condense/reorder`、block-table 更新之间的 `num_accepted_tokens_event` 等待顺序竞态。该候选尚未完成 r10 A/B 验证，causal-conv1d mutation 注册问题已被用户明确排除。
  title: r9定位dummy旧状态污染，r10转向跨流同步竞态
- block_id: 8b1047b1135597be3d23a069e03b0d9c79de73b21ac38e2d001cf0335c48e9de
  indexed: true
  sha256: 008975a2f57e3a02bf44a6392fd94d5efcf6456da2040f625c256eeba477cfa2
  source_range:
    end: 22390342
    part: 4
    start: 12448812
  summary: 只读审计建议在非 async DSpark 的 `_update_states()` 入口、parent remove/condense 前提前等待 `num_accepted_tokens_event`；通用 async 路径还需移除基于 `prev_positions` 的 accepted gather，避免对已搬移 CPU buffer double-remap。r9 的 `26/2/2/2` hot-rank case 仍为 30/32，失败请求在进入 ACLGraph FULL replay 前已出现 token0/NaN；均衡 `8/8/8/8` 为 32/32，但 dummy 输出仍为 NaN，说明 dummy reset 不是根修，问题更像 NONE 阶段的 KDA 状态污染或 DP dummy/collective 交互。r10 已完成 36 个定向 UT，并启动四 rank 完整权重服务准备复跑；截至该批次结束尚无服务完成或 A/B 成功证据。
  title: hot-rank失败早于FULL图，r10同步补丁待验证
- block_id: 1a53a964b4507bd0144129b2bcc877b4da0f21abc3f3fe07280115d6af1e9f95
  indexed: true
  sha256: ad899a153898b07c2dce9d29f29a580edb9612fd5da91bb910e4297397fd45d5
  source_range:
    end: 23383003
    part: 0
    start: 22390342
  summary: 只读审计建议仅在 `model_runner_v1.py` 的 `_update_states()` 中、父类调用前同步 `num_accepted_tokens_event`，并以通用 UT 验证同步/async、event 缺失等分支；同时明确剔除 ACL_DIAG、GDN reset、padding、PR49 Triton 和 shared-expert 多流等无关改动。r10 已完成 352/352 权重加载、四 rank size16 FULL ACLGraph 捕获并保持健康，但四个 2+2 请求虽通过，原样 hot-rank0 仅 31/32，失败结果为 `30` 后连续 `!`；对称四 DP 真实请求也仅 124/128，因此 early-wait 不是充分修复，PR 暂不更新。当前继续在不重启现场的条件下，用四个 DP 各 32 个相同复杂短请求对照混合长短请求，以区分请求 shrink/condense/reorder 生命周期问题与通用并发 buffer/event 问题；尚未形成验证成功结论。
  title: r10：early-wait 生命周期缺口已确认但未解决 hot-rank 并发错误
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 22d2a2e1ee445156dc803beda052459c93eeb6d01b0c09a1c8aab4364ac97b99
  kind: experience
  material_digest: 3188ddb24c0bdb6e11795da6fbbe100329407db3c668ee14164c513db91b9818
  revision: 042a0ab54d40faa814a92fc80c11fadbca301ae02ecd2270c266d67b94d5ef9b
  schema: mindie-entry/3
  summary: '此前 r3、r5、PR #54、无 PR49 隔离条件、r8 GDN 边界、r9 reset 及其失败证据继续保留；r10 的 early-wait 候选在健康启动和均衡短请求上有局部通过，但 hot-rank0 仍为 31/32，四 DP 对称并发仍为 124/128，故不能视为修复或更新 PR。审计仍要求从干净 HEAD 仅加入 early-wait 与通用 UT，并复跑 hot-rank 及 prefix cold/warm/reset；当前优先通过相同复杂短请求与混合长短请求对照，区分 shrink/condense/reorder、padding 元数据生命周期和通用并发 buffer/event 边界。'
  title: DSpark/K3：r10 early-wait 候选未解决 hot-rank 并发问题，继续定位状态生命周期
navigation: '此前 r3、r5、PR #54、无 PR49 隔离条件、r8 GDN 边界、r9 reset 及其失败证据继续保留；r10 的 early-wait 候选在健康启动和均衡短请求上有局部通过，但 hot-rank0 仍为 31/32，四 DP 对称并发仍为 124/128，故不能视为修复或更新 PR。审计仍要求从干净 HEAD 仅加入 early-wait 与通用 UT，并复跑 hot-rank 及 prefix cold/warm/reset；当前优先通过相同复杂短请求与混合长短请求对照，区分 shrink/condense/reorder、padding 元数据生命周期和通用并发 buffer/event 边界。'
schema: mindie-material-task/1
status: complete
task_id: 22d2a2e1ee445156dc803beda052459c93eeb6d01b0c09a1c8aab4364ac97b99
---

# DSpark/K3：r10 early-wait 候选未解决 hot-rank 并发问题，继续定位状态生命周期

此前 r3、r5、PR #54、无 PR49 隔离条件、r8 GDN 边界、r9 reset 及其失败证据继续保留；r10 的 early-wait 候选在健康启动和均衡短请求上有局部通过，但 hot-rank0 仍为 31/32，四 DP 对称并发仍为 124/128，故不能视为修复或更新 PR。审计仍要求从干净 HEAD 仅加入 early-wait 与通用 UT，并复跑 hot-rank 及 prefix cold/warm/reset；当前优先通过相同复杂短请求与混合长短请求对照，区分 shrink/condense/reorder、padding 元数据生命周期和通用并发 buffer/event 边界。

## Materials

- [DSpark 临时版范围、资源协调与实现路线](blocks/1d8f59e3882c25ae62dfdd532f637c60c6fc001469435abfd343a20c1246fef0.md): 任务目标是基于 ci-new 在四机上实现固定 K=7 的 DSpark 临时版本，并纳入 K3 五层 aux hidden capture、普通 prefix cache、KDA 状态、PD 分离及后续特性叠加验证。资源协调经历了共享专家多流占用、精确 PGID 清理和多次交接等待；期间明确不得抢占四机，部分两机冒烟计划后来取消，四机真实 NPU 验证仍未完成。实现与静态检查已覆盖 core、Ascend proposer、Mooncake P/D 和 prefix-cache 语义，但真实权重加载、DP4、接受率、精度及叠加场景仍属待验证项。
- [K3 prefix cache 固定点与 DSpark 测试要求](blocks/e55950694e42f1b0031807979ef13c5a44afcf8e52ffcb972c139e3d8829f581.md): 临时 prefix-cache 方案要求 Target MLA、KDA/Mamba 和 DSpark draft FullAttention 共同决定命中边界；任一 draft group 缺块时应回退公共命中，必要时降为 0，并由 Eagle last-block replay 重建尾块 draft KV。K3 的五层 aux capture、KDA verify 长度 8、7-query/8-target slot mapping、DP4/TP16 场景和 cold/warm 一致性均列为必须测试内容；本地实现者报告的 compile、Ruff、diff 检查通过，但 pytest 或真实设备结果不能由这些静态结果替代。后来 core 与 Ascend 侧报告已完成若干 DSpark/PD 改动，仍需保留对真实 checkpoint、NPU 状态和接受率的未完成标记。
- [Mooncake PD 启动审计与最终硬件验收条件](blocks/88b5fca3a28d303bcaec4e6a5a7923db6e7a0c6d9286a22e1f376da5e5534e92.md): PD 审计发现 Mooncake `kv_port` 使用 24000/25000 存在与 AscendDirectTransport `20000–35999` 冲突的风险，建议改为 P=36000、D=36100，并在四节点检查端口；验收还必须确认实际 `draft_tokens > 0`、提供 `--expected-text`，不能只依赖 draft 轮次或 external prefix hit。共享专家任务报告 ON 平均 TPOT 103.030ms、OFF 111.721ms，降低 7.78%，但这属于其性能 A/B，不是 DSpark 验证；当时仅剩 GPQA1，四机资源尚未明确交接。用户随后要求立即开始 256 并发算子编译，尽管此前为避免干扰 GPQA1 曾暂缓，该动作的实际执行结果未在材料中给出。
- [四机编译与验收口径收敛](blocks/c6d801d676046cfda90bdcd0340f63714f3dcdfafb4401129378e1791bd46309.md): 四机隔离构建首轮因复制目录中的 CMakeCache 路径缓存污染失败，修正后改用完整跟踪源码并以 `MAX_JOBS=256` 重编；源码集成最终强调不含 PR49 `attn_res.py`、不启用共享专家多流，并保留 FlashComm/shared-expert-DP。此前静态与 mock 测试曾报告通过，但尚未等同于真实 NPU 验收；用户要求先用完整权重验证功能、prefix cache、接受率和精度，再用 5 层权重验证 P/D KV 传输。交接时确认四机资源已清空，但后续完整权重服务启动仍需经过首条 `2+2=4` 质量门。
- [完整权重启动失败与配置修复](blocks/a8c5690c0695189f91c7f30aed0607ddcee3a7117b65a06eabf893bf57ef29a6.md): 四机算子构建均以 exit code 0 完成，产物包含 `dequant_situ_quant` 和 `recurrent_kda`；随后纯 ci-new + DSpark 的完整权重启动首轮因缺少 `specforge`、`DSparkDraftModel` 未归一化为 `Qwen3DSparkModel` 而失败。配置归一化修复后又在约 312/352 权重加载处触发 `KimiK3Config` 缺少 `image_token_index`，API 未监听且未发送推理请求；诊断确认这是 proposer 多模态 token 字段桥接缺口，K3 实际字段为 `media_placeholder_token_id`，并非 DSpark 算法或权重问题。后续提出将 K3 纳入 Kimi K2.5 同一兼容分支，相关本地回归报告为 94 passed，但真实四机验收结果仍未形成。
- [完整权重优先，随后再做五层 P/D](blocks/204a9f439d71b06d36165721d035cbe14b8626369827aabe5da85f85a9142f54.md): `media_placeholder_token_id` 问题源于 DSpark 启动时触发的通用 spec-decode proposer 多模态装载路径：K3 继承 `SupportsMultiModal`，原逻辑只特判 Kimi K2.5，错误回退读取不存在的 `image_token_index`；建议的最窄修复是仅在 proposer 中桥接现有字段，不向 `KimiK3Config` 增加静态别名。第三轮启动审计曾确认 64/64 worker、K=7、prefix cache ON、`multistream_overlap_shared_expert=false`，但材料没有给出完整权重推理验收已完成的证据。用户明确要求先完成 full-weight 的质量门、prefix cache、DSpark 接受率和 GPQA smoke，确认资源精确释放后，才启动使用完整 tokenizer、`8192` 上下文配置的 5 层 aggregate，再进入 P/D proxy 与 Mooncake KV 传输验证。
- [K3 DSpark：r3 失败根因与 outer wrapper 修复，第四轮待验收](blocks/02a7a61ceccd802075b3cf8d2f5575fe37dbeb0a8fa934a5043a30543f520028.md): 规划的验收命令要求检查 proxy health、cold/warm P/D 输出一致、5 层 aggregate、D 端 N−1 KV 命中、warm local_hits、draft/accepted tokens，以及无 `Skipping incomplete DSpark remote KV`；5 层 `target_layer_ids=[-1,0,1,2,3]` 仅用于 KV/功能 smoke，不能推出精度结论，且断言失败时 validator 可能先抛异常再写 JSON。r3 未发送推理请求，四机一致的首因是外层多模态 K3 wrapper 未暴露 `SupportsEagle3`/aux hidden-state 转发，`aux_hidden_state_outputs` 请求因此在 draft model 加载阶段失败，后续 TBE 和 `IndexError` 属于连带噪声；此前的 `media_placeholder_token_id` 与 `image_token_index` 是另一独立兼容缺口。提交 `f1611bdad` 的 Python-only 修复据报告已通过 K3 单测 30/30、DSpark proposer 12/12 和 Ruff，并完成远端接口静态确认，但第四轮完整权重启动及后续质量门仍只是进行中，尚无真实 NPU 成功证据。
- [r5启动成功但首条FULL图质量门失败](blocks/08f5f3e2d5fc62720dba9edbc43c7c0084df6df386c2dfd2c39cce71f427876d.md): 完整权重、DSpark draft、HCCL_BUFFSIZE=512、四机ACLGraph捕获和health=200均已观察到，但首条2+2返回“4”后连续“!”；因此prefix cache、接收率、GPQA和P/D验证均未完成。随后创建了vllm-ascend-kimi-k3#54草稿PR，明确ci-new单PR、core改动以插件patch承载、不包含PR49 Triton；此前一次跨fork创建失败，后续同仓分支创建成功。全eager对照返回干净的“4”，使问题范围收敛到target FULL_DECODE_ONLY/ACLGraph路径，但这仍不是最终根因证明。
- [ACLGraph输入审计发现GDN检测与padding缺口](blocks/630b66b2c1f090597c59412785512db7d3305d5363034b50cf24f0532d33f876.md): 审计排除了“vocab-parallel本地argmax”这一误判，并确认A3/A5不运行310P；dummy row的num_accepted_tokens由1改0与当前A3/A5 zero-length kernel行为近似等价，不能视为乱码根因。关键候选是check_gdn_layer只检查layer_types而漏掉Kimi K3的is_linear_attn=True，导致FIA/MLA的[0,8,16]边界错误地复用于GDN/KDA，而非应为[0,8,8]；该判断起初仍标为候选。单图size=16修复后的r8质量门中，四个DP首轮均精确返回“4”，并捕获到MLA[0,8,16]、GDN[0,8,8]、actual_seq_lengths=[0,8,0]；但复杂异构短请求32题仅28题输出干净，4题出现close token后连续“!”，说明仍有未定位的结束/填充或replay问题。
- [r8修复后简单门禁通过但复杂批次仍异常](blocks/bf13934bf18bc2eae74f95b09998eea8a0cbbfb09b157cdb80cfe435702e1c70.md): 静态复核确认Kimi K3未被check_gdn_layer识别会绕过已有GDN graph双边界修复，并同时影响verify边界与DSpark proposer的slot_mapping；这解释了普通ci-new精度正常而DSpark+FULL图暴露问题，但不能单独证明全部异常已解决。r8在完整权重、prefix cache开启、target FULL_DECODE_ONLY、draft eager、单图size=16和无PR49条件下启动成功，四路2+2首轮均返回精确“4”，drafted 28、accepted 24，但后续32个复杂短请求出现4个带“<|close|>”后连续“!”的脏输出。当前应保留该部分通过与失败证据，暂停prefix/GPQA，不把单轮通过或28/32干净结果升级为完整精度验收。
- [r9定位dummy旧状态污染，r10转向跨流同步竞态](blocks/e8691316a5f2fbd8218fd19dee312b4323882814330811a4e80b66839d6bbd14.md): prefix cache 已验证 cold 0 hit、warm 3072/3727 hit、reset 后 0 hit，PR 提交为 Draft 且未混入无效 accepted-token 改动；GDN `_dummy_run` 接线 UT 的静态检查通过，但本地 pytest 受缺少 `partial_json_parser` 等依赖阻塞。r9 的 4/32 尾部 `!` 复现表明，FULL graph dummy replay 在 fresh metadata 的 spec 字段为 `None` 时未覆盖 capture 遗留缓冲，旧 reset 修复了空闲 rank 路径但未解决 hot-rank 问题；随后静态审计将重点转向 `global_stream` 上一轮 Mamba/KDA state postprocess 与下一轮 `condense/reorder`、block-table 更新之间的 `num_accepted_tokens_event` 等待顺序竞态。该候选尚未完成 r10 A/B 验证，causal-conv1d mutation 注册问题已被用户明确排除。
- [hot-rank失败早于FULL图，r10同步补丁待验证](blocks/8b1047b1135597be3d23a069e03b0d9c79de73b21ac38e2d001cf0335c48e9de.md): 只读审计建议在非 async DSpark 的 `_update_states()` 入口、parent remove/condense 前提前等待 `num_accepted_tokens_event`；通用 async 路径还需移除基于 `prev_positions` 的 accepted gather，避免对已搬移 CPU buffer double-remap。r9 的 `26/2/2/2` hot-rank case 仍为 30/32，失败请求在进入 ACLGraph FULL replay 前已出现 token0/NaN；均衡 `8/8/8/8` 为 32/32，但 dummy 输出仍为 NaN，说明 dummy reset 不是根修，问题更像 NONE 阶段的 KDA 状态污染或 DP dummy/collective 交互。r10 已完成 36 个定向 UT，并启动四 rank 完整权重服务准备复跑；截至该批次结束尚无服务完成或 A/B 成功证据。
- [r10：early-wait 生命周期缺口已确认但未解决 hot-rank 并发错误](blocks/1a53a964b4507bd0144129b2bcc877b4da0f21abc3f3fe07280115d6af1e9f95.md): 只读审计建议仅在 `model_runner_v1.py` 的 `_update_states()` 中、父类调用前同步 `num_accepted_tokens_event`，并以通用 UT 验证同步/async、event 缺失等分支；同时明确剔除 ACL_DIAG、GDN reset、padding、PR49 Triton 和 shared-expert 多流等无关改动。r10 已完成 352/352 权重加载、四 rank size16 FULL ACLGraph 捕获并保持健康，但四个 2+2 请求虽通过，原样 hot-rank0 仅 31/32，失败结果为 `30` 后连续 `!`；对称四 DP 真实请求也仅 124/128，因此 early-wait 不是充分修复，PR 暂不更新。当前继续在不重启现场的条件下，用四个 DP 各 32 个相同复杂短请求对照混合长短请求，以区分请求 shrink/condense/reorder 生命周期问题与通用并发 buffer/event 问题；尚未形成验证成功结论。

---
blocks:
- block_id: 1b6f497342735590872f9e1df01cd14f5e6fc7f1ba7385488b17e2ac47dd818c
  indexed: true
  sha256: 7fd6f676fb3be964f3a71f719808ea5c5f2614eb8a18b9d163a7833fb103934c
  source_range:
    end: 7332592
    part: 0
    start: 0
  summary: '只读审计确认 PR #12950（base `81d3450`、head `4a1857b`）完整回退了 KV Pool #12801 的 lease-key 去重、日志降级和 `use_layerwise` 文档，并局部恢复了不应全局生效的 `QuantType.NONE` MegaMoE fallback；这些结论包含 patch 对比和 CI 失败证据。审计还识别出 shared-expert-DP/FlashComm1 的 `globalBS invalid`、DeepSeek V4 token mismatch 强嫌疑链、全局 `g_proj` 匹配误伤 Kimi、A2 expert 数阈值、W8A8MXFP、FIA split、build 清理及广域 Mooncake/patch 风险，其中部分仍需运行验证，不能视为已证明根因。后续决定保留 FIA、W8 MXFP 和 build 清理，恢复 KV Pool 修复，收窄 SiTU fallback，移除无关 KV spec 重构，并系统性拆分 FC1 与 shared-expert-DP 语义。'
  title: 'PR #12950 回退与全局语义风险审计'
- block_id: 8f48f059f37975f07481338b589ab2e818e99cd7464d6ceb2ecc8936389ec975
  indexed: true
  sha256: 10e5e22586cb5ed20d66c540330a80c29c5f86862cf84fa4fbff28962ddf2cee
  source_range:
    end: 7332592
    part: 1
    start: 0
  summary: 设计修正为模型无关的正交语义：FC1 表示两个完整 TP block 之间沿 token/sequence 轴的 gather 与 reduce-scatter，shared-expert-DP 只控制共享专家权重复制，不应改变 routed expert 的 MC2/All2All/AllGather token contract。静态检查和 mocked 回归已通过：FC1-only 44 项、shared-DP-only 13 项、FC1+shared-DP 33 passed/1 skipped；真实 DP2×TP4 尚未完成，不能将这些结果推广为真实分布式功能或精度成功。由于 153–156 曾被 Kubernetes DeepSeek-V4 服务占用，原 Kimi 服务已无存活进程；在用户要求保留量化任务后，已 cordon 节点并删除指定 K8s Pod，确认 NPU 进程为 0，随后启动三组真实 DP2×TP4 案例。
  title: FC1/共享专家解耦设计与验证准备
- block_id: 8e7a596c03e664264f99238e365df3a72a8ffd99bf495923d7d0a3ed61da761f
  indexed: true
  sha256: 96b993de3c1885c3e3213e9820cadcb596a007bb5168a13f6eefe7e21b5c0277
  source_range:
    end: 7332592
    part: 2
    start: 0
  summary: '用户确认 KV Pool #12801 回退是严重问题，认可收窄 MegaMoE，要求保留 A5 FIA、W8A8MXFP 和 build 清理，删除 SFA/Sliding 纯重构，并要求分析 `g_proj` 隔离及 FC1/shared-expert-DP 的系统解耦；`AscendMLAAttentionSpec.merge()` invariant 暂不处理。验证计划先做三种 DP2×TP4 真实组合（FC1-only、shared-DP-only、FC1+shared-DP），全部通过后再做四机精度验证；此前仅 mocked/隔离源码结果已完成，真实案例刚进入运行阶段。用户明确要求 K8s 任务直接 kill、量化任务保留，已有记录显示指定 Pod 被删除且节点资源已释放，但真实运行结果仍未给出。'
  title: 按用户标注落地范围与资源处置
- block_id: 2a7d62d3909930ee710249c67fa982a8072e157d6b67d20971ad6be2b9204b3f
  indexed: true
  sha256: 348e760b047c83eead1c8885daa72f7b1cc00d6359a479530b220f1b788e17b3
  source_range:
    end: 7332592
    part: 3
    start: 0
  summary: A（FC1-only）与 B（shared-expert-DP-only）的材料报告了 worker、HTTP 200、无 HCCL/shape/globalBS/Traceback 等现象；未启用确定性计算时动态并发输出漂移被用户表示可忽略，但这些仍是来源中的验证记录。C 曾出现真实请求 hang，随后被停止，且未完成重启后的推理门禁；用户随后明确 shared-expert-DP 必须依赖 FC，因此 C 不是有效场景。四机 FC1+shared-expert-DP 启动曾完成 64/64 worker 和部分权重加载，但后续任务转向 prefix-cache 场景。
  title: 单机 A/B 门禁与四机启动前状态
- block_id: b3aab110ca6eb2c7bf890b11ec35eca62878fca0428606b6e4223b1c576c538b
  indexed: true
  sha256: 5437f1c574a2063ccd3c762afb0f082dc6b8f16f2412907dc23d445027ff818d
  source_range:
    end: 7332592
    part: 4
    start: 0
  summary: '只读审计报告称 PR #51 的 shared-expert width 校验当前未包含，需要移植 active fused_moe.py 逻辑并补 routed/shared width 不同的回归测试；旧版 fused_moe_0_23_0.py 不应恢复。PR #52 的 A2 `num_experts > 512`→`ALLGATHER` 产品 guard 已存在，仅缺回归测试；PR #53 的 W8/W4 MXFP 条件也已存在，建议补 W8 覆盖，不直接 cherry-pick。GitHub connector 被报告为无私库权限，审计因此使用本机认证的 gh，且当时未编辑、fetch 或推送。'
  title: 'PR #51/#52/#53 纳入方式审计'
- block_id: 9c4c47ba46ebc359e6e4d8ac97d65964d6ddb9e4160ab5fc818c052151c1e9e0
  indexed: true
  sha256: 3aac0d624da0d3729604f3d988ee4acf1bddcbfb9202985184b6878182ecc5de
  source_range:
    end: 7332592
    part: 5
    start: 0
  summary: 'TP16 5-layer 对照因量化描述引用不存在的第 6 层而在模型加载阶段失败，未进入目标 forward；随后材料报告将 upstream #13022 的 DP padding contract 回退纳入候选，并为 #51/#52/#53 补入逻辑或测试。候选代码据称通过远端聚焦测试（56 passed），四机 `prefix on + FC1 on + shared-DP off` 的 health、数学题及四个 DP rank 的 cold→hit 曾被报告为成功，且每个 rank 的 cached tokens 达到 1536；这些结果仍属于来源中的运行记录。GPQA-Diamond 198 题尚在进行中（材料截点 41/198，40 对 1 错、0 request error/missing），而 PR #12950 当时仍为单提交但处于 CONFLICTING/DIRTY，存在失败/取消检查且缺少人工 approval，尚未满足合并条件。'
  title: 最终候选与四机 prefix-cache/GPQA 进展
- block_id: 3aaea4e15ecb0af728ee2eaa47645bc86bd0e1c1c90ab41df0976a86e8f36272
  indexed: true
  sha256: faaceba449d1a8c93e68541ed364c907702a599ac76bffd9a796daf80d6c9b71
  source_range:
    end: 17748710
    part: 0
    start: 7332592
  summary: 材料报告公共路径已将 Kimi/K3 身份判断改为能力、shape 或协议契约驱动，并保留 ModelRegistry、parser、renderer 等真正的模型/协议边界；但审计也指出 ModelSlim fallback、K3 packed mapping、renderer patch、A_log loader 和重复注册等风险，部分结论当时仍未全部落地。Qwen3.5 FlashComm+MTP 曾因 shared-expert-DP 被错误用作序列恢复条件而出现 `16 vs 8`，随后修复方向改为由 FlashComm 状态控制。PR 经多次 rebase/force-with-lease 更新，曾到 `a7901cd04`；单机 5 层 `/health=200`，但截断权重仅作启动验证，且后续 CI 仍暴露 Qwen MTP 问题。
  title: Kimi 公共路径去模型判断与 PR 初次更新
- block_id: 1cc3ae55647435ebe601d7648c997d6ed95cad6d1d1090bdfa47740951e72a5c
  indexed: true
  sha256: de962f654be811dfc945443ac4f725f1629e9dd9adb3c191ba1f32fb5908523d
  source_range:
    end: 17748710
    part: 1
    start: 7332592
  summary: 材料明确必须保留 `KimiK3For*` architecture key、`model_type`、checkpoint ABI mapping、parser 名称及 reasoning/tool/renderer 注册等协议或加载边界，不能将其与公共 MoE 执行路径混同。后续 CI 定位到 Qwen3.5-35B MTP 的 FlashComm 路径：shared-expert-DP 与 FlashComm 解耦后，proposer 仍依据错误状态恢复完整序列，导致 `16 vs 8`；服务和完整权重加载本身成功。服务器复现还经历缺少 CANN、显存不足、custom-op 路径未配置等环境阻碍，不能把这些失败当作代码失败。
  title: 协议边界保留与 Qwen3.5 CI 根因定位
- block_id: 62a86930f7c83daf055d4cdf2f4f8713a764fdb5c393691921078d0bf10a0697
  indexed: true
  sha256: 38387fb787dcaeb0302a0798ab168a33ca0c333ebb416bfb6626f20ae6471103
  source_range:
    end: 17748710
    part: 2
    start: 7332592
  summary: 复现进一步发现 `inputs_embeds`、multimodal 标签以及 `draft + VL` 的旧特判共同影响 token 形状；最终根因收敛为 shared expert 的 reduce-scatter 与 embedding 复制语义被同一 `maybe_*` 路径混用，导致 `16/8` 及后续 `4/2`。材料称通过独立公共 reduce-scatter 算子、移除模型分类特判并保留必要复制语义后，完整图模式和三道数学题（391、5、40）通过，无乱码或空结果；rebased tree 也完成等价验证。随后 `3c560c2e2` 因 CRLF/Ruff 格式问题修正为 `cf50b0513`，再加入 CPU UT mock 修复并 rebase 推到 `93b073decb`；CI 已报告关键 Qwen3.5、DeepSeek V4、FC1/prefix-cache 设备组通过，但当时仍有运行中的矩阵，最终全量状态未完全闭合。
  title: Qwen MTP 通信语义修复与 CI 追踪
- block_id: 60fd02bfbd4b792b76b4c45810f77262d525220b75d58abe80f30db9b23dd09c
  indexed: true
  sha256: 69f3f596cc9f8f92f2e8f0c0bf644b83fb0acddeee2f62ff390becce94e08116
  source_range:
    end: 17748710
    part: 3
    start: 7332592
  summary: 'PR #12950 最终 SHA `93b073decb` 的上一轮 CI 曾报告 19 个任务通过、11 个运行中、8 个排队且 0 个失败，但整轮尚未全绿；随后因拆分 commit 触发新的 pre-commit 规则，`codespell` 拒绝 `medias`，3 处 `init_logger(__name__)` 被 logger 检查拒绝。相关门禁问题据报告已在本地修复，并保留为 10 个主题 commit、无冲突 rebase 到最新 main；新 CI run `30504399399` 的原始 pre-commit hooks 已通过，但进入 mypy 后失败，精确类型错误及后续修复结果仍未给出。'
  title: CI 门禁修复后暴露 mypy 类型错误
- block_id: c0d949ad00782f0807c0bcb4301d5a842f0da4ac826c6e227dd7fe831221b5d3
  indexed: true
  sha256: 98793173e682b377957a03d8da871fec6201c81af0e4184f7fc3c44b676b2bad
  source_range:
    end: 17963650
    part: 0
    start: 17748710
  summary: 材料称首轮 mypy 因同一作用域复用 `target` 失败，后改为 `resolved_target`；随后 renderer 单测 fixture 被推断为 `object`，4 个 `in` 断言补充了明确映射类型。期间 upstream/main 从 `21382607d` 前进至 `b6fe31d11`，一次精确推送被阻止并完成 rebase，最终报告 Head 为 `08d057ece`、CI run 为 `30504886019`。材料报告 pre-commit 与 Python 3.10/3.11/3.12 mypy 已通过，CI 进入 `select-tests`，但设备用例因 SHA 重写重新排队，不能据此确认全部 required checks、长测、合并或最终正确性。
  title: mypy 修复后 CI 进入设备用例阶段
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3ed7b4bb42225b21c30625399e5e410278ab80de08aa90c7b12abf04f0f0743c
  kind: experience
  material_digest: 9f39826a8c042cda4a52305d9086ee312c2f5bf6c74dd64b4b9f7c0a0f975061
  revision: b0e1417ecf18f0cdb210aeed88b99d4a3cd4df15a2cd9cc9d102e72d28fac755
  schema: mindie-entry/3
  summary: 新材料补充了两类 mypy 失败及修复：生产代码中的可空查找结果类型问题，以及 renderer 单测 fixture 的 `object` 类型收窄问题；并记录了 upstream 前进、rebase 和最终 Head `08d057ece`。材料报告第三轮 pre-commit 与三个 Python 版本的 mypy 已通过，CI 已进入 `select-tests`，但 30 个设备用例因 SHA 重写重新排队，仍应独立核对该 run 的全部 required checks、设备长测、DCO、review、远端状态与合并状态；此前导航中的未完成项及本批材料的报告性质和不确定性仍然保留。
  title: 当前任务导航：mypy 据报告通过，设备用例与最终状态仍待核验
navigation: 新材料补充了两类 mypy 失败及修复：生产代码中的可空查找结果类型问题，以及 renderer 单测 fixture 的 `object` 类型收窄问题；并记录了 upstream 前进、rebase 和最终 Head `08d057ece`。材料报告第三轮 pre-commit 与三个 Python 版本的 mypy 已通过，CI 已进入 `select-tests`，但 30 个设备用例因 SHA 重写重新排队，仍应独立核对该 run 的全部 required checks、设备长测、DCO、review、远端状态与合并状态；此前导航中的未完成项及本批材料的报告性质和不确定性仍然保留。
schema: mindie-material-task/1
status: complete
task_id: 3ed7b4bb42225b21c30625399e5e410278ab80de08aa90c7b12abf04f0f0743c
---

# 当前任务导航：mypy 据报告通过，设备用例与最终状态仍待核验

新材料补充了两类 mypy 失败及修复：生产代码中的可空查找结果类型问题，以及 renderer 单测 fixture 的 `object` 类型收窄问题；并记录了 upstream 前进、rebase 和最终 Head `08d057ece`。材料报告第三轮 pre-commit 与三个 Python 版本的 mypy 已通过，CI 已进入 `select-tests`，但 30 个设备用例因 SHA 重写重新排队，仍应独立核对该 run 的全部 required checks、设备长测、DCO、review、远端状态与合并状态；此前导航中的未完成项及本批材料的报告性质和不确定性仍然保留。

## Materials

- [PR #12950 回退与全局语义风险审计](blocks/1b6f497342735590872f9e1df01cd14f5e6fc7f1ba7385488b17e2ac47dd818c.md): 只读审计确认 PR #12950（base `81d3450`、head `4a1857b`）完整回退了 KV Pool #12801 的 lease-key 去重、日志降级和 `use_layerwise` 文档，并局部恢复了不应全局生效的 `QuantType.NONE` MegaMoE fallback；这些结论包含 patch 对比和 CI 失败证据。审计还识别出 shared-expert-DP/FlashComm1 的 `globalBS invalid`、DeepSeek V4 token mismatch 强嫌疑链、全局 `g_proj` 匹配误伤 Kimi、A2 expert 数阈值、W8A8MXFP、FIA split、build 清理及广域 Mooncake/patch 风险，其中部分仍需运行验证，不能视为已证明根因。后续决定保留 FIA、W8 MXFP 和 build 清理，恢复 KV Pool 修复，收窄 SiTU fallback，移除无关 KV spec 重构，并系统性拆分 FC1 与 shared-expert-DP 语义。
- [FC1/共享专家解耦设计与验证准备](blocks/8f48f059f37975f07481338b589ab2e818e99cd7464d6ceb2ecc8936389ec975.md): 设计修正为模型无关的正交语义：FC1 表示两个完整 TP block 之间沿 token/sequence 轴的 gather 与 reduce-scatter，shared-expert-DP 只控制共享专家权重复制，不应改变 routed expert 的 MC2/All2All/AllGather token contract。静态检查和 mocked 回归已通过：FC1-only 44 项、shared-DP-only 13 项、FC1+shared-DP 33 passed/1 skipped；真实 DP2×TP4 尚未完成，不能将这些结果推广为真实分布式功能或精度成功。由于 153–156 曾被 Kubernetes DeepSeek-V4 服务占用，原 Kimi 服务已无存活进程；在用户要求保留量化任务后，已 cordon 节点并删除指定 K8s Pod，确认 NPU 进程为 0，随后启动三组真实 DP2×TP4 案例。
- [按用户标注落地范围与资源处置](blocks/8e7a596c03e664264f99238e365df3a72a8ffd99bf495923d7d0a3ed61da761f.md): 用户确认 KV Pool #12801 回退是严重问题，认可收窄 MegaMoE，要求保留 A5 FIA、W8A8MXFP 和 build 清理，删除 SFA/Sliding 纯重构，并要求分析 `g_proj` 隔离及 FC1/shared-expert-DP 的系统解耦；`AscendMLAAttentionSpec.merge()` invariant 暂不处理。验证计划先做三种 DP2×TP4 真实组合（FC1-only、shared-DP-only、FC1+shared-DP），全部通过后再做四机精度验证；此前仅 mocked/隔离源码结果已完成，真实案例刚进入运行阶段。用户明确要求 K8s 任务直接 kill、量化任务保留，已有记录显示指定 Pod 被删除且节点资源已释放，但真实运行结果仍未给出。
- [单机 A/B 门禁与四机启动前状态](blocks/2a7d62d3909930ee710249c67fa982a8072e157d6b67d20971ad6be2b9204b3f.md): A（FC1-only）与 B（shared-expert-DP-only）的材料报告了 worker、HTTP 200、无 HCCL/shape/globalBS/Traceback 等现象；未启用确定性计算时动态并发输出漂移被用户表示可忽略，但这些仍是来源中的验证记录。C 曾出现真实请求 hang，随后被停止，且未完成重启后的推理门禁；用户随后明确 shared-expert-DP 必须依赖 FC，因此 C 不是有效场景。四机 FC1+shared-expert-DP 启动曾完成 64/64 worker 和部分权重加载，但后续任务转向 prefix-cache 场景。
- [PR #51/#52/#53 纳入方式审计](blocks/b3aab110ca6eb2c7bf890b11ec35eca62878fca0428606b6e4223b1c576c538b.md): 只读审计报告称 PR #51 的 shared-expert width 校验当前未包含，需要移植 active fused_moe.py 逻辑并补 routed/shared width 不同的回归测试；旧版 fused_moe_0_23_0.py 不应恢复。PR #52 的 A2 `num_experts > 512`→`ALLGATHER` 产品 guard 已存在，仅缺回归测试；PR #53 的 W8/W4 MXFP 条件也已存在，建议补 W8 覆盖，不直接 cherry-pick。GitHub connector 被报告为无私库权限，审计因此使用本机认证的 gh，且当时未编辑、fetch 或推送。
- [最终候选与四机 prefix-cache/GPQA 进展](blocks/9c4c47ba46ebc359e6e4d8ac97d65964d6ddb9e4160ab5fc818c052151c1e9e0.md): TP16 5-layer 对照因量化描述引用不存在的第 6 层而在模型加载阶段失败，未进入目标 forward；随后材料报告将 upstream #13022 的 DP padding contract 回退纳入候选，并为 #51/#52/#53 补入逻辑或测试。候选代码据称通过远端聚焦测试（56 passed），四机 `prefix on + FC1 on + shared-DP off` 的 health、数学题及四个 DP rank 的 cold→hit 曾被报告为成功，且每个 rank 的 cached tokens 达到 1536；这些结果仍属于来源中的运行记录。GPQA-Diamond 198 题尚在进行中（材料截点 41/198，40 对 1 错、0 request error/missing），而 PR #12950 当时仍为单提交但处于 CONFLICTING/DIRTY，存在失败/取消检查且缺少人工 approval，尚未满足合并条件。
- [Kimi 公共路径去模型判断与 PR 初次更新](blocks/3aaea4e15ecb0af728ee2eaa47645bc86bd0e1c1c90ab41df0976a86e8f36272.md): 材料报告公共路径已将 Kimi/K3 身份判断改为能力、shape 或协议契约驱动，并保留 ModelRegistry、parser、renderer 等真正的模型/协议边界；但审计也指出 ModelSlim fallback、K3 packed mapping、renderer patch、A_log loader 和重复注册等风险，部分结论当时仍未全部落地。Qwen3.5 FlashComm+MTP 曾因 shared-expert-DP 被错误用作序列恢复条件而出现 `16 vs 8`，随后修复方向改为由 FlashComm 状态控制。PR 经多次 rebase/force-with-lease 更新，曾到 `a7901cd04`；单机 5 层 `/health=200`，但截断权重仅作启动验证，且后续 CI 仍暴露 Qwen MTP 问题。
- [协议边界保留与 Qwen3.5 CI 根因定位](blocks/1cc3ae55647435ebe601d7648c997d6ed95cad6d1d1090bdfa47740951e72a5c.md): 材料明确必须保留 `KimiK3For*` architecture key、`model_type`、checkpoint ABI mapping、parser 名称及 reasoning/tool/renderer 注册等协议或加载边界，不能将其与公共 MoE 执行路径混同。后续 CI 定位到 Qwen3.5-35B MTP 的 FlashComm 路径：shared-expert-DP 与 FlashComm 解耦后，proposer 仍依据错误状态恢复完整序列，导致 `16 vs 8`；服务和完整权重加载本身成功。服务器复现还经历缺少 CANN、显存不足、custom-op 路径未配置等环境阻碍，不能把这些失败当作代码失败。
- [Qwen MTP 通信语义修复与 CI 追踪](blocks/62a86930f7c83daf055d4cdf2f4f8713a764fdb5c393691921078d0bf10a0697.md): 复现进一步发现 `inputs_embeds`、multimodal 标签以及 `draft + VL` 的旧特判共同影响 token 形状；最终根因收敛为 shared expert 的 reduce-scatter 与 embedding 复制语义被同一 `maybe_*` 路径混用，导致 `16/8` 及后续 `4/2`。材料称通过独立公共 reduce-scatter 算子、移除模型分类特判并保留必要复制语义后，完整图模式和三道数学题（391、5、40）通过，无乱码或空结果；rebased tree 也完成等价验证。随后 `3c560c2e2` 因 CRLF/Ruff 格式问题修正为 `cf50b0513`，再加入 CPU UT mock 修复并 rebase 推到 `93b073decb`；CI 已报告关键 Qwen3.5、DeepSeek V4、FC1/prefix-cache 设备组通过，但当时仍有运行中的矩阵，最终全量状态未完全闭合。
- [CI 门禁修复后暴露 mypy 类型错误](blocks/60fd02bfbd4b792b76b4c45810f77262d525220b75d58abe80f30db9b23dd09c.md): PR #12950 最终 SHA `93b073decb` 的上一轮 CI 曾报告 19 个任务通过、11 个运行中、8 个排队且 0 个失败，但整轮尚未全绿；随后因拆分 commit 触发新的 pre-commit 规则，`codespell` 拒绝 `medias`，3 处 `init_logger(__name__)` 被 logger 检查拒绝。相关门禁问题据报告已在本地修复，并保留为 10 个主题 commit、无冲突 rebase 到最新 main；新 CI run `30504399399` 的原始 pre-commit hooks 已通过，但进入 mypy 后失败，精确类型错误及后续修复结果仍未给出。
- [mypy 修复后 CI 进入设备用例阶段](blocks/c0d949ad00782f0807c0bcb4301d5a842f0da4ac826c6e227dd7fe831221b5d3.md): 材料称首轮 mypy 因同一作用域复用 `target` 失败，后改为 `resolved_target`；随后 renderer 单测 fixture 被推断为 `object`，4 个 `in` 断言补充了明确映射类型。期间 upstream/main 从 `21382607d` 前进至 `b6fe31d11`，一次精确推送被阻止并完成 rebase，最终报告 Head 为 `08d057ece`、CI run 为 `30504886019`。材料报告 pre-commit 与 Python 3.10/3.11/3.12 mypy 已通过，CI 进入 `select-tests`，但设备用例因 SHA 重写重新排队，不能据此确认全部 required checks、长测、合并或最终正确性。

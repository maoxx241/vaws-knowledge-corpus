---
blocks:
- block_id: d0b3a99cabc30da49a20f919869f0f2cf1409c3a5fd2b6a06d19a75e8c17c110
  indexed: true
  sha256: ae518642914c3d5cc5831af6f67341d9b311426ffa9dbe34cd53b80d49c6e7a3
  source_range:
    end: 14739362
    part: 0
    start: 0
  summary: '围绕 Kimi K3 在 vLLM-Ascend 中实现 tool use 和 reasoning，确认其 XTML 协议、`encoding_k3.py`/`tokenization_kimi.py` 与 vLLM 0.23 注册缺口，并新增 `kimi_k3` parser、请求参数适配和测试。随后创建并多次 rebase PR #14，修复 `import regex as re` 等 CI 问题；后续采用正式 `KimiK3Renderer`，按 `model_type=kimi_k3` 自动选择并直接调用 `TikTokenTokenizer.apply_chat_template()`，移除手动 `--chat-template` 依赖。材料报告了大量本地、容器和远端验证结果，但四节点真实服务未在这些阶段启动或完成端到端生成，且部分 CI/验证过程仍有排队、环境和时序限制。'
  title: Kimi K3 parser适配与PR创建、正式Renderer方案
- block_id: 5c102b43e6723b15f85b279f14784eac0f33f9757ef33b1d6e254c6d7f9761f2
  indexed: true
  sha256: cb68114433aad104b333878bef8c0c2e0267bd7d1784f6e6298f1a322da05d4b
  source_range:
    end: 14739362
    part: 1
    start: 0
  summary: 'PR #14 从 `7d047c1b0` rebase 到 `bd8f769b0` 后恢复为 `MERGEABLE`，并通过 ruff、format、codespell、forbidden-imports 等检查；核心 lint 曾处于 runner 排队状态。随后正式 Renderer 方案更新到提交 `523b76f7c`，报告 24 个 K3 测试、真实 tokenizer 自动选择、多模态同步/异步路径及 pre-commit/mypy 通过。再次因 base force-push 从 `75300cd7f` 重放到 `0dde1ef85`，head 为 `33deb2609`，range-diff 显示功能补丁等价且冲突消失，但 PR Create 的系统依赖安装一度仍为 `UNSTABLE`。'
  title: PR rebase与K3 Renderer验证
- block_id: 79cdfbf9c001b09eaed56fb9856382bae76774ac23c61c0cb0df4ed1b3e0022a
  indexed: true
  sha256: 4ce31d8aea42ec3cac6ae8d6cbfe16e99f7ed26dce4860b0652a9e2073d6f60d
  source_range:
    end: 14739362
    part: 2
    start: 0
  summary: '验证任务 `019f8a35-2eb3-7cb3-a3a9-d05f9f9c80ee` 多次尝试启动 PR #14，先后遇到显存 admission、孤儿进程/启动时序、HCCL 默认端口 `16666` 冲突、DP handshake 5分钟限制及 NFS/devmm I/O 等环境问题；部分轮次完成 64-rank 初始化和 352-shard 权重加载，但未达到 health=200，未形成 PR 代码异常证据。针对任务 `019f8a6f-c2ac-71c1-a8e6-9e2ebf385f2d`，建议使用 `reasoning_effort=max`、`tool_choice=none`、`temperature=1.0`、`top_p=1.0`，评分只取 `message.content` 或流式 `delta.content`，不要再做通用 reasoning 剥离。最新请求是用 153 按最新 `ci-new` 加 PR 代码拉起服务并检查异常 warning/error；材料截至此处仅确认运行手册要求完整 TP16×DP4×EP64 集群，尚未报告该次启动结果。'
  title: 四节点验证、MMMU-Pro参数与153启动检查
- block_id: 0b7f015d49828015e70e04b86c958405bb821b8ac3d5e87e757ba24ed2e4d595
  indexed: true
  sha256: 414b8f76b5ade4c073119df70e185b500f284f897dbd7fd058951ec7b8a28678
  source_range:
    end: 14739362
    part: 3
    start: 0
  summary: 四节点代码已对齐 PR head `33deb2609`，进程和 worker 持续运行；153 的 DP RPC `14569` 已监听，但 API `8077` 曾在 ACLGraph capture 阶段延迟，随后服务启动并被外部 `SIGTERM` 终止，未确认是代码故障。只读分析显示普通无 tools 请求出现 `serving.py:1237` 的 ERROR fallback，初步归因于 K3 将 `tool_choice=none` 改写为 `auto`；BFCL 第5条则提供了完整 XTML 工具协议未被抽取的证据。后续审计进一步指出 `spaces_between_special_tokens`、renderer 二次编码和 fallback 控制流均需独立验证，不能把 workaround 或已报告启动成功视为最终验收。
  title: 四机启动与K3解析初步定位
- block_id: 85935f0ea1594c5c0d2df7cf8e81201d645e9d353c53c0f7fc61a1b27219c139
  indexed: true
  sha256: 3507a922a4fddf1818f927cbe9bb9a3b90a5585ab0e68667aaa9f9adc87ebba8
  source_range:
    end: 14739362
    part: 4
    start: 0
  summary: 两个只读 sol/max 审计结合 vLLM 0.23.0、Kimi K3 权重及 vLLM-Ascend 历史实现，确认当前适配属于 MUST FIX；未修改源码或操作四机服务。已确认的主要问题包括复合 XTML marker 被特殊 token 间空格破坏、renderer 通过 `tokenize=False` 拼接后再编码而丢失 `EncodeSegment.allow_special` 信任边界，以及 `none→auto` 导致无 tools 请求进入 `cannot determine` ERROR fallback。历史 DeepSeek V4/GLM5 实现支持保留原始 `tool_choice`、原生 token IDs、每 choice 独立状态机、增量解析和显式 flush；但这些经验不能直接替代 K3 的 XTML 实现。
  title: 独立审计确认Renderer与状态机存在P0
- block_id: 1b30b8b7abf3ffb415af04c04ac57faec12f33aadc3626588677b9b00f463bad
  indexed: true
  sha256: b4d61496a2d2969a63e2fda184e4a3dfe4689bf70fc23c28ea48af43d6603c1f
  source_range:
    end: 14739362
    part: 5
    start: 0
  summary: 建议将“XTML envelope 清理”和“是否允许工具调用”拆成两个维度：保留原始 `tool_choice`，`none` 仍可清理 wrapper 但必须返回正常 `tools_called=False`，避免任何伪造 auto 或隐藏 fallback。还需修复 required/named 的空成功、unknown/非法 JSON 静默吞错、`response_format` 静默降级、stream/non-stream 不一致、finished flush 和多模态 tokenization 边界。当前仍只有分析结论与回归要求，正式实现及真实 tokenizer/detokenizer、MMMU/GPQA/BFCL 隔离服务验收尚未完成；GPQA 两条已知证据是生成 1083/3266 tokens 但 output 为空，不能算修复成功。
  title: 正式修复边界与未完成验收矩阵
- block_id: 2a68c74254b1778ebebf34117d70ed419de098752c0ab04061125f7bbd400536
  indexed: true
  sha256: 00141bf91afac4d012fc263602e92531056e05215fa51bee98a10a9e129d4df9
  source_range:
    end: 14739362
    part: 6
    start: 0
  summary: 计划在独立 Tool Use 分支实现 K3-local unified XTML state machine、renderer 信任边界、多模态及严格工具语义；曾误用 `lala-153-k3` 创建临时 clone 并运行一次 pytest，随后停止，遗留目录未删除且无存活进程。随后本地真实 tokenizer/detokenizer 定向测试报告 58/58 通过，但只读审计仍发现 Responses 半支持、未跟踪运行时模块、`chat_template_kwargs`/`structured_outputs` 边界、显式 `null`、不完整 XTML、错误分类和 forced-tool 流非流一致性等问题；后续审计又确认 `MESSAGE_END` 严格性与显式 `tool_choice=null` 仍需处理，且尚无独立 commit 或端到端四机验证。
  title: K3 隔离修复与审计发现
- block_id: e0f1d359e7bf943861443f5ab874a14b57b34267c365d13fc37fbee25bc947cd
  indexed: true
  sha256: 0e1fec5d571475f3d36d436a4ac8fdf96426a2b1c80267cbd7bf74ecc80bf1bc
  source_range:
    end: 14739362
    part: 7
    start: 0
  summary: 审计指出 `final=True` 时缺失 `THINK_END`/`RESPONSE_END`、`MESSAGE_END` 的 malformed stop 仍可能被当作 HTTP 200 成功，且 parser 只有 `finished` 无法区分 `stop` 与 `length`；后续报告称 response/tools 路径已强制 `MESSAGE_END`，但这些结论均为只读审查或报告，未形成提交验证。另发现 vLLM 0.23 `ChatParams.with_defaults()` 会过滤显式 `tool_choice="auto"`，可能被服务端 `required` 默认值污染 prompt；正在考虑使用 K3 私有类型化控制键穿过 merge，但尚未证明修复完成。
  title: XTML 完整性与 auto 合并陷阱
- block_id: 8140a5112b1ca80dc27d83d65f066f519eacdc22aee1ed23e1738b9251ec6168
  indexed: true
  sha256: 70e7d04a49d2f94449b8a45a75580a1bda1804b089c06d94c5f566901ea664af
  source_range:
    end: 17745895
    part: 0
    start: 14739362
  summary: '本块记录 Kimi K3 在 vLLM 0.23 中的 XTML 工具解析实现、真实 tokenizer 回归及提交/PR 发布过程；曾报告 64 项测试通过、提交 `36fe4a442...`，并说明四机与 live benchmark 未执行。随后 GPQA 因把 `thinking` 放在 `chat_template_kwargs` 收到 HTTP 400，曾建议改用顶层 `reasoning_effort: "max"`；PR #22 的 CI 先因基线 Ruff 格式、runner 异常和 mypy 类型问题反复处理，最终 #22 合入时仍遗漏后续 `ToolMode` 类型修复，不能据此视为全部 CI 或端到端验证成功。'
  title: 'K3 工具解析修复、GPQA 契约与 PR #22 CI 追踪'
- block_id: e71bdad720c3e4a38f49f15dbbd8c676a979961528fc518e337d938e75d26aa2
  indexed: true
  sha256: d9cff6bf5adfe2a446f10f7d7045dce73768a89082b00082a6373fbdcee1f6d8
  source_range:
    end: 17745895
    part: 1
    start: 14739362
  summary: 'PR #23 从最新 `ci-new` 基线补入 `ToolMode` 及测试类型收窄修复；记录显示完整 pre-commit、Python 3.10/3.11/3.12 mypy 已通过，但 selected-tests 仍受自托管 NPU runner 排队和安装耗时影响，未形成完整成功结论。针对 HTTP 400 的追溯确认：Kimi K3 tokenizer 原生支持 `thinking`/`thinking_effort`，顶层 `reasoning_effort` 映射及冲突即 400 的保留字段策略来自本地 vLLM-Ascend 适配（PR #14/#22），并非 Kimi 官方强制要求；随后用户要求撤销这类额外拦截。'
  title: 'PR #23 类型修复排队与严格字段拦截来源分析'
- block_id: 005d30c981010888b975733746b20875d11f1ca600745b5495f8742a721ba81e
  indexed: true
  sha256: a37dfd0ced562113008226180cd2763a3086e02d7fadec7b6f1369b90c64aa25
  source_range:
    end: 17745895
    part: 2
    start: 14739362
  summary: '从最新 `ci-new` 基线单独创建的 PR #24 仅修改 3 个 K3 实现文件和 2 个对应单测，移除了额外 request-level 400 拦截，恢复保留 `chat_template_kwargs` 中的 `thinking`/`thinking_effort`，同时保留显式冲突与 XTML 输出正确性校验。材料报告合入提交 `7a74db55c...`、pre-commit 通过及真实 tokenizer/增量解码回归 64 passed；GitHub E2E 因 PR 立即合入而自动取消，不是测试失败，且未访问或重启四机。'
  title: '独立兼容修复 PR #24 已合入'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7705aa4803d7f6966524dc654137f31df2702790f61b81fe654517f78c2c5b38
  kind: experience
  material_digest: 4f24ce76fdc68086321053d4afdcab4f986d3704a37d53f4f8cc1cb936f546d4
  revision: b61f1232b432607ad31c5b5ea9c98ce1c735708e04f7af9ce1a8e7c806a5807c
  schema: mindie-entry/3
  summary: '当前材料显示：原先对 K3 `chat_template_kwargs` 保留字段返回 400 的规则是适配层新增策略，而非 Kimi K3 tokenizer 原生要求；该策略已通过独立 PR #24 移除并合入 `ci-new`，允许原生 `thinking`/`thinking_effort` kwargs，保留显式冲突和 XTML 输出校验。此前的 XTML 状态机、renderer、工具选择、多模态及 vLLM 0.23 边界审计仍是可检索背景；已报告的本地 64 项回归和 pre-commit 通过不等于四机或完整端到端成功，PR #23 的下游验证曾受 runner 队列影响，PR #24 的 GitHub E2E 则因合入自动取消。'
  title: K3 vLLM 0.23 兼容修复与验证导航
navigation: '当前材料显示：原先对 K3 `chat_template_kwargs` 保留字段返回 400 的规则是适配层新增策略，而非 Kimi K3 tokenizer 原生要求；该策略已通过独立 PR #24 移除并合入 `ci-new`，允许原生 `thinking`/`thinking_effort` kwargs，保留显式冲突和 XTML 输出校验。此前的 XTML 状态机、renderer、工具选择、多模态及 vLLM 0.23 边界审计仍是可检索背景；已报告的本地 64 项回归和 pre-commit 通过不等于四机或完整端到端成功，PR #23 的下游验证曾受 runner 队列影响，PR #24 的 GitHub E2E 则因合入自动取消。'
schema: mindie-material-task/1
status: complete
task_id: 7705aa4803d7f6966524dc654137f31df2702790f61b81fe654517f78c2c5b38
---

# K3 vLLM 0.23 兼容修复与验证导航

当前材料显示：原先对 K3 `chat_template_kwargs` 保留字段返回 400 的规则是适配层新增策略，而非 Kimi K3 tokenizer 原生要求；该策略已通过独立 PR #24 移除并合入 `ci-new`，允许原生 `thinking`/`thinking_effort` kwargs，保留显式冲突和 XTML 输出校验。此前的 XTML 状态机、renderer、工具选择、多模态及 vLLM 0.23 边界审计仍是可检索背景；已报告的本地 64 项回归和 pre-commit 通过不等于四机或完整端到端成功，PR #23 的下游验证曾受 runner 队列影响，PR #24 的 GitHub E2E 则因合入自动取消。

## Materials

- [Kimi K3 parser适配与PR创建、正式Renderer方案](blocks/d0b3a99cabc30da49a20f919869f0f2cf1409c3a5fd2b6a06d19a75e8c17c110.md): 围绕 Kimi K3 在 vLLM-Ascend 中实现 tool use 和 reasoning，确认其 XTML 协议、`encoding_k3.py`/`tokenization_kimi.py` 与 vLLM 0.23 注册缺口，并新增 `kimi_k3` parser、请求参数适配和测试。随后创建并多次 rebase PR #14，修复 `import regex as re` 等 CI 问题；后续采用正式 `KimiK3Renderer`，按 `model_type=kimi_k3` 自动选择并直接调用 `TikTokenTokenizer.apply_chat_template()`，移除手动 `--chat-template` 依赖。材料报告了大量本地、容器和远端验证结果，但四节点真实服务未在这些阶段启动或完成端到端生成，且部分 CI/验证过程仍有排队、环境和时序限制。
- [PR rebase与K3 Renderer验证](blocks/5c102b43e6723b15f85b279f14784eac0f33f9757ef33b1d6e254c6d7f9761f2.md): PR #14 从 `7d047c1b0` rebase 到 `bd8f769b0` 后恢复为 `MERGEABLE`，并通过 ruff、format、codespell、forbidden-imports 等检查；核心 lint 曾处于 runner 排队状态。随后正式 Renderer 方案更新到提交 `523b76f7c`，报告 24 个 K3 测试、真实 tokenizer 自动选择、多模态同步/异步路径及 pre-commit/mypy 通过。再次因 base force-push 从 `75300cd7f` 重放到 `0dde1ef85`，head 为 `33deb2609`，range-diff 显示功能补丁等价且冲突消失，但 PR Create 的系统依赖安装一度仍为 `UNSTABLE`。
- [四节点验证、MMMU-Pro参数与153启动检查](blocks/79cdfbf9c001b09eaed56fb9856382bae76774ac23c61c0cb0df4ed1b3e0022a.md): 验证任务 `019f8a35-2eb3-7cb3-a3a9-d05f9f9c80ee` 多次尝试启动 PR #14，先后遇到显存 admission、孤儿进程/启动时序、HCCL 默认端口 `16666` 冲突、DP handshake 5分钟限制及 NFS/devmm I/O 等环境问题；部分轮次完成 64-rank 初始化和 352-shard 权重加载，但未达到 health=200，未形成 PR 代码异常证据。针对任务 `019f8a6f-c2ac-71c1-a8e6-9e2ebf385f2d`，建议使用 `reasoning_effort=max`、`tool_choice=none`、`temperature=1.0`、`top_p=1.0`，评分只取 `message.content` 或流式 `delta.content`，不要再做通用 reasoning 剥离。最新请求是用 153 按最新 `ci-new` 加 PR 代码拉起服务并检查异常 warning/error；材料截至此处仅确认运行手册要求完整 TP16×DP4×EP64 集群，尚未报告该次启动结果。
- [四机启动与K3解析初步定位](blocks/0b7f015d49828015e70e04b86c958405bb821b8ac3d5e87e757ba24ed2e4d595.md): 四节点代码已对齐 PR head `33deb2609`，进程和 worker 持续运行；153 的 DP RPC `14569` 已监听，但 API `8077` 曾在 ACLGraph capture 阶段延迟，随后服务启动并被外部 `SIGTERM` 终止，未确认是代码故障。只读分析显示普通无 tools 请求出现 `serving.py:1237` 的 ERROR fallback，初步归因于 K3 将 `tool_choice=none` 改写为 `auto`；BFCL 第5条则提供了完整 XTML 工具协议未被抽取的证据。后续审计进一步指出 `spaces_between_special_tokens`、renderer 二次编码和 fallback 控制流均需独立验证，不能把 workaround 或已报告启动成功视为最终验收。
- [独立审计确认Renderer与状态机存在P0](blocks/85935f0ea1594c5c0d2df7cf8e81201d645e9d353c53c0f7fc61a1b27219c139.md): 两个只读 sol/max 审计结合 vLLM 0.23.0、Kimi K3 权重及 vLLM-Ascend 历史实现，确认当前适配属于 MUST FIX；未修改源码或操作四机服务。已确认的主要问题包括复合 XTML marker 被特殊 token 间空格破坏、renderer 通过 `tokenize=False` 拼接后再编码而丢失 `EncodeSegment.allow_special` 信任边界，以及 `none→auto` 导致无 tools 请求进入 `cannot determine` ERROR fallback。历史 DeepSeek V4/GLM5 实现支持保留原始 `tool_choice`、原生 token IDs、每 choice 独立状态机、增量解析和显式 flush；但这些经验不能直接替代 K3 的 XTML 实现。
- [正式修复边界与未完成验收矩阵](blocks/1b30b8b7abf3ffb415af04c04ac57faec12f33aadc3626588677b9b00f463bad.md): 建议将“XTML envelope 清理”和“是否允许工具调用”拆成两个维度：保留原始 `tool_choice`，`none` 仍可清理 wrapper 但必须返回正常 `tools_called=False`，避免任何伪造 auto 或隐藏 fallback。还需修复 required/named 的空成功、unknown/非法 JSON 静默吞错、`response_format` 静默降级、stream/non-stream 不一致、finished flush 和多模态 tokenization 边界。当前仍只有分析结论与回归要求，正式实现及真实 tokenizer/detokenizer、MMMU/GPQA/BFCL 隔离服务验收尚未完成；GPQA 两条已知证据是生成 1083/3266 tokens 但 output 为空，不能算修复成功。
- [K3 隔离修复与审计发现](blocks/2a68c74254b1778ebebf34117d70ed419de098752c0ab04061125f7bbd400536.md): 计划在独立 Tool Use 分支实现 K3-local unified XTML state machine、renderer 信任边界、多模态及严格工具语义；曾误用 `lala-153-k3` 创建临时 clone 并运行一次 pytest，随后停止，遗留目录未删除且无存活进程。随后本地真实 tokenizer/detokenizer 定向测试报告 58/58 通过，但只读审计仍发现 Responses 半支持、未跟踪运行时模块、`chat_template_kwargs`/`structured_outputs` 边界、显式 `null`、不完整 XTML、错误分类和 forced-tool 流非流一致性等问题；后续审计又确认 `MESSAGE_END` 严格性与显式 `tool_choice=null` 仍需处理，且尚无独立 commit 或端到端四机验证。
- [XTML 完整性与 auto 合并陷阱](blocks/e0f1d359e7bf943861443f5ab874a14b57b34267c365d13fc37fbee25bc947cd.md): 审计指出 `final=True` 时缺失 `THINK_END`/`RESPONSE_END`、`MESSAGE_END` 的 malformed stop 仍可能被当作 HTTP 200 成功，且 parser 只有 `finished` 无法区分 `stop` 与 `length`；后续报告称 response/tools 路径已强制 `MESSAGE_END`，但这些结论均为只读审查或报告，未形成提交验证。另发现 vLLM 0.23 `ChatParams.with_defaults()` 会过滤显式 `tool_choice="auto"`，可能被服务端 `required` 默认值污染 prompt；正在考虑使用 K3 私有类型化控制键穿过 merge，但尚未证明修复完成。
- [K3 工具解析修复、GPQA 契约与 PR #22 CI 追踪](blocks/8140a5112b1ca80dc27d83d65f066f519eacdc22aee1ed23e1738b9251ec6168.md): 本块记录 Kimi K3 在 vLLM 0.23 中的 XTML 工具解析实现、真实 tokenizer 回归及提交/PR 发布过程；曾报告 64 项测试通过、提交 `36fe4a442...`，并说明四机与 live benchmark 未执行。随后 GPQA 因把 `thinking` 放在 `chat_template_kwargs` 收到 HTTP 400，曾建议改用顶层 `reasoning_effort: "max"`；PR #22 的 CI 先因基线 Ruff 格式、runner 异常和 mypy 类型问题反复处理，最终 #22 合入时仍遗漏后续 `ToolMode` 类型修复，不能据此视为全部 CI 或端到端验证成功。
- [PR #23 类型修复排队与严格字段拦截来源分析](blocks/e71bdad720c3e4a38f49f15dbbd8c676a979961528fc518e337d938e75d26aa2.md): PR #23 从最新 `ci-new` 基线补入 `ToolMode` 及测试类型收窄修复；记录显示完整 pre-commit、Python 3.10/3.11/3.12 mypy 已通过，但 selected-tests 仍受自托管 NPU runner 排队和安装耗时影响，未形成完整成功结论。针对 HTTP 400 的追溯确认：Kimi K3 tokenizer 原生支持 `thinking`/`thinking_effort`，顶层 `reasoning_effort` 映射及冲突即 400 的保留字段策略来自本地 vLLM-Ascend 适配（PR #14/#22），并非 Kimi 官方强制要求；随后用户要求撤销这类额外拦截。
- [独立兼容修复 PR #24 已合入](blocks/005d30c981010888b975733746b20875d11f1ca600745b5495f8742a721ba81e.md): 从最新 `ci-new` 基线单独创建的 PR #24 仅修改 3 个 K3 实现文件和 2 个对应单测，移除了额外 request-level 400 拦截，恢复保留 `chat_template_kwargs` 中的 `thinking`/`thinking_effort`，同时保留显式冲突与 XTML 输出正确性校验。材料报告合入提交 `7a74db55c...`、pre-commit 通过及真实 tokenizer/增量解码回归 64 passed；GitHub E2E 因 PR 立即合入而自动取消，不是测试失败，且未访问或重启四机。

---
blocks:
- block_id: 950fdc0c0a99da872e160113372c8b14e8f621ae0b579d800f1200fb5d77b04e
  indexed: true
  sha256: 7502fc5728b25e1650cf448991cef3077d590a6c22586470c4160fcbb61037ec
  source_range:
    end: 12455122
    part: 0
    start: 0
  summary: 完成了对本地 Codex 历史记录的机器筛查和候选整理：约 549 个会话、553 个文件，形成 72 个知识候选；随后对照 7 个仓库当前 main，19 条重写保留、46 条剔除、7 条暂缓，25 条旧记录未恢复。之后设计并实现经验与知识的独立目录、索引和查询入口，自动 session 总结默认进入经验；测试和真实 OpenViking 联测有通过记录，但改动尚未切换现有环境或发布。后续开始把历史材料整理为经验，期间出现配置覆盖、旧客户端混查等问题及修复记录，均不应自动视为当前实现保证。
  title: 从全量会话扫描到经验/知识分流设计
- block_id: 637717e3796e359dc3c91a72ef4404be350307632d30839c7c7a324ed74024a8
  indexed: true
  sha256: 0343807fd11b3c7d96df41b93cb486f8f211b4488b26a94c3feba6554d4ad2fa
  source_range:
    end: 12455122
    part: 1
    start: 0
  summary: 经验整理曾得到 10 篇公共算子经验、29 篇通用经验和 62 篇候选，缺证据、重复、私有实现或第三方内容被 held、合并或删除；随后按授权对 vLLM Ascend 及其衍生私仓案例做匿名化抽象。分发链改为 `corpus/knowledge/` 与 `corpus/experience/`，采用新 schema `/2` 和 `content.layout=kinds/v1`，并按用户要求接受破坏性格式变更；报告称相关测试通过，但内容仍经历多轮复核和重放。跨 lane 后曾保留 64 篇，Kimi 主题冻结为 25 篇、2 项合并、3 项暂缓，之后又恢复匿名图档诊断经验及新增算子经验，最终数量随筛选规则变化，不能把中间数字当作最终事实。
  title: 经验语料整理、分发改造与提交准备
- block_id: 92a7fbae67b7912f4ad80805884c1a9f6319bea20f7ba8c1f7d16204af862b27
  indexed: true
  sha256: d9ece2a6fbcbae29f4530ef9605e0c6b9bb1be618ca43bea93ada62a14c2f97e
  source_range:
    end: 12455122
    part: 2
    start: 0
  summary: '用户明确允许将脱敏后的匿名私有模型经验及 vLLM Ascend 衍生私仓经验抽象贡献，同时要求移除 VAWS 自身工具、控制面、客户端和知识组件开发案例。最终报告称保留 50 篇业务经验（Kimi K3 26、算子 21、其他模型与量化 3），剔除 26 篇 VAWS 开发经验，并提交语料 PR #3；配套 workspace PR #150 的 CI 也被报告为通过、待人工合入。材料中仍保留了未完成设备验收、证据不足、旧结论被撤回和第三方实现被排除等边界，因此 PR 中经验较简单是否符合预期仍是待讨论的质量与粒度问题，而非已验证结论。'
  title: 提交业务经验并排除 VAWS 开发内容
- block_id: efd70315a07668556c7fb85b350b787c79f95fbd2ad883eeda550989f5a3d2c9
  indexed: true
  sha256: ff5c572800064bc6b13014893879913181207f67557fb7ad4d31ce30b7e8f33f
  source_range:
    end: 22543990
    part: 0
    start: 12455122
  summary: 围绕 50 篇经验稿进行了逐篇补证、交叉审阅和脱敏复核，材料提到 KDA、FIA、KV cache、causal conv、INT8 fusion 等案例的故障边界、公式、布局、调用链和验证限制；其中部分结果仍是历史报告或审阅结论，不能替代当前独立核验。后续文档身份方案改为按稳定 public_relpath 维护：知识使用分类固定路径，经验保留独立案例 ID，标题或正文修订更新原文件，content_digest 仅用于变化、幂等和校验。相关实现曾报告本地测试、模拟 GitHub 和多平台 CI 通过，但也明确未完成真实远端写入或合并，且曾出现分支复用、并发 pending 覆盖、候选按标题匹配等待修复风险。
  title: 经验语料深审与稳定文档身份
- block_id: 21362c3034449933cb82a865928c47e5014ef524ced6b4ecc8e48ba9a6e22e15
  indexed: true
  sha256: 7a139c625892934d846f1347c065398a37e051906889086b30e9595e10285900
  source_range:
    end: 22543990
    part: 1
    start: 12455122
  summary: 修订生命周期要求首次贡献固定路径、开放 PR 原路径更新、合并或关闭后从最新 upstream 建新分支，并通过显式 Git 路径或候选引用关联跨 Agent 修改；内容摘要不同应重新进入待提交状态，不能用标题、相似内容或新摘要推断身份。材料列出了 submit.py、publishing.py、capture.py、gitops.py 的提前返回、轮询和候选匹配问题，以及 queue_capture 与提交回写之间的锁竞争风险，并记录了大量本地验收和独立审查结果，但未声称完成真实 GitHub 验收。经验反馈最初考虑 GitHub Issue/reactions，随后改为轻量 `experience_feedback(ref, vote)`；反馈被定义为使用事件，+1 和 -1 均可重复累计，理由可省略，重试需用 request_id 去重。
  title: 稳定路径生命周期与经验反馈设计
- block_id: c42ca0adb25fcee33681697d0a715d3dad187e2b1a1e441fafdd9ebb8bad7c7e
  indexed: true
  sha256: 6be83e167086c6b0b18c353c2f16a7f2326ef873386e51d7ff0fddd274ec1cfd
  source_range:
    end: 22543990
    part: 2
    start: 12455122
  summary: 独立审查材料称同一账号的 +1、-1 都按独立评论事件累计，历史反馈不删除；相同作者和 request_id 的重试只计一次，普通 Issue 讨论不会计入票数，响应丢失可读回或返回可重试 ID。已报告覆盖跨作者隔离、并发重复评论、授权和失败恢复，并发现边界：整条响应丢失且后续调用未携带原事件 ID 时会被视为新反馈。后续记录声称实现、MCP/CLI、依赖 pin、语料检查和三个 PR 的 CI 均通过，52 篇语料路径和内容保持不变，但这些是来源中的验收报告，PR 仍标为待人工审核、尚未合并，且个别记录曾出现 Windows 时序失败和 GitHub API 限流。
  title: 正负反馈事件累计与最终验证状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a1ef34da9ef25f9e281b9954e6c1cffc33fd0476306b6889381dbecd0c712b51
  kind: experience
  material_digest: 63e29524cb43020ca303ed4157668d1d84f01a5adc7f4c9308d86a18c96be1d2
  revision: 75a1bcf09d2b3483650dc507173135706d2783facc534c3a61aad7f1d432528c
  schema: mindie-entry/3
  summary: 当前主线已从经验稿深审扩展到贡献生命周期：知识按分类固定路径维护，经验按稳定案例 ID 或 public_relpath 修订，content_digest 只用于检测变化和幂等，不决定身份。需继续保留原始失败、并发覆盖、候选误匹配、分支基线和真实远端未验证等边界；历史材料报告了 50/52 篇语料、多个本地测试和三项 PR CI 通过，但 PR 尚未合并，数量、提交、pin 和平台状态应以当前仓库为准。新增反馈机制记录经验使用事件，+1/-1 均可重复累计且不要求理由，同一 request_id 重试去重；响应完全丢失且没有事件 ID 时仍可能重复计数，普通 Issue 不应被误算为反馈。
  title: 稳定身份语料贡献与可重复经验反馈
navigation: 当前主线已从经验稿深审扩展到贡献生命周期：知识按分类固定路径维护，经验按稳定案例 ID 或 public_relpath 修订，content_digest 只用于检测变化和幂等，不决定身份。需继续保留原始失败、并发覆盖、候选误匹配、分支基线和真实远端未验证等边界；历史材料报告了 50/52 篇语料、多个本地测试和三项 PR CI 通过，但 PR 尚未合并，数量、提交、pin 和平台状态应以当前仓库为准。新增反馈机制记录经验使用事件，+1/-1 均可重复累计且不要求理由，同一 request_id 重试去重；响应完全丢失且没有事件 ID 时仍可能重复计数，普通 Issue 不应被误算为反馈。
schema: mindie-material-task/1
status: complete
task_id: a1ef34da9ef25f9e281b9954e6c1cffc33fd0476306b6889381dbecd0c712b51
---

# 稳定身份语料贡献与可重复经验反馈

当前主线已从经验稿深审扩展到贡献生命周期：知识按分类固定路径维护，经验按稳定案例 ID 或 public_relpath 修订，content_digest 只用于检测变化和幂等，不决定身份。需继续保留原始失败、并发覆盖、候选误匹配、分支基线和真实远端未验证等边界；历史材料报告了 50/52 篇语料、多个本地测试和三项 PR CI 通过，但 PR 尚未合并，数量、提交、pin 和平台状态应以当前仓库为准。新增反馈机制记录经验使用事件，+1/-1 均可重复累计且不要求理由，同一 request_id 重试去重；响应完全丢失且没有事件 ID 时仍可能重复计数，普通 Issue 不应被误算为反馈。

## Materials

- [从全量会话扫描到经验/知识分流设计](blocks/950fdc0c0a99da872e160113372c8b14e8f621ae0b579d800f1200fb5d77b04e.md): 完成了对本地 Codex 历史记录的机器筛查和候选整理：约 549 个会话、553 个文件，形成 72 个知识候选；随后对照 7 个仓库当前 main，19 条重写保留、46 条剔除、7 条暂缓，25 条旧记录未恢复。之后设计并实现经验与知识的独立目录、索引和查询入口，自动 session 总结默认进入经验；测试和真实 OpenViking 联测有通过记录，但改动尚未切换现有环境或发布。后续开始把历史材料整理为经验，期间出现配置覆盖、旧客户端混查等问题及修复记录，均不应自动视为当前实现保证。
- [经验语料整理、分发改造与提交准备](blocks/637717e3796e359dc3c91a72ef4404be350307632d30839c7c7a324ed74024a8.md): 经验整理曾得到 10 篇公共算子经验、29 篇通用经验和 62 篇候选，缺证据、重复、私有实现或第三方内容被 held、合并或删除；随后按授权对 vLLM Ascend 及其衍生私仓案例做匿名化抽象。分发链改为 `corpus/knowledge/` 与 `corpus/experience/`，采用新 schema `/2` 和 `content.layout=kinds/v1`，并按用户要求接受破坏性格式变更；报告称相关测试通过，但内容仍经历多轮复核和重放。跨 lane 后曾保留 64 篇，Kimi 主题冻结为 25 篇、2 项合并、3 项暂缓，之后又恢复匿名图档诊断经验及新增算子经验，最终数量随筛选规则变化，不能把中间数字当作最终事实。
- [提交业务经验并排除 VAWS 开发内容](blocks/92a7fbae67b7912f4ad80805884c1a9f6319bea20f7ba8c1f7d16204af862b27.md): 用户明确允许将脱敏后的匿名私有模型经验及 vLLM Ascend 衍生私仓经验抽象贡献，同时要求移除 VAWS 自身工具、控制面、客户端和知识组件开发案例。最终报告称保留 50 篇业务经验（Kimi K3 26、算子 21、其他模型与量化 3），剔除 26 篇 VAWS 开发经验，并提交语料 PR #3；配套 workspace PR #150 的 CI 也被报告为通过、待人工合入。材料中仍保留了未完成设备验收、证据不足、旧结论被撤回和第三方实现被排除等边界，因此 PR 中经验较简单是否符合预期仍是待讨论的质量与粒度问题，而非已验证结论。
- [经验语料深审与稳定文档身份](blocks/efd70315a07668556c7fb85b350b787c79f95fbd2ad883eeda550989f5a3d2c9.md): 围绕 50 篇经验稿进行了逐篇补证、交叉审阅和脱敏复核，材料提到 KDA、FIA、KV cache、causal conv、INT8 fusion 等案例的故障边界、公式、布局、调用链和验证限制；其中部分结果仍是历史报告或审阅结论，不能替代当前独立核验。后续文档身份方案改为按稳定 public_relpath 维护：知识使用分类固定路径，经验保留独立案例 ID，标题或正文修订更新原文件，content_digest 仅用于变化、幂等和校验。相关实现曾报告本地测试、模拟 GitHub 和多平台 CI 通过，但也明确未完成真实远端写入或合并，且曾出现分支复用、并发 pending 覆盖、候选按标题匹配等待修复风险。
- [稳定路径生命周期与经验反馈设计](blocks/21362c3034449933cb82a865928c47e5014ef524ced6b4ecc8e48ba9a6e22e15.md): 修订生命周期要求首次贡献固定路径、开放 PR 原路径更新、合并或关闭后从最新 upstream 建新分支，并通过显式 Git 路径或候选引用关联跨 Agent 修改；内容摘要不同应重新进入待提交状态，不能用标题、相似内容或新摘要推断身份。材料列出了 submit.py、publishing.py、capture.py、gitops.py 的提前返回、轮询和候选匹配问题，以及 queue_capture 与提交回写之间的锁竞争风险，并记录了大量本地验收和独立审查结果，但未声称完成真实 GitHub 验收。经验反馈最初考虑 GitHub Issue/reactions，随后改为轻量 `experience_feedback(ref, vote)`；反馈被定义为使用事件，+1 和 -1 均可重复累计，理由可省略，重试需用 request_id 去重。
- [正负反馈事件累计与最终验证状态](blocks/c42ca0adb25fcee33681697d0a715d3dad187e2b1a1e441fafdd9ebb8bad7c7e.md): 独立审查材料称同一账号的 +1、-1 都按独立评论事件累计，历史反馈不删除；相同作者和 request_id 的重试只计一次，普通 Issue 讨论不会计入票数，响应丢失可读回或返回可重试 ID。已报告覆盖跨作者隔离、并发重复评论、授权和失败恢复，并发现边界：整条响应丢失且后续调用未携带原事件 ID 时会被视为新反馈。后续记录声称实现、MCP/CLI、依赖 pin、语料检查和三个 PR 的 CI 均通过，52 篇语料路径和内容保持不变，但这些是来源中的验收报告，PR 仍标为待人工审核、尚未合并，且个别记录曾出现 Windows 时序失败和 GitHub API 限流。

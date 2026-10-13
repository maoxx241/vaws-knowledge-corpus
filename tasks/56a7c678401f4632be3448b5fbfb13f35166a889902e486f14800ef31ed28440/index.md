---
blocks:
- block_id: 998160092a6fc7362fb9086bfdc55690dc97fe8bd4fe0e2b03e5871c6fe17289
  indexed: true
  sha256: aa99382d1511c0fddcaacf4ae0dcfa7604211c821ef4a64c8e5c61b937f6c90d
  source_range:
    end: 8772063
    part: 0
    start: 0
  summary: 监控从 rebase 后 head `5e327cb7…` 开始，经历 mypy 的 `moe_mlp.py:725` Optional `beta` 类型错误，以及 head `4a96af94…` 上由全局 Ascend `MagicMock` 日志目录导致的 `FileExistsError`；两次均通知原任务并按 head、job、错误签名去重。修复后 head 更新为 `17d24917…`，PR 保持 OPEN/MERGEABLE；本地 mypy、CPU 定向 UT 和格式检查报告通过，但 E2E 仍未完成，不能据此视为整体成功。后续监控转入 acceptance 失败调查，且 `BLOCKED` 不被单独当作真实冲突。
  title: 'PR #14454 只读 CI 监控与两次代码相关失败'
- block_id: 21d6680cbe33e64e8fbc338b4f409d5b1f1ccaff7464813708bec2aec67cbc6c
  indexed: true
  sha256: 317746e317f730e4be8dafdaf399f42054aefac6af042f8f9f61bea031f33f37
  source_range:
    end: 8772063
    part: 1
    start: 0
  summary: CPU UT 失败被定位为 `test_linear.py` 留下全局 Ascend mock，SiTU 参考值构造完整 `VllmConfig` 后触发日志配置；修复仅调整既有测试为 mock `VllmConfig`、真实 `CompilationConfig`，并禁用参考值的 native compile，未改生产 logger/算子。父 PR 新 head 为 `17d24917…`，组合回归 67 passed，相关 mypy 与 format 检查报告通过；随后多数 E2E 分片成功，但 NPU/310p 等分片曾处于运行或排队状态。DSpark/DeepSeek-V4 acceptance 曾在 job `98506513821` 以 `acceptance_per_pos` 未达 golden 失败，位置 2 为 `0.7009029`（通过线 `0.71`，容差 `0.03`）；该结果的代码根因未确定，且与其他版本/历史 job 的通过结果不一致。
  title: MagicMock 日志配置测试修复后的 CI 监控
- block_id: 8d64b73109f367ce8fcff690362978ce4e24d81b55cbec1d8b8aad3f5c2d8933
  indexed: true
  sha256: 72369c3f83ce60fbde14198c319c28c1cedc28d8de230db0acba2452bd10b029
  source_range:
    end: 8772063
    part: 2
    start: 0
  summary: 原 acceptance 失败记录被保留；调查认为当前仅位置 2 未达标，不能由历史通过 job 证明无回归，且失败 job 的原样复跑因 `gh run rerun` 不可用及 REST 返回 `403 Must have admin rights to Repository` 而受权限阻塞。之后 maintainer 触发同一 head 的 E2E `attempt=2`，新 job `98534958344` 最终通过，确认原 acceptance 失败复跑成功。当前报告称活动 checks 均为 SUCCESS 或矩阵 SKIPPED，但 PR 仍 OPEN/MERGEABLE，监控继续进行；没有确认真实 `CONFLICTING`。
  title: Acceptance 复跑权限阻塞后成功
- block_id: 548ba8d62138c0c3eb71b30b77edaaa33ae2b8149100bc05fc402f90f6070133
  indexed: true
  sha256: 90abaf317d62689d3be8392652e2f7ae3cab0ecd62543a6d8741d99cfa92d9c6
  source_range:
    end: 8772063
    part: 3
    start: 0
  summary: '只读监控记录显示，PR #14454 从旧 head 切换到 `b106ea1b48dec9cd1e4b7d3107e2dca24b242df0` 后仍为 OPEN/MERGEABLE，基础检查通过，E2E 分片一度运行或排队，未据此判定新 head 全绿。随后切换到 `5dc5cb302cfea8d03c2786d0007f92695ddd0e29`，发现 `select-tests` 因 `tests/ut/ops/a3_2/test_kimi_kda_fused_norm_gate.py` 未登记 `estimated_times` 而失败，job `98589476494`，该问题被报告后又切换到 `3b6c845bbed21e786196272b2bb25250b67d1a99`。来源还记录了无 RoPE MLA 的 `None cos/sin` 切片 TypeError 修复、FLA/KDA fused RMSNorm gate 注册及相关 CPU/NPU/format/mypy 检查，但这些是报告的验证结果，不替代新 head 的完整 CI 结论。'
  title: 'PR #14454 新 head 与 MLA 修复监控'
- block_id: b8aafbaf51cb8f0c20fd35f0cebefa15becf52d97f85a8821f8c1e9207646597
  indexed: true
  sha256: e6575b258d818e8b27218413dc15d7ad67c7b897662a95106545ae644d262420
  source_range:
    end: 8772063
    part: 4
    start: 0
  summary: 在 head `3b6c845bbed21e786196272b2bb25250b67d1a99` 上，PR 仍 OPEN/MERGEABLE，`BLOCKED` 被持续区分为非真实冲突；基础检查、`select-tests`、缓存准备及部分 CPU 分片报告成功，其余 E2E 分片曾运行或排队。用户报告 A2 dummy DSpark 用例仍仅处于设计阶段，未编写或提交；截至该批次末尾，未报告新的代码失败或真实冲突。
  title: 新 head 3b6c845b 的 CI 运行状态
- block_id: 35e5ad5dca45b20b68fe913013110b7b86327ffafff8297a49ac65db8fd7bc6d
  indexed: true
  sha256: 62df75115887a216fed7153e3b0f1f698b4a91acc67286038f62c428678b3777
  source_range:
    end: 8772063
    part: 5
    start: 0
  summary: head `3b6c845b…` 的 ba07e4a 分片出现 DSpark DeepSeek-V4 acceptance mismatch：job `98593673963` 在 `test_dspark_deepseekv4.py:126` 报位置 2 为 `0.7009029`、golden `0.74`，容差后仍失败；同一 run 的 v0.27.1 对应 a3-800i-4 分片报告执行同测试并 `3 passed`，但日志没有最终聚合 `acceptance_per_pos`，不能仅凭绿色 job 推断最终采信率。随后新增 v0.27.1 A2 Qwen3/MRV2 失败，job `98593673921` 在 `tests/e2e/pull_request/one_card/model_runner_v2/test_basic.py:248` 出现 `1 failed / 13 passed` 的 golden mismatch；来源称尚未找到可证明的代码根因，当前 run `33093176000` 仍有分片运行，未执行主动复跑或修改。
  title: DSpark acceptance 失败与未收敛分片
- block_id: df51c7d2540871c6a1b8aa2f0a5bb4f34abf7bc38f6d259c9f2e846dd4633ac4
  indexed: true
  sha256: 15675bd86ae9d02b6e317e0b064f7c38f2183f0897be257f33b98921053ae892
  source_range:
    end: 8772063
    part: 6
    start: 0
  summary: 监控先后切换至 `8c8deed`、`b6dce8c` 和 `ba14b852`；PR 始终报告 OPEN/MERGEABLE，`BLOCKED` 未被视为真实冲突，旧 head 结果未沿用。`8c8deed` 曾出现 CPU UT 的 `supports_mm_inputs` 未初始化 AttributeError，后续 head 由 fixture 补 `False`；`ba14b852` 的 v0.27.1 A2 EAGLE acceptance 出现临界失败，`acceptance_per_pos` 首位约 `0.3294` 低于 `0.33` 下界，其余位置满足容差。父任务判断该结果可能是临界波动，要求不改代码或 golden，并要求 run `33106234415` 全部终态后再评估原样 `/rerun`；截至材料末尾尚无成功复跑。
  title: 'PR #14454 多次换 head 的只读 CI 监控'
- block_id: 9b5775d9a01286b2b1af1743c58cf61883cb743696c2c68cdaa2b2fc0ab93b8d
  indexed: true
  sha256: a5e78d59f45217bad8a282bb040fff75bc17876548497547472af7caa9bd4a54
  source_range:
    end: 8772063
    part: 7
    start: 0
  summary: 在 head `ba14b852`、PR OPEN/MERGEABLE 的背景下，run `33106234415` 长时间处于 queued 或部分运行状态，A2 part5 与两个 `<redacted:ticket-id:7a9ce89c9955>` job 曾未完成；已知唯一代码相关失败是 v0.27.1 A2 EAGLE job `98639485749`。父任务明确要求把 run 全部终态作为独立通知事件，并在确认无其他新失败、无人触发 attempt2 后由父任务决定是否发一次官方 `/rerun`；此前 `gh run rerun --job98639485749` 返回 `job cannot be rerun`，不能视为复跑已启动。材料中没有确认 run 已完成、attempt2 已产生或复跑成功。
  title: run 33106234415 等待终态与 EAGLE 复跑约束
- block_id: f4cc1c0b99645b2dd568af0735b0615ec7fb8c67350e857390dfa37cbe69f2b0
  indexed: true
  sha256: 616ffa8045bcbd98afacd5b5d4982b0bd168176d0e711b55a34d4dc4239e468e
  source_range:
    end: 8772063
    part: 8
    start: 0
  summary: 截至 2026-08-27 22:07，head `ba14b852`、base 未变，PR 仍 OPEN/MERGEABLE；run `33106234415` 仍报告 queued，两个 `<redacted:ticket-id:7a9ce89c9955>` job 仍排队。已知唯一失败仍是先前报告的 v0.27.1 A2 EAGLE job，未发现新的失败或真实合并冲突。该状态不是全部 CI 通过，也没有证据表明复跑已发起。
  title: run 33106234415 持续排队且无新增结论
- block_id: ea85a40bb21436ebb517ff2e62ca0e3e7c81136fbcecee684fe00723585b6db5
  indexed: true
  sha256: 2b92498a67d0022f47bec024f76875893ae5c884a700a18f0737de7f8576fafe
  source_range:
    end: 8772063
    part: 9
    start: 0
  summary: 只读监控记录 head `ba14b852`、PR OPEN/MERGEABLE，以及 run `33106234415` attempt1 仅有已知 v0.27.1 A2-1 EAGLE acceptance 失败（job `98639485749`）和下游 ci-gate 失败。原任务随后提交一次官方 `/rerun`；bot 已接受并创建 attempt2，新 EAGLE job `98695985385` 已启动，但截至本块末尾仍无结论，因此不能把复跑视为成功，也未发现真实合并冲突或新代码相关失败。
  title: 'PR #14454 官方复跑已触发并运行'
- block_id: 34a9e386fc002b7ec8bde0da48149bf55f00cb474787862ebf6088c0d4cf32c2
  indexed: true
  sha256: cd63afc34002e6f7af7d16bef27e38dd12c74cedb6526fc84814c392f5504742
  source_range:
    end: 8772063
    part: 10
    start: 0
  summary: attempt2 最终完成成功：run `33106234415` 的新 EAGLE job `98695985385` 通过，未重现 attempt1 的 EAGLE 失败。末尾只读状态显示 head `ba14b852`、PR 仍 OPEN/MERGEABLE，全部 checks 已终态且为 SUCCESS/SKIPPED；此前“仍运行/尚无结论”的状态已被后续结果修正。
  title: 'PR #14454 复跑通过，检查终态成功'
- block_id: f76db29140c3b9775700a3b254220c6e8e27579947817227dcb9682e421e9288
  indexed: true
  sha256: 55d123262710e590f08cf5a18b0a767e970f46e80cd1374337736460b50aef2e
  source_range:
    end: 8962178
    part: 0
    start: 8772063
  summary: '多次只读心跳均报告 head `ba14b852`、base 未变，PR 为 OPEN/MERGEABLE，全部 checks 处于 SUCCESS/SKIPPED，期间未发现新的代码失败或真实合并冲突。最后一次状态发生更正：PR #14454 已合入，head 仍为 `ba14b852`，原任务已获通知，监控自动化暂停；合并后 workflow 的两个任务仍排队，但被报告为不影响已合入状态。此前 attempt1 的 EAGLE acceptance/ci-gate 失败、旧 head 的 mismatch 和未完整聚合结果仍不能无条件归因于当前 head。'
  title: 'PR #14454 合入与监控终止'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 56a7c678401f4632be3448b5fbfb13f35166a889902e486f14800ef31ed28440
  kind: experience
  material_digest: f683103ce498069794868a644e9706e34407b5f1d31c9fb197b1a512318bc40a
  revision: 8d5cdf5e2371d75b434e90b8a76e64a0a1caee94414cd148a7ebfba659b91b90
  schema: mindie-entry/3
  summary: '最新材料将状态从此前的 OPEN/MERGEABLE 更正为 PR #14454 已合入，head `ba14b852`，原任务已通知且监控已暂停；合并后仍有两个 workflow 任务排队，但材料称其不影响已合入状态。保留历史上下文：此前全部 checks 曾为 SUCCESS/SKIPPED，attempt1 的 EAGLE acceptance 与 ci-gate 失败未确诊为代码问题，旧 head 的 DSV4 acceptance mismatch、v0.27.1 A2 Qwen3/MRV2 golden mismatch 及未完整聚合的 a3-800i-4 结果不可无条件归因于当前 head；材料未提供真实合并冲突证据。'
  title: 'PR #14454 ba14b852 已合入；历史 CI 不确定性保留'
navigation: '最新材料将状态从此前的 OPEN/MERGEABLE 更正为 PR #14454 已合入，head `ba14b852`，原任务已通知且监控已暂停；合并后仍有两个 workflow 任务排队，但材料称其不影响已合入状态。保留历史上下文：此前全部 checks 曾为 SUCCESS/SKIPPED，attempt1 的 EAGLE acceptance 与 ci-gate 失败未确诊为代码问题，旧 head 的 DSV4 acceptance mismatch、v0.27.1 A2 Qwen3/MRV2 golden mismatch 及未完整聚合的 a3-800i-4 结果不可无条件归因于当前 head；材料未提供真实合并冲突证据。'
schema: mindie-material-task/1
status: complete
task_id: 56a7c678401f4632be3448b5fbfb13f35166a889902e486f14800ef31ed28440
---

# PR #14454 ba14b852 已合入；历史 CI 不确定性保留

最新材料将状态从此前的 OPEN/MERGEABLE 更正为 PR #14454 已合入，head `ba14b852`，原任务已通知且监控已暂停；合并后仍有两个 workflow 任务排队，但材料称其不影响已合入状态。保留历史上下文：此前全部 checks 曾为 SUCCESS/SKIPPED，attempt1 的 EAGLE acceptance 与 ci-gate 失败未确诊为代码问题，旧 head 的 DSV4 acceptance mismatch、v0.27.1 A2 Qwen3/MRV2 golden mismatch 及未完整聚合的 a3-800i-4 结果不可无条件归因于当前 head；材料未提供真实合并冲突证据。

## Materials

- [PR #14454 只读 CI 监控与两次代码相关失败](blocks/998160092a6fc7362fb9086bfdc55690dc97fe8bd4fe0e2b03e5871c6fe17289.md): 监控从 rebase 后 head `5e327cb7…` 开始，经历 mypy 的 `moe_mlp.py:725` Optional `beta` 类型错误，以及 head `4a96af94…` 上由全局 Ascend `MagicMock` 日志目录导致的 `FileExistsError`；两次均通知原任务并按 head、job、错误签名去重。修复后 head 更新为 `17d24917…`，PR 保持 OPEN/MERGEABLE；本地 mypy、CPU 定向 UT 和格式检查报告通过，但 E2E 仍未完成，不能据此视为整体成功。后续监控转入 acceptance 失败调查，且 `BLOCKED` 不被单独当作真实冲突。
- [MagicMock 日志配置测试修复后的 CI 监控](blocks/21d6680cbe33e64e8fbc338b4f409d5b1f1ccaff7464813708bec2aec67cbc6c.md): CPU UT 失败被定位为 `test_linear.py` 留下全局 Ascend mock，SiTU 参考值构造完整 `VllmConfig` 后触发日志配置；修复仅调整既有测试为 mock `VllmConfig`、真实 `CompilationConfig`，并禁用参考值的 native compile，未改生产 logger/算子。父 PR 新 head 为 `17d24917…`，组合回归 67 passed，相关 mypy 与 format 检查报告通过；随后多数 E2E 分片成功，但 NPU/310p 等分片曾处于运行或排队状态。DSpark/DeepSeek-V4 acceptance 曾在 job `98506513821` 以 `acceptance_per_pos` 未达 golden 失败，位置 2 为 `0.7009029`（通过线 `0.71`，容差 `0.03`）；该结果的代码根因未确定，且与其他版本/历史 job 的通过结果不一致。
- [Acceptance 复跑权限阻塞后成功](blocks/8d64b73109f367ce8fcff690362978ce4e24d81b55cbec1d8b8aad3f5c2d8933.md): 原 acceptance 失败记录被保留；调查认为当前仅位置 2 未达标，不能由历史通过 job 证明无回归，且失败 job 的原样复跑因 `gh run rerun` 不可用及 REST 返回 `403 Must have admin rights to Repository` 而受权限阻塞。之后 maintainer 触发同一 head 的 E2E `attempt=2`，新 job `98534958344` 最终通过，确认原 acceptance 失败复跑成功。当前报告称活动 checks 均为 SUCCESS 或矩阵 SKIPPED，但 PR 仍 OPEN/MERGEABLE，监控继续进行；没有确认真实 `CONFLICTING`。
- [PR #14454 新 head 与 MLA 修复监控](blocks/548ba8d62138c0c3eb71b30b77edaaa33ae2b8149100bc05fc402f90f6070133.md): 只读监控记录显示，PR #14454 从旧 head 切换到 `b106ea1b48dec9cd1e4b7d3107e2dca24b242df0` 后仍为 OPEN/MERGEABLE，基础检查通过，E2E 分片一度运行或排队，未据此判定新 head 全绿。随后切换到 `5dc5cb302cfea8d03c2786d0007f92695ddd0e29`，发现 `select-tests` 因 `tests/ut/ops/a3_2/test_kimi_kda_fused_norm_gate.py` 未登记 `estimated_times` 而失败，job `98589476494`，该问题被报告后又切换到 `3b6c845bbed21e786196272b2bb25250b67d1a99`。来源还记录了无 RoPE MLA 的 `None cos/sin` 切片 TypeError 修复、FLA/KDA fused RMSNorm gate 注册及相关 CPU/NPU/format/mypy 检查，但这些是报告的验证结果，不替代新 head 的完整 CI 结论。
- [新 head 3b6c845b 的 CI 运行状态](blocks/b8aafbaf51cb8f0c20fd35f0cebefa15becf52d97f85a8821f8c1e9207646597.md): 在 head `3b6c845bbed21e786196272b2bb25250b67d1a99` 上，PR 仍 OPEN/MERGEABLE，`BLOCKED` 被持续区分为非真实冲突；基础检查、`select-tests`、缓存准备及部分 CPU 分片报告成功，其余 E2E 分片曾运行或排队。用户报告 A2 dummy DSpark 用例仍仅处于设计阶段，未编写或提交；截至该批次末尾，未报告新的代码失败或真实冲突。
- [DSpark acceptance 失败与未收敛分片](blocks/35e5ad5dca45b20b68fe913013110b7b86327ffafff8297a49ac65db8fd7bc6d.md): head `3b6c845b…` 的 ba07e4a 分片出现 DSpark DeepSeek-V4 acceptance mismatch：job `98593673963` 在 `test_dspark_deepseekv4.py:126` 报位置 2 为 `0.7009029`、golden `0.74`，容差后仍失败；同一 run 的 v0.27.1 对应 a3-800i-4 分片报告执行同测试并 `3 passed`，但日志没有最终聚合 `acceptance_per_pos`，不能仅凭绿色 job 推断最终采信率。随后新增 v0.27.1 A2 Qwen3/MRV2 失败，job `98593673921` 在 `tests/e2e/pull_request/one_card/model_runner_v2/test_basic.py:248` 出现 `1 failed / 13 passed` 的 golden mismatch；来源称尚未找到可证明的代码根因，当前 run `33093176000` 仍有分片运行，未执行主动复跑或修改。
- [PR #14454 多次换 head 的只读 CI 监控](blocks/df51c7d2540871c6a1b8aa2f0a5bb4f34abf7bc38f6d259c9f2e846dd4633ac4.md): 监控先后切换至 `8c8deed`、`b6dce8c` 和 `ba14b852`；PR 始终报告 OPEN/MERGEABLE，`BLOCKED` 未被视为真实冲突，旧 head 结果未沿用。`8c8deed` 曾出现 CPU UT 的 `supports_mm_inputs` 未初始化 AttributeError，后续 head 由 fixture 补 `False`；`ba14b852` 的 v0.27.1 A2 EAGLE acceptance 出现临界失败，`acceptance_per_pos` 首位约 `0.3294` 低于 `0.33` 下界，其余位置满足容差。父任务判断该结果可能是临界波动，要求不改代码或 golden，并要求 run `33106234415` 全部终态后再评估原样 `/rerun`；截至材料末尾尚无成功复跑。
- [run 33106234415 等待终态与 EAGLE 复跑约束](blocks/9b5775d9a01286b2b1af1743c58cf61883cb743696c2c68cdaa2b2fc0ab93b8d.md): 在 head `ba14b852`、PR OPEN/MERGEABLE 的背景下，run `33106234415` 长时间处于 queued 或部分运行状态，A2 part5 与两个 `<redacted:ticket-id:7a9ce89c9955>` job 曾未完成；已知唯一代码相关失败是 v0.27.1 A2 EAGLE job `98639485749`。父任务明确要求把 run 全部终态作为独立通知事件，并在确认无其他新失败、无人触发 attempt2 后由父任务决定是否发一次官方 `/rerun`；此前 `gh run rerun --job98639485749` 返回 `job cannot be rerun`，不能视为复跑已启动。材料中没有确认 run 已完成、attempt2 已产生或复跑成功。
- [run 33106234415 持续排队且无新增结论](blocks/f4cc1c0b99645b2dd568af0735b0615ec7fb8c67350e857390dfa37cbe69f2b0.md): 截至 2026-08-27 22:07，head `ba14b852`、base 未变，PR 仍 OPEN/MERGEABLE；run `33106234415` 仍报告 queued，两个 `<redacted:ticket-id:7a9ce89c9955>` job 仍排队。已知唯一失败仍是先前报告的 v0.27.1 A2 EAGLE job，未发现新的失败或真实合并冲突。该状态不是全部 CI 通过，也没有证据表明复跑已发起。
- [PR #14454 官方复跑已触发并运行](blocks/ea85a40bb21436ebb517ff2e62ca0e3e7c81136fbcecee684fe00723585b6db5.md): 只读监控记录 head `ba14b852`、PR OPEN/MERGEABLE，以及 run `33106234415` attempt1 仅有已知 v0.27.1 A2-1 EAGLE acceptance 失败（job `98639485749`）和下游 ci-gate 失败。原任务随后提交一次官方 `/rerun`；bot 已接受并创建 attempt2，新 EAGLE job `98695985385` 已启动，但截至本块末尾仍无结论，因此不能把复跑视为成功，也未发现真实合并冲突或新代码相关失败。
- [PR #14454 复跑通过，检查终态成功](blocks/34a9e386fc002b7ec8bde0da48149bf55f00cb474787862ebf6088c0d4cf32c2.md): attempt2 最终完成成功：run `33106234415` 的新 EAGLE job `98695985385` 通过，未重现 attempt1 的 EAGLE 失败。末尾只读状态显示 head `ba14b852`、PR 仍 OPEN/MERGEABLE，全部 checks 已终态且为 SUCCESS/SKIPPED；此前“仍运行/尚无结论”的状态已被后续结果修正。
- [PR #14454 合入与监控终止](blocks/f76db29140c3b9775700a3b254220c6e8e27579947817227dcb9682e421e9288.md): 多次只读心跳均报告 head `ba14b852`、base 未变，PR 为 OPEN/MERGEABLE，全部 checks 处于 SUCCESS/SKIPPED，期间未发现新的代码失败或真实合并冲突。最后一次状态发生更正：PR #14454 已合入，head 仍为 `ba14b852`，原任务已获通知，监控自动化暂停；合并后 workflow 的两个任务仍排队，但被报告为不影响已合入状态。此前 attempt1 的 EAGLE acceptance/ci-gate 失败、旧 head 的 mismatch 和未完整聚合结果仍不能无条件归因于当前 head。

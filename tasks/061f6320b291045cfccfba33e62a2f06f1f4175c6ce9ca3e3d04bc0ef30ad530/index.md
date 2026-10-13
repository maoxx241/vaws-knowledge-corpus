---
blocks:
- block_id: bf17564823c38dbdd1cba19c2de1e910c0f3d0f52abb14e2b53d9089e09488bf
  indexed: true
  sha256: d6bec62188bc275500611e129d6ed4d83d66b892461c0b4bafaf60ae07cfbd97
  source_range:
    end: 11158897
    part: 0
    start: 0
  summary: '围绕 Kimi K3 的 recurrent KDA，完成了独立 worktree/分支迁移、A3 构建、Torch binding 与单算子验证，并发现并修复了 device `cu_seqlens`、容量型 state pool、spec slot、padding、原地状态更新及 tiling context 生命周期等问题。外部 PR #231 的问题评论曾迁移到私仓 PR #10，随后删除；但 GitHub 自动生成的 `CrossReferencedEvent` 无法由普通用户删除，需 GitHub Support 处理，且相关私仓引用被视为潜在信息泄露。后续 PR #231 更新已同步，验证覆盖 K3 TP16 局部规格、ACLGraph 回放和 decode/MTP，但不泛化到 prefill chunk KDA。'
  title: recurrent KDA 迁移、隔离与 PR 评论处理
- block_id: e4b19184296d9b8f722fda2a49f285f932b899daf94a0407bdd21f87f47e773c
  indexed: true
  sha256: 5c05f1acd54bb6246fa0ec806017bf2baa797c74da4cbfc9abc9ef54f5fc4d7c
  source_range:
    end: 11158897
    part: 1
    start: 0
  summary: 'PR #10 将 Kimi K3 decode/MTP recurrent 路径切换到 AscendC，reference 与测试合并到同一文件，覆盖真实 BSND 单 token、1–8 token MTP、padding/spec state pool 和 ACLGraph 动态 metadata；prefill 保持不变。A3 设备验证、23 个 Kimi UT 和 8 个算子契约测试曾通过，随后四机整网发现 `The!!!!` 无限复读，说明小型单算子验证不足。提交 `f9a038e1` 修复了 `torch.compile/AOT` 对 aliased output 的 functionalization 契约，但四机复验确认该修复未消除真实多 token decode 语义损坏。'
  title: 'PR #10 的 decode 替换与 AOT 修复'
- block_id: 72d26e86a527c15c43ae2870324328e3ce6fe0f96a8394a01a103d6a2e2daca6
  indexed: true
  sha256: 0327f4f52d160020482a27f99f62d9c6a771eec6d4f4901e4724478e4f775f07
  source_range:
    end: 11158897
    part: 2
    start: 0
  summary: 四机 TP16×DP4×EP64、W4A8、FULL_DECODE_ONLY 配置下，两条 temperature=0 请求稳定返回 `content=null`，首个 reasoning token 为 `The`，之后连续输出 `!` 直至达到 `max_tokens`；服务 HTTP 200 且无 runtime error，GPQA 未启动。现有证据表明 `f9a038e1` 仅修复 AOT alias-return 契约，未修复真实多 token decode；需在同一 combined 配置和首个坏样本抓取 q/k/v/raw_gate/beta、输入输出 recurrent state 及 slot/index/stride/dtype/shape，并并排比较 AscendC、Triton 与 CPU reference。当前要求是在 PR10 隔离分支定位 mutable state、functionalization/NPUGraph replay 或 metadata 传递的最小责任点后提交下一版修复并回传逐 token/state 原始差异；尚无修复成功结论。
  title: 四机复现 The!!!!，待进行首步状态归因
- block_id: eb27d4f5e217513aec14733977f36b6b5f0376365b9402689d20f2211c7531b7
  indexed: true
  sha256: c7e590c4cbcea61d586e77a1789d399265a25297a80696a0044c92766d1e961b
  source_range:
    end: 11158897
    part: 3
    start: 0
  summary: 静态与真实诊断发现，早期单测未覆盖 prefill 产生的真实 cache、module-owned state、BF16 路径及 K/V 解释差异；四机 trace 还因 `dt_bias` `[768]`/`[6,128]` 形状错误和 worker 环境变量缺失而未获得张量证据。随后确认 5 层真实权重可用于 TP16 单机 eager 定位，并在真实 KDA 层抓取 A/B 数据；白盒审计指出 AscendC 非法或空 slot 分支可能整 core 提前返回，`empty_like` 会暴露未写显存，而 Triton 将 `state_idx <= 0` 视为无效、AscendC 将 slot 0 视为有效。所有后续编译约束均为 `MAX_JOBS=256`/`-j256`，四机已停止，相关结论曾明确未修改文件、未操作远端服务。
  title: K3真实缓存诊断与单机转向
- block_id: e39fc3e707995a4a8345a0587ca0923ec19b95fd05a24326e8f56a2de11cf646
  indexed: true
  sha256: 3977ffff2a15f0159ac79a1d6e3a12b3fbf8537b1af2463ff4cdb356d8f70761
  source_range:
    end: 11158897
    part: 4
    start: 0
  summary: 调用链审计确认 Python/native 契约当前返回单 Tensor，`None` 更可能是上层表示；主要风险是 AscendC 对 `cu_seqlens[-1] == capture capacity` 的严格校验，在 capacity 16/32、实际 token 为 1 且存在零长度 padding 时整 core 提前返回。Triton 会逐 row 跳过 `T==0` 的 padding，不保证未写的 output tail 为 0，但必须计算有效 row、更新 active state，并保持其他 state 不变；因此失败用例只比较 active token 和 state。该 TP16 graph-size-16 红测已加入 PR10，稳定出现 `49,583 / 1,671,168` state mismatch，原因说明已写入 PR，kernel 修复交由专门算子开发。
  title: Graph padding导致有效row整体漏写
- block_id: e015367b00ca9331342470894fc710b6e25995cdc3c2b7af1abc476e8cd58bb5
  indexed: true
  sha256: 758fa754a3879ae290d259003ece80a524ba1f44b77ffb47868cedfb36ce0868
  source_range:
    end: 11158897
    part: 5
    start: 0
  summary: '算子修复将两个架构 kernel 的终态校验放宽为有效 token 不超过 capture capacity；首次增量构建复用了旧二进制，clean build 且确认 kernel hash 变化后，graph-padding 失败用例及单 token、8步 state/NPUGraph、AOT 回归全部通过。最终 7 个 K3 NPU 场景加 8 个 kernel/binding 契约共 `15 passed`，但结论仅限单机单算子替换门禁，不代表整网复验；PR 未合入，且曾纠正“保持 Draft”为“保持未合入”。PR #10 的 13 个提交已在最新 `ci_new` 基线 `96069360` 上压为单个 commit `2d2a363a682c42d17dc17680f0b5c505105af718`，patch-id 保持一致。'
  title: Clean build后单机门禁通过并压缩提交
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 061f6320b291045cfccfba33e62a2f06f1f4175c6ce9ca3e3d04bc0ef30ad530
  kind: experience
  material_digest: 378b86942dcf786b2e15a5201e4a9f5d80be80d0395080680c841d9d2abe012b
  revision: 9ce0f35218382a59bf1f1cccbdfa2fa706383b54419a8ad4f3f73f74c6b0dcbd
  schema: mindie-entry/3
  summary: '当前已从四机 `The!!!!` 现象收敛到可复现的 graph-padding 根因：capture capacity 大于实际 token 时，AscendC 的严格 `cu_seqlens[-1] == capacity` 校验会让有效 row 与 state 一起漏写；Triton 则跳过零长度 padding、仅要求有效 row 正常处理，padding tail 不保证为零。专门算子修复已在 clean build 后通过原红测、7 个 K3 单机场景和 8 个契约检查，共 `15 passed`；旧增量包曾是假更新，需保留这一不确定性与 clean-build 纠正。PR #10 当前未合入，内容已压缩为 commit `2d2a363a682c42d17dc17680f0b5c505105af718`；尚未证明四机整网、多 token 真实服务场景已复验。'
  title: '当前任务导航：PR #10 已完成graph-padding算子修复与单机门禁'
navigation: '当前已从四机 `The!!!!` 现象收敛到可复现的 graph-padding 根因：capture capacity 大于实际 token 时，AscendC 的严格 `cu_seqlens[-1] == capacity` 校验会让有效 row 与 state 一起漏写；Triton 则跳过零长度 padding、仅要求有效 row 正常处理，padding tail 不保证为零。专门算子修复已在 clean build 后通过原红测、7 个 K3 单机场景和 8 个契约检查，共 `15 passed`；旧增量包曾是假更新，需保留这一不确定性与 clean-build 纠正。PR #10 当前未合入，内容已压缩为 commit `2d2a363a682c42d17dc17680f0b5c505105af718`；尚未证明四机整网、多 token 真实服务场景已复验。'
schema: mindie-material-task/1
status: complete
task_id: 061f6320b291045cfccfba33e62a2f06f1f4175c6ce9ca3e3d04bc0ef30ad530
---

# 当前任务导航：PR #10 已完成graph-padding算子修复与单机门禁

当前已从四机 `The!!!!` 现象收敛到可复现的 graph-padding 根因：capture capacity 大于实际 token 时，AscendC 的严格 `cu_seqlens[-1] == capacity` 校验会让有效 row 与 state 一起漏写；Triton 则跳过零长度 padding、仅要求有效 row 正常处理，padding tail 不保证为零。专门算子修复已在 clean build 后通过原红测、7 个 K3 单机场景和 8 个契约检查，共 `15 passed`；旧增量包曾是假更新，需保留这一不确定性与 clean-build 纠正。PR #10 当前未合入，内容已压缩为 commit `2d2a363a682c42d17dc17680f0b5c505105af718`；尚未证明四机整网、多 token 真实服务场景已复验。

## Materials

- [recurrent KDA 迁移、隔离与 PR 评论处理](blocks/bf17564823c38dbdd1cba19c2de1e910c0f3d0f52abb14e2b53d9089e09488bf.md): 围绕 Kimi K3 的 recurrent KDA，完成了独立 worktree/分支迁移、A3 构建、Torch binding 与单算子验证，并发现并修复了 device `cu_seqlens`、容量型 state pool、spec slot、padding、原地状态更新及 tiling context 生命周期等问题。外部 PR #231 的问题评论曾迁移到私仓 PR #10，随后删除；但 GitHub 自动生成的 `CrossReferencedEvent` 无法由普通用户删除，需 GitHub Support 处理，且相关私仓引用被视为潜在信息泄露。后续 PR #231 更新已同步，验证覆盖 K3 TP16 局部规格、ACLGraph 回放和 decode/MTP，但不泛化到 prefill chunk KDA。
- [PR #10 的 decode 替换与 AOT 修复](blocks/e4b19184296d9b8f722fda2a49f285f932b899daf94a0407bdd21f87f47e773c.md): PR #10 将 Kimi K3 decode/MTP recurrent 路径切换到 AscendC，reference 与测试合并到同一文件，覆盖真实 BSND 单 token、1–8 token MTP、padding/spec state pool 和 ACLGraph 动态 metadata；prefill 保持不变。A3 设备验证、23 个 Kimi UT 和 8 个算子契约测试曾通过，随后四机整网发现 `The!!!!` 无限复读，说明小型单算子验证不足。提交 `f9a038e1` 修复了 `torch.compile/AOT` 对 aliased output 的 functionalization 契约，但四机复验确认该修复未消除真实多 token decode 语义损坏。
- [四机复现 The!!!!，待进行首步状态归因](blocks/72d26e86a527c15c43ae2870324328e3ce6fe0f96a8394a01a103d6a2e2daca6.md): 四机 TP16×DP4×EP64、W4A8、FULL_DECODE_ONLY 配置下，两条 temperature=0 请求稳定返回 `content=null`，首个 reasoning token 为 `The`，之后连续输出 `!` 直至达到 `max_tokens`；服务 HTTP 200 且无 runtime error，GPQA 未启动。现有证据表明 `f9a038e1` 仅修复 AOT alias-return 契约，未修复真实多 token decode；需在同一 combined 配置和首个坏样本抓取 q/k/v/raw_gate/beta、输入输出 recurrent state 及 slot/index/stride/dtype/shape，并并排比较 AscendC、Triton 与 CPU reference。当前要求是在 PR10 隔离分支定位 mutable state、functionalization/NPUGraph replay 或 metadata 传递的最小责任点后提交下一版修复并回传逐 token/state 原始差异；尚无修复成功结论。
- [K3真实缓存诊断与单机转向](blocks/eb27d4f5e217513aec14733977f36b6b5f0376365b9402689d20f2211c7531b7.md): 静态与真实诊断发现，早期单测未覆盖 prefill 产生的真实 cache、module-owned state、BF16 路径及 K/V 解释差异；四机 trace 还因 `dt_bias` `[768]`/`[6,128]` 形状错误和 worker 环境变量缺失而未获得张量证据。随后确认 5 层真实权重可用于 TP16 单机 eager 定位，并在真实 KDA 层抓取 A/B 数据；白盒审计指出 AscendC 非法或空 slot 分支可能整 core 提前返回，`empty_like` 会暴露未写显存，而 Triton 将 `state_idx <= 0` 视为无效、AscendC 将 slot 0 视为有效。所有后续编译约束均为 `MAX_JOBS=256`/`-j256`，四机已停止，相关结论曾明确未修改文件、未操作远端服务。
- [Graph padding导致有效row整体漏写](blocks/e39fc3e707995a4a8345a0587ca0923ec19b95fd05a24326e8f56a2de11cf646.md): 调用链审计确认 Python/native 契约当前返回单 Tensor，`None` 更可能是上层表示；主要风险是 AscendC 对 `cu_seqlens[-1] == capture capacity` 的严格校验，在 capacity 16/32、实际 token 为 1 且存在零长度 padding 时整 core 提前返回。Triton 会逐 row 跳过 `T==0` 的 padding，不保证未写的 output tail 为 0，但必须计算有效 row、更新 active state，并保持其他 state 不变；因此失败用例只比较 active token 和 state。该 TP16 graph-size-16 红测已加入 PR10，稳定出现 `49,583 / 1,671,168` state mismatch，原因说明已写入 PR，kernel 修复交由专门算子开发。
- [Clean build后单机门禁通过并压缩提交](blocks/e015367b00ca9331342470894fc710b6e25995cdc3c2b7af1abc476e8cd58bb5.md): 算子修复将两个架构 kernel 的终态校验放宽为有效 token 不超过 capture capacity；首次增量构建复用了旧二进制，clean build 且确认 kernel hash 变化后，graph-padding 失败用例及单 token、8步 state/NPUGraph、AOT 回归全部通过。最终 7 个 K3 NPU 场景加 8 个 kernel/binding 契约共 `15 passed`，但结论仅限单机单算子替换门禁，不代表整网复验；PR 未合入，且曾纠正“保持 Draft”为“保持未合入”。PR #10 的 13 个提交已在最新 `ci_new` 基线 `96069360` 上压为单个 commit `2d2a363a682c42d17dc17680f0b5c505105af718`，patch-id 保持一致。

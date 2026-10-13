---
blocks:
- block_id: ca5d790883fe7e728d78022fabb8fe32cdae70fa2de5aa593b5cdd9ce8910c1e
  indexed: true
  sha256: d55bb036cc7d8e8cf3b2754d71e5ec953cbc2ac7086f5a5e29e88a73a9d0661e
  source_range:
    end: 3958928
    part: 0
    start: 0
  summary: 源码审计先认为 VA KDA 的两次 padding mask 用于处理 AscendC recurrent kernel 对 `seqLen==0` 行直接 `continue`、而 `empty_like` 输出未写的问题，后经与 GDN 路径对比修正为：KDA 与 GDN kernel 都可能留下未写区域，KDA 的多层清零缺乏充分的独立必要性证据。当前更稳妥的判断是保留 post-norm mask、优先删除 recurrent 后 mask 和 merge 前 `zero_()`；仅将外层改为 `torch.zeros` 并删除两个 mask 不安全，因为临时输出的整段 copy 会覆盖外层零，且 `0 * NaN` 仍可能产生 NaN。GPU 主要采用 padding 不可观察的较弱契约，spec 零长度路径也未证明尾部为零；任何进一步删 mask 或改为 caller-owned output 都需要 graph padding、spec replay、idle DP 的删除式 A/B 验证。
  title: VA KDA padding 清零逻辑审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9373d7490066bdd8a80ef009ac9629446110baeebf63919fe8c5ba3b22d59c40
  kind: experience
  material_digest: 6212f663e30c2522f20e1c347ce70fe3f0144b56e0c02c30403d04dde9ec6310
  revision: b7a81eb4e50b80f2a2617e1376d81e110cbe41e1c690910aaa8b88a39b629363
  schema: mindie-entry/3
  summary: 当前材料集中审计 Ascend VA 中 KDA 与 GDN 的 padding 输出契约、`empty`/`zeros` buffer、recurrent kernel 的零长度分支、merge 整段 copy 及 post-norm NaN 风险。已保留后续修正：KDA 两次 mask 并非已证实都必要，外层改零也不是单行安全修复；最小候选改动是保留最终 mask并移除前两层清零，但仍需通过 graph padding、spec/non-spec replay 和 idle DP A/B 验证。GPU 的差异更可能是 padding don’t-care 与消费范围假设，而非已证明其 kernel 全面清零。
  title: VA KDA/GDN padding 清零逻辑与修复边界
navigation: 当前材料集中审计 Ascend VA 中 KDA 与 GDN 的 padding 输出契约、`empty`/`zeros` buffer、recurrent kernel 的零长度分支、merge 整段 copy 及 post-norm NaN 风险。已保留后续修正：KDA 两次 mask 并非已证实都必要，外层改零也不是单行安全修复；最小候选改动是保留最终 mask并移除前两层清零，但仍需通过 graph padding、spec/non-spec replay 和 idle DP A/B 验证。GPU 的差异更可能是 padding don’t-care 与消费范围假设，而非已证明其 kernel 全面清零。
schema: mindie-material-task/1
status: complete
task_id: 9373d7490066bdd8a80ef009ac9629446110baeebf63919fe8c5ba3b22d59c40
---

# VA KDA/GDN padding 清零逻辑与修复边界

当前材料集中审计 Ascend VA 中 KDA 与 GDN 的 padding 输出契约、`empty`/`zeros` buffer、recurrent kernel 的零长度分支、merge 整段 copy 及 post-norm NaN 风险。已保留后续修正：KDA 两次 mask 并非已证实都必要，外层改零也不是单行安全修复；最小候选改动是保留最终 mask并移除前两层清零，但仍需通过 graph padding、spec/non-spec replay 和 idle DP A/B 验证。GPU 的差异更可能是 padding don’t-care 与消费范围假设，而非已证明其 kernel 全面清零。

## Materials

- [VA KDA padding 清零逻辑审计](blocks/ca5d790883fe7e728d78022fabb8fe32cdae70fa2de5aa593b5cdd9ce8910c1e.md): 源码审计先认为 VA KDA 的两次 padding mask 用于处理 AscendC recurrent kernel 对 `seqLen==0` 行直接 `continue`、而 `empty_like` 输出未写的问题，后经与 GDN 路径对比修正为：KDA 与 GDN kernel 都可能留下未写区域，KDA 的多层清零缺乏充分的独立必要性证据。当前更稳妥的判断是保留 post-norm mask、优先删除 recurrent 后 mask 和 merge 前 `zero_()`；仅将外层改为 `torch.zeros` 并删除两个 mask 不安全，因为临时输出的整段 copy 会覆盖外层零，且 `0 * NaN` 仍可能产生 NaN。GPU 主要采用 padding 不可观察的较弱契约，spec 零长度路径也未证明尾部为零；任何进一步删 mask 或改为 caller-owned output 都需要 graph padding、spec replay、idle DP 的删除式 A/B 验证。

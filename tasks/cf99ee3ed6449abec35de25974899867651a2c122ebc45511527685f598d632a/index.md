---
blocks:
- block_id: e6c0a1dd7411e42e92096f9cd0cff9eb2a14c7f0ad74384cb9d4a2bce90ae529
  indexed: true
  sha256: b8505012382ca9f0413aa1a2d7ab26ba4fde8a8b9fed026bc84533e9528a4bd3
  source_range:
    end: 4088308
    part: 0
    start: 0
  summary: '围绕 vllm-ascend 的 Kimi K3 roadmap，记录了从 DSV4 对照、release/main 分界、MegaMoE、MLA/DSpark、量化、PD、缓存与文档等方面形成 RFC 的过程；其中曾误将 #13065 视为 main 目标，后更正为等待 main2main 追上包含上游 K3 修改的 vLLM。RFC #13452 后续吸收了另一份清单并区分已合入基础与未完成 K3 接入，但部分状态、PR 关联和“已确认”说法属于当时报告，不能视为独立验证的成功结果；期间还记录了标签权限不足和一次校验命令错误。用户与 JimWang2881 共同制作 RFC，已在 CC List 中加入 `**Co-author:** @JimWang2881`，未输出邮箱等私密信息。'
  title: Kimi K3 RFC 初始盘点与共同作者署名
- block_id: 7846798b91b9162513b72e1de5a6c993184c24590404d5bb6ec0119d200bed71
  indexed: true
  sha256: 9c16803d1ef8608e18f27b353295d738a80ea73efa8d19233727a31032c0c2aa
  source_range:
    end: 4088308
    part: 1
    start: 0
  summary: 'RFC 分类被重构为版本 → Framework/Operators → 特性，重点区分 `releases/v0.26.0rc` 与 `main`，并将 MRV2/EPLB、DSpark 主线适配和 main2main 前置条件单独处理。后续修正了硬件与通信语义：A2 为 Ascend 910B，A3 为 Ascend 910C；A2 的 MC2 全局专家数超过 512 时回退 AllGather，prefill/decode 均走 AllGather，而 A3/A5 采用 prefill AllToAll EP、decode MC2，A3/A5 不再强调 512；“A2 因带宽较差”仅是未获代码/PR 支持的推测。量化表改为 A2/A3 的 INT W4A8 与 A5 的 MXFP W4A8（MXFP4 权重/MXFP8 激活），并移除测试、验收、性能基线及泛化类任务，同时声称扫描 #12950 后 PR 并补入若干 KDA/MLA/MegaMoE/MRV2/DSpark 特性；这些 PR 状态和补入结果仍应作为可检索的当时记录，而非已验证案例。'
  title: RFC 按版本、硬件边界与特性重构
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: cf99ee3ed6449abec35de25974899867651a2c122ebc45511527685f598d632a
  kind: experience
  material_digest: 1249070630e3f7b67462c50fc8913ecf1ce335282424142f5fccae8c49343b97
  revision: 57e847f691dae82e2956b1209168f268d74076fe3c66f0a859713ddb5b1895a1
  schema: mindie-entry/3
  summary: '当前脉络集中在 RFC #13452：先建立 Kimi K3 能力清单并吸收外部 roadmap，再按 `0.26.0rc` 与 `main` 分层，细化 A2/A3/A5 的通信、SiTU 与量化边界。重要修正包括移除 #13065 目标说法、将 A2 的 512 限制归因于 MC2 全局参数上限而非单卡专家数、保留 main2main 前置条件，以及把尚未证实的性能原因标为不确定。后续记录还声称清理了测试/泛化任务并扫描 #12950 后 PR 补充特性，但这些属于当时整理结果，原始材料仍需独立检索；RFC 同时保留 JimWang2881 的共同作者署名。'
  title: Kimi K3 RFC 路线图整理与修正
navigation: '当前脉络集中在 RFC #13452：先建立 Kimi K3 能力清单并吸收外部 roadmap，再按 `0.26.0rc` 与 `main` 分层，细化 A2/A3/A5 的通信、SiTU 与量化边界。重要修正包括移除 #13065 目标说法、将 A2 的 512 限制归因于 MC2 全局参数上限而非单卡专家数、保留 main2main 前置条件，以及把尚未证实的性能原因标为不确定。后续记录还声称清理了测试/泛化任务并扫描 #12950 后 PR 补充特性，但这些属于当时整理结果，原始材料仍需独立检索；RFC 同时保留 JimWang2881 的共同作者署名。'
schema: mindie-material-task/1
status: complete
task_id: cf99ee3ed6449abec35de25974899867651a2c122ebc45511527685f598d632a
---

# Kimi K3 RFC 路线图整理与修正

当前脉络集中在 RFC #13452：先建立 Kimi K3 能力清单并吸收外部 roadmap，再按 `0.26.0rc` 与 `main` 分层，细化 A2/A3/A5 的通信、SiTU 与量化边界。重要修正包括移除 #13065 目标说法、将 A2 的 512 限制归因于 MC2 全局参数上限而非单卡专家数、保留 main2main 前置条件，以及把尚未证实的性能原因标为不确定。后续记录还声称清理了测试/泛化任务并扫描 #12950 后 PR 补充特性，但这些属于当时整理结果，原始材料仍需独立检索；RFC 同时保留 JimWang2881 的共同作者署名。

## Materials

- [Kimi K3 RFC 初始盘点与共同作者署名](blocks/e6c0a1dd7411e42e92096f9cd0cff9eb2a14c7f0ad74384cb9d4a2bce90ae529.md): 围绕 vllm-ascend 的 Kimi K3 roadmap，记录了从 DSV4 对照、release/main 分界、MegaMoE、MLA/DSpark、量化、PD、缓存与文档等方面形成 RFC 的过程；其中曾误将 #13065 视为 main 目标，后更正为等待 main2main 追上包含上游 K3 修改的 vLLM。RFC #13452 后续吸收了另一份清单并区分已合入基础与未完成 K3 接入，但部分状态、PR 关联和“已确认”说法属于当时报告，不能视为独立验证的成功结果；期间还记录了标签权限不足和一次校验命令错误。用户与 JimWang2881 共同制作 RFC，已在 CC List 中加入 `**Co-author:** @JimWang2881`，未输出邮箱等私密信息。
- [RFC 按版本、硬件边界与特性重构](blocks/7846798b91b9162513b72e1de5a6c993184c24590404d5bb6ec0119d200bed71.md): RFC 分类被重构为版本 → Framework/Operators → 特性，重点区分 `releases/v0.26.0rc` 与 `main`，并将 MRV2/EPLB、DSpark 主线适配和 main2main 前置条件单独处理。后续修正了硬件与通信语义：A2 为 Ascend 910B，A3 为 Ascend 910C；A2 的 MC2 全局专家数超过 512 时回退 AllGather，prefill/decode 均走 AllGather，而 A3/A5 采用 prefill AllToAll EP、decode MC2，A3/A5 不再强调 512；“A2 因带宽较差”仅是未获代码/PR 支持的推测。量化表改为 A2/A3 的 INT W4A8 与 A5 的 MXFP W4A8（MXFP4 权重/MXFP8 激活），并移除测试、验收、性能基线及泛化类任务，同时声称扫描 #12950 后 PR 并补入若干 KDA/MLA/MegaMoE/MRV2/DSpark 特性；这些 PR 状态和补入结果仍应作为可检索的当时记录，而非已验证案例。

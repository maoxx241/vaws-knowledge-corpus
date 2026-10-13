---
blocks:
- block_id: 1ecfdc4da66041e6d37c3fcc9ce724ff32ce5779ac95dd7e01b56d3eabdc9524
  indexed: true
  sha256: 642042d01947e48407cf3a856f224303e6b2ad321d388c0ab86144cbd9153dd7
  source_range:
    end: 1975722
    part: 0
    start: 0
  summary: '围绕 issue #13452，将原有 74 个 checkbox 收敛为按 release/main 和用户可感知能力组织的 roadmap，删除 generic foundation、历史细节及重复项。随后根据评审意见把 PD、Prefix Cache、Chunked Prefill、128K/1M 等标为 #12950 已支持，并拆分 MoE、Quantization、Attention 与具体算子；同时记录 EPD、W4A8C8、multistream、GMMSituQuant 等尚未确认或待完成事项。相关操作和检查曾使用 GitHub/live issue 与 PR 核对，但该块中的完成状态和匹配结论属于当时记录，不能视为重新验证。'
  title: Kimi K3 roadmap 初版收敛
- block_id: 83c68b645394f4b08edb76281f2314fd9fedb2e2d79795bd1b145493c1029edb
  indexed: true
  sha256: 2d25dd75f842fbe7b488535b0272d195f417d35f8ed9580b5d5f3e4566a363d4
  source_range:
    end: 1975722
    part: 1
    start: 0
  summary: '用户评审要求删除多节点多模态、async load/COW/recompute、prefix-aware causal convolution 和 QuaRot 等项，并将 EPD、offload、shared-expert multistream、W4A8C8、shared-expert W4A8、GQA DSpark 精度问题及独立融合算子重新组织。记录中的版本将 EPLB 明确为依赖 `MegaMoE` 或 `GMMSituQuant`，并声称已刷新 issue #13452；后续用户又要求恢复硬件执行表，说明 roadmap 内容仍在持续修订。该块还包含 response annotations，要求保留 speaker attribution 与不确定的 PR 检查结果。'
  title: 评审意见驱动的 roadmap 重组
- block_id: c7ab75946872d8bf86a0b6f229e44c66f5a221a42f36d0bdef736b7c258a6775
  indexed: true
  sha256: 51a98af384a1153e9373828f0b00248211e763ee5bd8c7f82861c414625a063b
  source_range:
    end: 1975722
    part: 2
    start: 0
  summary: 对方英文润色稿删除了 MoE communication matrix、A2 的 896 experts/MC2 512 限制说明、sparse offload 和 MRV2 通信项，并将 roadmap 从 58 项缩为 57 项；比较记录指出这些是范围变化而非单纯语言润色。用户随后要求按润色稿直接删除，并进一步明确 GQA DSpark 不在 `releases/v0.26.0rc` 支持，计划在 `main`/MRV2 采用 KV cache 首轴非连续布局支持。记录中曾报告 issue 更新为 55 项（10 已完成、45 待完成），但这些状态来自历史操作记录。
  title: 英文润色版对比与 GQA DSpark 调整
- block_id: b85b86ce1205dae4dd97ae28a63d50c583e24ea2a236a41008bf1dc781c770e3
  indexed: true
  sha256: 753cab51cc8e9d31534d3e7934beb4ad43d41f61cd9c68e1cb3d72fa72056db4
  source_range:
    end: 1975722
    part: 3
    start: 0
  summary: '对仓库 PR 的匹配记录将 #13989 关联到 KDA QKV 融合，#13928 关联原生 MLA head count、`fused_norm_gate`、recurrent KDA 和 attention-residual，#13821 关联 MLA/GQA 首轴非连续 PA cache，并补充 #13683、#13994、#13836、#13954 等候选。核对后明确纠正：#13509 最新正文排除了 KDA target-verify 和 causal-convolution target-verify，因此不能继续挂在这两项，只保留在 `fused_norm_gate` 与 learned attention-residual 相关项。记录称未找到可准确匹配的 Kimi K3 EPD、KDA/QKV 多流、`GMMSituQuant`、A5 FlashDecoding 或 `main` 核心适配 PR，并报告 roadmap 为 57 项、10 已完成、47 待完成。'
  title: Roadmap PR 匹配与旧关联纠正
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f0ab4ba07734cf5ad6f4bf268fee2523c328f60b6b26d806e8d6da85b6f910dd
  kind: experience
  material_digest: 42b9b25a5e8cb688b443660b2bc7daffefee0dc1c89ec405e047a8577a3b216c
  revision: 62884327e51519f4c6e2c2f88c0bec7bcf1351abf4236124c68c3bdde21112ee
  schema: mindie-entry/3
  summary: '核心任务是维护 #13452 的 Kimi K3 capability roadmap：区分 `releases/v0.26.0rc` 与 `main`，保留硬件/量化背景、未完成项和不确定 PR 匹配。最新材料集中在按润色稿删除内容、将 GQA DSpark 移出 0.26.0rc并规划首轴非连续 KV cache方案，以及把候选 PR 精确匹配到 KDA、MLA、EPLB、融合算子等条目；#13509 对 target-verify 的旧关联已被纠正。原始块仍需独立检索，历史完成计数、远端哈希和 PR 匹配均是记录中的报告，不能直接提升为当前已验证事实。'
  title: 'Kimi K3 roadmap / #13452 当前任务导航'
navigation: '核心任务是维护 #13452 的 Kimi K3 capability roadmap：区分 `releases/v0.26.0rc` 与 `main`，保留硬件/量化背景、未完成项和不确定 PR 匹配。最新材料集中在按润色稿删除内容、将 GQA DSpark 移出 0.26.0rc并规划首轴非连续 KV cache方案，以及把候选 PR 精确匹配到 KDA、MLA、EPLB、融合算子等条目；#13509 对 target-verify 的旧关联已被纠正。原始块仍需独立检索，历史完成计数、远端哈希和 PR 匹配均是记录中的报告，不能直接提升为当前已验证事实。'
schema: mindie-material-task/1
status: complete
task_id: f0ab4ba07734cf5ad6f4bf268fee2523c328f60b6b26d806e8d6da85b6f910dd
---

# Kimi K3 roadmap / #13452 当前任务导航

核心任务是维护 #13452 的 Kimi K3 capability roadmap：区分 `releases/v0.26.0rc` 与 `main`，保留硬件/量化背景、未完成项和不确定 PR 匹配。最新材料集中在按润色稿删除内容、将 GQA DSpark 移出 0.26.0rc并规划首轴非连续 KV cache方案，以及把候选 PR 精确匹配到 KDA、MLA、EPLB、融合算子等条目；#13509 对 target-verify 的旧关联已被纠正。原始块仍需独立检索，历史完成计数、远端哈希和 PR 匹配均是记录中的报告，不能直接提升为当前已验证事实。

## Materials

- [Kimi K3 roadmap 初版收敛](blocks/1ecfdc4da66041e6d37c3fcc9ce724ff32ce5779ac95dd7e01b56d3eabdc9524.md): 围绕 issue #13452，将原有 74 个 checkbox 收敛为按 release/main 和用户可感知能力组织的 roadmap，删除 generic foundation、历史细节及重复项。随后根据评审意见把 PD、Prefix Cache、Chunked Prefill、128K/1M 等标为 #12950 已支持，并拆分 MoE、Quantization、Attention 与具体算子；同时记录 EPD、W4A8C8、multistream、GMMSituQuant 等尚未确认或待完成事项。相关操作和检查曾使用 GitHub/live issue 与 PR 核对，但该块中的完成状态和匹配结论属于当时记录，不能视为重新验证。
- [评审意见驱动的 roadmap 重组](blocks/83c68b645394f4b08edb76281f2314fd9fedb2e2d79795bd1b145493c1029edb.md): 用户评审要求删除多节点多模态、async load/COW/recompute、prefix-aware causal convolution 和 QuaRot 等项，并将 EPD、offload、shared-expert multistream、W4A8C8、shared-expert W4A8、GQA DSpark 精度问题及独立融合算子重新组织。记录中的版本将 EPLB 明确为依赖 `MegaMoE` 或 `GMMSituQuant`，并声称已刷新 issue #13452；后续用户又要求恢复硬件执行表，说明 roadmap 内容仍在持续修订。该块还包含 response annotations，要求保留 speaker attribution 与不确定的 PR 检查结果。
- [英文润色版对比与 GQA DSpark 调整](blocks/c7ab75946872d8bf86a0b6f229e44c66f5a221a42f36d0bdef736b7c258a6775.md): 对方英文润色稿删除了 MoE communication matrix、A2 的 896 experts/MC2 512 限制说明、sparse offload 和 MRV2 通信项，并将 roadmap 从 58 项缩为 57 项；比较记录指出这些是范围变化而非单纯语言润色。用户随后要求按润色稿直接删除，并进一步明确 GQA DSpark 不在 `releases/v0.26.0rc` 支持，计划在 `main`/MRV2 采用 KV cache 首轴非连续布局支持。记录中曾报告 issue 更新为 55 项（10 已完成、45 待完成），但这些状态来自历史操作记录。
- [Roadmap PR 匹配与旧关联纠正](blocks/b85b86ce1205dae4dd97ae28a63d50c583e24ea2a236a41008bf1dc781c770e3.md): 对仓库 PR 的匹配记录将 #13989 关联到 KDA QKV 融合，#13928 关联原生 MLA head count、`fused_norm_gate`、recurrent KDA 和 attention-residual，#13821 关联 MLA/GQA 首轴非连续 PA cache，并补充 #13683、#13994、#13836、#13954 等候选。核对后明确纠正：#13509 最新正文排除了 KDA target-verify 和 causal-convolution target-verify，因此不能继续挂在这两项，只保留在 `fused_norm_gate` 与 learned attention-residual 相关项。记录称未找到可准确匹配的 Kimi K3 EPD、KDA/QKV 多流、`GMMSituQuant`、A5 FlashDecoding 或 `main` 核心适配 PR，并报告 roadmap 为 57 项、10 已完成、47 待完成。

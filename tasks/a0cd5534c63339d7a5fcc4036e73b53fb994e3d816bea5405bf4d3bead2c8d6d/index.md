---
blocks:
- block_id: 2be959c37fd3ec8fbc573b21030c47a9e093aefcd0c9a26f4aceeb82081834a5
  indexed: true
  sha256: c8b389797d1771c129e2cc93bd8dddc5c74733b0e9c0d860fb950a5799219e14
  source_range:
    end: 5185241
    part: 0
    start: 0
  summary: 用户最初要求创建私密 vllm-ascend 镜像并将 ci-new 拆分为面向 main 与 release/0.23.0、按 csrc/non-csrc 隔离的四个 Draft PR；此前记录称仓库、分支和 PR 已创建，但该过程包含跨版本整树差异、尾随空格和未运行 runtime/NPU 测试等限制。随后 PR1/PR3 的 Torch Meta 修改、PR4 的 review 来源审计与按用户授权的重构，以及 PR2 的主线适配均被报告为完成，但这些均是过程报告而非独立验证结论。后续又合并为 PR2/PR4 两个 Python+C++ PR，并多次重写 description、Co-author 与代码范围；记录中出现过错误整树覆盖、误带无关算子、删改 top-k/top-p 及版权头审计等更正。
  title: 私密镜像仓四个 PR 的创建与后续审查
- block_id: 6a9997884fa33cab8d9ca15026074e0f749ff8185aa7e3677f60a249acd333a5
  indexed: true
  sha256: 173cc86d1f6fe4ace0571f6400245818c8dc3ff57612c9979b2c6066297df676
  source_range:
    end: 5185241
    part: 1
    start: 0
  summary: 用户对 PR4 review 来源作出明确授权：移除 `.github/**`，SFA/DSA、AscendStore、`linear_op` 和 sampler 遵照官方 release，按意见修改 `ascend_forward_context.py`，并合并含其他作者代码的 Kimi 模型文件。记录称 PR4 更新至提交 `557883fe`，静态检查通过、pytest 因缺少 torch 未启动；随后相同逻辑被应用到 PR2，并为 25 个新增文件补充指定的 2026 Huawei vLLM Ascend 版权头。之后用户要求合并 C++ 与 Python PR，记录称 PR2/PR4 分别形成联合 PR，关闭 PR1/PR3，但同时保留了 CRLF/trailing-whitespace 与未执行完整构建/NPU runtime 等限制。
  title: PR4 review 来源授权与 PR2 主线同步
- block_id: b46c050ff422cde18745c5a68b5d0d951bf5f5d18fd55b9641741589a97fb707
  indexed: true
  sha256: 988c2016dbd6cc86179ceb21ca4046c7ab95167cedce6806c601a81e2fd16360
  source_range:
    end: 5185241
    part: 2
    start: 0
  summary: '用户指出 PR2 错误带入大量无关算子；后续审计承认 C++ 初始策略把 release ci-new 的整棵 csrc 覆盖到 main，报告删除 133 个额外路径并重建 PR2，仅保留筛选后的 Kimi K3 C++ 增量。用户又要求 Co-author 仅显示 GitHub ID、更新邮箱、按字母排序，并持续要求 description 参考上游 PR #9270、删除私仓/本地/测试清理等内部信息，以及移除未实际接入的 custom top-k/top-p。后续材料报告从 PR #9228 和附件表格补充部分邮箱并更新 PR2/PR4，但这些状态、邮箱来源和最终代码/描述内容仍应视为可检索的过程性报告，而非已独立验证的事实。'
  title: PR 描述、Co-author 与主线算子范围的反复修正
- block_id: f26685022cf9508ccd481dd11b43af5673473522972897e27a94305d6f909505
  indexed: true
  sha256: 358b02534b41b1449c6595309491170744596f60fe0c1e549a302c9574913ee8
  source_range:
    end: 5185241
    part: 3
    start: 0
  summary: '该块包含一份含员工标识、GitHub 用户名及部分邮箱映射的数据，其中敏感字段多数以占位符保留，另有一项 noreply 邮箱；随后消息报告称已向 vllm-project/vllm-ascend 创建 PR #12950（main）和 #12951（releases/v0.23.0），并声称未添加 Codex/connector 标识。后续报告称两个 PR 分别被 squash 为单个提交，提交主题为 `support Kimi-K3`，SHA 分别为 `aae2cf7f102c8b0edead54414a6578f89b6d1fa7` 和 `1fd66cfa568d63ee1228ca2bd164f3a2a5604a2d`，保留 `Signed-off-by`；这些都是来源中的过程性报告，不能据此视为独立验证的成功。'
  title: Kimi-K3 两个 PR 的创建与 squash 报告
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a0cd5534c63339d7a5fcc4036e73b53fb994e3d816bea5405bf4d3bead2c8d6d
  kind: experience
  material_digest: 305fec04ae4794890b4be0e08808abb6e1d02c4c67eba96f8184d4238930da8a
  revision: 022dc81432720094373e5faa3356914cd2c3f984fce9ffcfbe178751d7a085ea
  schema: mindie-entry/3
  summary: '当前任务涉及 maoxx241/vllm-ascend 向 vllm-project/vllm-ascend 提交 Kimi-K3 改动的两个 PR：PR #12950 面向 main，PR #12951 面向 releases/v0.23.0。新增材料报告了作者映射、PR 创建、base/head 检查、无 Codex/connector 标识，以及将每个 PR squash 为一个 `support Kimi-K3` 提交；这些状态、SHA、合并性和邮箱映射均应作为可检索但易错的来源报告保留，而不是已验证事实。此前导航中的代码范围、review 来源、Co-author 排序、版权头和未完成的 pytest/C++/NPU/runtime 验证仍然有效；后续修正或未完成结果不得被遗漏。'
  title: vllm-ascend PR 创建、squash 与提交准备索引
navigation: '当前任务涉及 maoxx241/vllm-ascend 向 vllm-project/vllm-ascend 提交 Kimi-K3 改动的两个 PR：PR #12950 面向 main，PR #12951 面向 releases/v0.23.0。新增材料报告了作者映射、PR 创建、base/head 检查、无 Codex/connector 标识，以及将每个 PR squash 为一个 `support Kimi-K3` 提交；这些状态、SHA、合并性和邮箱映射均应作为可检索但易错的来源报告保留，而不是已验证事实。此前导航中的代码范围、review 来源、Co-author 排序、版权头和未完成的 pytest/C++/NPU/runtime 验证仍然有效；后续修正或未完成结果不得被遗漏。'
schema: mindie-material-task/1
status: complete
task_id: a0cd5534c63339d7a5fcc4036e73b53fb994e3d816bea5405bf4d3bead2c8d6d
---

# vllm-ascend PR 创建、squash 与提交准备索引

当前任务涉及 maoxx241/vllm-ascend 向 vllm-project/vllm-ascend 提交 Kimi-K3 改动的两个 PR：PR #12950 面向 main，PR #12951 面向 releases/v0.23.0。新增材料报告了作者映射、PR 创建、base/head 检查、无 Codex/connector 标识，以及将每个 PR squash 为一个 `support Kimi-K3` 提交；这些状态、SHA、合并性和邮箱映射均应作为可检索但易错的来源报告保留，而不是已验证事实。此前导航中的代码范围、review 来源、Co-author 排序、版权头和未完成的 pytest/C++/NPU/runtime 验证仍然有效；后续修正或未完成结果不得被遗漏。

## Materials

- [私密镜像仓四个 PR 的创建与后续审查](blocks/2be959c37fd3ec8fbc573b21030c47a9e093aefcd0c9a26f4aceeb82081834a5.md): 用户最初要求创建私密 vllm-ascend 镜像并将 ci-new 拆分为面向 main 与 release/0.23.0、按 csrc/non-csrc 隔离的四个 Draft PR；此前记录称仓库、分支和 PR 已创建，但该过程包含跨版本整树差异、尾随空格和未运行 runtime/NPU 测试等限制。随后 PR1/PR3 的 Torch Meta 修改、PR4 的 review 来源审计与按用户授权的重构，以及 PR2 的主线适配均被报告为完成，但这些均是过程报告而非独立验证结论。后续又合并为 PR2/PR4 两个 Python+C++ PR，并多次重写 description、Co-author 与代码范围；记录中出现过错误整树覆盖、误带无关算子、删改 top-k/top-p 及版权头审计等更正。
- [PR4 review 来源授权与 PR2 主线同步](blocks/6a9997884fa33cab8d9ca15026074e0f749ff8185aa7e3677f60a249acd333a5.md): 用户对 PR4 review 来源作出明确授权：移除 `.github/**`，SFA/DSA、AscendStore、`linear_op` 和 sampler 遵照官方 release，按意见修改 `ascend_forward_context.py`，并合并含其他作者代码的 Kimi 模型文件。记录称 PR4 更新至提交 `557883fe`，静态检查通过、pytest 因缺少 torch 未启动；随后相同逻辑被应用到 PR2，并为 25 个新增文件补充指定的 2026 Huawei vLLM Ascend 版权头。之后用户要求合并 C++ 与 Python PR，记录称 PR2/PR4 分别形成联合 PR，关闭 PR1/PR3，但同时保留了 CRLF/trailing-whitespace 与未执行完整构建/NPU runtime 等限制。
- [PR 描述、Co-author 与主线算子范围的反复修正](blocks/b46c050ff422cde18745c5a68b5d0d951bf5f5d18fd55b9641741589a97fb707.md): 用户指出 PR2 错误带入大量无关算子；后续审计承认 C++ 初始策略把 release ci-new 的整棵 csrc 覆盖到 main，报告删除 133 个额外路径并重建 PR2，仅保留筛选后的 Kimi K3 C++ 增量。用户又要求 Co-author 仅显示 GitHub ID、更新邮箱、按字母排序，并持续要求 description 参考上游 PR #9270、删除私仓/本地/测试清理等内部信息，以及移除未实际接入的 custom top-k/top-p。后续材料报告从 PR #9228 和附件表格补充部分邮箱并更新 PR2/PR4，但这些状态、邮箱来源和最终代码/描述内容仍应视为可检索的过程性报告，而非已独立验证的事实。
- [Kimi-K3 两个 PR 的创建与 squash 报告](blocks/f26685022cf9508ccd481dd11b43af5673473522972897e27a94305d6f909505.md): 该块包含一份含员工标识、GitHub 用户名及部分邮箱映射的数据，其中敏感字段多数以占位符保留，另有一项 noreply 邮箱；随后消息报告称已向 vllm-project/vllm-ascend 创建 PR #12950（main）和 #12951（releases/v0.23.0），并声称未添加 Codex/connector 标识。后续报告称两个 PR 分别被 squash 为单个提交，提交主题为 `support Kimi-K3`，SHA 分别为 `aae2cf7f102c8b0edead54414a6578f89b6d1fa7` 和 `1fd66cfa568d63ee1228ca2bd164f3a2a5604a2d`，保留 `Signed-off-by`；这些都是来源中的过程性报告，不能据此视为独立验证的成功。

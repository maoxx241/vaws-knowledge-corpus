---
blocks:
- block_id: c6e8e64826939407f575fb1be47b77c991c4156071f1ce6abd474164354056de
  indexed: true
  sha256: bf9fd2e639230f1fbd2fcf53c601cf2daf915ed49c90000208b9034572470ddd
  source_range:
    end: 3997150
    part: 0
    start: 0
  summary: 源码只读审计报告称，hit=384 时仅首个 FullAttention group 被 trim，而 g5 GQA 仍有 2 个 384-token block；HiddenState/as_strided 分支被判断为不活跃，Mamba fresh/COW state 未清零则被保留为真实但未能直接解释 token0 的并发正确性风险。报告还称 r13 四机仅文件部署并以 PREFLIGHT_ONLY=1 回读 SHA、未启动服务；r14 在独立 source copy 上先使旧实现测试失败，再使 targeted 测试与整文件测试通过，但未合入 r13 或做四机部署。
  title: hit384 调度契约审计与 r13/r14 隔离验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d37738c726103dbf297908d9c5fd72a5ddee3bb990a779669de7da0611d6e174
  kind: experience
  material_digest: 1db8367c33d93d33f83972fbb0b8ab89a8d9e6aaafb5db957e39108c7a509236
  revision: 63e298b3f742c8ba31a8c038cedd0126baefe76ac850ba941de3fb5346aacc6a
  schema: mindie-entry/3
  summary: 重点检索 hit384 下 FullAttention 多 group 的 trim 契约、GQA block count、Mamba fresh/COW state 初始化，以及 A（HiddenState/as_strided）与 B（state 未清零）的因果区分。现有材料报告 r13 已完成停服四机 preflight 与 SHA 回读，r14 的 trim-all-full patch/UT 仅在独立环境完成；这些均是来源中的报告性结果，后续仍需保留其未启动、未合入和未部署状态。
  title: 当前任务导航：hit384 调度审计、隔离补丁与停服部署
navigation: 重点检索 hit384 下 FullAttention 多 group 的 trim 契约、GQA block count、Mamba fresh/COW state 初始化，以及 A（HiddenState/as_strided）与 B（state 未清零）的因果区分。现有材料报告 r13 已完成停服四机 preflight 与 SHA 回读，r14 的 trim-all-full patch/UT 仅在独立环境完成；这些均是来源中的报告性结果，后续仍需保留其未启动、未合入和未部署状态。
schema: mindie-material-task/1
status: complete
task_id: d37738c726103dbf297908d9c5fd72a5ddee3bb990a779669de7da0611d6e174
---

# 当前任务导航：hit384 调度审计、隔离补丁与停服部署

重点检索 hit384 下 FullAttention 多 group 的 trim 契约、GQA block count、Mamba fresh/COW state 初始化，以及 A（HiddenState/as_strided）与 B（state 未清零）的因果区分。现有材料报告 r13 已完成停服四机 preflight 与 SHA 回读，r14 的 trim-all-full patch/UT 仅在独立环境完成；这些均是来源中的报告性结果，后续仍需保留其未启动、未合入和未部署状态。

## Materials

- [hit384 调度契约审计与 r13/r14 隔离验证](blocks/c6e8e64826939407f575fb1be47b77c991c4156071f1ce6abd474164354056de.md): 源码只读审计报告称，hit=384 时仅首个 FullAttention group 被 trim，而 g5 GQA 仍有 2 个 384-token block；HiddenState/as_strided 分支被判断为不活跃，Mamba fresh/COW state 未清零则被保留为真实但未能直接解释 token0 的并发正确性风险。报告还称 r13 四机仅文件部署并以 PREFLIGHT_ONLY=1 回读 SHA、未启动服务；r14 在独立 source copy 上先使旧实现测试失败，再使 targeted 测试与整文件测试通过，但未合入 r13 或做四机部署。

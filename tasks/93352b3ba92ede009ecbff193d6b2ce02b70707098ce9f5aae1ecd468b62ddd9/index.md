---
blocks:
- block_id: a53804f5cd04a41b5d60448a85a8714db03604aed0ae1a607a12137b195946aa
  indexed: true
  sha256: 1d511043f3ee25546b85767fad663acfca2b2f0660915852fef039c144d8604c
  source_range:
    end: 3262963
    part: 0
    start: 0
  summary: '据记录，已创建 vllm-ascend PR #14426，范围收窄为 releases/v0.26.0rc 中已合入的 K3 AscendC 算子及相关构建、绑定和直接测试，排除 Triton、模型、DSpark、MoE 等接线，并对齐 vLLM v0.27.1。记录称格式检查、113 文件范围审计、A3 分片构建及 7 个直接 NPU 用例通过，KDA 干净包测试为 4 passed，A5 MX-SiTU 仅完成 Ascend 950 交叉编译，未声称硬件执行。期间曾出现版本注入、动态 kernel JSON 缺失、扩展加载路径等验证环境问题；官方 selected-tests 尚未闭环，ci-gate 红灯被归因于 Draft/ready 标签门禁，相关成功状态均属于记录中的报告而非独立核验。'
  title: K3 AscendC 算子 Draft PR 与验证状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 93352b3ba92ede009ecbff193d6b2ce02b70707098ce9f5aae1ecd468b62ddd9
  kind: experience
  material_digest: f49f8439bd59c3d7f2ef22273127298746f025935ddcc445b7b66c6d544a037f
  revision: b01a7d857d8357468ee2972a78c1c6020fa23078b0fd96c6032d991b8668ce5f
  schema: mindie-entry/3
  summary: '唯一已索引材料围绕基于主线提交 K3 AscendC 算子 Draft PR #14426：包含范围边界、分支/提交/推送与 PR 操作记录、A3 构建和小规模执行验证、A5 交叉编译，以及未完成的官方 selected-tests。检索重点包括 v0.26.0rc、v0.27.1、KDA、chunk gated delta rule、Dequant-SiTU、MX-SiTU、_C_ascend、动态 kernel JSON、Python 扩展加载、ready/ready-a5 标签和 ci-gate；原始材料仍应独立检索，不能把其中的报告性通过结论视为已验证案例。'
  title: 当前任务导航：K3 AscendC 算子 PR 索引
navigation: '唯一已索引材料围绕基于主线提交 K3 AscendC 算子 Draft PR #14426：包含范围边界、分支/提交/推送与 PR 操作记录、A3 构建和小规模执行验证、A5 交叉编译，以及未完成的官方 selected-tests。检索重点包括 v0.26.0rc、v0.27.1、KDA、chunk gated delta rule、Dequant-SiTU、MX-SiTU、_C_ascend、动态 kernel JSON、Python 扩展加载、ready/ready-a5 标签和 ci-gate；原始材料仍应独立检索，不能把其中的报告性通过结论视为已验证案例。'
schema: mindie-material-task/1
status: complete
task_id: 93352b3ba92ede009ecbff193d6b2ce02b70707098ce9f5aae1ecd468b62ddd9
---

# 当前任务导航：K3 AscendC 算子 PR 索引

唯一已索引材料围绕基于主线提交 K3 AscendC 算子 Draft PR #14426：包含范围边界、分支/提交/推送与 PR 操作记录、A3 构建和小规模执行验证、A5 交叉编译，以及未完成的官方 selected-tests。检索重点包括 v0.26.0rc、v0.27.1、KDA、chunk gated delta rule、Dequant-SiTU、MX-SiTU、_C_ascend、动态 kernel JSON、Python 扩展加载、ready/ready-a5 标签和 ci-gate；原始材料仍应独立检索，不能把其中的报告性通过结论视为已验证案例。

## Materials

- [K3 AscendC 算子 Draft PR 与验证状态](blocks/a53804f5cd04a41b5d60448a85a8714db03604aed0ae1a607a12137b195946aa.md): 据记录，已创建 vllm-ascend PR #14426，范围收窄为 releases/v0.26.0rc 中已合入的 K3 AscendC 算子及相关构建、绑定和直接测试，排除 Triton、模型、DSpark、MoE 等接线，并对齐 vLLM v0.27.1。记录称格式检查、113 文件范围审计、A3 分片构建及 7 个直接 NPU 用例通过，KDA 干净包测试为 4 passed，A5 MX-SiTU 仅完成 Ascend 950 交叉编译，未声称硬件执行。期间曾出现版本注入、动态 kernel JSON 缺失、扩展加载路径等验证环境问题；官方 selected-tests 尚未闭环，ci-gate 红灯被归因于 Draft/ready 标签门禁，相关成功状态均属于记录中的报告而非独立核验。

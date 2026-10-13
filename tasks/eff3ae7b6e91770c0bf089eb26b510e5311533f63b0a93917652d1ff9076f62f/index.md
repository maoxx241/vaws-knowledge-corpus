---
blocks:
- block_id: ff86fd89f677c6e31a7e689de181397619e99acd0738288d6b5dcf6b4c568cfb
  indexed: true
  sha256: 98f1e5e02e4aca43cc120b0a44fbf6b8991379cc6ffce11586ecc2828296ff08
  source_range:
    end: 3123028
    part: 0
    start: 0
  summary: 只读审计称四平面 forward 视图、默认 384 对齐的 Prefix Cache replay 在静态上闭合；但 zeroer 不属于当前 DSpark 生产路径，plane-major CoW helper 是潜在但默认未触发的兼容缺口。审计将 Mamba copy metadata 生命周期标为可能导致“前一步异常、下一步首个 KDA all-gather 暴露”的活跃候选，并指出其仅在 `copy_bufs.offset>0` 时成立；offset=0 的短 c64 场景尚未归因。结论未修改文件、未操作远端，且仍缺少四 lane CoW、时序、gate 和输出一致性测试。
  title: Ascend 四平面 KV 契约与 DSpark 卡死排查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: eff3ae7b6e91770c0bf089eb26b510e5311533f63b0a93917652d1ff9076f62f
  kind: experience
  material_digest: c71459b26df0293c291258c4b7456c5d53e3e28c3998b4e901f5686f4c837e2f
  revision: 581f172922203234b7616a7fb72d5432c76cf08f004da16e57dbb6177bae9082
  schema: mindie-entry/3
  summary: 重点检索四平面 page/view/stride/data_ptr 契约、384 对齐 Prefix replay、DSpark zeroer gate、plane-major CoW 风险，以及 Mamba metadata H2D 与 KDA all-gather 的时序候选。保留关键不确定性：`copy_bufs.offset=0` 可排除 metadata-copy 假设，短 c64 的实际阻塞原因仍未确定；相关修复候选和测试缺口均未被验证为成功。
  title: 当前任务导航：Ascend KV 四平面与 DSpark 排查
navigation: 重点检索四平面 page/view/stride/data_ptr 契约、384 对齐 Prefix replay、DSpark zeroer gate、plane-major CoW 风险，以及 Mamba metadata H2D 与 KDA all-gather 的时序候选。保留关键不确定性：`copy_bufs.offset=0` 可排除 metadata-copy 假设，短 c64 的实际阻塞原因仍未确定；相关修复候选和测试缺口均未被验证为成功。
schema: mindie-material-task/1
status: complete
task_id: eff3ae7b6e91770c0bf089eb26b510e5311533f63b0a93917652d1ff9076f62f
---

# 当前任务导航：Ascend KV 四平面与 DSpark 排查

重点检索四平面 page/view/stride/data_ptr 契约、384 对齐 Prefix replay、DSpark zeroer gate、plane-major CoW 风险，以及 Mamba metadata H2D 与 KDA all-gather 的时序候选。保留关键不确定性：`copy_bufs.offset=0` 可排除 metadata-copy 假设，短 c64 的实际阻塞原因仍未确定；相关修复候选和测试缺口均未被验证为成功。

## Materials

- [Ascend 四平面 KV 契约与 DSpark 卡死排查](blocks/ff86fd89f677c6e31a7e689de181397619e99acd0738288d6b5dcf6b4c568cfb.md): 只读审计称四平面 forward 视图、默认 384 对齐的 Prefix Cache replay 在静态上闭合；但 zeroer 不属于当前 DSpark 生产路径，plane-major CoW helper 是潜在但默认未触发的兼容缺口。审计将 Mamba copy metadata 生命周期标为可能导致“前一步异常、下一步首个 KDA all-gather 暴露”的活跃候选，并指出其仅在 `copy_bufs.offset>0` 时成立；offset=0 的短 c64 场景尚未归因。结论未修改文件、未操作远端，且仍缺少四 lane CoW、时序、gate 和输出一致性测试。

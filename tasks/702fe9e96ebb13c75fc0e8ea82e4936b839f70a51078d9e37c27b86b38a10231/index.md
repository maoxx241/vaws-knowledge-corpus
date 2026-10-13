---
blocks:
- block_id: 790855a1c46bc25039fe5577c49326448021cd45134aea701b48f50d4243aba2
  indexed: true
  sha256: 7ddd4ae9ed8448c1dbbbdd4e784d4f41363cedf3b5be4393b929948dc1bfaf6f
  source_range:
    end: 2769308
    part: 0
    start: 0
  summary: 对照 SGLang 与 vLLM 后，观察到 SGLang 将 graph padding 的 KDA state slot 保持为 `-1`，并在 target verify 使用独立 scratch、接受后再提交；当前 vLLM ACLGraph 路径可能将 dummy slot 从 `-1` 改为 block 0，疑似与首轮 fallback token 0 有关，但仍属待验证假设。建议用 `spec_query_start_loc[-1]` 的 device mask 清零 `core_spec` 尾部，并仅对 Kimi K3 AscendC KDA 保留 `state=-1`，普通 Triton GDN 继续执行 `-1 → 0`；需补充 metadata、data_ptr、输出尾部、mixed batch 与 ACLGraph replay 断言。静态检查通过但因本机缺少 `torch` 未运行 pytest，尚未修改文件；若修复后接收率仍低，还需继续核对 speculative scratch 与 accept 后 commit。
  title: K=7 图执行中 KDA padding 与 fallback token 0 排查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 702fe9e96ebb13c75fc0e8ea82e4936b839f70a51078d9e37c27b86b38a10231
  kind: experience
  material_digest: 2029a91dfa74a8515599302720dd4f231f54fc034b41a8b39cda2cf97ea2afa0
  revision: 60042073aee94a522ab7447bfcdbe6628234dc7088a1ad058447a42e3e556891
  schema: mindie-entry/3
  summary: 当前重点是验证 ACLGraph 下 KDA dummy 行的 padding state 是否因被改为 0 或读取未初始化值而产生首轮 fallback token 0。已记录 mixed batch mask 边界、Kimi AscendC 与通用 Triton GDN 的分支约束，以及未完成的运行时断言和 scratch/commit 对照；静态检查通过，但 pytest 未执行、实现未修改，相关根因仍未证实。
  title: K=7 KDA padding / fallback 排查索引
navigation: 当前重点是验证 ACLGraph 下 KDA dummy 行的 padding state 是否因被改为 0 或读取未初始化值而产生首轮 fallback token 0。已记录 mixed batch mask 边界、Kimi AscendC 与通用 Triton GDN 的分支约束，以及未完成的运行时断言和 scratch/commit 对照；静态检查通过，但 pytest 未执行、实现未修改，相关根因仍未证实。
schema: mindie-material-task/1
status: complete
task_id: 702fe9e96ebb13c75fc0e8ea82e4936b839f70a51078d9e37c27b86b38a10231
---

# K=7 KDA padding / fallback 排查索引

当前重点是验证 ACLGraph 下 KDA dummy 行的 padding state 是否因被改为 0 或读取未初始化值而产生首轮 fallback token 0。已记录 mixed batch mask 边界、Kimi AscendC 与通用 Triton GDN 的分支约束，以及未完成的运行时断言和 scratch/commit 对照；静态检查通过，但 pytest 未执行、实现未修改，相关根因仍未证实。

## Materials

- [K=7 图执行中 KDA padding 与 fallback token 0 排查](blocks/790855a1c46bc25039fe5577c49326448021cd45134aea701b48f50d4243aba2.md): 对照 SGLang 与 vLLM 后，观察到 SGLang 将 graph padding 的 KDA state slot 保持为 `-1`，并在 target verify 使用独立 scratch、接受后再提交；当前 vLLM ACLGraph 路径可能将 dummy slot 从 `-1` 改为 block 0，疑似与首轮 fallback token 0 有关，但仍属待验证假设。建议用 `spec_query_start_loc[-1]` 的 device mask 清零 `core_spec` 尾部，并仅对 Kimi K3 AscendC KDA 保留 `state=-1`，普通 Triton GDN 继续执行 `-1 → 0`；需补充 metadata、data_ptr、输出尾部、mixed batch 与 ACLGraph replay 断言。静态检查通过但因本机缺少 `torch` 未运行 pytest，尚未修改文件；若修复后接收率仍低，还需继续核对 speculative scratch 与 accept 后 commit。

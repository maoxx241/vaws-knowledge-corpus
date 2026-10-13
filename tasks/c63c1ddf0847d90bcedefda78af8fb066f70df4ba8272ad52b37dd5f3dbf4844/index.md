---
blocks:
- block_id: 3eb65e4f33fbd43e23cd8df72d1daba5d044ef4732c8018d7bbaeedc7aa531c9
  indexed: true
  sha256: e6675265bbe67187103681588d9f9dc5b3fac6a002f2f4a2b55a05ca6019e3cc
  source_range:
    end: 4223727
    part: 0
    start: 0
  summary: 审计记录认为 896/hit384 的 miss 与 hit 均执行 512-token KDA，hit 的 col0 来自 prefix cache；首个 target token 已为 0，因此 acceptance 100% 不能证明结果正确。旧 DSpark zeroer 未启用且不清 Mamba，当前仍怀疑 cached Mamba/MLA source 或共享 plane view；r13 runner/verifier 已准备但未发送请求、未修改服务，旧 r12 坏结果仅完成只读负向回放。结论和根因定位仍属待验证假设，后续建议用完整字节 hash 及首处分叉层的 Triton/AscendC A/B 分二分验证。
  title: hit384 缓存状态审计与 r13 验证准备
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c63c1ddf0847d90bcedefda78af8fb066f70df4ba8272ad52b37dd5f3dbf4844
  kind: experience
  material_digest: e9247f92d44191bd566a7c8c4d58624c85b85255c8540a69319fd4c31f98f0f3
  revision: 589b19a0977691d80f09dc93988bb683a7767d3cde6550d56cd15cbb000e7b5d
  schema: mindie-entry/3
  summary: 当前材料集中于 hit384 的 prefix-cached Mamba/MLA 状态、首 token 为 0 与 acceptance 100% 的错误解释风险，以及 r12 zeroer 未启用这一更正。r13 runner/verifier、哈希和 golden 仅已准备或回放验证，尚无在线请求、服务变更或根因确认；完整字节 hash、诊断重启和 exact-input 算子 A/B 仍未完成。原始材料块保留独立可检索，未提供的更早细节不作记忆性补充。
  title: 当前任务导航：hit384 缓存路径审计与 r13 待验证
navigation: 当前材料集中于 hit384 的 prefix-cached Mamba/MLA 状态、首 token 为 0 与 acceptance 100% 的错误解释风险，以及 r12 zeroer 未启用这一更正。r13 runner/verifier、哈希和 golden 仅已准备或回放验证，尚无在线请求、服务变更或根因确认；完整字节 hash、诊断重启和 exact-input 算子 A/B 仍未完成。原始材料块保留独立可检索，未提供的更早细节不作记忆性补充。
schema: mindie-material-task/1
status: complete
task_id: c63c1ddf0847d90bcedefda78af8fb066f70df4ba8272ad52b37dd5f3dbf4844
---

# 当前任务导航：hit384 缓存路径审计与 r13 待验证

当前材料集中于 hit384 的 prefix-cached Mamba/MLA 状态、首 token 为 0 与 acceptance 100% 的错误解释风险，以及 r12 zeroer 未启用这一更正。r13 runner/verifier、哈希和 golden 仅已准备或回放验证，尚无在线请求、服务变更或根因确认；完整字节 hash、诊断重启和 exact-input 算子 A/B 仍未完成。原始材料块保留独立可检索，未提供的更早细节不作记忆性补充。

## Materials

- [hit384 缓存状态审计与 r13 验证准备](blocks/3eb65e4f33fbd43e23cd8df72d1daba5d044ef4732c8018d7bbaeedc7aa531c9.md): 审计记录认为 896/hit384 的 miss 与 hit 均执行 512-token KDA，hit 的 col0 来自 prefix cache；首个 target token 已为 0，因此 acceptance 100% 不能证明结果正确。旧 DSpark zeroer 未启用且不清 Mamba，当前仍怀疑 cached Mamba/MLA source 或共享 plane view；r13 runner/verifier 已准备但未发送请求、未修改服务，旧 r12 坏结果仅完成只读负向回放。结论和根因定位仍属待验证假设，后续建议用完整字节 hash 及首处分叉层的 Triton/AscendC A/B 分二分验证。

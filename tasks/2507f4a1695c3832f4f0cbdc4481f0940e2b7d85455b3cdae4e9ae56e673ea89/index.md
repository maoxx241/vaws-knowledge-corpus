---
blocks:
- block_id: 10a810ff3660141ef944782eb7c0934613dffaf76b4c66ecddcd1026a6ffd37e
  indexed: true
  sha256: ef4562960412f82864b6c0a2da93b6b7315b5d842470d1b3a72d6e0b98b7c66a
  source_range:
    end: 930909
    part: 0
    start: 0
  summary: 已对齐 second-hit、full-output runner 与 r12 契约，规划并准备了只部署不发请求的 P3 目录，包含 129792-token shared-prefix sentinel、131072-token poison/full 提示、原始 SSE、payload、token IDs/hash 与分阶段 metrics。脚本覆盖 reset、9 次请求流程、replay=129408 校验、preemption/DP0/accepted scalar 一致性检查及独立 verifier；合成同结构证据的正向自测通过，sentinel token 和 metrics 篡改的反向验证也按预期失败。消息称已部署但未发送请求，实际实验结果目录不存在，因此尚未确认真实服务执行或成功。
  title: P3 128K history/poison 部署准备与验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2507f4a1695c3832f4f0cbdc4481f0940e2b7d85455b3cdae4e9ae56e673ea89
  kind: experience
  material_digest: 09ded995d65550196288ee8be8efd7ae4fb0715eec5fb7a0f57a1924dda5d3ef
  revision: 246e2aad8a25d6e63863cd3e66fc396484f7616c8cc8cb282f9fa25ec91d160b
  schema: mindie-entry/3
  summary: 当前状态是实验脚本与验证器已准备并完成合成证据自测，真实实验尚未执行；计划流程为两段 reset、六次 129792-token sentinel、两次 131072-token poison，随后 reset、三次 sentinel 和一次 full。检索重点包括 shared-prefix token/hash 一致性、raw SSE/payload/metrics 留存、129408 replay、输出质量、preemption、DP0 变化及独立重解析；部署或静态验证不应视为真实请求成功。
  title: P3 128K history/poison 任务导航
navigation: 当前状态是实验脚本与验证器已准备并完成合成证据自测，真实实验尚未执行；计划流程为两段 reset、六次 129792-token sentinel、两次 131072-token poison，随后 reset、三次 sentinel 和一次 full。检索重点包括 shared-prefix token/hash 一致性、raw SSE/payload/metrics 留存、129408 replay、输出质量、preemption、DP0 变化及独立重解析；部署或静态验证不应视为真实请求成功。
schema: mindie-material-task/1
status: complete
task_id: 2507f4a1695c3832f4f0cbdc4481f0940e2b7d85455b3cdae4e9ae56e673ea89
---

# P3 128K history/poison 任务导航

当前状态是实验脚本与验证器已准备并完成合成证据自测，真实实验尚未执行；计划流程为两段 reset、六次 129792-token sentinel、两次 131072-token poison，随后 reset、三次 sentinel 和一次 full。检索重点包括 shared-prefix token/hash 一致性、raw SSE/payload/metrics 留存、129408 replay、输出质量、preemption、DP0 变化及独立重解析；部署或静态验证不应视为真实请求成功。

## Materials

- [P3 128K history/poison 部署准备与验证](blocks/10a810ff3660141ef944782eb7c0934613dffaf76b4c66ecddcd1026a6ffd37e.md): 已对齐 second-hit、full-output runner 与 r12 契约，规划并准备了只部署不发请求的 P3 目录，包含 129792-token shared-prefix sentinel、131072-token poison/full 提示、原始 SSE、payload、token IDs/hash 与分阶段 metrics。脚本覆盖 reset、9 次请求流程、replay=129408 校验、preemption/DP0/accepted scalar 一致性检查及独立 verifier；合成同结构证据的正向自测通过，sentinel token 和 metrics 篡改的反向验证也按预期失败。消息称已部署但未发送请求，实际实验结果目录不存在，因此尚未确认真实服务执行或成功。

---
blocks:
- block_id: 50512213b2e501307d0d7e917366df5272b89f77d7b2ff636f2cb3b769eec3f4
  indexed: true
  sha256: b11862528f9c2f7b456dcd242e4a0348ced59866af1dcabdf3368b590f2fac4a
  source_range:
    end: 1396398
    part: 0
    start: 0
  summary: 该审计确认 shared-expert-DP 在 FlashComm1=false、TP>1+EP 配置下存在 globalBS shape/capacity contract 不一致；DeepSeek V4 的 shared-expert 分片与归约路径变化已确认，但 exact-token mismatch 的唯一因果仍属 strong suspect，未被 runtime/bisect 证明。另识别出多项广域风险，包括非量化 MoE generic dispatch、KV Pool lease/refcount 去重缺失、全局 g_proj 匹配导致的 Kimi MLA gate 问题、RoPE 缓存 alias、进程级 custom-op 注册、A2 大专家数 AllGather、W8A8MXFP 扩展及多处 hybrid/monkey-patch 作用域扩大；KV spec 的布局字段校验缺口也已确认，但实际触发尚不确定。建议保留 Kimi 局部实现，将 shared-expert、SiTU、linear prefix 等改为显式 capability，并恢复 standalone shared-expert-DP 与 KV lease 去重语义；结论来自固定 head 的静态只读审计，未读取工作区改动、未改文件、未启动服务。
  title: 固定范围只读审计：跨模型回归与 Kimi 作用域风险
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b96ab938ec352048e65aeef9f87fdf870e99e6a7c1b9dacdaf53c4277271ece8
  kind: experience
  material_digest: 73e30f0b7b3500745f879c39b739ee4c18a140d9551086711e3a78f0b3747db5
  revision: fd48ec54402d315a27e9d2cc7b27b33b83d2f01a73e0d35b10c18153ee8f4802
  schema: mindie-entry/3
  summary: 唯一新材料块记录了固定范围 `81d3450..4a1857b` 的静态审计结论：globalBS invalid 为 confirmed，DeepSeek V4 token mismatch 的路径变化为 confirmed 但唯一根因未验证；其余条目按 confirmed、suspect、作用域风险和建议收窄方向区分。审计未执行 runtime/bisect，未将工作区未提交修改视为 PR 事实；后续检索应优先按 shared-expert-DP、FlashComm1、KV Pool lease、Kimi g_proj、KV spec invariant 等术语定位。
  title: 当前任务导航：固定范围只读审计索引
navigation: 唯一新材料块记录了固定范围 `81d3450..4a1857b` 的静态审计结论：globalBS invalid 为 confirmed，DeepSeek V4 token mismatch 的路径变化为 confirmed 但唯一根因未验证；其余条目按 confirmed、suspect、作用域风险和建议收窄方向区分。审计未执行 runtime/bisect，未将工作区未提交修改视为 PR 事实；后续检索应优先按 shared-expert-DP、FlashComm1、KV Pool lease、Kimi g_proj、KV spec invariant 等术语定位。
schema: mindie-material-task/1
status: complete
task_id: b96ab938ec352048e65aeef9f87fdf870e99e6a7c1b9dacdaf53c4277271ece8
---

# 当前任务导航：固定范围只读审计索引

唯一新材料块记录了固定范围 `81d3450..4a1857b` 的静态审计结论：globalBS invalid 为 confirmed，DeepSeek V4 token mismatch 的路径变化为 confirmed 但唯一根因未验证；其余条目按 confirmed、suspect、作用域风险和建议收窄方向区分。审计未执行 runtime/bisect，未将工作区未提交修改视为 PR 事实；后续检索应优先按 shared-expert-DP、FlashComm1、KV Pool lease、Kimi g_proj、KV spec invariant 等术语定位。

## Materials

- [固定范围只读审计：跨模型回归与 Kimi 作用域风险](blocks/50512213b2e501307d0d7e917366df5272b89f77d7b2ff636f2cb3b769eec3f4.md): 该审计确认 shared-expert-DP 在 FlashComm1=false、TP>1+EP 配置下存在 globalBS shape/capacity contract 不一致；DeepSeek V4 的 shared-expert 分片与归约路径变化已确认，但 exact-token mismatch 的唯一因果仍属 strong suspect，未被 runtime/bisect 证明。另识别出多项广域风险，包括非量化 MoE generic dispatch、KV Pool lease/refcount 去重缺失、全局 g_proj 匹配导致的 Kimi MLA gate 问题、RoPE 缓存 alias、进程级 custom-op 注册、A2 大专家数 AllGather、W8A8MXFP 扩展及多处 hybrid/monkey-patch 作用域扩大；KV spec 的布局字段校验缺口也已确认，但实际触发尚不确定。建议保留 Kimi 局部实现，将 shared-expert、SiTU、linear prefix 等改为显式 capability，并恢复 standalone shared-expert-DP 与 KV lease 去重语义；结论来自固定 head 的静态只读审计，未读取工作区改动、未改文件、未启动服务。

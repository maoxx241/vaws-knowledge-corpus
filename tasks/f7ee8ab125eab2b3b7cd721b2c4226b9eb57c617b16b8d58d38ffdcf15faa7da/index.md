---
blocks:
- block_id: 920f6f525807b89266672c19c37261ac00152eca1126cb8858dcb0cafc236654
  indexed: true
  sha256: 719e89b1d75d620d095409f6878629e7defc303e3044b37b7878f83cecf04a98
  source_range:
    end: 217397
    part: 0
    start: 0
  summary: 用户授权对真实 MindIE Agent 进行一次手动 activate，要求不设置 CODEX_THREAD_ID、不查询或使用知识、不 remote、不调查或修改源码、不重试、不 deactivate，并仅读取指定的既有记录；记录标记为 CANDIDATE_ACTIVATED_NO_QUERY_DONE。助手声称已读取技能说明并执行一次原生 activate，但该材料未提供可独立核验的命令输出。后续总结称这是 2026-09-18 的历史隔离 RMSNorm 样例：物理设备 8 映射为 npu:0，CPU float32 参考，FP16/BF16 误差阈值分别为 0.002/0.016，且报告了断言、同步、哈希、退出码、120 秒和 CPU 张量预算检查；容器内无 npu-smi，不能外推为整模或生产验收结论。
  title: MindIE Agent 激活与历史 RMSNorm 验收记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f7ee8ab125eab2b3b7cd721b2c4226b9eb57c617b16b8d58d38ffdcf15faa7da
  kind: experience
  material_digest: eecc9faef50e113daff032a1a7a628c058fbb8ee52b8826063033efe67a89d16
  revision: 0a6a5e9a96186cfc4c94b698c48819b98294dbce65088cb3193daf7263517b6c
  schema: mindie-entry/3
  summary: 当前材料涉及一次受限的 MindIE Agent activate 操作及一条历史 RMSNorm 隔离样例记录。激活状态标记为 CANDIDATE_ACTIVATED_NO_QUERY_DONE；实际命令结果未在材料中完整呈现，因此不应视为已验证成功。历史记录包含 NPU 映射、误差阈值与退出条件，但明确不能代表整模或生产级验收。
  title: MindIE Agent 验收材料导航
navigation: 当前材料涉及一次受限的 MindIE Agent activate 操作及一条历史 RMSNorm 隔离样例记录。激活状态标记为 CANDIDATE_ACTIVATED_NO_QUERY_DONE；实际命令结果未在材料中完整呈现，因此不应视为已验证成功。历史记录包含 NPU 映射、误差阈值与退出条件，但明确不能代表整模或生产级验收。
schema: mindie-material-task/1
status: complete
task_id: f7ee8ab125eab2b3b7cd721b2c4226b9eb57c617b16b8d58d38ffdcf15faa7da
---

# MindIE Agent 验收材料导航

当前材料涉及一次受限的 MindIE Agent activate 操作及一条历史 RMSNorm 隔离样例记录。激活状态标记为 CANDIDATE_ACTIVATED_NO_QUERY_DONE；实际命令结果未在材料中完整呈现，因此不应视为已验证成功。历史记录包含 NPU 映射、误差阈值与退出条件，但明确不能代表整模或生产级验收。

## Materials

- [MindIE Agent 激活与历史 RMSNorm 验收记录](blocks/920f6f525807b89266672c19c37261ac00152eca1126cb8858dcb0cafc236654.md): 用户授权对真实 MindIE Agent 进行一次手动 activate，要求不设置 CODEX_THREAD_ID、不查询或使用知识、不 remote、不调查或修改源码、不重试、不 deactivate，并仅读取指定的既有记录；记录标记为 CANDIDATE_ACTIVATED_NO_QUERY_DONE。助手声称已读取技能说明并执行一次原生 activate，但该材料未提供可独立核验的命令输出。后续总结称这是 2026-09-18 的历史隔离 RMSNorm 样例：物理设备 8 映射为 npu:0，CPU float32 参考，FP16/BF16 误差阈值分别为 0.002/0.016，且报告了断言、同步、哈希、退出码、120 秒和 CPU 张量预算检查；容器内无 npu-smi，不能外推为整模或生产验收结论。

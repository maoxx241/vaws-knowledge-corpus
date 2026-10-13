---
blocks:
- block_id: 6d5842ab74cc6fb5ca3649fd49d89fdb809f3390652d778afcf3166f05aac7bb
  indexed: true
  sha256: a44b8956cf288a17ada87478131c3744d226b89d55641a4c7fc9e4a14bffb442
  source_range:
    end: 2484216
    part: 0
    start: 0
  summary: 在 155/156 上完成了 Kimi K3 P/D 与 proxy 的多轮启动、故障定位和重启；曾遇到 tokenizer 缓存只读、容器级 SIGTERM/SIGKILL、端口占用及自动化链干扰，均保留了 R1–R5 失败或中断记录。156 的日志直接确认使用 `vllm_ascend.core.recompute_scheduler.AsyncRecomputeScheduler`，最终基线压力覆盖 c=1/2/4/8/16、8192 输入和 256 输出，31/31 请求成功、KV transfer 全 rank 完整，但没有触发 preemption 或 recompute 分支。qjj_dspark 未操作，153/154 未访问。
  title: K3 双机 P/D 启动与基线压力验证
- block_id: f9cb6d19a34521c254a3eb374b38acc5903d464b92a4db0bd79dfe1a04df967c
  indexed: true
  sha256: 5d7da7c481038f81a6ec8bbd1aa3e9c6fbd4b5e1c0488ef9e7281e233544f42a
  source_range:
    end: 2484216
    part: 1
    start: 0
  summary: 基线日志、失败尝试和结果已复制到 host artifacts；随后进行了单变量 D 端 KV 容量实验：R6 将容量降至 107,777 tokens，R7 降至 64,814 tokens。R6/R7 的 c=16 请求仍分别 16/16 成功且无 preemption，R7 达到 92.6% KV、7 running/9 waiting，显示 scheduler 通过等待控制并发，但未覆盖 recompute 恢复路径。最终保留 P=R5、D=R7、proxy=R5，服务健康、16/16 rank 初始化完成、Traceback/ERROR 为 0；旧 kimi_c8 容器仅为空闲 bash，未运行 vLLM。
  title: R6/R7 容量诊断与终态审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 216fb4ddb8d3d4bb0ddcf0fa60601cde450f8b58b950a9b657be804c6aaa1183
  kind: experience
  material_digest: 47c5f3118a506102d5ab916776a449be9cd37b5d5698570402e54682fbef1442
  revision: b27b09b5a41cb5de5dd9b39c1b4bdf30d479a62e83d1c59c8d52004d60e50eee
  schema: mindie-entry/3
  summary: 已建立两块材料的独立检索索引：第一块记录 155/156 的边界控制、启动失败与修正，以及 R5 基线五档压力成功但未触发 recompute；第二块记录 artifacts 固化、R6/R7 降低 D 端 KV 容量的诊断实验和最终运行状态。当前最可靠结论是 scheduler 已由直接日志证实启用，但规定 workload 只证明请求完成与等待控制，尚未证明 preemption/recompute 分支实际执行；失败尝试、外部终止、端口冲突和自动化链干扰均应保留为可检索上下文。
  title: K3 P/D recompute scheduler 验证索引
navigation: 已建立两块材料的独立检索索引：第一块记录 155/156 的边界控制、启动失败与修正，以及 R5 基线五档压力成功但未触发 recompute；第二块记录 artifacts 固化、R6/R7 降低 D 端 KV 容量的诊断实验和最终运行状态。当前最可靠结论是 scheduler 已由直接日志证实启用，但规定 workload 只证明请求完成与等待控制，尚未证明 preemption/recompute 分支实际执行；失败尝试、外部终止、端口冲突和自动化链干扰均应保留为可检索上下文。
schema: mindie-material-task/1
status: complete
task_id: 216fb4ddb8d3d4bb0ddcf0fa60601cde450f8b58b950a9b657be804c6aaa1183
---

# K3 P/D recompute scheduler 验证索引

已建立两块材料的独立检索索引：第一块记录 155/156 的边界控制、启动失败与修正，以及 R5 基线五档压力成功但未触发 recompute；第二块记录 artifacts 固化、R6/R7 降低 D 端 KV 容量的诊断实验和最终运行状态。当前最可靠结论是 scheduler 已由直接日志证实启用，但规定 workload 只证明请求完成与等待控制，尚未证明 preemption/recompute 分支实际执行；失败尝试、外部终止、端口冲突和自动化链干扰均应保留为可检索上下文。

## Materials

- [K3 双机 P/D 启动与基线压力验证](blocks/6d5842ab74cc6fb5ca3649fd49d89fdb809f3390652d778afcf3166f05aac7bb.md): 在 155/156 上完成了 Kimi K3 P/D 与 proxy 的多轮启动、故障定位和重启；曾遇到 tokenizer 缓存只读、容器级 SIGTERM/SIGKILL、端口占用及自动化链干扰，均保留了 R1–R5 失败或中断记录。156 的日志直接确认使用 `vllm_ascend.core.recompute_scheduler.AsyncRecomputeScheduler`，最终基线压力覆盖 c=1/2/4/8/16、8192 输入和 256 输出，31/31 请求成功、KV transfer 全 rank 完整，但没有触发 preemption 或 recompute 分支。qjj_dspark 未操作，153/154 未访问。
- [R6/R7 容量诊断与终态审计](blocks/f9cb6d19a34521c254a3eb374b38acc5903d464b92a4db0bd79dfe1a04df967c.md): 基线日志、失败尝试和结果已复制到 host artifacts；随后进行了单变量 D 端 KV 容量实验：R6 将容量降至 107,777 tokens，R7 降至 64,814 tokens。R6/R7 的 c=16 请求仍分别 16/16 成功且无 preemption，R7 达到 92.6% KV、7 running/9 waiting，显示 scheduler 通过等待控制并发，但未覆盖 recompute 恢复路径。最终保留 P=R5、D=R7、proxy=R5，服务健康、16/16 rank 初始化完成、Traceback/ERROR 为 0；旧 kimi_c8 容器仅为空闲 bash，未运行 vLLM。

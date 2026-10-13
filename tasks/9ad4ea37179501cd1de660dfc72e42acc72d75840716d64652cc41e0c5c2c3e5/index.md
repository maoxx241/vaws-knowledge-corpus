---
blocks:
- block_id: 5e998400b43b1c92385d4f3a920f142db865fa91f5c74a9dea75daaf2624a3bc
  indexed: true
  sha256: 2e28bef56a024a83525094400def46405b59ce4688f53ca16fb85898f1e2ef34
  source_range:
    end: 730878
    part: 0
    start: 0
  summary: 据记录，四个 DP rank 经历了分布式 Worker 启动、权重加载、torch.compile/AOT、KV profile 与 ACLGraph capture；日志中观察到 pending NCCL work、共享内存等待、KV padding、slow tokenizer 等 warning，未见记录所述的 ERROR、Traceback、RuntimeError、OOM 或 HCCL failure。后续消息报告各 rank 的 capture 和 Engine init 耗时，并称 rank0 出现 Application startup complete、门禁与 health PASS；这些是报告中的检查结果，不能据此独立验证最终成功状态。此前中间阶段明确尚未出现 Graph capturing finished 或 fatal，且监控结束时未发送请求、未执行远端写入或启停。
  title: 153–156 R20 rank 启动监控记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9ad4ea37179501cd1de660dfc72e42acc72d75840716d64652cc41e0c5c2c3e5
  kind: experience
  material_digest: b5ec634d715870045d647c40b4c70957697fd93598147ca83fca32f7216475a8
  revision: e2f2fbf101895119e0c81fb57c0edfa8eaa6dab1722c7e6cba6a7c34764da11b
  schema: mindie-entry/3
  summary: 新材料集中记录 153–156 四个 DP rank 从加载到编译、KV profile 和 ACLGraph capture 的只读监控过程，包含 NCCL/共享内存等待等 warning 与最终报告的状态检查。最终 PASS、startup complete 及错误筛选均属于来源中的报告，保留为未独立验证的结果；中间等待阶段和未见 Graph capturing finished/fatal 的不确定性也已保留。原始材料仍可按 block_id 独立检索。
  title: 当前任务导航：R20 rank 启动与图捕获索引
navigation: 新材料集中记录 153–156 四个 DP rank 从加载到编译、KV profile 和 ACLGraph capture 的只读监控过程，包含 NCCL/共享内存等待等 warning 与最终报告的状态检查。最终 PASS、startup complete 及错误筛选均属于来源中的报告，保留为未独立验证的结果；中间等待阶段和未见 Graph capturing finished/fatal 的不确定性也已保留。原始材料仍可按 block_id 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 9ad4ea37179501cd1de660dfc72e42acc72d75840716d64652cc41e0c5c2c3e5
---

# 当前任务导航：R20 rank 启动与图捕获索引

新材料集中记录 153–156 四个 DP rank 从加载到编译、KV profile 和 ACLGraph capture 的只读监控过程，包含 NCCL/共享内存等待等 warning 与最终报告的状态检查。最终 PASS、startup complete 及错误筛选均属于来源中的报告，保留为未独立验证的结果；中间等待阶段和未见 Graph capturing finished/fatal 的不确定性也已保留。原始材料仍可按 block_id 独立检索。

## Materials

- [153–156 R20 rank 启动监控记录](blocks/5e998400b43b1c92385d4f3a920f142db865fa91f5c74a9dea75daaf2624a3bc.md): 据记录，四个 DP rank 经历了分布式 Worker 启动、权重加载、torch.compile/AOT、KV profile 与 ACLGraph capture；日志中观察到 pending NCCL work、共享内存等待、KV padding、slow tokenizer 等 warning，未见记录所述的 ERROR、Traceback、RuntimeError、OOM 或 HCCL failure。后续消息报告各 rank 的 capture 和 Engine init 耗时，并称 rank0 出现 Application startup complete、门禁与 health PASS；这些是报告中的检查结果，不能据此独立验证最终成功状态。此前中间阶段明确尚未出现 Graph capturing finished 或 fatal，且监控结束时未发送请求、未执行远端写入或启停。

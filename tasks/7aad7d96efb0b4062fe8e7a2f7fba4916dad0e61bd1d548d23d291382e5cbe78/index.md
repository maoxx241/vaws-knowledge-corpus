---
blocks:
- block_id: 9b836619abf89f79513f3a678fae3d7730ef3573e98349ad49e801b023985f70
  indexed: true
  sha256: ef006c69b93b81fb9ae724e4cd8446786bc6db9579b936c8fc1b2c37176b4682
  source_range:
    end: 870367
    part: 0
    start: 0
  summary: 只读跟踪显示四个主进程及64个worker稳定运行，经历模型构造、权重加载和torch.compile/NPU图编译；编译期间曾出现一次共享内存等待告警，但未见异常栈。随后服务返回/health=200，四个原始PID/PGID未变化，served id与r13一致，四机均命中r13 source root且CANDIDATE_SOURCE SHA一致。最终回读记录fatal=0、ERROR=0；未发送推理请求，也未执行stop/kill，但“可供主任务开始P1实验”属于后续可用性判断，并非推理成功验证。
  title: r13 四机服务启动与就绪记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7aad7d96efb0b4062fe8e7a2f7fba4916dad0e61bd1d548d23d291382e5cbe78
  kind: experience
  material_digest: 935fda6401e62b92379c61db929c34bda5ae7c984c2bb3e187b8fc00e6e256a4
  revision: 4cc66a30e73ca2d9f018ff36a5acdabddc7ce9eedd5b7f8945ef66e6141a9a0e
  schema: mindie-entry/3
  summary: 已索引本批唯一材料：r13 四机启动过程、64 worker、权重加载、torch.compile/NPU图编译、共享内存等待告警及最终/health=200状态。当前记录支持服务进程存活和健康检查通过；未包含推理请求或P1实验结果，且原始材料中的“可供主任务开始”仍是未验证的后续条件。
  title: 当前任务导航：r13 服务就绪索引
navigation: 已索引本批唯一材料：r13 四机启动过程、64 worker、权重加载、torch.compile/NPU图编译、共享内存等待告警及最终/health=200状态。当前记录支持服务进程存活和健康检查通过；未包含推理请求或P1实验结果，且原始材料中的“可供主任务开始”仍是未验证的后续条件。
schema: mindie-material-task/1
status: complete
task_id: 7aad7d96efb0b4062fe8e7a2f7fba4916dad0e61bd1d548d23d291382e5cbe78
---

# 当前任务导航：r13 服务就绪索引

已索引本批唯一材料：r13 四机启动过程、64 worker、权重加载、torch.compile/NPU图编译、共享内存等待告警及最终/health=200状态。当前记录支持服务进程存活和健康检查通过；未包含推理请求或P1实验结果，且原始材料中的“可供主任务开始”仍是未验证的后续条件。

## Materials

- [r13 四机服务启动与就绪记录](blocks/9b836619abf89f79513f3a678fae3d7730ef3573e98349ad49e801b023985f70.md): 只读跟踪显示四个主进程及64个worker稳定运行，经历模型构造、权重加载和torch.compile/NPU图编译；编译期间曾出现一次共享内存等待告警，但未见异常栈。随后服务返回/health=200，四个原始PID/PGID未变化，served id与r13一致，四机均命中r13 source root且CANDIDATE_SOURCE SHA一致。最终回读记录fatal=0、ERROR=0；未发送推理请求，也未执行stop/kill，但“可供主任务开始P1实验”属于后续可用性判断，并非推理成功验证。

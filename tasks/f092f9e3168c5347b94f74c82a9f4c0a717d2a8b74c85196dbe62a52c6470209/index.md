---
blocks:
- block_id: 89397193268cae7a4eae0d8d848f8ffe27a7ef8d3a779334073c1af846a386b9
  indexed: true
  sha256: 5ba5ad290e7b820741449be2a4f0dce80adda14f4f09c4bccd573663d4ad6c6d
  source_range:
    end: 3963956
    part: 0
    start: 0
  summary: 在 155/156 上完成 Kimi K3 P/D scheduler-on 验证：标准 KV 配置下 c=1、2、4、8、16 共 31/31 请求完成；将 D 的 KV 降至约 64,814 tokens 后，c=16、8192/1024 真实触发 7 次 recompute，16/16 请求完成且无卡死。过程中保留了只读 tokenizer cache、跨节点 plugin 不完整、端口 TIME-WAIT、外部 SIGTERM/docker stop 等失败或干扰证据；早期 recompute 压力曾出现流截断，后定位为 proxy 续跑问题，不能把所有中间结果都视为成功。最终 D 恢复 gpu-memory-utilization=0.80 并完成 smoke，服务按记录保留运行，但即时状态回查曾受 SSH 认证拒绝影响。
  title: K3 双机 RecomputeScheduler 压力验证与外部中断排查
- block_id: 0c320cd246a8d0890962709596068d8412d6e76fddb2620b24faacb95bc9150f
  indexed: true
  sha256: 6037e1005b811be12e24430afa45da27c3dba359d6b80605129fda427feb4f95
  source_range:
    end: 3963956
    part: 1
    start: 0
  summary: 真实 KV 压力显示 RecomputeScheduler 能退出重计算流程，但示例 P/D proxy 曾因 token-ID prompt 与字符串拼接、未同步收缩 min_tokens、错误按 HTTP chunk 解析 SSE，以及 retry 后 usage/metadata 未聚合而造成异常或不完整输出。修复改为使用 token IDs 重放 prompt、强制 return_token_ids=true、同步调整 min/max tokens、按 SSE line 解码，并聚合 usage 与原始 prompt metadata；补充了 recompute retry、terminal、DONE 日志。回归测试 6/6、py_compile 和 git diff --check 通过，未 commit/push，且既有 recompute_scheduler.py 脏改动未触碰；服务健康和运行状态、日志路径及可写 tokenizer cache 要求均有记录。
  title: P/D proxy recompute 续跑完整性修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f092f9e3168c5347b94f74c82a9f4c0a717d2a8b74c85196dbe62a52c6470209
  kind: experience
  material_digest: 7641d8dc18987f50c5066dfd30624b35f40b37188061d986f88fdaa6b916dfef
  revision: 31bd7371421ebfeaa653ddb44789063ae22b15ce1126766ab8f9b9d81018a4bc
  schema: mindie-entry/3
  summary: 当前证据支持：155/156 的双机 scheduler-on 在标准压力和真实 KV recompute 压力下均完成请求、无卡死；相关 proxy 续跑完整性问题已修复并通过 6/6 回归。需保留的限制包括早期外部停止与环境失败、曾出现的流截断/参数错误，以及 SSH 即时回查受认证阻塞；最终服务按记录恢复正式 D 配置并保持运行。原始材料块仍应独立检索，未在此索引中重建被省略的私有路径或密钥。
  title: K3 RecomputeScheduler 验证与 proxy 修复索引
navigation: 当前证据支持：155/156 的双机 scheduler-on 在标准压力和真实 KV recompute 压力下均完成请求、无卡死；相关 proxy 续跑完整性问题已修复并通过 6/6 回归。需保留的限制包括早期外部停止与环境失败、曾出现的流截断/参数错误，以及 SSH 即时回查受认证阻塞；最终服务按记录恢复正式 D 配置并保持运行。原始材料块仍应独立检索，未在此索引中重建被省略的私有路径或密钥。
schema: mindie-material-task/1
status: complete
task_id: f092f9e3168c5347b94f74c82a9f4c0a717d2a8b74c85196dbe62a52c6470209
---

# K3 RecomputeScheduler 验证与 proxy 修复索引

当前证据支持：155/156 的双机 scheduler-on 在标准压力和真实 KV recompute 压力下均完成请求、无卡死；相关 proxy 续跑完整性问题已修复并通过 6/6 回归。需保留的限制包括早期外部停止与环境失败、曾出现的流截断/参数错误，以及 SSH 即时回查受认证阻塞；最终服务按记录恢复正式 D 配置并保持运行。原始材料块仍应独立检索，未在此索引中重建被省略的私有路径或密钥。

## Materials

- [K3 双机 RecomputeScheduler 压力验证与外部中断排查](blocks/89397193268cae7a4eae0d8d848f8ffe27a7ef8d3a779334073c1af846a386b9.md): 在 155/156 上完成 Kimi K3 P/D scheduler-on 验证：标准 KV 配置下 c=1、2、4、8、16 共 31/31 请求完成；将 D 的 KV 降至约 64,814 tokens 后，c=16、8192/1024 真实触发 7 次 recompute，16/16 请求完成且无卡死。过程中保留了只读 tokenizer cache、跨节点 plugin 不完整、端口 TIME-WAIT、外部 SIGTERM/docker stop 等失败或干扰证据；早期 recompute 压力曾出现流截断，后定位为 proxy 续跑问题，不能把所有中间结果都视为成功。最终 D 恢复 gpu-memory-utilization=0.80 并完成 smoke，服务按记录保留运行，但即时状态回查曾受 SSH 认证拒绝影响。
- [P/D proxy recompute 续跑完整性修复](blocks/0c320cd246a8d0890962709596068d8412d6e76fddb2620b24faacb95bc9150f.md): 真实 KV 压力显示 RecomputeScheduler 能退出重计算流程，但示例 P/D proxy 曾因 token-ID prompt 与字符串拼接、未同步收缩 min_tokens、错误按 HTTP chunk 解析 SSE，以及 retry 后 usage/metadata 未聚合而造成异常或不完整输出。修复改为使用 token IDs 重放 prompt、强制 return_token_ids=true、同步调整 min/max tokens、按 SSE line 解码，并聚合 usage 与原始 prompt metadata；补充了 recompute retry、terminal、DONE 日志。回归测试 6/6、py_compile 和 git diff --check 通过，未 commit/push，且既有 recompute_scheduler.py 脏改动未触碰；服务健康和运行状态、日志路径及可写 tokenizer cache 要求均有记录。

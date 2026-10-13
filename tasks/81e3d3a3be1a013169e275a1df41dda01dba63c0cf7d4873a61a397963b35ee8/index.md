---
blocks:
- block_id: 668b836fe38b46c343d791a747916d2e3eff20dd5f699dd248b71afbaf73f3bd
  indexed: true
  sha256: b84b98318c7c1541f8c64be7aaae30c1a31aec6abc64c812c54dc4e831baa6ad
  source_range:
    end: 323371
    part: 0
    start: 0
  summary: 只读监控显示 16/16 个 rank 已完成 HCCL 初始化，未见 `KdaGateCumsum` 错误；P 实例随后因 Mooncake TP0/TP1 绑定端口 20001/20002 报 `Address already in use` 而退出，并触发 KV Cache 收发线程启动失败，`/health` 始终未就绪。失败后复查时端口已无监听者，未执行修改或 kill；该结果是失败记录而非已验证成功。
  title: 156 P 实例因 Mooncake 端口冲突启动失败
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 81e3d3a3be1a013169e275a1df41dda01dba63c0cf7d4873a61a397963b35ee8
  kind: experience
  material_digest: 45fe71e26c25aa7ac262f167170e48d80e3b91ea75f8e4495b39351df0006fd3
  revision: 79a0d912f75452a570b3f32179762250615d8a78e12663a8a892626cd4cddf29
  schema: mindie-entry/3
  summary: 已记录 156 上 P 实例的失败启动批次：HCCL 初始化完成，但 Mooncake 端口占用导致 EngineCore 退出，且 `/health` 未就绪；本轮未发现 `KdaGateCumsum` 错误，也未执行修改或进程操作。端口事后无监听者属于复查观察，不代表问题已解决。
  title: 当前任务导航：156 P 实例启动失败排查
navigation: 已记录 156 上 P 实例的失败启动批次：HCCL 初始化完成，但 Mooncake 端口占用导致 EngineCore 退出，且 `/health` 未就绪；本轮未发现 `KdaGateCumsum` 错误，也未执行修改或进程操作。端口事后无监听者属于复查观察，不代表问题已解决。
schema: mindie-material-task/1
status: complete
task_id: 81e3d3a3be1a013169e275a1df41dda01dba63c0cf7d4873a61a397963b35ee8
---

# 当前任务导航：156 P 实例启动失败排查

已记录 156 上 P 实例的失败启动批次：HCCL 初始化完成，但 Mooncake 端口占用导致 EngineCore 退出，且 `/health` 未就绪；本轮未发现 `KdaGateCumsum` 错误，也未执行修改或进程操作。端口事后无监听者属于复查观察，不代表问题已解决。

## Materials

- [156 P 实例因 Mooncake 端口冲突启动失败](blocks/668b836fe38b46c343d791a747916d2e3eff20dd5f699dd248b71afbaf73f3bd.md): 只读监控显示 16/16 个 rank 已完成 HCCL 初始化，未见 `KdaGateCumsum` 错误；P 实例随后因 Mooncake TP0/TP1 绑定端口 20001/20002 报 `Address already in use` 而退出，并触发 KV Cache 收发线程启动失败，`/health` 始终未就绪。失败后复查时端口已无监听者，未执行修改或 kill；该结果是失败记录而非已验证成功。

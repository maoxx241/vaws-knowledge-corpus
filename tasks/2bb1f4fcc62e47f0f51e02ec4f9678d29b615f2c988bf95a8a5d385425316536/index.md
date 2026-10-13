---
blocks:
- block_id: 805e377060134d99a8bd2ab8dc95bf2c8f8326469ac2aa07d232f02410fa1031
  indexed: true
  sha256: 3d9b5fbece7ba9ac4503dadc781c762105c91729f41d2f71a82f979cdf758ae2
  source_range:
    end: 3139574
    part: 0
    start: 0
  summary: 在只读审计中，将未提交工作树范围限定为 `HEAD → working tree`，并区分了 MoE `_preprocess` 的首次强同步/EP collective 暴露点与实际根因。首要嫌疑是 zeroer 的临时 pinned/device ID tensor、异步 H2D、缺乏持久 ownership/并发槽，以及 Triton kernel 缺少 block-id 边界保护；次要嫌疑是 GPU-only Mamba align 的跨流依赖和缺少边界检查。结论仍未证实，建议保持 zeroing 语义执行环形持久 buffer A/B，并单独关闭 GPU Mamba 或增加同步定位；未修改代码或操作服务。
  title: zeroer 与 GPU-only Mamba 卡死嫌疑分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2bb1f4fcc62e47f0f51e02ec4f9678d29b615f2c988bf95a8a5d385425316536
  kind: experience
  material_digest: 43a95552ff0fa62d75c474c59e5fea76a010f8d6b21deac5656e4f821685b6df
  revision: 77c8fedca3a268c28d7bf6ac1d860efecec60ea731510195e67a2d11d8b74619
  schema: mindie-entry/3
  summary: 重点追踪 `DP3 zero enqueue → MoE 首次 CPU/NPU 同步或 EP collective 暴露 → 其他 rank 等待 → sample_tokens RPC timeout`。优先验证 zeroer 的异步 ID staging、kernel 地址/边界与 tensor lifetime；随后验证 GPU-only Mamba align 的跨流 ordering、copy kernel 边界和 legacy event 路径。现有证据是静态分析与运行现象的因果排序，不是已验证成功或已证实根因；zeroer A/B、Mamba 开关及同步插桩仍未完成。
  title: 当前任务导航：Ascend 推理卡死的只读根因排查
navigation: 重点追踪 `DP3 zero enqueue → MoE 首次 CPU/NPU 同步或 EP collective 暴露 → 其他 rank 等待 → sample_tokens RPC timeout`。优先验证 zeroer 的异步 ID staging、kernel 地址/边界与 tensor lifetime；随后验证 GPU-only Mamba align 的跨流 ordering、copy kernel 边界和 legacy event 路径。现有证据是静态分析与运行现象的因果排序，不是已验证成功或已证实根因；zeroer A/B、Mamba 开关及同步插桩仍未完成。
schema: mindie-material-task/1
status: complete
task_id: 2bb1f4fcc62e47f0f51e02ec4f9678d29b615f2c988bf95a8a5d385425316536
---

# 当前任务导航：Ascend 推理卡死的只读根因排查

重点追踪 `DP3 zero enqueue → MoE 首次 CPU/NPU 同步或 EP collective 暴露 → 其他 rank 等待 → sample_tokens RPC timeout`。优先验证 zeroer 的异步 ID staging、kernel 地址/边界与 tensor lifetime；随后验证 GPU-only Mamba align 的跨流 ordering、copy kernel 边界和 legacy event 路径。现有证据是静态分析与运行现象的因果排序，不是已验证成功或已证实根因；zeroer A/B、Mamba 开关及同步插桩仍未完成。

## Materials

- [zeroer 与 GPU-only Mamba 卡死嫌疑分析](blocks/805e377060134d99a8bd2ab8dc95bf2c8f8326469ac2aa07d232f02410fa1031.md): 在只读审计中，将未提交工作树范围限定为 `HEAD → working tree`，并区分了 MoE `_preprocess` 的首次强同步/EP collective 暴露点与实际根因。首要嫌疑是 zeroer 的临时 pinned/device ID tensor、异步 H2D、缺乏持久 ownership/并发槽，以及 Triton kernel 缺少 block-id 边界保护；次要嫌疑是 GPU-only Mamba align 的跨流依赖和缺少边界检查。结论仍未证实，建议保持 zeroing 语义执行环形持久 buffer A/B，并单独关闭 GPU Mamba 或增加同步定位；未修改代码或操作服务。

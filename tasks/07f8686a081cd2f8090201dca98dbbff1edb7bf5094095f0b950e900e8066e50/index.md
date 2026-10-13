---
blocks:
- block_id: 57ade0a59421cca5350bbe5ccf96ebc793a997ce1392ff677a07f3d4ead77399
  indexed: true
  sha256: 4da5332cd08333c7ffaedc78ee5a4fff4d818425c7007f6b663c7412a54cb9f7
  source_range:
    end: 2066012
    part: 0
    start: 0
  summary: 只读复核认为 lifecycle-refresh 方案在目标四机配置下暂未发现功能 blocker：默认/dummy 加载、checkpoint layerwise reload、v1/v2 reload 和 kernel-format direct update 后会刷新 packed buffer，partial reload 保留未更新的 K/V，热路径不再执行 transpose/cat/copy；git diff --check 与 9 个改动文件 AST 解析通过。此前考察的参数直接别名方案因 conv 参数为 FP32、packed 为激活 dtype，以及 prefetch/UVA 可能替换 param.data 等风险而未采用，SGLang 合并 FP32 QKV 实现也不能直接佐证 activation-dtype alias。仍有未验证或受限边界：本地缺少 torch/torch_npu，真实 NPU copy、完整编译和 ACLGraph 行为需远端确认；prefetch offload 与运行时 reload 可能因共享 static slot 和陈旧 _cpu_storage 读错权重，且 ACLGraph capture 后 _apply 仍可能换地址。
  title: KDA pack lifecycle-refresh 只读复核结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 07f8686a081cd2f8090201dca98dbbff1edb7bf5094095f0b950e900e8066e50
  kind: experience
  material_digest: e9d61b4ddd1b95be259d4d987c74f6ae27b75652cec09deb1a4f002557b54c57
  revision: c6ec5558b007b1f91bad8b8aa26f382a5a62b22d1eb533260212ddbbfde38bf3
  schema: mindie-entry/3
  summary: 当前唯一已索引材料是 lifecycle-refresh 方案的只读复核。检索重点包括 packed buffer、普通 alias、meta reload、layerwise reload、sleep/wake、prefetch/UVA offload、direct weight update、compile/ACLGraph 地址生命周期，以及 FP32 conv 参数与 activation-dtype packed buffer 的不兼容风险；结论是目标配置暂未发现 blocker，但 NPU/ACLGraph 未在本地验证，prefetch offload + 运行时 reload 仍为未解决限制。
  title: 当前任务导航：KDA pack 优化索引
navigation: 当前唯一已索引材料是 lifecycle-refresh 方案的只读复核。检索重点包括 packed buffer、普通 alias、meta reload、layerwise reload、sleep/wake、prefetch/UVA offload、direct weight update、compile/ACLGraph 地址生命周期，以及 FP32 conv 参数与 activation-dtype packed buffer 的不兼容风险；结论是目标配置暂未发现 blocker，但 NPU/ACLGraph 未在本地验证，prefetch offload + 运行时 reload 仍为未解决限制。
schema: mindie-material-task/1
status: complete
task_id: 07f8686a081cd2f8090201dca98dbbff1edb7bf5094095f0b950e900e8066e50
---

# 当前任务导航：KDA pack 优化索引

当前唯一已索引材料是 lifecycle-refresh 方案的只读复核。检索重点包括 packed buffer、普通 alias、meta reload、layerwise reload、sleep/wake、prefetch/UVA offload、direct weight update、compile/ACLGraph 地址生命周期，以及 FP32 conv 参数与 activation-dtype packed buffer 的不兼容风险；结论是目标配置暂未发现 blocker，但 NPU/ACLGraph 未在本地验证，prefetch offload + 运行时 reload 仍为未解决限制。

## Materials

- [KDA pack lifecycle-refresh 只读复核结论](blocks/57ade0a59421cca5350bbe5ccf96ebc793a997ce1392ff677a07f3d4ead77399.md): 只读复核认为 lifecycle-refresh 方案在目标四机配置下暂未发现功能 blocker：默认/dummy 加载、checkpoint layerwise reload、v1/v2 reload 和 kernel-format direct update 后会刷新 packed buffer，partial reload 保留未更新的 K/V，热路径不再执行 transpose/cat/copy；git diff --check 与 9 个改动文件 AST 解析通过。此前考察的参数直接别名方案因 conv 参数为 FP32、packed 为激活 dtype，以及 prefetch/UVA 可能替换 param.data 等风险而未采用，SGLang 合并 FP32 QKV 实现也不能直接佐证 activation-dtype alias。仍有未验证或受限边界：本地缺少 torch/torch_npu，真实 NPU copy、完整编译和 ACLGraph 行为需远端确认；prefetch offload 与运行时 reload 可能因共享 static slot 和陈旧 _cpu_storage 读错权重，且 ACLGraph capture 后 _apply 仍可能换地址。

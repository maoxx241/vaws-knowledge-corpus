---
blocks:
- block_id: 6a0809bffa77b26cd6553bd5d85985c08b73bb5a1665d695d6d82c69c51d2f41
  indexed: true
  sha256: bb19e34ac99d729cceef7490db9202eb75bff6d46b53be6a33f86a2f32922de1
  source_range:
    end: 306227
    part: 0
    start: 0
  summary: 分析指出 `token_dispatch` 中 `dispatch_with_quant` 对 `QuantType.MXFP4` 固定返回 `True`，可能将 A5 原生 MXFP4 权重误当成需要激活通信量化，并在 `npu_moe_init_routing_v2` 路径触发 packed FP4 数据类型或 shape 识别错误。提出 All2All 暂时关闭 dispatcher 激活量化、保留 BF16/FP16 通信，并同步保护 `dynamic_scale_after_all2all` 的 `.view()` 和 `scale_type` 判断；这只是建议，未修改代码，也未验证为最终原因。AllGather 已被提及存在类似保护，但实际报错路径仍需完整堆栈确认。
  title: A5 MXFP4 token dispatcher 分支问题分析
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 45ecfa9bd4ad6bc4e3d7b315d3b8b200a7bd07674c0940e91f602a4083c4cd21
  kind: experience
  material_digest: a16d60fda9ebf6315e22c44cf70d8e6d045d8f89fe69c19b35877ee40fdeeab4
  revision: 49a9c3f91d534dd4bff9a4d9503cc234e2f7c6fc698d31f1440bdbb215361e4e
  schema: mindie-entry/3
  summary: 当前批次记录了一个关于 A5 原生 MXFP4 权重与 token dispatcher 的未验证分析：重点检索 `token_dispatch`、`dispatch_with_quant`、`with_quant`、`npu_moe_init_routing_v2`、`dynamic_scale_after_all2all` 以及 All2All/AllGather 路径。建议方向是区分权重量化与激活通信量化，并在关闭 dispatcher 量化时同步处理 `None` scale；代码尚未修改，根因和适用路径仍待完整堆栈或实测确认。
  title: 当前任务导航：A5 MXFP4 dispatcher 分析
navigation: 当前批次记录了一个关于 A5 原生 MXFP4 权重与 token dispatcher 的未验证分析：重点检索 `token_dispatch`、`dispatch_with_quant`、`with_quant`、`npu_moe_init_routing_v2`、`dynamic_scale_after_all2all` 以及 All2All/AllGather 路径。建议方向是区分权重量化与激活通信量化，并在关闭 dispatcher 量化时同步处理 `None` scale；代码尚未修改，根因和适用路径仍待完整堆栈或实测确认。
schema: mindie-material-task/1
status: complete
task_id: 45ecfa9bd4ad6bc4e3d7b315d3b8b200a7bd07674c0940e91f602a4083c4cd21
---

# 当前任务导航：A5 MXFP4 dispatcher 分析

当前批次记录了一个关于 A5 原生 MXFP4 权重与 token dispatcher 的未验证分析：重点检索 `token_dispatch`、`dispatch_with_quant`、`with_quant`、`npu_moe_init_routing_v2`、`dynamic_scale_after_all2all` 以及 All2All/AllGather 路径。建议方向是区分权重量化与激活通信量化，并在关闭 dispatcher 量化时同步处理 `None` scale；代码尚未修改，根因和适用路径仍待完整堆栈或实测确认。

## Materials

- [A5 MXFP4 token dispatcher 分支问题分析](blocks/6a0809bffa77b26cd6553bd5d85985c08b73bb5a1665d695d6d82c69c51d2f41.md): 分析指出 `token_dispatch` 中 `dispatch_with_quant` 对 `QuantType.MXFP4` 固定返回 `True`，可能将 A5 原生 MXFP4 权重误当成需要激活通信量化，并在 `npu_moe_init_routing_v2` 路径触发 packed FP4 数据类型或 shape 识别错误。提出 All2All 暂时关闭 dispatcher 激活量化、保留 BF16/FP16 通信，并同步保护 `dynamic_scale_after_all2all` 的 `.view()` 和 `scale_type` 判断；这只是建议，未修改代码，也未验证为最终原因。AllGather 已被提及存在类似保护，但实际报错路径仍需完整堆栈确认。

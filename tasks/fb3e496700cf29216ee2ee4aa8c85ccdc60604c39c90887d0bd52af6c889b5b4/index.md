---
blocks:
- block_id: 3e95be2b99772150741581ffae5d5b4ec0889dbc30686ff4154e7a877ca8bd50
  indexed: true
  sha256: a8f3591aab2a6cd8c27729fb73572d0d582248e38c73b3e7813192996c1a28c3
  source_range:
    end: 8114649
    part: 0
    start: 0
  summary: 围绕接入 AttnRes 后 MLA DSpark 8K/1K 采信率下降，实验先将问题定位到 AttnRes，并发现 `npu_rms_norm + FP32 matmul` 与预合并权重路径的舍入和归约轨迹不同；早期只恢复乘法顺序的修复未通过，随后采用仅对 MLA DSpark 生效的参考计算回退。四机请求最终恢复到 830/196、Mean AL 5.23469，但该结论后来被上游实现对齐结果修正：官方 Kimi-K3 与 vLLM 上游实际采用预合并 `norm_weight * projection_weight` 的 canonical 小算子语义，原始 AscendC 并非错误。期间保留了编译错误、BF16 权重 UB copy 导致 NaN、错误模型检测条件、目录 `FileExistsError`、SSH/脚本解析问题及未执行的工具检查等失败和不完整结果。
  title: AttnRes 数值偏差定位与 MLA DSpark 参考路径回退
- block_id: ff21c2407c62e750d7653961ce4a6511ae3b6675f979abeb525431e243d16135
  indexed: true
  sha256: 77a4764f6d71901e4ef802b731bcb47931cb07db505210f4cda6a7b1fbb9185a
  source_range:
    end: 8114649
    part: 1
    start: 0
  summary: '修复提交曾以 `a58a319f4` 推送并补齐 DCO，随后 coverage 映射提交 `0230abcad` 解决 `select-tests` 对 `vllm_ascend/ops/attn_res.py` 的唯一覆盖缺口；Ruff、pre-commit、7/7 聚焦单测和 CI 门禁通过。四机 SGLang GQA prefix-on 服务曾恢复并完成 health=200、真实短请求验证，之后按精确 PGID 停止并释放，容器和日志保留。后续上游/私仓核对修正了根因叙述：官方模型、vLLM NVIDIA/AMD/SM100 实现及 AscendC 均使用预合并权重，真正的语义漂移来自 Ascend 私仓 PR #18 引入的 legacy `npu_rms_norm + matmul` 路径；此前将该路径称为模型 reference 的说法应撤回。'
  title: PR 修复、CI 覆盖映射与服务恢复
- block_id: 88596ec0643dd5311737c197a87f776a9cab438b33fcd3d009df149aa00f6517
  indexed: true
  sha256: b460413e9f9a79320990c15b68b3b2fc97372b8b519b37972384045ffbe9c3ce
  source_range:
    end: 8114649
    part: 2
    start: 0
  summary: 'PR #13315 切换到 canonical golden 小算子：计算 inverse RMS，预合并 `norm_weight * projection_weight`，再执行 reduction、softmax 和 value mix；删除 MLA/GQA 差异化 fallback，并以提交 `cdf6a4904` 推送。四机曾因外部 `qjj_dspark` 作业占卡和 HCCL 端口 `16666` 冲突失败，停止该作业后 r3 完成 64-rank 启动、权重加载、ACLGraph 捕获和唯一 8K/1K 请求。实测为 871/1071、Mean AL 6.6928、采信率 81.3259%，高于 #13277 的 868/1099、6.5287、78.9809%；结果已更新 PR，但输出 hash 与延迟变化意味着目标轨迹和时延并未完全相同。'
  title: Canonical golden 小算子切换与四机 8K/1K 验证
- block_id: 3f363daaf48ae200b990437930cc1a5a463ff911d5e778362263a5de5129772d
  indexed: true
  sha256: c0def931eee5e55c477233e17dc9917dd0d0971b4fc5b179d0d6c477b393664a
  source_range:
    end: 8114649
    part: 3
    start: 0
  summary: '最终记录认为原始 AscendC 数学逻辑正确：先算 inverse RMS，再使用预组合的 `norm_weight * projection_weight` 计算 score；并不存在真正把 RMSNorm 与 matmul 交换位置，早期实验改变的是 gamma 乘法的浮点分组。偏差归因于历史 `npu_rms_norm + matmul` 兼容路径，而非 AscendC；golden 小算子相对 #13277 采信率提升 2.345 个百分点、Mean AL 提升 0.1641，但单请求延迟约增加 3%，不应据此宣称吞吐改善。PR #13315 当前记录为 CLEAN、检查通过、工作树干净；四机已释放，8/8 NPU 空闲，`qjj_dspark` 容器保留为 stopped/exited。'
  title: 最终结论、性能差异与四机释放状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: fb3e496700cf29216ee2ee4aa8c85ccdc60604c39c90887d0bd52af6c889b5b4
  kind: experience
  material_digest: 90ec1f181352ec1d3ae694492019e420274f3cfb005605f9f5cc3e80c1b46d75
  revision: 0f8e6f08fb83ad4b72054dc3848f8d2571cc8809c17474fe3225d009a6dc53df
  schema: mindie-entry/3
  summary: '当前主线已从“修复 AscendC 数值错误”修正为“区分 canonical golden 小算子与 legacy `npu_rms_norm + matmul` 轨迹，以及它们对 speculative decoding draft 对齐的影响”。可检索的关键证据包括：三方实现对齐、PR #18 的 legacy 路径漂移、真实 layer-12 BF16 mismatch 与宽松 golden 容差、失败的内核重排实验、MLA-only fallback 的中间结果，以及最终 `cdf6a4904` golden 小算子四机 8K/1K 结果。最终已知状态是 PR #13315 更新完成并通过检查，四机释放；采信率数据已改善但输出 hash 和时延变化仍需与“公式正确性”“逐 bit 一致性”“draft-checkpoint 兼容性”分别理解，不能把单次 case 提升为普遍性能结论。'
  title: AttnRes / Kimi-K3 MLA DSpark 检索导航
navigation: '当前主线已从“修复 AscendC 数值错误”修正为“区分 canonical golden 小算子与 legacy `npu_rms_norm + matmul` 轨迹，以及它们对 speculative decoding draft 对齐的影响”。可检索的关键证据包括：三方实现对齐、PR #18 的 legacy 路径漂移、真实 layer-12 BF16 mismatch 与宽松 golden 容差、失败的内核重排实验、MLA-only fallback 的中间结果，以及最终 `cdf6a4904` golden 小算子四机 8K/1K 结果。最终已知状态是 PR #13315 更新完成并通过检查，四机释放；采信率数据已改善但输出 hash 和时延变化仍需与“公式正确性”“逐 bit 一致性”“draft-checkpoint 兼容性”分别理解，不能把单次 case 提升为普遍性能结论。'
schema: mindie-material-task/1
status: complete
task_id: fb3e496700cf29216ee2ee4aa8c85ccdc60604c39c90887d0bd52af6c889b5b4
---

# AttnRes / Kimi-K3 MLA DSpark 检索导航

当前主线已从“修复 AscendC 数值错误”修正为“区分 canonical golden 小算子与 legacy `npu_rms_norm + matmul` 轨迹，以及它们对 speculative decoding draft 对齐的影响”。可检索的关键证据包括：三方实现对齐、PR #18 的 legacy 路径漂移、真实 layer-12 BF16 mismatch 与宽松 golden 容差、失败的内核重排实验、MLA-only fallback 的中间结果，以及最终 `cdf6a4904` golden 小算子四机 8K/1K 结果。最终已知状态是 PR #13315 更新完成并通过检查，四机释放；采信率数据已改善但输出 hash 和时延变化仍需与“公式正确性”“逐 bit 一致性”“draft-checkpoint 兼容性”分别理解，不能把单次 case 提升为普遍性能结论。

## Materials

- [AttnRes 数值偏差定位与 MLA DSpark 参考路径回退](blocks/3e95be2b99772150741581ffae5d5b4ec0889dbc30686ff4154e7a877ca8bd50.md): 围绕接入 AttnRes 后 MLA DSpark 8K/1K 采信率下降，实验先将问题定位到 AttnRes，并发现 `npu_rms_norm + FP32 matmul` 与预合并权重路径的舍入和归约轨迹不同；早期只恢复乘法顺序的修复未通过，随后采用仅对 MLA DSpark 生效的参考计算回退。四机请求最终恢复到 830/196、Mean AL 5.23469，但该结论后来被上游实现对齐结果修正：官方 Kimi-K3 与 vLLM 上游实际采用预合并 `norm_weight * projection_weight` 的 canonical 小算子语义，原始 AscendC 并非错误。期间保留了编译错误、BF16 权重 UB copy 导致 NaN、错误模型检测条件、目录 `FileExistsError`、SSH/脚本解析问题及未执行的工具检查等失败和不完整结果。
- [PR 修复、CI 覆盖映射与服务恢复](blocks/ff21c2407c62e750d7653961ce4a6511ae3b6675f979abeb525431e243d16135.md): 修复提交曾以 `a58a319f4` 推送并补齐 DCO，随后 coverage 映射提交 `0230abcad` 解决 `select-tests` 对 `vllm_ascend/ops/attn_res.py` 的唯一覆盖缺口；Ruff、pre-commit、7/7 聚焦单测和 CI 门禁通过。四机 SGLang GQA prefix-on 服务曾恢复并完成 health=200、真实短请求验证，之后按精确 PGID 停止并释放，容器和日志保留。后续上游/私仓核对修正了根因叙述：官方模型、vLLM NVIDIA/AMD/SM100 实现及 AscendC 均使用预合并权重，真正的语义漂移来自 Ascend 私仓 PR #18 引入的 legacy `npu_rms_norm + matmul` 路径；此前将该路径称为模型 reference 的说法应撤回。
- [Canonical golden 小算子切换与四机 8K/1K 验证](blocks/88596ec0643dd5311737c197a87f776a9cab438b33fcd3d009df149aa00f6517.md): PR #13315 切换到 canonical golden 小算子：计算 inverse RMS，预合并 `norm_weight * projection_weight`，再执行 reduction、softmax 和 value mix；删除 MLA/GQA 差异化 fallback，并以提交 `cdf6a4904` 推送。四机曾因外部 `qjj_dspark` 作业占卡和 HCCL 端口 `16666` 冲突失败，停止该作业后 r3 完成 64-rank 启动、权重加载、ACLGraph 捕获和唯一 8K/1K 请求。实测为 871/1071、Mean AL 6.6928、采信率 81.3259%，高于 #13277 的 868/1099、6.5287、78.9809%；结果已更新 PR，但输出 hash 与延迟变化意味着目标轨迹和时延并未完全相同。
- [最终结论、性能差异与四机释放状态](blocks/3f363daaf48ae200b990437930cc1a5a463ff911d5e778362263a5de5129772d.md): 最终记录认为原始 AscendC 数学逻辑正确：先算 inverse RMS，再使用预组合的 `norm_weight * projection_weight` 计算 score；并不存在真正把 RMSNorm 与 matmul 交换位置，早期实验改变的是 gamma 乘法的浮点分组。偏差归因于历史 `npu_rms_norm + matmul` 兼容路径，而非 AscendC；golden 小算子相对 #13277 采信率提升 2.345 个百分点、Mean AL 提升 0.1641，但单请求延迟约增加 3%，不应据此宣称吞吐改善。PR #13315 当前记录为 CLEAN、检查通过、工作树干净；四机已释放，8/8 NPU 空闲，`qjj_dspark` 容器保留为 stopped/exited。

---
blocks:
- block_id: 6a913f7876aedcded3968d06a509f3e696b646bcf0dcdcbe3382f373fe0ecfc9
  indexed: true
  sha256: 509d3bfb00832924d93a2310297475c65ca5b63b91169773d9ab89e3a4172c59
  source_range:
    end: 15167434
    part: 0
    start: 0
  summary: 围绕 DeepSeek V4.1 适配昇腾 A2/A3，宣传稿逐步从细节化技术说明调整为高层、全栈协同表达，同时保留 SparseFlashMLA、QLI V2、Triton、mHC、Vector/Cube 多流、Engram 和 DSpark 等技术点。材料记录了局部专项测试结果，包括查询量化加速、RMSNormCast 降耗和路由前处理优化，但未验证整网性能；不同阶段出现约 6.4 倍与最高约 9.5 倍等不一致表述，应视为待核对的局部结果。Engram 仅有启动和有限样本验证，完整生成质量及端到端收益仍不确定。
  title: DeepSeek V4.1 昇腾宣传稿迭代与性能表述边界
- block_id: b9a1ee31ea029faf74d32928b95add23e86458aad5a1d48b85b2c3f0e3344039
  indexed: true
  sha256: 7bc33bbac4142476e959c8886589b968fd1cc4fa577fbc8c7786f5c043fef274
  source_range:
    end: 15167434
    part: 1
    start: 0
  summary: 补充内容聚焦 Vector/Cube 多流并行与计算空泡优化、NPU Graph、DSpark 投机解码、Prefill/Decode 分离，以及 Hybrid KV Cache 和 Engram HBM 分片与 INT8 存储。材料将这些能力描述为减少等待、调度、拷贝和存储压力的优化路径，但没有提供新的端到端验证数据。性能比例被限定为算子或局部链路专项对比，实际收益取决于硬件配置和业务负载；后续推理镜像仍属于计划交付。
  title: 多流、图执行、缓存与服务链路宣传稿补充
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 956d5864211cbfd219c79ae49c547fda44cda3c738d8b10a43a58c2b37496cfb
  kind: experience
  material_digest: ca8e7c908f96c99dd7febcfe83d575a92f915ab966c3e825ccf59e0637db327b
  revision: 97be80ed55c7c1c0ac79d26cee01bd8d49ed2785a8e380d22ff6a4bebb15e2df
  schema: mindie-entry/3
  summary: 当前任务形成了一版面向昇腾 A2/A3 的高端宣传表达，覆盖 Ascend C/Triton 融合算子、SparseFlashMLA、QLI V2、多流并行、空泡优化、NPU Graph、DSpark、混合缓存、Engram 分片及多模态/智能体服务。局部测试可用于点示性能改善，但不能升级为整网提升或已验证的整体成功；约 6.4 倍与最高约 9.5 倍的查询量化表述存在待核对差异，Engram 质量、端到端收益和推理镜像交付仍未完成或未充分验证。
  title: DeepSeek V4.1 昇腾宣传稿：技术亮点与验证边界
navigation: 当前任务形成了一版面向昇腾 A2/A3 的高端宣传表达，覆盖 Ascend C/Triton 融合算子、SparseFlashMLA、QLI V2、多流并行、空泡优化、NPU Graph、DSpark、混合缓存、Engram 分片及多模态/智能体服务。局部测试可用于点示性能改善，但不能升级为整网提升或已验证的整体成功；约 6.4 倍与最高约 9.5 倍的查询量化表述存在待核对差异，Engram 质量、端到端收益和推理镜像交付仍未完成或未充分验证。
schema: mindie-material-task/1
status: complete
task_id: 956d5864211cbfd219c79ae49c547fda44cda3c738d8b10a43a58c2b37496cfb
---

# DeepSeek V4.1 昇腾宣传稿：技术亮点与验证边界

当前任务形成了一版面向昇腾 A2/A3 的高端宣传表达，覆盖 Ascend C/Triton 融合算子、SparseFlashMLA、QLI V2、多流并行、空泡优化、NPU Graph、DSpark、混合缓存、Engram 分片及多模态/智能体服务。局部测试可用于点示性能改善，但不能升级为整网提升或已验证的整体成功；约 6.4 倍与最高约 9.5 倍的查询量化表述存在待核对差异，Engram 质量、端到端收益和推理镜像交付仍未完成或未充分验证。

## Materials

- [DeepSeek V4.1 昇腾宣传稿迭代与性能表述边界](blocks/6a913f7876aedcded3968d06a509f3e696b646bcf0dcdcbe3382f373fe0ecfc9.md): 围绕 DeepSeek V4.1 适配昇腾 A2/A3，宣传稿逐步从细节化技术说明调整为高层、全栈协同表达，同时保留 SparseFlashMLA、QLI V2、Triton、mHC、Vector/Cube 多流、Engram 和 DSpark 等技术点。材料记录了局部专项测试结果，包括查询量化加速、RMSNormCast 降耗和路由前处理优化，但未验证整网性能；不同阶段出现约 6.4 倍与最高约 9.5 倍等不一致表述，应视为待核对的局部结果。Engram 仅有启动和有限样本验证，完整生成质量及端到端收益仍不确定。
- [多流、图执行、缓存与服务链路宣传稿补充](blocks/b9a1ee31ea029faf74d32928b95add23e86458aad5a1d48b85b2c3f0e3344039.md): 补充内容聚焦 Vector/Cube 多流并行与计算空泡优化、NPU Graph、DSpark 投机解码、Prefill/Decode 分离，以及 Hybrid KV Cache 和 Engram HBM 分片与 INT8 存储。材料将这些能力描述为减少等待、调度、拷贝和存储压力的优化路径，但没有提供新的端到端验证数据。性能比例被限定为算子或局部链路专项对比，实际收益取决于硬件配置和业务负载；后续推理镜像仍属于计划交付。

---
blocks:
- block_id: 9e3958e5948f5e95162474e8f89b77cab8158d78310ac1d89a64ce173d5bcbce
  indexed: true
  sha256: a7b7006bd7244977da2b4440cb39d5d356fd4ff701ec9c0f06fd08e78a572706
  source_range:
    end: 3116191
    part: 0
    start: 0
  summary: 在 153 停止已确认的 qjj_k3_main 作业后，使用集成头 ba14b852、vLLM 0.27.1、TP16/DP1 的 93 层 16-expert QuaRot W4A8 权重进行 MRV2 验证；16 个 rank 和 333 个权重分片加载完成，但真实推理未开始。初始化 KV 元数据时出现 AscendMLABackend.get_kv_cache_shape() 不接受 cache_dtype_str 的接口参数错误，且最小复现显示上游 KVBlockZeroer 会跳过 Ascend 的 (K,V) tuple，导致清零元数据缺失。42 项 CPU 单测通过，但 prefix、ACLGraph、DSpark 和真实 completion/chat 均未验证；此前将首轴非连续 KV 布局列为 gap 已被更正，现有连续布局并非 MRV2 强制要求。
  title: K3 单机 MRV2 实测阻塞与已确认缺口
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3f72624a5554b0d50bbc0c9813859a5092f02115b612d92fa441b2b2e1c9e850
  kind: experience
  material_digest: b525d79ad28aa509eed6acc538c9798e57ed52f9d32225792fdaf66e9ef8d37e
  revision: 41f848c08941aa9a551fe2afcf3e00770b8b4caaeef604da81aaad8d42d779af
  schema: mindie-entry/3
  summary: 当前唯一实机批次在 KV 初始化阶段失败，尚无真实请求成功记录；主要待修问题是 Ascend backend 的 cache_dtype_str 参数不匹配及 KVBlockZeroer 对 split-KV tuple 的清零适配。源码已有 DSpark、混合 KDA/MLA 状态路径，但 prefix hit、accept/reject、ACLGraph 回放和 DSpark 仍未完成实测；现有 K3 e2e 测试固定 MRV1，不能作为 MRV2 覆盖。
  title: MRV2 K3 单机验证：初始化失败，功能验证未完成
navigation: 当前唯一实机批次在 KV 初始化阶段失败，尚无真实请求成功记录；主要待修问题是 Ascend backend 的 cache_dtype_str 参数不匹配及 KVBlockZeroer 对 split-KV tuple 的清零适配。源码已有 DSpark、混合 KDA/MLA 状态路径，但 prefix hit、accept/reject、ACLGraph 回放和 DSpark 仍未完成实测；现有 K3 e2e 测试固定 MRV1，不能作为 MRV2 覆盖。
schema: mindie-material-task/1
status: complete
task_id: 3f72624a5554b0d50bbc0c9813859a5092f02115b612d92fa441b2b2e1c9e850
---

# MRV2 K3 单机验证：初始化失败，功能验证未完成

当前唯一实机批次在 KV 初始化阶段失败，尚无真实请求成功记录；主要待修问题是 Ascend backend 的 cache_dtype_str 参数不匹配及 KVBlockZeroer 对 split-KV tuple 的清零适配。源码已有 DSpark、混合 KDA/MLA 状态路径，但 prefix hit、accept/reject、ACLGraph 回放和 DSpark 仍未完成实测；现有 K3 e2e 测试固定 MRV1，不能作为 MRV2 覆盖。

## Materials

- [K3 单机 MRV2 实测阻塞与已确认缺口](blocks/9e3958e5948f5e95162474e8f89b77cab8158d78310ac1d89a64ce173d5bcbce.md): 在 153 停止已确认的 qjj_k3_main 作业后，使用集成头 ba14b852、vLLM 0.27.1、TP16/DP1 的 93 层 16-expert QuaRot W4A8 权重进行 MRV2 验证；16 个 rank 和 333 个权重分片加载完成，但真实推理未开始。初始化 KV 元数据时出现 AscendMLABackend.get_kv_cache_shape() 不接受 cache_dtype_str 的接口参数错误，且最小复现显示上游 KVBlockZeroer 会跳过 Ascend 的 (K,V) tuple，导致清零元数据缺失。42 项 CPU 单测通过，但 prefix、ACLGraph、DSpark 和真实 completion/chat 均未验证；此前将首轴非连续 KV 布局列为 gap 已被更正，现有连续布局并非 MRV2 强制要求。

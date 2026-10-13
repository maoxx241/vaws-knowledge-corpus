---
blocks:
- block_id: 8a553379c96483a26a01362c31157664b40f02ae52a9d32d98a4e3924f84a2c8
  indexed: true
  sha256: d591b9547215f1a0258613c04046990a202a2ae203dc2e47c3403a22576659a8
  source_range:
    end: 198743
    part: 0
    start: 0
  summary: 来源记录了一次真实 Codex 原生验收：入口报告 sharing 未配置、知识域为 vllm-ascend，并记录和激活了 read-only；这些是来源中的运行报告，未独立验证。knowledge_query("rms_norm") 据报返回 2 个命中并读取了引用 mindie://vllm-ascend/89c756f28fbec41b@56d1855fb3921637；其限定实验涉及 torch_npu 2.10.0.post2、torch 2.10.0+cpu、Ascend910_9362、形状 [5,1536]、epsilon=1e-6、FP16/BF16 和 allclose，参考值需基于目标 dtype 舍入后的输入。knowledge_explain 对 mindie://vllm-ascend/1111111111111111 据报以 read_rejected 拒绝，execution 为 not_started、未自动重试；之后的正常查询据报成功，但该证据不覆盖服务性能、其他形状或多设备结果。
  title: 只读知识查询与无效引用拒绝
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 46d79232744c7c2d81a64c0969e3d2939c58db7b5654377b435ca7b0090690be
  kind: experience
  material_digest: 35645b6c7aa22e472acbf8e7b94c68abfc1fa3fe4cd94f400c318950cc2f720b
  revision: 19921f15c757c8aa8d99f8cc6c40325ef220fc571f040a65a8318d779104bfba
  schema: mindie-entry/3
  summary: 本批包含 1 个材料块，主题为 vllm-ascend 的 read-only 激活、rms_norm 知识查询、真实引用读取及无效引用拒绝。保留的限制和不确定性包括：实验范围仅限所述版本、设备、形状与 dtype；ASCEND_RT_VISIBLE_DEVICES=8 的失败叙述未被独立验证；后续正常查询据报成功，且未进行贡献、代码修改、远端开发调用或子代理启动。
  title: 当前任务导航：只读知识验收索引
navigation: 本批包含 1 个材料块，主题为 vllm-ascend 的 read-only 激活、rms_norm 知识查询、真实引用读取及无效引用拒绝。保留的限制和不确定性包括：实验范围仅限所述版本、设备、形状与 dtype；ASCEND_RT_VISIBLE_DEVICES=8 的失败叙述未被独立验证；后续正常查询据报成功，且未进行贡献、代码修改、远端开发调用或子代理启动。
schema: mindie-material-task/1
status: complete
task_id: 46d79232744c7c2d81a64c0969e3d2939c58db7b5654377b435ca7b0090690be
---

# 当前任务导航：只读知识验收索引

本批包含 1 个材料块，主题为 vllm-ascend 的 read-only 激活、rms_norm 知识查询、真实引用读取及无效引用拒绝。保留的限制和不确定性包括：实验范围仅限所述版本、设备、形状与 dtype；ASCEND_RT_VISIBLE_DEVICES=8 的失败叙述未被独立验证；后续正常查询据报成功，且未进行贡献、代码修改、远端开发调用或子代理启动。

## Materials

- [只读知识查询与无效引用拒绝](blocks/8a553379c96483a26a01362c31157664b40f02ae52a9d32d98a4e3924f84a2c8.md): 来源记录了一次真实 Codex 原生验收：入口报告 sharing 未配置、知识域为 vllm-ascend，并记录和激活了 read-only；这些是来源中的运行报告，未独立验证。knowledge_query("rms_norm") 据报返回 2 个命中并读取了引用 mindie://vllm-ascend/89c756f28fbec41b@56d1855fb3921637；其限定实验涉及 torch_npu 2.10.0.post2、torch 2.10.0+cpu、Ascend910_9362、形状 [5,1536]、epsilon=1e-6、FP16/BF16 和 allclose，参考值需基于目标 dtype 舍入后的输入。knowledge_explain 对 mindie://vllm-ascend/1111111111111111 据报以 read_rejected 拒绝，execution 为 not_started、未自动重试；之后的正常查询据报成功，但该证据不覆盖服务性能、其他形状或多设备结果。

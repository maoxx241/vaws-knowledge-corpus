---
blocks:
- block_id: e87d1f55ff1742a8f1d4cf2514ee798fc3b4bab87be0ce60fb6e101d521d93d7
  indexed: true
  sha256: 84282b1339bdc5dcb648cb80d48f600974585107a402443731e7d19c61ffbd59
  source_range:
    end: 5085134
    part: 0
    start: 0
  summary: 据该材料所述，ModelScope 的 Eco-Tech/Kimi-K3-w4a8 与服务器上的 0726 全量旋转量化目录被报告为拥有相同的 345 个权重分片、config.json、权重索引及 optional/quarot.safetensors；这些结果属于只读比对记录，未在本索引中独立验证。材料称唯一语义差异出现在 quant_model_description.json：服务器版本增加了 optional.quarot.rotation_map.global_rotation 对 optional/quarot.safetensors 的引用，文件大小相差 78 字节；同时记录了发布辅助文件差异、[REDACTED_SECRET] 路径及全程未修改权重。
  title: ModelScope 与服务器旋转量化权重差异
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c50bf0b5f41c768f3785a1d699236c5c63ab64588e2cc18ee4109cc3a8390f97
  kind: experience
  material_digest: 53edf0df5390316af0e8b59dc9c139212cb5b9d97c035e795356ebd3a2ebb6e4
  revision: b8ea63b7e1f3a73e1cbeb25afd92da6ef1ea4374c2fe406566efc65db994200f
  schema: mindie-entry/3
  summary: 已收录 1 个新材料块，主题为 ModelScope 在线权重与服务器旋转量化权重的只读目录、哈希及量化描述差异。后续检索应关注 345 个分片、SHA-256、quant_model_description.json、QuaRot rotation_map、optional/quarot.safetensors、.ms_upload_cache，以及该结论仍属于可能有误的参考记录而非已验证案例报告。
  title: 当前任务导航：权重对比索引
navigation: 已收录 1 个新材料块，主题为 ModelScope 在线权重与服务器旋转量化权重的只读目录、哈希及量化描述差异。后续检索应关注 345 个分片、SHA-256、quant_model_description.json、QuaRot rotation_map、optional/quarot.safetensors、.ms_upload_cache，以及该结论仍属于可能有误的参考记录而非已验证案例报告。
schema: mindie-material-task/1
status: complete
task_id: c50bf0b5f41c768f3785a1d699236c5c63ab64588e2cc18ee4109cc3a8390f97
---

# 当前任务导航：权重对比索引

已收录 1 个新材料块，主题为 ModelScope 在线权重与服务器旋转量化权重的只读目录、哈希及量化描述差异。后续检索应关注 345 个分片、SHA-256、quant_model_description.json、QuaRot rotation_map、optional/quarot.safetensors、.ms_upload_cache，以及该结论仍属于可能有误的参考记录而非已验证案例报告。

## Materials

- [ModelScope 与服务器旋转量化权重差异](blocks/e87d1f55ff1742a8f1d4cf2514ee798fc3b4bab87be0ce60fb6e101d521d93d7.md): 据该材料所述，ModelScope 的 Eco-Tech/Kimi-K3-w4a8 与服务器上的 0726 全量旋转量化目录被报告为拥有相同的 345 个权重分片、config.json、权重索引及 optional/quarot.safetensors；这些结果属于只读比对记录，未在本索引中独立验证。材料称唯一语义差异出现在 quant_model_description.json：服务器版本增加了 optional.quarot.rotation_map.global_rotation 对 optional/quarot.safetensors 的引用，文件大小相差 78 字节；同时记录了发布辅助文件差异、[REDACTED_SECRET] 路径及全程未修改权重。

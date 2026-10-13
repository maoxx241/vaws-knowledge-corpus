---
blocks:
- block_id: 50bf6a27418596d0537dee39e4507edf895db27213c2498b2df096b2163f543a
  indexed: true
  sha256: 342b513e1a65615d1ef0f83d3a843ab29d344c49e65fb85f8585ab4422ab6397
  source_range:
    end: 909391
    part: 0
    start: 0
  summary: 在 156 容器中曾完成指定文件 SHA-256 同步、C++ binding 增量重编并成功链接，且记录了已有 deprecated warning；计划使用临时 Ruff 0.14.0 做只读 lint，并检查 Meta dispatch、KDA symbolic shape、shape 列表和 dtype。后续核对发现 154、156 的本地镜像及现有容器均未通过 install.info 确认为 CANN 9.1，候选镜像实际为 9.0.1，因此此前 9.0.1 的编译和 Meta 测试只能作为诊断，不能升级为 9.1 的验证成功；最终验证等待明确为 9.1 的镜像。
  title: CANN 9.1 验证受镜像版本阻塞
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d05f2e63f79df40b7532788e3843d50ad3d5a919adaf58898073f3571e809ddd
  kind: experience
  material_digest: 105202d0ec240ac261023fb57fbe11291226c38b6acb82140a07de52ba42779e
  revision: 4c40021a4078f85e2e807468dd5a948c0544efb14b47c3a5b60279930490d6d7
  schema: mindie-entry/3
  summary: 核心状态是 CANN 9.1 验证被镜像前置条件阻塞：已知的 9.0.1 编译、链接和 Meta 结果不构成最终结论。后续需取得或拉取 install.info 明确为 9.1 的镜像，再独立完成源码门禁、重编译及 Meta/KDA shape 与 dtype 验证；未触碰权重容器或占用 NPU。
  title: 当前任务导航：CANN 9.1 验证
navigation: 核心状态是 CANN 9.1 验证被镜像前置条件阻塞：已知的 9.0.1 编译、链接和 Meta 结果不构成最终结论。后续需取得或拉取 install.info 明确为 9.1 的镜像，再独立完成源码门禁、重编译及 Meta/KDA shape 与 dtype 验证；未触碰权重容器或占用 NPU。
schema: mindie-material-task/1
status: complete
task_id: d05f2e63f79df40b7532788e3843d50ad3d5a919adaf58898073f3571e809ddd
---

# 当前任务导航：CANN 9.1 验证

核心状态是 CANN 9.1 验证被镜像前置条件阻塞：已知的 9.0.1 编译、链接和 Meta 结果不构成最终结论。后续需取得或拉取 install.info 明确为 9.1 的镜像，再独立完成源码门禁、重编译及 Meta/KDA shape 与 dtype 验证；未触碰权重容器或占用 NPU。

## Materials

- [CANN 9.1 验证受镜像版本阻塞](blocks/50bf6a27418596d0537dee39e4507edf895db27213c2498b2df096b2163f543a.md): 在 156 容器中曾完成指定文件 SHA-256 同步、C++ binding 增量重编并成功链接，且记录了已有 deprecated warning；计划使用临时 Ruff 0.14.0 做只读 lint，并检查 Meta dispatch、KDA symbolic shape、shape 列表和 dtype。后续核对发现 154、156 的本地镜像及现有容器均未通过 install.info 确认为 CANN 9.1，候选镜像实际为 9.0.1，因此此前 9.0.1 的编译和 Meta 测试只能作为诊断，不能升级为 9.1 的验证成功；最终验证等待明确为 9.1 的镜像。

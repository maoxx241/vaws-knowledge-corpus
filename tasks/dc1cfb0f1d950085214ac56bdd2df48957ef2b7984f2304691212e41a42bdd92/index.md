---
blocks:
- block_id: a425d3afdb9a7da8562afc1b2e99b7c069be8a30ea0205691820cde694a61493
  indexed: true
  sha256: 14874a043047b84a3c4c03a40acbcd6206212350fb6c75c9c750c1ac12c0369f
  source_range:
    end: 807068
    part: 0
    start: 0
  summary: 该块报告通过静态追踪 Mooncake、MultiConnector 和 AscendStore 初始化链路，提出 P 的 ZMQ 端口 `kv_port + rank` 可能与 Mooncake 0.3.11.post1 ADXL 的随机端口窗口重叠；并称 TP0 曾观察到 `20044`、`20070` 被占用，随后 ZMQ 使用 `20001`。报告建议将 P 的 `kv_port` 从 `20001` 改为 `30001`，D 无需同步修改，但这些端口占用、绑定关系和规避效果应视为待核验结论；永久方案包括启动前冲突检查、文档更新及保留 `remote_port + rank` 映射。
  title: Mooncake/ADXL 与 ZMQ 端口冲突审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: dc1cfb0f1d950085214ac56bdd2df48957ef2b7984f2304691212e41a42bdd92
  kind: experience
  material_digest: e2c10b0aae1bf6c71e59d0b3fc47dd75f2f6b6dfb21b70dc712deab53f2111ec
  revision: d35c6b337fe9ebef383aa36ea8aa0f60412a7171f8fd64bf8ad5fadde924806d
  schema: mindie-entry/3
  summary: 当前唯一新材料聚焦 Mooncake/AscendStore/ADXL 与 ZMQ 的端口冲突假设及其规避建议，涉及 `kv_port + rank`、ADXL 随机端口窗口、`remote_port` 透传和 `remote_port + rank` 连接规则。材料保留了静态审计、观测数据与未核验的确定性表述；后续检索应区分已报告的检查、假设和实际验证结果。
  title: 当前任务导航
navigation: 当前唯一新材料聚焦 Mooncake/AscendStore/ADXL 与 ZMQ 的端口冲突假设及其规避建议，涉及 `kv_port + rank`、ADXL 随机端口窗口、`remote_port` 透传和 `remote_port + rank` 连接规则。材料保留了静态审计、观测数据与未核验的确定性表述；后续检索应区分已报告的检查、假设和实际验证结果。
schema: mindie-material-task/1
status: complete
task_id: dc1cfb0f1d950085214ac56bdd2df48957ef2b7984f2304691212e41a42bdd92
---

# 当前任务导航

当前唯一新材料聚焦 Mooncake/AscendStore/ADXL 与 ZMQ 的端口冲突假设及其规避建议，涉及 `kv_port + rank`、ADXL 随机端口窗口、`remote_port` 透传和 `remote_port + rank` 连接规则。材料保留了静态审计、观测数据与未核验的确定性表述；后续检索应区分已报告的检查、假设和实际验证结果。

## Materials

- [Mooncake/ADXL 与 ZMQ 端口冲突审计](blocks/a425d3afdb9a7da8562afc1b2e99b7c069be8a30ea0205691820cde694a61493.md): 该块报告通过静态追踪 Mooncake、MultiConnector 和 AscendStore 初始化链路，提出 P 的 ZMQ 端口 `kv_port + rank` 可能与 Mooncake 0.3.11.post1 ADXL 的随机端口窗口重叠；并称 TP0 曾观察到 `20044`、`20070` 被占用，随后 ZMQ 使用 `20001`。报告建议将 P 的 `kv_port` 从 `20001` 改为 `30001`，D 无需同步修改，但这些端口占用、绑定关系和规避效果应视为待核验结论；永久方案包括启动前冲突检查、文档更新及保留 `remote_port + rank` 映射。

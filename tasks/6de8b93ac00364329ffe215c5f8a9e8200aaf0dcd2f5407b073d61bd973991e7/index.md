---
blocks:
- block_id: 207bd060cb8e4fbdfa8fec780e6a452f2bde7f46a9febdb30341f04bc4134375
  indexed: true
  sha256: eb252290387742b0698841ed90a53d540170693b2c29ba74462e2473c88d0452
  source_range:
    end: 168421
    part: 0
    start: 0
  summary: '记录显示四台远端管理节点的 SSH、对应容器、数据网和基础资源只读检查均报告正常；每台列为 16/16 个逻辑 NPU，Health: OK，异常数为 0，且未发现残留 K3/vLLM 进程。报告还提到根文件系统使用率约为 37%、40%、49%、14%，基础 HBM 占用约 2.9–3.2 GiB、AICore 使用率为 0%。这些是当时的检查结果与后续可用性判断，不构成持续或独立验证；消息同时保留了“准备后续启动 K3 服务”的未执行状态。'
  title: 四台管理节点与64个逻辑NPU状态检查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6de8b93ac00364329ffe215c5f8a9e8200aaf0dcd2f5407b073d61bd973991e7
  kind: experience
  material_digest: 990128d0edb9bb38d678f61e6cc832227392ad6df5265dd16e0c44bf84fd916e
  revision: a5e0bc79644faf487ec8306e9e02cfc93754ed3617f8bbe9eadffbc03d01456f
  schema: mindie-entry/3
  summary: 本批材料记录四台管理节点及共64个逻辑NPU在一次只读检查中的报告状态：SSH、容器、数据网和NPU均被描述为正常，未发现K3/vLLM残留进程。该结论属于可检索的检查记录，不能视为持续验证；后续启动K3服务仍只是建议或未完成事项。
  title: 远端节点/NPU可用性检查
navigation: 本批材料记录四台管理节点及共64个逻辑NPU在一次只读检查中的报告状态：SSH、容器、数据网和NPU均被描述为正常，未发现K3/vLLM残留进程。该结论属于可检索的检查记录，不能视为持续验证；后续启动K3服务仍只是建议或未完成事项。
schema: mindie-material-task/1
status: complete
task_id: 6de8b93ac00364329ffe215c5f8a9e8200aaf0dcd2f5407b073d61bd973991e7
---

# 远端节点/NPU可用性检查

本批材料记录四台管理节点及共64个逻辑NPU在一次只读检查中的报告状态：SSH、容器、数据网和NPU均被描述为正常，未发现K3/vLLM残留进程。该结论属于可检索的检查记录，不能视为持续验证；后续启动K3服务仍只是建议或未完成事项。

## Materials

- [四台管理节点与64个逻辑NPU状态检查](blocks/207bd060cb8e4fbdfa8fec780e6a452f2bde7f46a9febdb30341f04bc4134375.md): 记录显示四台远端管理节点的 SSH、对应容器、数据网和基础资源只读检查均报告正常；每台列为 16/16 个逻辑 NPU，Health: OK，异常数为 0，且未发现残留 K3/vLLM 进程。报告还提到根文件系统使用率约为 37%、40%、49%、14%，基础 HBM 占用约 2.9–3.2 GiB、AICore 使用率为 0%。这些是当时的检查结果与后续可用性判断，不构成持续或独立验证；消息同时保留了“准备后续启动 K3 服务”的未执行状态。

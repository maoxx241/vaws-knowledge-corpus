---
blocks:
- block_id: 7e493bee4b23d42ac3036fb5105ade639c8f3e226cf2c0dda1cc0964be2e0de0
  indexed: true
  sha256: 463183c29629ff407630f4dba273c19e5ad743b07f799aefe943a6c64cf430be
  source_range:
    end: 1789907
    part: 0
    start: 0
  summary: 记录了 Kimi-K3-DSpark、MLA、GQA-日期版本及 Block5 权重的下载、断点续传、共享 NFS 上传、SHA-256 校验和四节点可见性检查；服务器直连 Hugging Face 曾超时或被重置，过程中也出现过 shell 引号、TLS 和安全策略导致的失败尝试。Block5 经 config、README 与 safetensors 结构判断为 MLA compressed latent KV + RoPE key，而非普通 GQA；其部署结果报告为完成，但部分远端检查命令曾失败后改用其他只读核验。另记录了 SWR 镜像在四台服务器拉取并核对 digest，以及 Block5/Block7 并非仅 block 数不同、结构和训练权重均不同的比较结论。
  title: 多批次权重与镜像部署记录
- block_id: cc7ade01fdde7bff2201410d86bbaadf15dcc3ff7a1fd3ce5747d3c44650b26b
  indexed: true
  sha256: 8d169c96f556b8950089aef210690b2a3a26feee8ef4f005757f9fa11e3b1b76
  source_range:
    end: 1789907
    part: 1
    start: 0
  summary: Block5 主权重完成本地 SHA-256 校验、共享 NFS 上传、远端校验和原子发布，报告四台均可读取；期间一个 config_sha 格式化命令因远端引号导致 cut 报错，随后改用原始 sha256sum。safetensors 张量形状进一步支持 MLA 判断：kv_a_proj_with_mqa 为 [576,7168]，包含 512 维 latent 与 64 维 RoPE 分量，kv_b_proj 为 [24576,512]。后续盘点报告远端存在 7 份 DeepSeek-V4 权重及约 132 个有效模型目录，但模型清单是简要归类，且只做文件/索引完整性检查，未进行模型加载验证。
  title: Block5 完成部署及远端模型盘点
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d72cf8291e66ac8b04d2a9c88f2e2cd760c5c5b9dd2084e2c52c99de76cd1a3d
  kind: experience
  material_digest: ff218464185b334c9057dc651101cd4a11c88e4bf2515c7522aeee56f850fe2a
  revision: e477e25ab2e4a32ea34b5dc0d6faf016026ebe63c8b9983088fb0c0c7a6568d1
  schema: mindie-entry/3
  summary: 已覆盖多批次 Kimi/DSpark 权重和 SWR 镜像的下载部署记录、校验与环境异常，以及 Block5/Block7 的结构差异和 MLA KV 判断。当前可继续检索 Block5 的部署校验细节、DeepSeek-V4 候选权重完整性，或服务器共享盘的模型概览；需注意部分检查命令曾失败，模型可用性未通过实际加载验证。
  title: 当前任务导航：权重部署与服务器模型盘点
navigation: 已覆盖多批次 Kimi/DSpark 权重和 SWR 镜像的下载部署记录、校验与环境异常，以及 Block5/Block7 的结构差异和 MLA KV 判断。当前可继续检索 Block5 的部署校验细节、DeepSeek-V4 候选权重完整性，或服务器共享盘的模型概览；需注意部分检查命令曾失败，模型可用性未通过实际加载验证。
schema: mindie-material-task/1
status: complete
task_id: d72cf8291e66ac8b04d2a9c88f2e2cd760c5c5b9dd2084e2c52c99de76cd1a3d
---

# 当前任务导航：权重部署与服务器模型盘点

已覆盖多批次 Kimi/DSpark 权重和 SWR 镜像的下载部署记录、校验与环境异常，以及 Block5/Block7 的结构差异和 MLA KV 判断。当前可继续检索 Block5 的部署校验细节、DeepSeek-V4 候选权重完整性，或服务器共享盘的模型概览；需注意部分检查命令曾失败，模型可用性未通过实际加载验证。

## Materials

- [多批次权重与镜像部署记录](blocks/7e493bee4b23d42ac3036fb5105ade639c8f3e226cf2c0dda1cc0964be2e0de0.md): 记录了 Kimi-K3-DSpark、MLA、GQA-日期版本及 Block5 权重的下载、断点续传、共享 NFS 上传、SHA-256 校验和四节点可见性检查；服务器直连 Hugging Face 曾超时或被重置，过程中也出现过 shell 引号、TLS 和安全策略导致的失败尝试。Block5 经 config、README 与 safetensors 结构判断为 MLA compressed latent KV + RoPE key，而非普通 GQA；其部署结果报告为完成，但部分远端检查命令曾失败后改用其他只读核验。另记录了 SWR 镜像在四台服务器拉取并核对 digest，以及 Block5/Block7 并非仅 block 数不同、结构和训练权重均不同的比较结论。
- [Block5 完成部署及远端模型盘点](blocks/cc7ade01fdde7bff2201410d86bbaadf15dcc3ff7a1fd3ce5747d3c44650b26b.md): Block5 主权重完成本地 SHA-256 校验、共享 NFS 上传、远端校验和原子发布，报告四台均可读取；期间一个 config_sha 格式化命令因远端引号导致 cut 报错，随后改用原始 sha256sum。safetensors 张量形状进一步支持 MLA 判断：kv_a_proj_with_mqa 为 [576,7168]，包含 512 维 latent 与 64 维 RoPE 分量，kv_b_proj 为 [24576,512]。后续盘点报告远端存在 7 份 DeepSeek-V4 权重及约 132 个有效模型目录，但模型清单是简要归类，且只做文件/索引完整性检查，未进行模型加载验证。

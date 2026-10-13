---
blocks:
- block_id: e0654e766640c679470d9e906f13739f3a3727a15de9e6a2f3730b74dfb64d4d
  indexed: true
  sha256: 958c2463e807e3f740bcd7796ad647525af3cd1065bc7137e8df82656df4dd09
  source_range:
    end: 1867574
    part: 0
    start: 0
  summary: 在最新 ci-new `0dde1ef85` + PR14 `33deb2609` 上部署四节点 TP16×DP4×EP64 多模态验证，确认 `kimi_k3` reasoning/tool parser 注册、ViT 路径和无图片数限制参数均已启用。过程中发现并修正了错误的 parser 自检 API、显存准入不足、遗留 worker/HCCL 端口冲突、启动器漏启 headless rank 以及 5 分钟 DP handshake 超时等环境或启动问题；相关进程均按精确 PID/进程组处理，PR14 源码未修改。使用专用 HCCL 端口范围后，64 rank 能够汇合并进入 W4A8 权重加载，但本批结束时仍未完成 health=200、ACLGraph 和完整真实请求矩阵；后续记录显示 rank2 的 NFS→NPU 慢速加载是主要未完成因素。
  title: PR14 多模态实机启动与环境阻塞定位
- block_id: e74ab176104601b94ad3cfccc0c3c83bd065df51c44c9d739b5abc956328ef69
  indexed: true
  sha256: 9e699a54b798798fee3e7e2e6e487e39a38f2541252439e1c7966a5a12a6ff74
  source_range:
    end: 1867574
    part: 1
    start: 0
  summary: 只读检查确认 timeout20 + 专用 HCCL `20000-20127` 启动仍在运行，四个 EngineCore 均创建并完成 64 worker 汇合；`16666` 端口冲突未复现，timeout20 profile 生效。期间出现 vLLM usage telemetry 的 `cpuinfo` `JSONDecodeError`，但它来自后台 CPU 信息上报线程，未进入 PR14 路径；rank2 的真实瓶颈是硬挂载 NFS 上的权重经 Ascend `devmm` mmap/pin/memcpy 到 NPU 时极慢，非 OOM 或静态死锁。最终 rank2 TP0 以约 783.80 秒完成权重加载，随后仍需完成 DP 汇合、KV cache、ACLGraph capture、health=200 及 reasoning/tool choice、流式/非流式、多工具和 1/2/多图片 ViT 请求验证。
  title: timeout20 启动继续推进至 rank2 权重完成
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e70222e7ad6dfc9b622e0d67d52a0d6f0e8a1c328aabf10215b20de529b243cf
  kind: experience
  material_digest: 6a21ec94cdad53682a1287eaeb78381981076a2bfa5d9dc53413f4a7a5fd3f40
  revision: 2c06c9d33d41cc94cb1898637fbd7f8bf9cccd54f3dbfffdf72f20be9efb05b1
  schema: mindie-entry/3
  summary: 当前基线为 ci-new `0dde1ef85` + PR14 `33deb2609`，验证启动器已使用多模态、ViT、无图片数量限制、专用 HCCL 端口和 timeout20 profile。环境问题已逐步定位并绕过，四节点 64 rank 已完成 worker 汇合，rank2 慢速 NFS→NPU 权重加载最终推进到 TP0 完成；但现有材料尚未证明 health=200、ACLGraph 完成或任何完整真实请求矩阵成功，后续应继续保持进程并核验这些未完成项目。
  title: PR14 多模态实机验证导航
navigation: 当前基线为 ci-new `0dde1ef85` + PR14 `33deb2609`，验证启动器已使用多模态、ViT、无图片数量限制、专用 HCCL 端口和 timeout20 profile。环境问题已逐步定位并绕过，四节点 64 rank 已完成 worker 汇合，rank2 慢速 NFS→NPU 权重加载最终推进到 TP0 完成；但现有材料尚未证明 health=200、ACLGraph 完成或任何完整真实请求矩阵成功，后续应继续保持进程并核验这些未完成项目。
schema: mindie-material-task/1
status: complete
task_id: e70222e7ad6dfc9b622e0d67d52a0d6f0e8a1c328aabf10215b20de529b243cf
---

# PR14 多模态实机验证导航

当前基线为 ci-new `0dde1ef85` + PR14 `33deb2609`，验证启动器已使用多模态、ViT、无图片数量限制、专用 HCCL 端口和 timeout20 profile。环境问题已逐步定位并绕过，四节点 64 rank 已完成 worker 汇合，rank2 慢速 NFS→NPU 权重加载最终推进到 TP0 完成；但现有材料尚未证明 health=200、ACLGraph 完成或任何完整真实请求矩阵成功，后续应继续保持进程并核验这些未完成项目。

## Materials

- [PR14 多模态实机启动与环境阻塞定位](blocks/e0654e766640c679470d9e906f13739f3a3727a15de9e6a2f3730b74dfb64d4d.md): 在最新 ci-new `0dde1ef85` + PR14 `33deb2609` 上部署四节点 TP16×DP4×EP64 多模态验证，确认 `kimi_k3` reasoning/tool parser 注册、ViT 路径和无图片数限制参数均已启用。过程中发现并修正了错误的 parser 自检 API、显存准入不足、遗留 worker/HCCL 端口冲突、启动器漏启 headless rank 以及 5 分钟 DP handshake 超时等环境或启动问题；相关进程均按精确 PID/进程组处理，PR14 源码未修改。使用专用 HCCL 端口范围后，64 rank 能够汇合并进入 W4A8 权重加载，但本批结束时仍未完成 health=200、ACLGraph 和完整真实请求矩阵；后续记录显示 rank2 的 NFS→NPU 慢速加载是主要未完成因素。
- [timeout20 启动继续推进至 rank2 权重完成](blocks/e74ab176104601b94ad3cfccc0c3c83bd065df51c44c9d739b5abc956328ef69.md): 只读检查确认 timeout20 + 专用 HCCL `20000-20127` 启动仍在运行，四个 EngineCore 均创建并完成 64 worker 汇合；`16666` 端口冲突未复现，timeout20 profile 生效。期间出现 vLLM usage telemetry 的 `cpuinfo` `JSONDecodeError`，但它来自后台 CPU 信息上报线程，未进入 PR14 路径；rank2 的真实瓶颈是硬挂载 NFS 上的权重经 Ascend `devmm` mmap/pin/memcpy 到 NPU 时极慢，非 OOM 或静态死锁。最终 rank2 TP0 以约 783.80 秒完成权重加载，随后仍需完成 DP 汇合、KV cache、ACLGraph capture、health=200 及 reasoning/tool choice、流式/非流式、多工具和 1/2/多图片 ViT 请求验证。

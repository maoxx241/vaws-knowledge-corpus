---
blocks:
- block_id: 6eb70f3d9ccf3e03c609399170582d860e8861dd0d2a049caab15adb1c4f7b12
  indexed: true
  sha256: b1b9c8f12dfd4f2c44fac15c0eb371a4b25be5046fc0b96ef6b67892bd087bf7
  source_range:
    end: 1142755
    part: 0
    start: 0
  summary: 该材料报告称，K3 可复用 Qwen3.5 的路由框架、ModelSlim 参数注册和 packed 权重存储，但其 SiTU 路径需拆为 GMM1 → SiTU → 重量化 → GMM2；共享专家融合也不能直接复用。报告记录的边界包括：W4A8 为两个 int4 存入一个 int8，转 int32 仅是 ABI view；Qwen scale 为 `[E,N]`，K3 通用 GMM 需在调用处恢复为 `[E,1,N]`；MXFP4（FP4 E2M1、group32 E8M0 scale）不能沿用整数 W4A8 loader 语义。材料还称 A3 减层权重 shape 符合该 int4/int8 布局，并指出 routed-latent、权重名称映射和 config 解析仍是相关需求；这些均保留为报告性结论，未独立验证。
  title: Qwen3.5 与 K3 MoE/W4A8 对照结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bb55fd4ae73cc730b76f2008bbe6424fbdbe8d408ddcc09c5f1bc2a8c9889f58
  kind: experience
  material_digest: e1baa4581c82d454ad19d30c6c84d3f8efb8ff2952e9ff3018c416de08f1f9f3
  revision: 3ed2c7205bb1443c16b30aec8345a0b3c7a2d7c1e5a566257eff99683199edcc
  schema: mindie-entry/3
  summary: 新增材料聚焦 K3 与 Qwen3.5 的可复用边界：路由、ModelSlim 注册及 packed 存储据称可借鉴，但 SiTU、共享专家、scale 维度和 MXFP4/W4A8 loader 语义存在差异。后续检索应关注 `[E,N]` 与 `[E,1,N]` scale、int4/int8 ABI view、routed-latent、权重映射、config 解析及尚未独立验证的 A3 shape 结论。
  title: 当前任务导航：Qwen3.5/K3 MoE 与量化布局对照
navigation: 新增材料聚焦 K3 与 Qwen3.5 的可复用边界：路由、ModelSlim 注册及 packed 存储据称可借鉴，但 SiTU、共享专家、scale 维度和 MXFP4/W4A8 loader 语义存在差异。后续检索应关注 `[E,N]` 与 `[E,1,N]` scale、int4/int8 ABI view、routed-latent、权重映射、config 解析及尚未独立验证的 A3 shape 结论。
schema: mindie-material-task/1
status: complete
task_id: bb55fd4ae73cc730b76f2008bbe6424fbdbe8d408ddcc09c5f1bc2a8c9889f58
---

# 当前任务导航：Qwen3.5/K3 MoE 与量化布局对照

新增材料聚焦 K3 与 Qwen3.5 的可复用边界：路由、ModelSlim 注册及 packed 存储据称可借鉴，但 SiTU、共享专家、scale 维度和 MXFP4/W4A8 loader 语义存在差异。后续检索应关注 `[E,N]` 与 `[E,1,N]` scale、int4/int8 ABI view、routed-latent、权重映射、config 解析及尚未独立验证的 A3 shape 结论。

## Materials

- [Qwen3.5 与 K3 MoE/W4A8 对照结论](blocks/6eb70f3d9ccf3e03c609399170582d860e8861dd0d2a049caab15adb1c4f7b12.md): 该材料报告称，K3 可复用 Qwen3.5 的路由框架、ModelSlim 参数注册和 packed 权重存储，但其 SiTU 路径需拆为 GMM1 → SiTU → 重量化 → GMM2；共享专家融合也不能直接复用。报告记录的边界包括：W4A8 为两个 int4 存入一个 int8，转 int32 仅是 ABI view；Qwen scale 为 `[E,N]`，K3 通用 GMM 需在调用处恢复为 `[E,1,N]`；MXFP4（FP4 E2M1、group32 E8M0 scale）不能沿用整数 W4A8 loader 语义。材料还称 A3 减层权重 shape 符合该 int4/int8 布局，并指出 routed-latent、权重名称映射和 config 解析仍是相关需求；这些均保留为报告性结论，未独立验证。

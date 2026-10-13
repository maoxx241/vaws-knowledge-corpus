---
blocks:
- block_id: a4010ff18b6057bced567e64450de2987557d0b7ab5a2e6458917c7a3febd9b1
  indexed: true
  sha256: d18a7eeeacef8f8ef8ab949773f2e2856d27bd1e31c52a28ca13985b8c3862f3
  source_range:
    end: 8857194
    part: 0
    start: 0
  summary: 已将 DequantSituQuant（A2/A3）和 SituMxQuant（A5）迁移至私仓并创建 PR；初始验证发现 `op_host/tiling_base.h`、`op_host/tiling_util.h` 等路径错误，随后参考 DequantSwigluQuant 修复 include、日志依赖和标量路径。A3/A5 完整构建、标量与混合参数 smoke、Kimi K3 真实规格 `[16,6144]` 用例均曾报告通过，但 PR 仍有基线冲突/标题 lint，且后续整网接入尚未完成。当前方向收敛为复用 SwiGLU 小算子的数据契约（INT32、scale/bias、activation scale、group_index），先完成单算子 golden，再做最小外围分支接入。
  title: DequantSituQuant/A5 迁移与契约演进
- block_id: f48a66a1c5c0d9c425aedb3971415d194ac7129785e4211e1b9e831c9f4a8479
  indexed: true
  sha256: 852c3f6c57a7e9c42b165acc616dd452a6231b33aa138a1b1cbc983c218c3c42
  source_range:
    end: 8857194
    part: 1
    start: 0
  summary: 首次 NPU 调用成功进入 kernel，但 dynamic 标量 scale 返回全 0；定位为 GlobalTensor size 判断错误及 scale 偏移/重编译问题，随后通过显式标志修复并以强制重编译确认 scalar dynamic 与混合 static 路径数值通过。Kimi K3 实际权重规格为 routed expert `w1/w3` packed shape `(1536,3584)`、融合输入 `[16,6144]`、scale/bias 向量 `(3072,1)`；新增真实规格测试曾报告 `1 passed`。该标量修复是通用 API 契约修复，并非当前 K3 向量主路径必需；后续讨论要求 PR 明确这一范围，并倾向不改算子接口、只做规格判断。
  title: 标量路径修复与 Kimi K3 真实用例
- block_id: 4b7e46c656596e5fb6905e9e12f04a92435debada06144e1f7edba550db62bdd
  indexed: true
  sha256: 903d4e4c1e2c079b53e66e50fcc17571240bb48f58d5df54bf4684aa12ca6e9f
  source_range:
    end: 8857194
    part: 2
    start: 0
  summary: 外围接入逐步收敛为复用现有共享/路由专家的 DequantSwigluQuant 流水，仅按激活类型选择 Situ 或 SwiGLU，不新增 loader 字段、FP32 scale 副本或 fallback；SituMxQuant kernel 的只读审查认为无需修改，但 A5 实机 build/golden 仍未闭环。共享专家 TP1/2/4/8/16 对应本地宽度 12288/6144/3072/1536/768，用户要求宽度判断放在算子真实执行/host 校验，Meta 不绑定 TP 枚举；同时准备 prefill/decode 真实 shape 的整网用例。最近发现 `Ops::NN::OpTiling` 实际来自仓库现有 `dequant_swiglu_quant/tiling_base/tiling_util.h`，先前将其归因于 CANN 缺失并绕过是错误的，需恢复 `IsRegbaseSocVersion(context_)` 并补齐该仓库头文件后重新 clean build。
  title: SwiGLU 对齐、SituMxQuant 审查与 TP/整网验证准备
- block_id: c42abb1c1829971fa5c38e6ff1e309f1444014fae61a6028ec8ede987a2763cc
  indexed: true
  sha256: cbccf70931e2967314adde01a15e3de9717caadcf272a2ededfa32a5b44c8097
  source_range:
    end: 8857194
    part: 3
    start: 0
  summary: 'DequantSituQuant 已完成 A3 clean package build，并通过 13/13 增强 numerical golden；独立 sol/max review 未发现已确认的 P0 或 correctness P1，但指出 routed 核间负载不均、queue depth=1 无流水、热循环读取 GM 标量等 P1 性能问题。SiTU 参数链随后改为 activation kind 加独立 beta/linear_beta 字段，删除 SituActivationConfig，shared W4A8 融合路径不再依赖 act_fn；相关回归与 golden 曾报告通过。PR #16 已更新并推送多个提交，但后续仍需谨慎区分已验证范围与未完成的整网验证。'
  title: DequantSituQuant 单算子验证与接入
- block_id: 0882f3fbfce7311a764f338edaf840646bde10a4d0bec7986e48eee1f42343d5
  indexed: true
  sha256: bea86b006cfc5584e2a10a2241faba9265051a32724e5e61a14b41269cbc6da2
  source_range:
    end: 8857194
    part: 4
    start: 0
  summary: '删除 situ_activation 别名后，routed SiTU、原 SwiGLU、完整 moe_mlp 与 DequantSituQuant numerical suite 曾报告通过；进一步确认上游 vLLM 0.23 的 MoEActivation 不含 SITU，因此当前实现保留构造期用 silu 通过校验、Ascend runtime 保存 situ/4.0/25.0 的兼容 shim。PR #16 已补充算子接口、dtype/layout、典型 shape、公式、边界、kernel/tiling/binding/MoE 接入及测试说明，并修复了 mypy 问题，最新已报告 head 为 e5bf94227；这些报告不等同于后续四机全特性验证成功。'
  title: SiTU 参数透传与 PR 更新
- block_id: 5849561f6e06983cf1c51758331076527dce4416f6c10749649b582cb6d11a40
  indexed: true
  sha256: 7f7a07f2f0a330cdcdbe931b472780f5bb25b59397c206e7a06410b6f2865a46
  source_range:
    end: 8857194
    part: 5
    start: 0
  summary: 'PR #16 的四机全特性合并验证在 A3 multicard W4A8 服务启动期失败：SiTU routed/shared 的 raw-GMM 路径向 torch_npu.npu_grouped_matmul 显式请求 output_dtype=torch.int32，而 aclnnGroupedMatmulWeightNz 对 A8W4 仅支持 INT8-INT4-BF16/FP16，返回错误 161002。现有单测 mock 掉真实 GMM，未覆盖该硬件约束；责任范围包括 moe_mlp.py 的两个 uses_situ 分支及 fused_moe.py/fused_moe_0_23_0.py 的 shared 同类路径。要求在 PR16 隔离分支建立正确的 A3 WeightNz W4A8 与 DequantSituQuant 契约，补未 mock 的 routed/shared multicard NPU smoke，重复 profile_run，并提供 before/after 原始证据；当前材料只记录了修复任务与计划，未证明修复完成。'
  title: A3 multicard WeightNz 启动失败待修复
- block_id: 817314de1cd42b9fed4566e273a28d42e9940fa282efd0a469a905e478fbf3d5
  indexed: true
  sha256: 34022fb84e7649b8fcb9ff4b529746041e8cc5fef74c66e1ef3881f6d63aab8f
  source_range:
    end: 19192585
    part: 0
    start: 8857194
  summary: 先后确认 K3 routed 专家是 W4A8 WeightNz，GMM 必须消费 scale/per-token-scale/bias 并输出 BF16，再由 DequantSituQuant 做 SiTU 与动态 INT8 量化；shared expert 实际是 W8A8_DYNAMIC，合法保留 INT32 累加路径。A3 单算子、真实权重 routed/shared、两轮 TP8 profile、KV 分配、图捕获及 HTTP prefill/decode 均曾报告通过，但第二轮也保留了独立 binding 未加载的环境失败证据；随后隔离服务 PID 39139 被精确 SIGTERM 停止并释放 NPU。PR16 曾更新多个提交，最终记录的 CI 中仍有无关 lint pending/基线格式问题，不能把所有验证表述为无条件成功。
  title: A3 WeightNz SiTU 契约与隔离实机验证
- block_id: 6158edf329a36e3bde80511ce98929ce897c3ced1e3460774b52459466ce6565
  indexed: true
  sha256: d069fbc72634013412ee8ffcd38d7879838b65708e27965a491c36369f083a80
  source_range:
    end: 19192585
    part: 1
    start: 8857194
  summary: 四机曾因 editable build 保留旧 csrc/build，导致新 BF16 adapter 与旧 ACLNN SupportInfo、proto 和 kernel binary 混装并报 EZ1001；随后记录了清理 build/output/build_out 后完整 47-op 构建成功、INT32/BF16 重新生成及真实 smoke 的结果。框架方案又收缩为仅修改 moe_mlp.py、fused_moe.py、fused_moe_0_23_0.py，使用 `_w4a8_situ_apply_mlp`，保留 SituActivationConfig，普通量化路径不变；PR21 的 W4A8_MXFP 被判断无需新增 kernel/框架分支，但只建议补 packed 权重测试和 A5 smoke，且 PR21 当时没有有效硬件证据。之后 PR 描述被多次改写为逐参数 Kimi K3 schema，并开始按 GitCode situ 仓库直接同步 kernel、host/tiling，相关最终编译/提交状态在材料末尾仍未完整给出。
  title: 构建缓存修复、最小框架改动与 A5 规格评估
- block_id: fd7a1194b10ba75c9be73c2c3847865d4bf4676a98e89a411ef0f9e8c4f0be17
  indexed: true
  sha256: 44fc5a81030412bbc6ceb4ba6cac605fbd92766c4395dce113f4b7eaeda7b6f4
  source_range:
    end: 19192585
    part: 2
    start: 8857194
  summary: 材料确认 A5 W4A8_MXFP 的链路为 FP4 packed 权重、E8M0 weight scale、E4M3 activation，经 GMM1 输出 BF16，再由 SituMxQuant 产生 E4M3 与 E8M0 scale；W4A16_MXFP4 则不使用 SituMxQuant。随后决定 kernel 按 GitCode situ 最新版本逐文件同步，不修改 kernel，并移除重复的 K3 规格白名单，复用已有 DequantSwiGLUQuant tiling 基础设施；但用户要求的离线编译尚未形成可核验结果，且明确不要调用 NPU。最后的分析指出 routed WeightNz GMM 应输出 BF16/FP16、不能输出 INT32，而 shared INT32 carrier 可能与上游 INT32 reinterpret 契约不一致；关于 DequantSwiGLUQuant 的完整历史处理方式在材料结尾被截断，仍需核对。
  title: 上游 situ kernel 同步与 A3 输入契约争议
- block_id: 76b80c9b8ca40db803fbed8358fb305cb4e5e8f165d52b4935592298170affd8
  indexed: true
  sha256: 8eafa93cd25d76e23072a7a681f12e619f813b012f94440fa5eb6e518c268f89
  source_range:
    end: 19192585
    part: 3
    start: 8857194
  summary: 材料指出 A3 routed WeightNz W4A8 的链路应为 BF16 GMM 输出后执行 SiTU 与 per-token dynamic quant，旧 INT32 DequantSwiGLUQuant 分支不适用；现有 kernel 的 BF16 分支被报告为可复用，但相关结论主要来自静态契约和离线编译，未运行 NPU。PR16 曾完成冲突解决、rebase 和单 commit 压缩，保留 KDA、DequantSituQuant 与 SituMxQuant；随后开始将上游 SituGlu 迁移到 A5 W4A16_MXFP4，并发现并修正私有 `error_util.h`、`OPS_CHECK_NULL_WITH_CONTEXT` 等 ops-nn 依赖差异。材料末尾报告 SituGlu 单算子包、Torch/Meta C++ 检查和定向 UT 已通过，但当时最终 clean build 与 PR 提交状态尚在收口，后续结果见另一块。
  title: A3 SiTU 契约、PR16 整合与 A5 SituGlu 迁移过程
- block_id: 3688e496e6f65fbedc0c19a53bfdd17d39eff743d7ad6113f9a8bd220d0a376a
  indexed: true
  sha256: 87cae678f52745bc91a35da7238e8eba646beaaab678d52d581f54221242a663
  source_range:
    end: 19192585
    part: 4
    start: 8857194
  summary: 'A5 `SituGlu` 已提交至目标分支 `releases/v0.23.0_ci_new` 的 PR #32，单 commit 为 `f9d38c26d7836730e30b02e28e077b85f43c0ebe`，当前标记为 MERGEABLE、CI 排队中。实现仅让 A5 W4A16_MXFP4 使用 `GMM1(BF16) → SituGlu → GMM2`，A5 W4A8_MXFP 仍用 SituMxQuant，A3 仍用 DequantSituQuant；kernel 源文件据报告与上游字节一致，并通过了构建依赖适配。156 隔离开发容器完成 clean 单算子编译打包，Torch binding/Meta 编译、定向 UT（2 passed）和源码契约测试（5 passed）均有报告，但验证未调用 NPU且未操作四机服务。'
  title: 'A5 SituGlu PR #32 与离线验证结果'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f819200170f7564f86ff167525d08aa97ae78739973fa9597e5006d70d676871
  kind: experience
  material_digest: 45565c72540592189aef77cee252c59bb58c72f8a30bede44a1f6a914014790e
  revision: 46886ffe526845355c39a20d77cadc3240b2fc75af5f29eb3e2ad2fb6a4417df
  schema: mindie-entry/3
  summary: '已补充并保留两条主线：A3 routed WeightNz W4A8 应走 BF16 GMM → SiTU/dynamic quant，shared INT32 分支需区分数值型 accumulator 与旧 reinterpret 语义；相关证据目前是静态检查和离线编译，未运行 NPU。A5 SituGlu 已独立迁移并提交 PR #32 到 `releases/v0.23.0_ci_new`，仅服务 W4A16_MXFP4；ops-nn 私有头和宏已按现有仓库接口适配，离线编译、打包、C++ 检查及定向测试已有报告，但 CI 仍排队，尚不能据此宣称合并或真实设备验证成功。PR16 的冲突处理和单 commit 压缩已记录为先前上下文；原始材料仍应独立检索，未在此重述被省略的细节。'
  title: 当前任务导航：A3 SiTU 与 A5 SituGlu 已完成离线接入，等待 CI/后续运行证据
navigation: '已补充并保留两条主线：A3 routed WeightNz W4A8 应走 BF16 GMM → SiTU/dynamic quant，shared INT32 分支需区分数值型 accumulator 与旧 reinterpret 语义；相关证据目前是静态检查和离线编译，未运行 NPU。A5 SituGlu 已独立迁移并提交 PR #32 到 `releases/v0.23.0_ci_new`，仅服务 W4A16_MXFP4；ops-nn 私有头和宏已按现有仓库接口适配，离线编译、打包、C++ 检查及定向测试已有报告，但 CI 仍排队，尚不能据此宣称合并或真实设备验证成功。PR16 的冲突处理和单 commit 压缩已记录为先前上下文；原始材料仍应独立检索，未在此重述被省略的细节。'
schema: mindie-material-task/1
status: complete
task_id: f819200170f7564f86ff167525d08aa97ae78739973fa9597e5006d70d676871
---

# 当前任务导航：A3 SiTU 与 A5 SituGlu 已完成离线接入，等待 CI/后续运行证据

已补充并保留两条主线：A3 routed WeightNz W4A8 应走 BF16 GMM → SiTU/dynamic quant，shared INT32 分支需区分数值型 accumulator 与旧 reinterpret 语义；相关证据目前是静态检查和离线编译，未运行 NPU。A5 SituGlu 已独立迁移并提交 PR #32 到 `releases/v0.23.0_ci_new`，仅服务 W4A16_MXFP4；ops-nn 私有头和宏已按现有仓库接口适配，离线编译、打包、C++ 检查及定向测试已有报告，但 CI 仍排队，尚不能据此宣称合并或真实设备验证成功。PR16 的冲突处理和单 commit 压缩已记录为先前上下文；原始材料仍应独立检索，未在此重述被省略的细节。

## Materials

- [DequantSituQuant/A5 迁移与契约演进](blocks/a4010ff18b6057bced567e64450de2987557d0b7ab5a2e6458917c7a3febd9b1.md): 已将 DequantSituQuant（A2/A3）和 SituMxQuant（A5）迁移至私仓并创建 PR；初始验证发现 `op_host/tiling_base.h`、`op_host/tiling_util.h` 等路径错误，随后参考 DequantSwigluQuant 修复 include、日志依赖和标量路径。A3/A5 完整构建、标量与混合参数 smoke、Kimi K3 真实规格 `[16,6144]` 用例均曾报告通过，但 PR 仍有基线冲突/标题 lint，且后续整网接入尚未完成。当前方向收敛为复用 SwiGLU 小算子的数据契约（INT32、scale/bias、activation scale、group_index），先完成单算子 golden，再做最小外围分支接入。
- [标量路径修复与 Kimi K3 真实用例](blocks/f48a66a1c5c0d9c425aedb3971415d194ac7129785e4211e1b9e831c9f4a8479.md): 首次 NPU 调用成功进入 kernel，但 dynamic 标量 scale 返回全 0；定位为 GlobalTensor size 判断错误及 scale 偏移/重编译问题，随后通过显式标志修复并以强制重编译确认 scalar dynamic 与混合 static 路径数值通过。Kimi K3 实际权重规格为 routed expert `w1/w3` packed shape `(1536,3584)`、融合输入 `[16,6144]`、scale/bias 向量 `(3072,1)`；新增真实规格测试曾报告 `1 passed`。该标量修复是通用 API 契约修复，并非当前 K3 向量主路径必需；后续讨论要求 PR 明确这一范围，并倾向不改算子接口、只做规格判断。
- [SwiGLU 对齐、SituMxQuant 审查与 TP/整网验证准备](blocks/4b7e46c656596e5fb6905e9e12f04a92435debada06144e1f7edba550db62bdd.md): 外围接入逐步收敛为复用现有共享/路由专家的 DequantSwigluQuant 流水，仅按激活类型选择 Situ 或 SwiGLU，不新增 loader 字段、FP32 scale 副本或 fallback；SituMxQuant kernel 的只读审查认为无需修改，但 A5 实机 build/golden 仍未闭环。共享专家 TP1/2/4/8/16 对应本地宽度 12288/6144/3072/1536/768，用户要求宽度判断放在算子真实执行/host 校验，Meta 不绑定 TP 枚举；同时准备 prefill/decode 真实 shape 的整网用例。最近发现 `Ops::NN::OpTiling` 实际来自仓库现有 `dequant_swiglu_quant/tiling_base/tiling_util.h`，先前将其归因于 CANN 缺失并绕过是错误的，需恢复 `IsRegbaseSocVersion(context_)` 并补齐该仓库头文件后重新 clean build。
- [DequantSituQuant 单算子验证与接入](blocks/c42abb1c1829971fa5c38e6ff1e309f1444014fae61a6028ec8ede987a2763cc.md): DequantSituQuant 已完成 A3 clean package build，并通过 13/13 增强 numerical golden；独立 sol/max review 未发现已确认的 P0 或 correctness P1，但指出 routed 核间负载不均、queue depth=1 无流水、热循环读取 GM 标量等 P1 性能问题。SiTU 参数链随后改为 activation kind 加独立 beta/linear_beta 字段，删除 SituActivationConfig，shared W4A8 融合路径不再依赖 act_fn；相关回归与 golden 曾报告通过。PR #16 已更新并推送多个提交，但后续仍需谨慎区分已验证范围与未完成的整网验证。
- [SiTU 参数透传与 PR 更新](blocks/0882f3fbfce7311a764f338edaf840646bde10a4d0bec7986e48eee1f42343d5.md): 删除 situ_activation 别名后，routed SiTU、原 SwiGLU、完整 moe_mlp 与 DequantSituQuant numerical suite 曾报告通过；进一步确认上游 vLLM 0.23 的 MoEActivation 不含 SITU，因此当前实现保留构造期用 silu 通过校验、Ascend runtime 保存 situ/4.0/25.0 的兼容 shim。PR #16 已补充算子接口、dtype/layout、典型 shape、公式、边界、kernel/tiling/binding/MoE 接入及测试说明，并修复了 mypy 问题，最新已报告 head 为 e5bf94227；这些报告不等同于后续四机全特性验证成功。
- [A3 multicard WeightNz 启动失败待修复](blocks/5849561f6e06983cf1c51758331076527dce4416f6c10749649b582cb6d11a40.md): PR #16 的四机全特性合并验证在 A3 multicard W4A8 服务启动期失败：SiTU routed/shared 的 raw-GMM 路径向 torch_npu.npu_grouped_matmul 显式请求 output_dtype=torch.int32，而 aclnnGroupedMatmulWeightNz 对 A8W4 仅支持 INT8-INT4-BF16/FP16，返回错误 161002。现有单测 mock 掉真实 GMM，未覆盖该硬件约束；责任范围包括 moe_mlp.py 的两个 uses_situ 分支及 fused_moe.py/fused_moe_0_23_0.py 的 shared 同类路径。要求在 PR16 隔离分支建立正确的 A3 WeightNz W4A8 与 DequantSituQuant 契约，补未 mock 的 routed/shared multicard NPU smoke，重复 profile_run，并提供 before/after 原始证据；当前材料只记录了修复任务与计划，未证明修复完成。
- [A3 WeightNz SiTU 契约与隔离实机验证](blocks/817314de1cd42b9fed4566e273a28d42e9940fa282efd0a469a905e478fbf3d5.md): 先后确认 K3 routed 专家是 W4A8 WeightNz，GMM 必须消费 scale/per-token-scale/bias 并输出 BF16，再由 DequantSituQuant 做 SiTU 与动态 INT8 量化；shared expert 实际是 W8A8_DYNAMIC，合法保留 INT32 累加路径。A3 单算子、真实权重 routed/shared、两轮 TP8 profile、KV 分配、图捕获及 HTTP prefill/decode 均曾报告通过，但第二轮也保留了独立 binding 未加载的环境失败证据；随后隔离服务 PID 39139 被精确 SIGTERM 停止并释放 NPU。PR16 曾更新多个提交，最终记录的 CI 中仍有无关 lint pending/基线格式问题，不能把所有验证表述为无条件成功。
- [构建缓存修复、最小框架改动与 A5 规格评估](blocks/6158edf329a36e3bde80511ce98929ce897c3ced1e3460774b52459466ce6565.md): 四机曾因 editable build 保留旧 csrc/build，导致新 BF16 adapter 与旧 ACLNN SupportInfo、proto 和 kernel binary 混装并报 EZ1001；随后记录了清理 build/output/build_out 后完整 47-op 构建成功、INT32/BF16 重新生成及真实 smoke 的结果。框架方案又收缩为仅修改 moe_mlp.py、fused_moe.py、fused_moe_0_23_0.py，使用 `_w4a8_situ_apply_mlp`，保留 SituActivationConfig，普通量化路径不变；PR21 的 W4A8_MXFP 被判断无需新增 kernel/框架分支，但只建议补 packed 权重测试和 A5 smoke，且 PR21 当时没有有效硬件证据。之后 PR 描述被多次改写为逐参数 Kimi K3 schema，并开始按 GitCode situ 仓库直接同步 kernel、host/tiling，相关最终编译/提交状态在材料末尾仍未完整给出。
- [上游 situ kernel 同步与 A3 输入契约争议](blocks/fd7a1194b10ba75c9be73c2c3847865d4bf4676a98e89a411ef0f9e8c4f0be17.md): 材料确认 A5 W4A8_MXFP 的链路为 FP4 packed 权重、E8M0 weight scale、E4M3 activation，经 GMM1 输出 BF16，再由 SituMxQuant 产生 E4M3 与 E8M0 scale；W4A16_MXFP4 则不使用 SituMxQuant。随后决定 kernel 按 GitCode situ 最新版本逐文件同步，不修改 kernel，并移除重复的 K3 规格白名单，复用已有 DequantSwiGLUQuant tiling 基础设施；但用户要求的离线编译尚未形成可核验结果，且明确不要调用 NPU。最后的分析指出 routed WeightNz GMM 应输出 BF16/FP16、不能输出 INT32，而 shared INT32 carrier 可能与上游 INT32 reinterpret 契约不一致；关于 DequantSwiGLUQuant 的完整历史处理方式在材料结尾被截断，仍需核对。
- [A3 SiTU 契约、PR16 整合与 A5 SituGlu 迁移过程](blocks/76b80c9b8ca40db803fbed8358fb305cb4e5e8f165d52b4935592298170affd8.md): 材料指出 A3 routed WeightNz W4A8 的链路应为 BF16 GMM 输出后执行 SiTU 与 per-token dynamic quant，旧 INT32 DequantSwiGLUQuant 分支不适用；现有 kernel 的 BF16 分支被报告为可复用，但相关结论主要来自静态契约和离线编译，未运行 NPU。PR16 曾完成冲突解决、rebase 和单 commit 压缩，保留 KDA、DequantSituQuant 与 SituMxQuant；随后开始将上游 SituGlu 迁移到 A5 W4A16_MXFP4，并发现并修正私有 `error_util.h`、`OPS_CHECK_NULL_WITH_CONTEXT` 等 ops-nn 依赖差异。材料末尾报告 SituGlu 单算子包、Torch/Meta C++ 检查和定向 UT 已通过，但当时最终 clean build 与 PR 提交状态尚在收口，后续结果见另一块。
- [A5 SituGlu PR #32 与离线验证结果](blocks/3688e496e6f65fbedc0c19a53bfdd17d39eff743d7ad6113f9a8bd220d0a376a.md): A5 `SituGlu` 已提交至目标分支 `releases/v0.23.0_ci_new` 的 PR #32，单 commit 为 `f9d38c26d7836730e30b02e28e077b85f43c0ebe`，当前标记为 MERGEABLE、CI 排队中。实现仅让 A5 W4A16_MXFP4 使用 `GMM1(BF16) → SituGlu → GMM2`，A5 W4A8_MXFP 仍用 SituMxQuant，A3 仍用 DequantSituQuant；kernel 源文件据报告与上游字节一致，并通过了构建依赖适配。156 隔离开发容器完成 clean 单算子编译打包，Torch binding/Meta 编译、定向 UT（2 passed）和源码契约测试（5 passed）均有报告，但验证未调用 NPU且未操作四机服务。

---
blocks:
- block_id: 9055f5072c864c0948be3b069e26da8dc174f6afa170d40b333746842ed2fb3a
  indexed: true
  sha256: 7828a7c8cdd300f4958109899880af9b192a7701faf1825df876f44071089a3b
  source_range:
    end: 16777948
    part: 0
    start: 0
  summary: 最初的 CANN 9.1 GQA/MLA 非连续通过结论因错误 shape 和可能的隐式 contiguous 被撤回；改用 Kimi K3 的真实布局后，stock CANN 9.1 明确报 `key tensor must be contiguous` 和 `keyRope does not support non-contiguous`。后续确认应显式调用 `torch.ops._C_ascend.npu_fused_infer_attention_score_v2`，检查 `ASCEND_CUSTOM_OPP_PATH` 是否指向 vllm-ascend，并用独立 FP32 分页 KV reference 验证精度；此前仅测试底层 torch_npu/CANN 入口，不能证明走了 vllm-ascend plugin。静态追踪还发现 MLA wrapper 曾在 kernel 前执行 `MakeTensorContiguous`，已计划扩展 TensorV2 PA 场景以保留 key/value/keyRope stride；真实 plugin 精度结果尚未完成。
  title: FIA 首轴非连续验证结论与插件入口修正
- block_id: 0e179e5962ef5f2c9a00b35e2fe36c9279b61ff84efe1212368fbe7de9d3c5bc
  indexed: true
  sha256: 8528957769dafd3978cecf2ec5fa79c9497e951c3c9cee98b5dc82b5f44f752c
  source_range:
    end: 16777948
    part: 1
    start: 0
  summary: 测试已从此前的 4D GQA 修正为生产路径的 3D `[num_blocks, block_size, num_kv_heads * head_dim]` view，MLA 使用 K3 的 latent 512/rope 64，并要求显式 `_C_ascend` 入口、stock 负例和独立精度基准。153 容器中的 custom OPP 构建经历了 `mc2_log.h`、tiling 头路径、自动生成 inner schema、AppleDouble `._*` 文件、FIA/IFA/PFA 依赖、`NOT_DYNAMIC_COMPILE`、arch35 布局枚举和 `kernel_meta.lock` 等问题；部分已通过兼容头、构建参数和临时清理修正。构建随后进入大量 OPC/FATBIN tiling key 编译，但全量组合耗时过长，尚未产生可用于 `_C_ascend` 精度结论的最终包。
  title: 真实 GQA/MLA 形状与 9.1 custom OPP 构建排障
- block_id: cd93f0e159193037c6c3c48839ecc9d332642c2b5f059cc45f0bdf5512c07928
  indexed: true
  sha256: 534058628c00739dffe56c59bdd433cab385869cca033cd7b381d138b9503ffa
  source_range:
    end: 16777948
    part: 2
    start: 0
  summary: 已将构建范围收窄为 K3 GQA/MLA 所需的 FIA BF16 非量化组合，排除 float16、int8 和 int4 等无关 tiling key。当前定向 custom OPP 构建仍在生成 tiling key，已有输出仅为 warning、未见新的 error；后续需安装临时包并通过 `_C_ascend` 入口运行真实用例。该阶段尚未证明 plugin 调用链或精度结果成功。
  title: FIA BF16 定向构建仍在进行
- block_id: 93f1fad8b9cec6623c3cd81602abd31190525be0849c69d58e21957f9b91430c
  indexed: true
  sha256: 5091c5b042e8bb42ffc91ebf1d56c31d280344fdabd9a0da787f5a9815e4aaf6
  source_range:
    end: 33555259
    part: 0
    start: 16777948
  summary: 已修正多处 `_C_ascend`/ACLNN ABI、workspace 生命周期、空参数 dangling reference 和 wrapper 传参问题，并确认连续 baseline 可运行，但首轴非连续 GQA 仍有约 0.5332 的误差，MLA 的 `keyRope` 仍被 CANN 9.1 拒绝；这些都不是精度通过结论。进一步定位显示实际 GQA/MLA 使用的是 `fia_tiling_nonquant_gqa.cpp` 的 `FiaBaseParams`，此前修改未将 stride 写入真实 tiling data，且曾多次误加载 stock object 或旧 host library；当前正进行增量构建和 custom host/device 链路验证。
  title: FIA 迁移内核与 stride 写入排查
- block_id: a5f46fec3922a65eaff0c153ac67de40501adbedaaf335e0fb163381a530ca92
  indexed: true
  sha256: 46bb040bdb4d28dfab09dce439a66612e8ed000f5df95aae2fa92d6072de67a2
  source_range:
    end: 33555259
    part: 1
    start: 16777948
  summary: 新的 op-host 库已构建并放入 custom OPP，但尚未观察到自编译 stride 日志；当前仅能确认 custom device `.o` 被选中，不能证明 custom host tiling 回调执行。已补齐 `op_master_device/lib/libcust_opmaster.so` 并加入诊断标记，结果仍为约 48.7% mismatch，随后转向排查 CANN 9.1 的 host 注册入口、OPP 优先级及 ACL tiling cache 复用；该 mismatch 不能归因于阈值或视为精度失败/成功结论。MLA 测试口径也在收紧为仅让真实 latent KV cache 非连续，并使用独立 FP32 reference。
  title: custom OPP host tiling 加载与缓存隔离
- block_id: 034184e8632ca84b75fce1f686b6e87b3e881c29065e59d8c3cefe1826c9c25e
  indexed: true
  sha256: b20e7c87e5c14c7aabff0c25d98782b3560edc3ffd67426bd1fabbb24e90b1f2
  source_range:
    end: 50334807
    part: 0
    start: 33555259
  summary: 确认 CANN 9.1 内置 FIA 与 vllm-ascend custom host/device 注册存在同名 tiling 冲突；此前 custom host-first 通过、MLA 缺 kernel key、手工 opc 产物及旧缓存均不能作为有效精度证据。围绕 `105000000000222220` MLA 与 `5000000000010200200` GQA，完成隔离构建、双 key device package 和显式 `enable_custom_op()`/`torch.ops._C_ascend` 测试准备；期间保留了缺 key、构建缓存、AppleDouble、protobuf 打包等失败或不完整结果。最终该块记录的正式回归结果见后续块：需以同一 custom package、真实局部 shape 和独立 CPU FP32 reference 为准。
  title: FIA 9.1 custom OPP 构建与 MLA/GQA 路由排查
- block_id: 78468172b90a66839d1990a0769decd6115a13ba38c0b5eb4ad2dae22aa58877
  indexed: true
  sha256: 7fbc483031a454b3675f51a557d96dd7967b72295a1be22bfcd5f2860e88aee2
  source_range:
    end: 50334807
    part: 1
    start: 33555259
  summary: 修正 arch22 PA 初始化中的 key/value/key-rope 独立 stride，并确认 MLA host 已收到非连续 stride，但 `FiaKernelNonQuantMla` 未将 6 个字段拷贝到 device `ConstInfo`；因此此前 MLA 非连续输出最大绝对差约 `0.7709`，实际仍按连续布局读取。干净 device 重建后新旧 `.o` hash 不同，DSpark GQA 与 Kimi K3 MLA 均通过非连续/连续对照及独立 CPU FP32 reference：分别为 `1.016676e-03` 与 `1.956940e-03`，pytest `2 passed`。随后清除诊断残留并提交 `b61dee5a5`，但该块后续仍记录了 PR 标题、DCO、迁移文件和 CI 修正过程，不能把早期“通过”与最终 PR 状态混淆。
  title: 修复 MLA device stride 传递并完成双用例回归
- block_id: 2bb4acd83d59d58f4407267a9c856a5abf2262da76137dec66d656ba86276feb
  indexed: true
  sha256: f863e9f25a2960e02230bc09cf33803b11da3e42be21b3a3da416ccb59251933
  source_range:
    end: 50334807
    part: 2
    start: 33555259
  summary: 'pre-commit 最初因迁移的历史测试、golden 文件、文档和 vendored attention 源树触发全量 ruff/codespell；随后仅保留 FIA 编译链、vllm-ascend `_C_ascend` plugin 及 K3/DSpark 真实用例，并对四个 vendored attention 目录增加 codespell 跳过边界。PR head 依次更新至 `6bef6c98`、`567a1201`，pre-commit、mypy、test-select、hitest recommendation、main、DCO 等检查通过；selected-tests 因仓库规则被 skip，自动 failure-analysis job 曾处于收尾状态。最终口径明确撤回 CANN 原生 `torch_npu` 结果，只认可显式 custom plugin、stride 传至 device kernel、独立 FP32 reference 和真实形状下的 `2 passed`；PR #13821 仍为 OPEN/Draft。'
  title: PR 精简、lint 修复与 CI 状态
- block_id: d54c3e3284337341fefed84c74aec5bd4f1756a59f234532d6195be2825779e5
  indexed: true
  sha256: d1bbad381b55e773971fdb774fa6638a6bcb53bfaf31f3d1d511d9abbc6acce7
  source_range:
    end: 50477885
    part: 0
    start: 50334807
  summary: '助手记录称已将相对 `releases/v0.26.0rc` 的 5 个提交（含最早测试提交）合成为 `63900e810d1c7bb671f078bf1d423dcc17e76815`，提交信息为 `feat(ops): migrate FIA with strided PA support`。记录还称工作区干净、代码树一致，并依据旧远端 head `567a1201` 使用 `--force-with-lease` 推送到 PR #13821；CI 在推送后重新触发且当时为 pending。上述内容是报告性记录，未在本索引中独立验证。'
  title: 'PR #13821 提交压缩与 force-with-lease 推送记录'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2c3d9764ad65c0bd06f7d739cf23d63bfcf6b20a637255379ca89dfe2d8888d6
  kind: experience
  material_digest: 720f9be8e7f174f2f1f9d9cc95e2d5a2a54247fd53aac5ba973658182d244e14
  revision: e44c1e884e142408201976e7948fae201110105adfe9db86fc9cfb3b5678639a
  schema: mindie-entry/3
  summary: '此前上下文涉及 arch22 MLA kernel 的 6 个 stride 字段修复、custom package 下 DSpark GQA/Kimi K3 MLA 的局部形状 CPU FP32 对照结果，以及若干被判定无效或未完成的 CANN、wrapper、缓存和 custom host 尝试。新增记录报告 PR #13821（目标 `releases/v0.26.0rc`）已将 5 个提交压为 `63900e810` 并以 `--force-with-lease` 推送，CI 随后重新触发且曾处于 pending；这与先前导航中记录的 OPEN/Draft、主要检查通过及 selected-tests 被 skip 的状态可能是时间上的后续变化，合并和最终 CI 结果仍需分别确认。'
  title: FIA/CANN 验证、PR 提交历史与状态
navigation: '此前上下文涉及 arch22 MLA kernel 的 6 个 stride 字段修复、custom package 下 DSpark GQA/Kimi K3 MLA 的局部形状 CPU FP32 对照结果，以及若干被判定无效或未完成的 CANN、wrapper、缓存和 custom host 尝试。新增记录报告 PR #13821（目标 `releases/v0.26.0rc`）已将 5 个提交压为 `63900e810` 并以 `--force-with-lease` 推送，CI 随后重新触发且曾处于 pending；这与先前导航中记录的 OPEN/Draft、主要检查通过及 selected-tests 被 skip 的状态可能是时间上的后续变化，合并和最终 CI 结果仍需分别确认。'
schema: mindie-material-task/1
status: complete
task_id: 2c3d9764ad65c0bd06f7d739cf23d63bfcf6b20a637255379ca89dfe2d8888d6
---

# FIA/CANN 验证、PR 提交历史与状态

此前上下文涉及 arch22 MLA kernel 的 6 个 stride 字段修复、custom package 下 DSpark GQA/Kimi K3 MLA 的局部形状 CPU FP32 对照结果，以及若干被判定无效或未完成的 CANN、wrapper、缓存和 custom host 尝试。新增记录报告 PR #13821（目标 `releases/v0.26.0rc`）已将 5 个提交压为 `63900e810` 并以 `--force-with-lease` 推送，CI 随后重新触发且曾处于 pending；这与先前导航中记录的 OPEN/Draft、主要检查通过及 selected-tests 被 skip 的状态可能是时间上的后续变化，合并和最终 CI 结果仍需分别确认。

## Materials

- [FIA 首轴非连续验证结论与插件入口修正](blocks/9055f5072c864c0948be3b069e26da8dc174f6afa170d40b333746842ed2fb3a.md): 最初的 CANN 9.1 GQA/MLA 非连续通过结论因错误 shape 和可能的隐式 contiguous 被撤回；改用 Kimi K3 的真实布局后，stock CANN 9.1 明确报 `key tensor must be contiguous` 和 `keyRope does not support non-contiguous`。后续确认应显式调用 `torch.ops._C_ascend.npu_fused_infer_attention_score_v2`，检查 `ASCEND_CUSTOM_OPP_PATH` 是否指向 vllm-ascend，并用独立 FP32 分页 KV reference 验证精度；此前仅测试底层 torch_npu/CANN 入口，不能证明走了 vllm-ascend plugin。静态追踪还发现 MLA wrapper 曾在 kernel 前执行 `MakeTensorContiguous`，已计划扩展 TensorV2 PA 场景以保留 key/value/keyRope stride；真实 plugin 精度结果尚未完成。
- [真实 GQA/MLA 形状与 9.1 custom OPP 构建排障](blocks/0e179e5962ef5f2c9a00b35e2fe36c9279b61ff84efe1212368fbe7de9d3c5bc.md): 测试已从此前的 4D GQA 修正为生产路径的 3D `[num_blocks, block_size, num_kv_heads * head_dim]` view，MLA 使用 K3 的 latent 512/rope 64，并要求显式 `_C_ascend` 入口、stock 负例和独立精度基准。153 容器中的 custom OPP 构建经历了 `mc2_log.h`、tiling 头路径、自动生成 inner schema、AppleDouble `._*` 文件、FIA/IFA/PFA 依赖、`NOT_DYNAMIC_COMPILE`、arch35 布局枚举和 `kernel_meta.lock` 等问题；部分已通过兼容头、构建参数和临时清理修正。构建随后进入大量 OPC/FATBIN tiling key 编译，但全量组合耗时过长，尚未产生可用于 `_C_ascend` 精度结论的最终包。
- [FIA BF16 定向构建仍在进行](blocks/cd93f0e159193037c6c3c48839ecc9d332642c2b5f059cc45f0bdf5512c07928.md): 已将构建范围收窄为 K3 GQA/MLA 所需的 FIA BF16 非量化组合，排除 float16、int8 和 int4 等无关 tiling key。当前定向 custom OPP 构建仍在生成 tiling key，已有输出仅为 warning、未见新的 error；后续需安装临时包并通过 `_C_ascend` 入口运行真实用例。该阶段尚未证明 plugin 调用链或精度结果成功。
- [FIA 迁移内核与 stride 写入排查](blocks/93f1fad8b9cec6623c3cd81602abd31190525be0849c69d58e21957f9b91430c.md): 已修正多处 `_C_ascend`/ACLNN ABI、workspace 生命周期、空参数 dangling reference 和 wrapper 传参问题，并确认连续 baseline 可运行，但首轴非连续 GQA 仍有约 0.5332 的误差，MLA 的 `keyRope` 仍被 CANN 9.1 拒绝；这些都不是精度通过结论。进一步定位显示实际 GQA/MLA 使用的是 `fia_tiling_nonquant_gqa.cpp` 的 `FiaBaseParams`，此前修改未将 stride 写入真实 tiling data，且曾多次误加载 stock object 或旧 host library；当前正进行增量构建和 custom host/device 链路验证。
- [custom OPP host tiling 加载与缓存隔离](blocks/a5f46fec3922a65eaff0c153ac67de40501adbedaaf335e0fb163381a530ca92.md): 新的 op-host 库已构建并放入 custom OPP，但尚未观察到自编译 stride 日志；当前仅能确认 custom device `.o` 被选中，不能证明 custom host tiling 回调执行。已补齐 `op_master_device/lib/libcust_opmaster.so` 并加入诊断标记，结果仍为约 48.7% mismatch，随后转向排查 CANN 9.1 的 host 注册入口、OPP 优先级及 ACL tiling cache 复用；该 mismatch 不能归因于阈值或视为精度失败/成功结论。MLA 测试口径也在收紧为仅让真实 latent KV cache 非连续，并使用独立 FP32 reference。
- [FIA 9.1 custom OPP 构建与 MLA/GQA 路由排查](blocks/034184e8632ca84b75fce1f686b6e87b3e881c29065e59d8c3cefe1826c9c25e.md): 确认 CANN 9.1 内置 FIA 与 vllm-ascend custom host/device 注册存在同名 tiling 冲突；此前 custom host-first 通过、MLA 缺 kernel key、手工 opc 产物及旧缓存均不能作为有效精度证据。围绕 `105000000000222220` MLA 与 `5000000000010200200` GQA，完成隔离构建、双 key device package 和显式 `enable_custom_op()`/`torch.ops._C_ascend` 测试准备；期间保留了缺 key、构建缓存、AppleDouble、protobuf 打包等失败或不完整结果。最终该块记录的正式回归结果见后续块：需以同一 custom package、真实局部 shape 和独立 CPU FP32 reference 为准。
- [修复 MLA device stride 传递并完成双用例回归](blocks/78468172b90a66839d1990a0769decd6115a13ba38c0b5eb4ad2dae22aa58877.md): 修正 arch22 PA 初始化中的 key/value/key-rope 独立 stride，并确认 MLA host 已收到非连续 stride，但 `FiaKernelNonQuantMla` 未将 6 个字段拷贝到 device `ConstInfo`；因此此前 MLA 非连续输出最大绝对差约 `0.7709`，实际仍按连续布局读取。干净 device 重建后新旧 `.o` hash 不同，DSpark GQA 与 Kimi K3 MLA 均通过非连续/连续对照及独立 CPU FP32 reference：分别为 `1.016676e-03` 与 `1.956940e-03`，pytest `2 passed`。随后清除诊断残留并提交 `b61dee5a5`，但该块后续仍记录了 PR 标题、DCO、迁移文件和 CI 修正过程，不能把早期“通过”与最终 PR 状态混淆。
- [PR 精简、lint 修复与 CI 状态](blocks/2bb4acd83d59d58f4407267a9c856a5abf2262da76137dec66d656ba86276feb.md): pre-commit 最初因迁移的历史测试、golden 文件、文档和 vendored attention 源树触发全量 ruff/codespell；随后仅保留 FIA 编译链、vllm-ascend `_C_ascend` plugin 及 K3/DSpark 真实用例，并对四个 vendored attention 目录增加 codespell 跳过边界。PR head 依次更新至 `6bef6c98`、`567a1201`，pre-commit、mypy、test-select、hitest recommendation、main、DCO 等检查通过；selected-tests 因仓库规则被 skip，自动 failure-analysis job 曾处于收尾状态。最终口径明确撤回 CANN 原生 `torch_npu` 结果，只认可显式 custom plugin、stride 传至 device kernel、独立 FP32 reference 和真实形状下的 `2 passed`；PR #13821 仍为 OPEN/Draft。
- [PR #13821 提交压缩与 force-with-lease 推送记录](blocks/d54c3e3284337341fefed84c74aec5bd4f1756a59f234532d6195be2825779e5.md): 助手记录称已将相对 `releases/v0.26.0rc` 的 5 个提交（含最早测试提交）合成为 `63900e810d1c7bb671f078bf1d423dcc17e76815`，提交信息为 `feat(ops): migrate FIA with strided PA support`。记录还称工作区干净、代码树一致，并依据旧远端 head `567a1201` 使用 `--force-with-lease` 推送到 PR #13821；CI 在推送后重新触发且当时为 pending。上述内容是报告性记录，未在本索引中独立验证。

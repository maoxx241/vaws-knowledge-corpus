---
blocks:
- block_id: 33e9910761652d621a186b2869b854b9889e75fee26e975aff505eeb8c8a80e2
  indexed: true
  sha256: d5f0751f1b041de75c1d6f3172c57eb0adc0f4fb3db2ce7e0c221fa8d75edb5e
  source_range:
    end: 16777512
    part: 0
    start: 0
  summary: '任务围绕将 ops-transformer 的 compressor、quant lightning indexer 等算子同步到 main 和 releases/v0.26.0rc，并叠加 PR #10201；后续又将 DSV4 的 scatter_nd_update/v2 路径迁移到 cann-recipes-infer 的 scatter_nd_update_asc。过程中出现 QLI metadata CMake 新旧接口不兼容、内部 header 被错误加 vllm_ 前缀、pip 依赖解析失败等问题，分别通过最小适配、修正命名和 --no-deps 构建处理；PR #15001/#15002 已创建并多次更新。ASC 算子编译及 3/3 数值回归曾通过，但模型验证尚未完成，随后因 cache view 适配边界被质疑而暂停。'
  title: 算子同步、PR 更新与 DSV4 验证进展
- block_id: e2bf644b8ba52e47c82f28ddcfcf05c06f2f9684a85e737bd8283042b08b15d5
  indexed: true
  sha256: d92fb3dcb117a0386eb2e6282cd8f422e90208b39a93d450bc42c970b2f6e7b8
  source_range:
    end: 16777512
    part: 1
    start: 0
  summary: '审查确认将四维 cache 直接 view 为二维输入并不适用于所有旧 v2 路径，尤其可能把 [block, offset] slot mapping 错误变成 2N×1；因此修正为仅 DSV4 使用 ASC，非 V4 路径继续使用 CANN 的 npu_scatter_nd_update_。验证组合改为 0.26 原生 PR #14994 + #15001，组合 SHA d89d88974 的 metadata-cache 单测和四维 cache NPU 回归已报告通过，完整构建与包路径也已锁定。按 A3 文档启动 DP4×TP4、16 NPU 的 DeepSeek-V4-Flash 后，首次启动在权重加载前因 14 号卡 HDC/TSD 超时失败；尚未获得 GPQA 精度结果，当前待精确终止进程组并进行单卡隔离检查。'
  title: 修正 ASC 作用域并准备 GPQA 组合验证
- block_id: 61240845af261571c772273735dddb9cb02e6f324ce64a8f21f271b21b2ac788
  indexed: true
  sha256: 38d0fefd7c3d1389a4c5d841f1eda488dfb478b7e7255d2d4afb5c93743cbdfa
  source_range:
    end: 31925760
    part: 0
    start: 16777512
  summary: '155/156 被另一容器的 vaws-offload GLM-5.2 作业长期占用后，改选空闲单机 153；组合 #14994+#15001（初始 SHA d89d8897）完成迁移并通过环境、导入和定向测试。153 的启动先暴露 Triton 0.26 环境不匹配，随后真实权重和 ACLGraph 验证发现 Compressor 的 BF16 normWeight ABI、padded state-cache stride 及 ASC scatter 的 cache.view 假设等问题，分别更新 #15001、#15002，并固定 GLOO_SOCKET_IFNAME=lo。后续 FP32 与 stride 修复生效，但仍需结合新的 padded scatter 和 metadata 修复完成最终验证。'
  title: 153 单机迁移与 A3 兼容性修复
- block_id: bf68299b387da609ea8b2cb4b2ee46033d01800fd3bc96c53dd6eddbf995510b
  indexed: true
  sha256: dc6b7843829566dfa130bdb94738b0f2caa9be4de29c59e89e256e227b842b49
  source_range:
    end: 31925760
    part: 1
    start: 16777512
  summary: 增量重编译生成新的 A3 Compressor kernel 后，r6/r7 先遇到重复 detached 启动导致的 HCCL 16666 端口冲突，改用独立端口并单实例启动；组合提交 d73d970 的 16/16 rank、17/17 decode graph capture、API ready 和真实 HTTP 200 请求均报告成功。GPQA 首轮因 runner 使用错误的 3600 秒 timeout，产生超时和不可计入精度的结果；恢复为 7200 秒后，先合并 78 条有效结果，后续多段后台续跑累计达到 151/198，服务相关 errors 仍为 0。答案抽取存在大量 pred=None/missing，且续跑客户端曾异常退出，因此完整 198 题 summary、最终精度和 PR 验证更新仍未完成。
  title: A3 服务通过 graph capture 并启动 GPQA
- block_id: 35efd2a16a9913e3ffe2cf65821784f4ee4a55f12e40bb9ff66cdd8109850c0b
  indexed: true
  sha256: a69dfeaa2082849c205cb7463d72ee7e3d46c2485cdbcc3d0b9e3f41fbfcb04d
  source_range:
    end: 31925760
    part: 2
    start: 16777512
  summary: GPQA 从 64/198 推进到 78/198 时确认 `--timeout 3600` 会触发三次重试和 10803 秒后返回的伪 missing，遂停止该轮，仅保留 78 条 `error=null` 结果，并按默认 7200 秒重跑其余题目。续跑曾在 46/120 后客户端退出，随后改为合并 prior 文件并在 153 独立后台进程中继续；截至该块末尾 r11 新增 23/70，累计有效覆盖 151/198，errors=0，服务健康 200。已观察到的正确率和 missing 仅是中间样本，不能视为最终 GPQA 结论；仍有 47 题及最终合并、PR 验证段待完成。
  title: GPQA 续跑纠正 timeout 并保持未完成
- block_id: ff0523d07baff121313289bf83be95f4d57554a5c09c25c66190a72a2cc2501c
  indexed: true
  sha256: e1458154afeac086c6ef6e65a040c90719bc7932a37667cdfb5d32ac6b0100e2
  source_range:
    end: 38612878
    part: 0
    start: 31925760
  summary: '组合验证最终完成 198/198，98 correct、官方 accuracy 49.4949%、errors=0，但严格 `Answer: [A-D]` parser 产生 89 个 missing，其中 60 个达到 65,536 token 上限；该结果不能宣称精度通过。全服务日志未发现 traceback、Compressor shape、ASC scatter 或 HTTP 5xx，#14994/#15001 的组合证据已写回 PR。随后决定从纯 `releases/v0.26.0rc@3d74c7ba5` 开始，按纯基线、#14994 metadata、Compressor、QLI+QLI metadata、ASC scatter 的顺序隔离验证。'
  title: 组合验证完成并转向隔离增量基线
- block_id: 011a77c10302da28a855f8c5854365d49af752ef30afd19264c619ba7c53f466
  indexed: true
  sha256: 0c03b88ff3bc340b8a67dfda4094d1ebfe979ff6dd434e2af996ebe531c21d88
  source_range:
    end: 38612878
    part: 1
    start: 31925760
  summary: '纯基线构建经历了 Catlass 子模块缺失、AppleDouble `._*.cpp` 被编译器误识别等失败尝试，清理后成功完成；import 和算子清单确认未包含 #14994、新 QLI/metadata 或 ASC scatter。基线服务在 A3 DP4×TP4/EP/MTP=1、FULL_DECODE_ONLY 配置下通过 HTTP 200 真实请求，但完整 198 题尚未完成；在 107 条可完整解析且 ID 唯一的记录处冻结为 93/107、86.9159%、errors=0、missing=7，另有一条停止时被截断的 JSONL 行被记录而未静默忽略。已明确 QLI 与 QLI metadata 必须作为不可拆分的 tiling 配套原子组。'
  title: 纯基线构建、启动与 107 题冻结
- block_id: fb19470972fa8df84d17f6163dccf0958f1128eafb15bb781636df041d94d9ab
  indexed: true
  sha256: d168c7709765fad7ff870495c382244d25a948830b0f018da2cef9b3c461ac3a
  source_range:
    end: 38612878
    part: 2
    start: 31925760
  summary: '#14994 隔离环境核验通过：Python 源码哈希一致，自定义算子二进制与纯基线逐文件一致，新增 metadata cache 单测为 `1 passed`，因此当前唯一变量是 metadata Python 生命周期逻辑。metadata-only 服务已在相同 A3 配置下启动并通过 HTTP 200 返回 `42`，随后按冻结的 107 个 ID 开始 GPQA；截至该块结束为 66/107、服务和请求错误均为 0，但仍处于长尾运行中，没有最终 accuracy、missing 或完整结果结论。温度为 1，单题结果可能有采样波动，比较应以固定子集的汇总指标为准。'
  title: 仅 metadata 阶段开始并进行固定子集复测
- block_id: bb589c9d59c85cfcea2bf74595cd53543175b041384725aa59826a921dfcaf13
  indexed: true
  sha256: cbe7d6209f0e31964ef1809d04d119e925c2c051d8c624bdbc3fc9e065b14dc0
  source_range:
    end: 49379553
    part: 0
    start: 38612878
  summary: metadata-only 随机采样完成为 90/107（84.1121%，missing=9），低于纯基线 93/107（86.9159%，missing=7）；随后固定 seed 对照仍为 metadata 90/107、missing=12，而纯基线最终结果尚未在本块结束前完成。此前 eager 单请求 dump 在 24 组比较中未发现输入、缓存改写或现场重算差异，但该证据不覆盖 FULL_DECODE_ONLY 的 graph replay 生命周期。
  title: metadata-only 与固定 seed 对照结果
- block_id: 86763910f24b96f3ef2631727945086afd8fd85685759b186585b8dd8bee8ef6
  indexed: true
  sha256: 14aad78a394d4cc139b390e075fca05de0bf5889a4c0fb42786c5acc9d2938b2
  source_range:
    end: 49379553
    part: 1
    start: 38612878
  summary: '固定 seed 对照完成后，纯基线为 99/107（92.5234%，missing=5），metadata 为 90/107（84.1121%，missing=12）；9 道题单向从基线正确变为 metadata 错答或 length/missing，精确 McNemar 检验 p=0.00390625，因此 #14994 未通过精度门槛。参考 K3 的 device 快照、图外 materialized-output 边界和延迟 D2H 方案后，单条真实 graph 请求的 4 个 TP rank、3 个 cache group 比较无差异，但这只覆盖一次小批次首次复用，不能证明逐层、多 step 或并发正确。'
  title: 固定 seed 回退与 graph 路径单请求诊断
- block_id: 7351e0b9ccfb918aef8a294dd470e943bdb35ef8f785bb4c8e10676f6fcaa7dc
  indexed: true
  sha256: 8cd6575697705c0dcceca853d69864d834bcc2aa6d9e33658b44c6967e1d9c4c
  source_range:
    end: 49379553
    part: 2
    start: 38612878
  summary: C4 indexer、C4 attention compressor、C128 attention compressor 使用不同 cache group 和 metadata key，prefill/decode 也分开；普通 forward 创建新 ForwardContext，同一 context 的 MTP 子步会显式清缓存。现有 dump 只证明 4 个 TP rank 上各组的首次存入、读取和重新计算采样值一致，未覆盖后续层、跨 step 的刷新结果、多个 draft 子步、真实并发及动态 batch 进出。
  title: C4 metadata 分组与未覆盖验证范围
- block_id: ff40a1c7d7f61fd423c4960e0fca95ce5eb8511af6d1536d1cb87bcc69360b84
  indexed: true
  sha256: 6f65806ad83a4f08b52f3d6c6371d2415255bdf93097783ab5c90e2dd27233c1
  source_range:
    end: 57932929
    part: 0
    start: 49379553
  summary: 对9道回退题进行 temperature=0 的 PR 前后 A/B，9/9均分叉，最早为题13第3个生成 token，但早期差异受并发分批未固定及 PR 探针影响，不能据此定责。启用 nightly 的 `HCCL_DETERMINISTIC=true`、`LCCL_DETERMINISTIC=1` 后，题13 prefill 与单请求 decode 中，C4 indexer、C4 compressor、C128 的缓存值、现算值、hidden states、cos/sin 和 slot mapping 均一致；decode 检查覆盖384 tokens、209个 replay、跨256/384/512边界，未发现 metadata 复用或刷新错误。随后启动9题并发长生成，8题进入同一DP，累计检查约260万次复用比较和274万次 cos/sin 消费，仍无差异；MTP因压缩比为0不经过该复用路径。
  title: 题13与多阶段 metadata 复用排查
- block_id: 3e5149f3ee750ee83c38fdb774f73f661e255a23e134c5d848d45237d3679985
  indexed: true
  sha256: 69fb0fae6b8a484e3c9caddf37217c48bf4a804147b577cc43edce32f9878b7a
  source_range:
    end: 57932929
    part: 1
    start: 49379553
  summary: 用户要求停止后，本轮测试请求和 dump 已关闭，服务保留；同一DP的8并发长生成最长约1.9万 token，未达到设置的65,536上限。累计约330万次 metadata 复用比较、346万次实际 cos/sin 检查均无差异，也未发现请求失败或跨请求 slot 串用。结论仅限已覆盖的并发和长度：未发现0.26 metadata PR 的问题，但不构成完整65K精度验收，也没有解释此前99/107降至90/107的精度差异。
  title: 并发长生成提前停止的覆盖结论
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a28f7360596fb619d74bb8bedb7102f530e81ac0d172fefa685d34dcec77d542
  kind: experience
  material_digest: 48fc03ef3d5a9a584f8132f0493432dbb11b324a1cba3d418633e2d454308760
  revision: 40e6351e421c493b44bb4ce6d2549fe1ba42e23dbf65d38cf938c1716b0a3fe2
  schema: mindie-entry/3
  summary: 此前固定 seed 的 metadata-only 与纯基线对照仍显示90/107对99/107，9道差异均由基线正确侧单向回退；该差异尚未解释，不能按采样噪声或本轮检查结果判定PR整体正确。新增证据覆盖题13的确定性 prefill、单请求decode，以及同一DP多请求并发长生成：C4/C128 metadata缓存与现算值、compressor实际cos/sin、slot mapping和跨边界刷新均未发现不一致，累计数百万次检查为零差异；但并发最长约1.9万而非65K，MTP路径未覆盖该复用逻辑，且多请求长生成在用户要求下提前停止。因此当前可表述为“已覆盖的metadata复用链路未复现错误”，仍需保留graph replay生命周期、输入buffer更新、刷新边界、请求行对应关系以及长decode精度回退等未解释问题，不能升级为0.26 PR无任何问题或完整精度通过。
  title: 当前任务导航：metadata 复用链路局部通过，精度回退仍未闭环
navigation: 此前固定 seed 的 metadata-only 与纯基线对照仍显示90/107对99/107，9道差异均由基线正确侧单向回退；该差异尚未解释，不能按采样噪声或本轮检查结果判定PR整体正确。新增证据覆盖题13的确定性 prefill、单请求decode，以及同一DP多请求并发长生成：C4/C128 metadata缓存与现算值、compressor实际cos/sin、slot mapping和跨边界刷新均未发现不一致，累计数百万次检查为零差异；但并发最长约1.9万而非65K，MTP路径未覆盖该复用逻辑，且多请求长生成在用户要求下提前停止。因此当前可表述为“已覆盖的metadata复用链路未复现错误”，仍需保留graph replay生命周期、输入buffer更新、刷新边界、请求行对应关系以及长decode精度回退等未解释问题，不能升级为0.26 PR无任何问题或完整精度通过。
schema: mindie-material-task/1
status: complete
task_id: a28f7360596fb619d74bb8bedb7102f530e81ac0d172fefa685d34dcec77d542
---

# 当前任务导航：metadata 复用链路局部通过，精度回退仍未闭环

此前固定 seed 的 metadata-only 与纯基线对照仍显示90/107对99/107，9道差异均由基线正确侧单向回退；该差异尚未解释，不能按采样噪声或本轮检查结果判定PR整体正确。新增证据覆盖题13的确定性 prefill、单请求decode，以及同一DP多请求并发长生成：C4/C128 metadata缓存与现算值、compressor实际cos/sin、slot mapping和跨边界刷新均未发现不一致，累计数百万次检查为零差异；但并发最长约1.9万而非65K，MTP路径未覆盖该复用逻辑，且多请求长生成在用户要求下提前停止。因此当前可表述为“已覆盖的metadata复用链路未复现错误”，仍需保留graph replay生命周期、输入buffer更新、刷新边界、请求行对应关系以及长decode精度回退等未解释问题，不能升级为0.26 PR无任何问题或完整精度通过。

## Materials

- [算子同步、PR 更新与 DSV4 验证进展](blocks/33e9910761652d621a186b2869b854b9889e75fee26e975aff505eeb8c8a80e2.md): 任务围绕将 ops-transformer 的 compressor、quant lightning indexer 等算子同步到 main 和 releases/v0.26.0rc，并叠加 PR #10201；后续又将 DSV4 的 scatter_nd_update/v2 路径迁移到 cann-recipes-infer 的 scatter_nd_update_asc。过程中出现 QLI metadata CMake 新旧接口不兼容、内部 header 被错误加 vllm_ 前缀、pip 依赖解析失败等问题，分别通过最小适配、修正命名和 --no-deps 构建处理；PR #15001/#15002 已创建并多次更新。ASC 算子编译及 3/3 数值回归曾通过，但模型验证尚未完成，随后因 cache view 适配边界被质疑而暂停。
- [修正 ASC 作用域并准备 GPQA 组合验证](blocks/e2bf644b8ba52e47c82f28ddcfcf05c06f2f9684a85e737bd8283042b08b15d5.md): 审查确认将四维 cache 直接 view 为二维输入并不适用于所有旧 v2 路径，尤其可能把 [block, offset] slot mapping 错误变成 2N×1；因此修正为仅 DSV4 使用 ASC，非 V4 路径继续使用 CANN 的 npu_scatter_nd_update_。验证组合改为 0.26 原生 PR #14994 + #15001，组合 SHA d89d88974 的 metadata-cache 单测和四维 cache NPU 回归已报告通过，完整构建与包路径也已锁定。按 A3 文档启动 DP4×TP4、16 NPU 的 DeepSeek-V4-Flash 后，首次启动在权重加载前因 14 号卡 HDC/TSD 超时失败；尚未获得 GPQA 精度结果，当前待精确终止进程组并进行单卡隔离检查。
- [153 单机迁移与 A3 兼容性修复](blocks/61240845af261571c772273735dddb9cb02e6f324ce64a8f21f271b21b2ac788.md): 155/156 被另一容器的 vaws-offload GLM-5.2 作业长期占用后，改选空闲单机 153；组合 #14994+#15001（初始 SHA d89d8897）完成迁移并通过环境、导入和定向测试。153 的启动先暴露 Triton 0.26 环境不匹配，随后真实权重和 ACLGraph 验证发现 Compressor 的 BF16 normWeight ABI、padded state-cache stride 及 ASC scatter 的 cache.view 假设等问题，分别更新 #15001、#15002，并固定 GLOO_SOCKET_IFNAME=lo。后续 FP32 与 stride 修复生效，但仍需结合新的 padded scatter 和 metadata 修复完成最终验证。
- [A3 服务通过 graph capture 并启动 GPQA](blocks/bf68299b387da609ea8b2cb4b2ee46033d01800fd3bc96c53dd6eddbf995510b.md): 增量重编译生成新的 A3 Compressor kernel 后，r6/r7 先遇到重复 detached 启动导致的 HCCL 16666 端口冲突，改用独立端口并单实例启动；组合提交 d73d970 的 16/16 rank、17/17 decode graph capture、API ready 和真实 HTTP 200 请求均报告成功。GPQA 首轮因 runner 使用错误的 3600 秒 timeout，产生超时和不可计入精度的结果；恢复为 7200 秒后，先合并 78 条有效结果，后续多段后台续跑累计达到 151/198，服务相关 errors 仍为 0。答案抽取存在大量 pred=None/missing，且续跑客户端曾异常退出，因此完整 198 题 summary、最终精度和 PR 验证更新仍未完成。
- [GPQA 续跑纠正 timeout 并保持未完成](blocks/35efd2a16a9913e3ffe2cf65821784f4ee4a55f12e40bb9ff66cdd8109850c0b.md): GPQA 从 64/198 推进到 78/198 时确认 `--timeout 3600` 会触发三次重试和 10803 秒后返回的伪 missing，遂停止该轮，仅保留 78 条 `error=null` 结果，并按默认 7200 秒重跑其余题目。续跑曾在 46/120 后客户端退出，随后改为合并 prior 文件并在 153 独立后台进程中继续；截至该块末尾 r11 新增 23/70，累计有效覆盖 151/198，errors=0，服务健康 200。已观察到的正确率和 missing 仅是中间样本，不能视为最终 GPQA 结论；仍有 47 题及最终合并、PR 验证段待完成。
- [组合验证完成并转向隔离增量基线](blocks/ff0523d07baff121313289bf83be95f4d57554a5c09c25c66190a72a2cc2501c.md): 组合验证最终完成 198/198，98 correct、官方 accuracy 49.4949%、errors=0，但严格 `Answer: [A-D]` parser 产生 89 个 missing，其中 60 个达到 65,536 token 上限；该结果不能宣称精度通过。全服务日志未发现 traceback、Compressor shape、ASC scatter 或 HTTP 5xx，#14994/#15001 的组合证据已写回 PR。随后决定从纯 `releases/v0.26.0rc@3d74c7ba5` 开始，按纯基线、#14994 metadata、Compressor、QLI+QLI metadata、ASC scatter 的顺序隔离验证。
- [纯基线构建、启动与 107 题冻结](blocks/011a77c10302da28a855f8c5854365d49af752ef30afd19264c619ba7c53f466.md): 纯基线构建经历了 Catlass 子模块缺失、AppleDouble `._*.cpp` 被编译器误识别等失败尝试，清理后成功完成；import 和算子清单确认未包含 #14994、新 QLI/metadata 或 ASC scatter。基线服务在 A3 DP4×TP4/EP/MTP=1、FULL_DECODE_ONLY 配置下通过 HTTP 200 真实请求，但完整 198 题尚未完成；在 107 条可完整解析且 ID 唯一的记录处冻结为 93/107、86.9159%、errors=0、missing=7，另有一条停止时被截断的 JSONL 行被记录而未静默忽略。已明确 QLI 与 QLI metadata 必须作为不可拆分的 tiling 配套原子组。
- [仅 metadata 阶段开始并进行固定子集复测](blocks/fb19470972fa8df84d17f6163dccf0958f1128eafb15bb781636df041d94d9ab.md): #14994 隔离环境核验通过：Python 源码哈希一致，自定义算子二进制与纯基线逐文件一致，新增 metadata cache 单测为 `1 passed`，因此当前唯一变量是 metadata Python 生命周期逻辑。metadata-only 服务已在相同 A3 配置下启动并通过 HTTP 200 返回 `42`，随后按冻结的 107 个 ID 开始 GPQA；截至该块结束为 66/107、服务和请求错误均为 0，但仍处于长尾运行中，没有最终 accuracy、missing 或完整结果结论。温度为 1，单题结果可能有采样波动，比较应以固定子集的汇总指标为准。
- [metadata-only 与固定 seed 对照结果](blocks/bb589c9d59c85cfcea2bf74595cd53543175b041384725aa59826a921dfcaf13.md): metadata-only 随机采样完成为 90/107（84.1121%，missing=9），低于纯基线 93/107（86.9159%，missing=7）；随后固定 seed 对照仍为 metadata 90/107、missing=12，而纯基线最终结果尚未在本块结束前完成。此前 eager 单请求 dump 在 24 组比较中未发现输入、缓存改写或现场重算差异，但该证据不覆盖 FULL_DECODE_ONLY 的 graph replay 生命周期。
- [固定 seed 回退与 graph 路径单请求诊断](blocks/86763910f24b96f3ef2631727945086afd8fd85685759b186585b8dd8bee8ef6.md): 固定 seed 对照完成后，纯基线为 99/107（92.5234%，missing=5），metadata 为 90/107（84.1121%，missing=12）；9 道题单向从基线正确变为 metadata 错答或 length/missing，精确 McNemar 检验 p=0.00390625，因此 #14994 未通过精度门槛。参考 K3 的 device 快照、图外 materialized-output 边界和延迟 D2H 方案后，单条真实 graph 请求的 4 个 TP rank、3 个 cache group 比较无差异，但这只覆盖一次小批次首次复用，不能证明逐层、多 step 或并发正确。
- [C4 metadata 分组与未覆盖验证范围](blocks/7351e0b9ccfb918aef8a294dd470e943bdb35ef8f785bb4c8e10676f6fcaa7dc.md): C4 indexer、C4 attention compressor、C128 attention compressor 使用不同 cache group 和 metadata key，prefill/decode 也分开；普通 forward 创建新 ForwardContext，同一 context 的 MTP 子步会显式清缓存。现有 dump 只证明 4 个 TP rank 上各组的首次存入、读取和重新计算采样值一致，未覆盖后续层、跨 step 的刷新结果、多个 draft 子步、真实并发及动态 batch 进出。
- [题13与多阶段 metadata 复用排查](blocks/ff40a1c7d7f61fd423c4960e0fca95ce5eb8511af6d1536d1cb87bcc69360b84.md): 对9道回退题进行 temperature=0 的 PR 前后 A/B，9/9均分叉，最早为题13第3个生成 token，但早期差异受并发分批未固定及 PR 探针影响，不能据此定责。启用 nightly 的 `HCCL_DETERMINISTIC=true`、`LCCL_DETERMINISTIC=1` 后，题13 prefill 与单请求 decode 中，C4 indexer、C4 compressor、C128 的缓存值、现算值、hidden states、cos/sin 和 slot mapping 均一致；decode 检查覆盖384 tokens、209个 replay、跨256/384/512边界，未发现 metadata 复用或刷新错误。随后启动9题并发长生成，8题进入同一DP，累计检查约260万次复用比较和274万次 cos/sin 消费，仍无差异；MTP因压缩比为0不经过该复用路径。
- [并发长生成提前停止的覆盖结论](blocks/3e5149f3ee750ee83c38fdb774f73f661e255a23e134c5d848d45237d3679985.md): 用户要求停止后，本轮测试请求和 dump 已关闭，服务保留；同一DP的8并发长生成最长约1.9万 token，未达到设置的65,536上限。累计约330万次 metadata 复用比较、346万次实际 cos/sin 检查均无差异，也未发现请求失败或跨请求 slot 串用。结论仅限已覆盖的并发和长度：未发现0.26 metadata PR 的问题，但不构成完整65K精度验收，也没有解释此前99/107降至90/107的精度差异。

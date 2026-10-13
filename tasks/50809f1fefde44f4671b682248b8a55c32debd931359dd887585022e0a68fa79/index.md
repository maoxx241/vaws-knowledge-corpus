---
blocks:
- block_id: c05d4d8ae191f57c6f51b360b7ccca0432241255d1fc44ef5e4783d9d5b6a808
  indexed: true
  sha256: 42814aa7f3aed1fa3edf0a47c42d3d2c00435a4fa0a4b1290924252cd09f13db
  source_range:
    end: 16882039
    part: 0
    start: 0
  summary: 目标是在 154 的 K3 vLLM 0.27.1 容器中，将 prefill 保持旧 vLLM QLI、decode 切换到 CANN QLI v2，模型为 DSV4 Flash+MTP；QLI v2 来自 PR `a3925fd49`，涉及非连续 PA stride、metadata、AICPU/host tiling 及 arch22→arch32 适配。Grok 旧方案实际采用反向路由，混合版曾为 GPQA 66/107（35 条无法解析、28 条命中 length），双 `libcust_opapi.so` 冲突只是未完成 A/B 的推测；旧基线 93/107、full-v2 94/107 仅作为历史参照，不能视为本次混合版已验证成功。当前 K3 环境版本已匹配，但增量编译仍在修复迁移缺口：CANN 宏签名不兼容、缺少 `op_host/tiling_util.h`，以及 `ACLNN_LOG_FMT_CHECK` 下的重复日志宏冲突；尚未完成服务启动、路径命中确认或 107 题精度验证。
  title: DSV4 Flash+MTP 混合 QLI 路由迁移与构建
- block_id: d4b0cd0f2634ccb05532f4d21a87d3e7ff45c4fb492cd1fed8e405ef7a92ff33
  indexed: true
  sha256: 9157f9c5606cddfd4a0e60af0231267838e429560ae0b4bc20ee1bd74fe08a1e
  source_range:
    end: 33661454
    part: 0
    start: 16882039
  summary: 在 K3/vLLM 0.27.1 环境中，QLI v2 编译、no-deps 安装、45 项路由 UT 和 16 卡服务启动均被报告完成；期间保留了 metadata 宏冲突、AICPU `std::transform` lambda 缺失、submodule 路径、vLLM 模块配对及 SSH 变量展开等失败与修正。首次 split-mix 实测 GPQA 为 98/107（0 missing、0 error、107 stop），性能数据随后显示 8K/C16 等场景相对历史 Full v2 回退，但历史基线为 vLLM 0.26.0，不能据此归因于 QLI 路由。后续代码改为仅纯 decode 使用 QLI v2、含 prefill 的整批使用旧 QLI，8K/C16 报告从 458.15 提升至 488.43 output TPS；metadata 仍使用两个独立 device buffer，当前未验证共享物理 buffer。
  title: 0.27.1 混合路由构建与性能验证
- block_id: f51d5313318d1606b2e680ef6e0722d2cbde2512753225b8d25198616f98b573
  indexed: true
  sha256: 62f8457cd5ab86e5b74292b1dd9656409ca7842d3e1624af4af9389e6ee9ffd8
  source_range:
    end: 33661454
    part: 1
    start: 16882039
  summary: 该块给出了 Old QLI、Full QLI v2 与 Mixed 在 8K/C1、8K/C16、32K/C1、32K/C16、128K/C1、128K/C8 六个场景的 Output TPS、Decode TPS p50、Req/s，以及 E2E、TTFT、TPOT 的 mean/p50/p99。数据报告 50/50 请求成功，但 Old/Full v2 与 Mixed 的运行环境和版本存在差异：历史数据为 vLLM 0.26.0，当前 Mixed 为 0.27.1。后续补充表又给出 Compressor-only、Full v2 与当前 Mix 的另一组结果，因此这些数字应作为可检索的实测记录，不能直接视为严格同版本因果比较。
  title: 三种 QLI 方案的完整性能数据
- block_id: ebd1a50ec4fe144e406c7e6b11dd5a70f9b30be4608ad4551b4dbef445b120c0
  indexed: true
  sha256: 4e9d10ebbe5d95652a10c59af165f43758131df3dff019f83a99942a39889ab8
  source_range:
    end: 33661454
    part: 2
    start: 16882039
  summary: '报告称 0.26 Full v2 源码与既有 QLI v2 二进制通过入口注册兼容检查，且 subagent 已向 vllm-ascend 提交 PR #15320（commit `d6df145b...`）；但新增 UT 未执行，也未完成算子编译、NPU 精度或性能验证，CI 仍排队。按用户要求，生产代码拟移除 `_run_qli_v2`、`_qli_compressed_seq` 及无意义变量/UT，改为原函数内直接分支，并补非连续 PA/连续输入一致性的 NPU e2e 算子用例；材料随后报告 0.26 Full v2 在 ACLGraph capture 因 Compressor `normWeight` dtype 不匹配失败，尚未证明与 QLI 路由相关。'
  title: 0.26 Full v2 PR 与重构中的启动故障
- block_id: 1ac71ab0ca44ebb889822060e75a93a7b1ab01c6b5ba3e967f0a75e70855f366
  indexed: true
  sha256: 3b62eda183996b55c394c325e6ea04354face13456c0a01cdcdd4e6081e5f17c
  source_range:
    end: 48079737
    part: 0
    start: 33661454
  summary: '0.26 Full v2 PR #15320 多次 amend，最终记录的 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`；QLI v2 使用独立 binding，删除了字段级 Optional assert，并将 `decode_qli_metadata` 改为非 Optional。0.26 算子编译安装完成，非连续 PA NPU e2e 报告为 `1 passed`；Full v2 fixed-107 精度报告为 `100/107 = 93.4579%`，Mix 性能请求 `50/50` 成功。PR 的代码检查和 DCO 多次通过，但 selected tests 因当前账号无权添加 `ready` label 尚未触发，且未将该权限阻塞提升为成功结论。'
  title: 0.26 Full v2 PR、编译与服务实测
- block_id: ca6ee71b8f41f6c9d6469b35b3e6a8a30ec04bbd162233259db5da80102e9fe3
  indexed: true
  sha256: 4543651f9ae3983ee91e139cab18c1a8ee410e64a92b42dfbde0d88907b5647d
  source_range:
    end: 48079737
    part: 1
    start: 33661454
  summary: 早期 0.26 表格记录了 Compressor-only、Full v2 和 Mix 的 TPS/TTFT/TPOT；随后通过 `qli_seqused_k.max().item()` 的设备侧 reduction 加 `.item()` 识别出潜在 D2H 同步，并将 Full v2 与 Mix 的相关路径改为复用 CPU 侧长度。修改后报告的三方性能为 150/150 请求成功，但严格 A/B 进一步显示 Mix prefill-only TTFT 与 native QLI 基线基本重合：8K/C1 mean 为 1.3185s 对 1.3133s，32K/C1 为 5.0288s 对 5.0306s。材料仍指出跨服务历史数据、Prefix Cache、并发 decode 排队和运行时波动会影响 TTFT；因此没有证据证明 Mix prefill 本身退化。
  title: 0.26 三方案性能、D2H 假设与严格 A/B
- block_id: d4af10358122b12e2715c046852d95458289a1620e30ccb43363c8c92c5f57a8
  indexed: true
  sha256: c6fa542bdff6769f2f306b8faa501a9bf91ec9a1d6c7e03e5d853631d016d387
  source_range:
    end: 48079737
    part: 2
    start: 33661454
  summary: 实验完成 Native A → Mix B → Native A2 的同机、同容器、同编译产物对照，使用唯一 prompt、`max_tokens=1`、无共享前缀，避免纯 prefill 测量进入 QLI v2 decode。报告称 96 个计量请求和 9 个预热请求全部通过；8K/C1 mean 分别为 1.3133s、1.3185s、1.3206s，32K/C1 为 5.0306s、5.0288s、5.0440s，差异处于运行噪声范围。宿主机路径映射曾导致 A2 启动命令提前退出，但未占卡或发请求；修正到容器内后实验完成，结论仍只能支持“未观察到 Mix prefill 特有 TTFT 退化”。
  title: Native–Mix–Native TTFT 回切实验
- block_id: efc3f39f55e3de1dbeff3976fc03bf93bcdf8070c80f21fd8c542312a4386f2e
  indexed: true
  sha256: edc423bd0518ab1af074882915f8e8af704b191b3e631dd983e6141e9e94969f
  source_range:
    end: 48079737
    part: 3
    start: 33661454
  summary: 对 Compressor-only、Full v2、Mix 做了 4K 至 256K 的自适应粗扫，粗扫为每个长度两次；结果没有形成清晰、连续且显著的 v2 优势区间，Mix 的 16K 高点、96K 慢点后来被重复测量证明主要是波动。7 个长度的四次确认显示：4K Mix 比 Compressor 低约 0.84%，16K 和 24K 基本一致，32K 低约 0.96%，64K 高约 0.76%，96K 高约 0.28%；这些差异均很小，尚未证明稳定的性能分界。当前已完成 Compressor 与 Mix 确认集，Full v2 确认集仍在进行，后续整体建议不能提前宣称三方案已完成全量确认。
  title: 4K–256K 粗扫与重复确认
- block_id: 1e2c95ce73d56f45d086b58398d960fa1aefcc8306b85d21c13d6d48fc645509
  indexed: true
  sha256: a2eb08ae5219e12dc9de2fc1ebdb8f24df610c6369644d818bccb7ad6a2d54ca
  source_range:
    end: 48499941
    part: 0
    start: 48079737
  summary: Full v2 在 4K–96K 四次确认中未形成连续、可重复的优势区间：仅 16K 高于 Compressor 约 1.25%，24K 低约 2.29%，其余多在约 ±1% 内，先前 32K 领先未复现。32K/C16 三波复测显示 Mix 为 491.93→548.06→635.03 TPS，Compressor 为 489.64→539.33→615.00 TPS；Mix 优势随共享前缀缓存铺开从约 0.5% 增至 3.3%，旧的 581 TPS 仅代表中间缓存状态，不能视为稳定优势。Full v2 的并发三波仍待完成，硬件实验和最终推荐尚未闭合。
  title: Full v2 多序列确认与 32K/C16 缓存爬升复测
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 50809f1fefde44f4671b682248b8a55c32debd931359dd887585022e0a68fa79
  kind: experience
  material_digest: 4e24e90519993177baa3d849e46e36a9a6e5b013ba57c11a8e93f7aa40be82cc
  revision: da154875a8cb47af68d5b3c9288d2cc052ef7f498215c5629dde005f8d66e174
  schema: mindie-entry/3
  summary: 当前材料覆盖 PR/编译状态、严格 Native–Mix–Native 实验、4K–256K 粗扫及三方案部分确认；未发现 Mix prefill 固有 TTFT 退化，差异仍可能受 Prefix Cache、并发排队、跨服务数据和运行波动影响，不能当作已验证归因。Compressor、Mix 与 Full v2 在 4K–96K 多数差异约处于 ±1% 内，Full v2 仅在 16K 出现一次约 1.25% 候选优势，未形成连续优势区间；32K/C16 复测显示 Mix 的优势随缓存爬升而扩大，但旧 581 TPS 不稳定。Full v2 并发三波、最终推荐及此前未完成的 CI/硬件验证仍待完成；selected tests 的 ready label 权限阻塞、失败尝试和不确定性继续保留。
  title: 0.26 QLI v2 三方案对比：序列确认与并发缓存效应
navigation: 当前材料覆盖 PR/编译状态、严格 Native–Mix–Native 实验、4K–256K 粗扫及三方案部分确认；未发现 Mix prefill 固有 TTFT 退化，差异仍可能受 Prefix Cache、并发排队、跨服务数据和运行波动影响，不能当作已验证归因。Compressor、Mix 与 Full v2 在 4K–96K 多数差异约处于 ±1% 内，Full v2 仅在 16K 出现一次约 1.25% 候选优势，未形成连续优势区间；32K/C16 复测显示 Mix 的优势随缓存爬升而扩大，但旧 581 TPS 不稳定。Full v2 并发三波、最终推荐及此前未完成的 CI/硬件验证仍待完成；selected tests 的 ready label 权限阻塞、失败尝试和不确定性继续保留。
schema: mindie-material-task/1
status: complete
task_id: 50809f1fefde44f4671b682248b8a55c32debd931359dd887585022e0a68fa79
---

# 0.26 QLI v2 三方案对比：序列确认与并发缓存效应

当前材料覆盖 PR/编译状态、严格 Native–Mix–Native 实验、4K–256K 粗扫及三方案部分确认；未发现 Mix prefill 固有 TTFT 退化，差异仍可能受 Prefix Cache、并发排队、跨服务数据和运行波动影响，不能当作已验证归因。Compressor、Mix 与 Full v2 在 4K–96K 多数差异约处于 ±1% 内，Full v2 仅在 16K 出现一次约 1.25% 候选优势，未形成连续优势区间；32K/C16 复测显示 Mix 的优势随缓存爬升而扩大，但旧 581 TPS 不稳定。Full v2 并发三波、最终推荐及此前未完成的 CI/硬件验证仍待完成；selected tests 的 ready label 权限阻塞、失败尝试和不确定性继续保留。

## Materials

- [DSV4 Flash+MTP 混合 QLI 路由迁移与构建](blocks/c05d4d8ae191f57c6f51b360b7ccca0432241255d1fc44ef5e4783d9d5b6a808.md): 目标是在 154 的 K3 vLLM 0.27.1 容器中，将 prefill 保持旧 vLLM QLI、decode 切换到 CANN QLI v2，模型为 DSV4 Flash+MTP；QLI v2 来自 PR `a3925fd49`，涉及非连续 PA stride、metadata、AICPU/host tiling 及 arch22→arch32 适配。Grok 旧方案实际采用反向路由，混合版曾为 GPQA 66/107（35 条无法解析、28 条命中 length），双 `libcust_opapi.so` 冲突只是未完成 A/B 的推测；旧基线 93/107、full-v2 94/107 仅作为历史参照，不能视为本次混合版已验证成功。当前 K3 环境版本已匹配，但增量编译仍在修复迁移缺口：CANN 宏签名不兼容、缺少 `op_host/tiling_util.h`，以及 `ACLNN_LOG_FMT_CHECK` 下的重复日志宏冲突；尚未完成服务启动、路径命中确认或 107 题精度验证。
- [0.27.1 混合路由构建与性能验证](blocks/d4b0cd0f2634ccb05532f4d21a87d3e7ff45c4fb492cd1fed8e405ef7a92ff33.md): 在 K3/vLLM 0.27.1 环境中，QLI v2 编译、no-deps 安装、45 项路由 UT 和 16 卡服务启动均被报告完成；期间保留了 metadata 宏冲突、AICPU `std::transform` lambda 缺失、submodule 路径、vLLM 模块配对及 SSH 变量展开等失败与修正。首次 split-mix 实测 GPQA 为 98/107（0 missing、0 error、107 stop），性能数据随后显示 8K/C16 等场景相对历史 Full v2 回退，但历史基线为 vLLM 0.26.0，不能据此归因于 QLI 路由。后续代码改为仅纯 decode 使用 QLI v2、含 prefill 的整批使用旧 QLI，8K/C16 报告从 458.15 提升至 488.43 output TPS；metadata 仍使用两个独立 device buffer，当前未验证共享物理 buffer。
- [三种 QLI 方案的完整性能数据](blocks/f51d5313318d1606b2e680ef6e0722d2cbde2512753225b8d25198616f98b573.md): 该块给出了 Old QLI、Full QLI v2 与 Mixed 在 8K/C1、8K/C16、32K/C1、32K/C16、128K/C1、128K/C8 六个场景的 Output TPS、Decode TPS p50、Req/s，以及 E2E、TTFT、TPOT 的 mean/p50/p99。数据报告 50/50 请求成功，但 Old/Full v2 与 Mixed 的运行环境和版本存在差异：历史数据为 vLLM 0.26.0，当前 Mixed 为 0.27.1。后续补充表又给出 Compressor-only、Full v2 与当前 Mix 的另一组结果，因此这些数字应作为可检索的实测记录，不能直接视为严格同版本因果比较。
- [0.26 Full v2 PR 与重构中的启动故障](blocks/ebd1a50ec4fe144e406c7e6b11dd5a70f9b30be4608ad4551b4dbef445b120c0.md): 报告称 0.26 Full v2 源码与既有 QLI v2 二进制通过入口注册兼容检查，且 subagent 已向 vllm-ascend 提交 PR #15320（commit `d6df145b...`）；但新增 UT 未执行，也未完成算子编译、NPU 精度或性能验证，CI 仍排队。按用户要求，生产代码拟移除 `_run_qli_v2`、`_qli_compressed_seq` 及无意义变量/UT，改为原函数内直接分支，并补非连续 PA/连续输入一致性的 NPU e2e 算子用例；材料随后报告 0.26 Full v2 在 ACLGraph capture 因 Compressor `normWeight` dtype 不匹配失败，尚未证明与 QLI 路由相关。
- [0.26 Full v2 PR、编译与服务实测](blocks/1ac71ab0ca44ebb889822060e75a93a7b1ab01c6b5ba3e967f0a75e70855f366.md): 0.26 Full v2 PR #15320 多次 amend，最终记录的 head 为 `4322d87d0893b16d2b7c8a7e546d1c0bfc6c6a2f`；QLI v2 使用独立 binding，删除了字段级 Optional assert，并将 `decode_qli_metadata` 改为非 Optional。0.26 算子编译安装完成，非连续 PA NPU e2e 报告为 `1 passed`；Full v2 fixed-107 精度报告为 `100/107 = 93.4579%`，Mix 性能请求 `50/50` 成功。PR 的代码检查和 DCO 多次通过，但 selected tests 因当前账号无权添加 `ready` label 尚未触发，且未将该权限阻塞提升为成功结论。
- [0.26 三方案性能、D2H 假设与严格 A/B](blocks/ca6ee71b8f41f6c9d6469b35b3e6a8a30ec04bbd162233259db5da80102e9fe3.md): 早期 0.26 表格记录了 Compressor-only、Full v2 和 Mix 的 TPS/TTFT/TPOT；随后通过 `qli_seqused_k.max().item()` 的设备侧 reduction 加 `.item()` 识别出潜在 D2H 同步，并将 Full v2 与 Mix 的相关路径改为复用 CPU 侧长度。修改后报告的三方性能为 150/150 请求成功，但严格 A/B 进一步显示 Mix prefill-only TTFT 与 native QLI 基线基本重合：8K/C1 mean 为 1.3185s 对 1.3133s，32K/C1 为 5.0288s 对 5.0306s。材料仍指出跨服务历史数据、Prefix Cache、并发 decode 排队和运行时波动会影响 TTFT；因此没有证据证明 Mix prefill 本身退化。
- [Native–Mix–Native TTFT 回切实验](blocks/d4af10358122b12e2715c046852d95458289a1620e30ccb43363c8c92c5f57a8.md): 实验完成 Native A → Mix B → Native A2 的同机、同容器、同编译产物对照，使用唯一 prompt、`max_tokens=1`、无共享前缀，避免纯 prefill 测量进入 QLI v2 decode。报告称 96 个计量请求和 9 个预热请求全部通过；8K/C1 mean 分别为 1.3133s、1.3185s、1.3206s，32K/C1 为 5.0306s、5.0288s、5.0440s，差异处于运行噪声范围。宿主机路径映射曾导致 A2 启动命令提前退出，但未占卡或发请求；修正到容器内后实验完成，结论仍只能支持“未观察到 Mix prefill 特有 TTFT 退化”。
- [4K–256K 粗扫与重复确认](blocks/efc3f39f55e3de1dbeff3976fc03bf93bcdf8070c80f21fd8c542312a4386f2e.md): 对 Compressor-only、Full v2、Mix 做了 4K 至 256K 的自适应粗扫，粗扫为每个长度两次；结果没有形成清晰、连续且显著的 v2 优势区间，Mix 的 16K 高点、96K 慢点后来被重复测量证明主要是波动。7 个长度的四次确认显示：4K Mix 比 Compressor 低约 0.84%，16K 和 24K 基本一致，32K 低约 0.96%，64K 高约 0.76%，96K 高约 0.28%；这些差异均很小，尚未证明稳定的性能分界。当前已完成 Compressor 与 Mix 确认集，Full v2 确认集仍在进行，后续整体建议不能提前宣称三方案已完成全量确认。
- [Full v2 多序列确认与 32K/C16 缓存爬升复测](blocks/1e2c95ce73d56f45d086b58398d960fa1aefcc8306b85d21c13d6d48fc645509.md): Full v2 在 4K–96K 四次确认中未形成连续、可重复的优势区间：仅 16K 高于 Compressor 约 1.25%，24K 低约 2.29%，其余多在约 ±1% 内，先前 32K 领先未复现。32K/C16 三波复测显示 Mix 为 491.93→548.06→635.03 TPS，Compressor 为 489.64→539.33→615.00 TPS；Mix 优势随共享前缀缓存铺开从约 0.5% 增至 3.3%，旧的 581 TPS 仅代表中间缓存状态，不能视为稳定优势。Full v2 的并发三波仍待完成，硬件实验和最终推荐尚未闭合。

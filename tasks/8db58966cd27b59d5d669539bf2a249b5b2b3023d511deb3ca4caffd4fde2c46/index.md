---
blocks:
- block_id: 55e20bf40e96152930ad859ccaa62f8834ba3124be3e67b69e0ba2e6985bb6fe
  indexed: true
  sha256: 96dbe3992ab43940ed0d72f62347b56e34be19ce977dd7ca9be884e509a43afe
  source_range:
    end: 15887542
    part: 0
    start: 0
  summary: '围绕 PR #13327、#13407 与指定 QuaRot 权重，完成四机环境、权重哈希、进程归属及组合分支核验；复现了 128K/1K 下 token 0/“!”、NaN logits 和 100% 假采信，且 DP0/DP1 一致。定位过程中排除了权重缺失、KDA 长度本身溢出及 draft 首因，初步发现 speculative conv state 的新增槽未初始化，并修复了 #13327 warmup 的 `BLOCK_VERIFY` 参数兼容问题。'
  title: 定位 QuaRot 长上下文异常与初步根因
- block_id: a65a69bf696c57669f9bf46eada18e8789b73e997bdb12aed911e444b447a5c9
  indexed: true
  sha256: 4367064e9d449eada390a071cdf94aac9d7e7fa710ee42a00d6f9b510ac06178
  source_range:
    end: 15887542
    part: 1
    start: 0
  summary: 同步探针会改变时序，不能作为修复证据；target-only 四 DP 的 128K 请求均正常，证明 QuaRot 权重和 target prefill 并非必然失效。单卡生产形状测试确认 speculative `state_len=10` 的新增 7 个槽未初始化，清零修复后无缓存路径通过，但 prefix hit 98.73% 时仍出现 `!`×1024 与 100% 采信；最终运行时清单、静态检查和针对性测试已通过。
  title: 修复 speculative state 尾槽并发现 prefix-cache 残余故障
- block_id: 3b430ca752d199da20d94740b4be441b0568fbba86d8aea287a7cfd320bbf72d
  indexed: true
  sha256: 35d414e2a39f7d5b2ededd963e16acb28583a61d0affc493c65963974b8f2186
  source_range:
    end: 15887542
    part: 2
    start: 0
  summary: 'target-only 在完整 128K/1K 请求后连续前缀复用仍正常，确认故障来自 speculative 跨请求状态生命周期，而非旋转量化权重。修复 accepted-token 异步回写后，主 128K/1K 请求恢复正常的 46.6667% 采信率，但相同 prompt 的第二次 warmup 仍返回 token 0/“!”，指向 exact-boundary recurrent state cache admission 缺口；当前正参考上游 PR #50457 做 v0.26 兼容的最小 backport，尚未完成验证。'
  title: 追踪跨请求状态污染与缓存边界缺口
- block_id: 143d5afdea17b84e3112cdc8c55c4d380170080089245cf988717ced6c20beaa
  indexed: true
  sha256: 652102e9ba59854259cebd9fae25410d1339ff5a8065685574460a3894a6d9cf
  source_range:
    end: 15887542
    part: 3
    start: 0
  summary: 边界 admission 单测、r3/r4 部署校验及完整 22-test 回归最终通过；两次未加载 CANN `acl` 的测试收集失败被保留为环境配置问题。r4 四机 health=200，冷启动 128K/1K 与基线一致（`fd1aee0…`、采信 784/1680=46.67%、prefix hit 0），但两次 1-token warmup 仍输出 `<|close|>` 与 `!`，因此显式 `AscendMambaManager` 工厂路由并非完整根因。进一步实验显示 129792 边界缓存恢复与冷算首 token 都为 ` It`，异常集中在边界先缓存后继续运行的路径，当前怀疑 KDA 持久状态在后续续算/解码中被覆盖；任务仍未完成。
  title: r4 工厂路由修复后仍复现 warmup 异常
- block_id: 4aa6356182ec6f99e2bf4963f3ac59a0f53b73c858c0042fe318b0d800b05da2
  indexed: true
  sha256: 3bf73238b08fad0481405d4d6883e916de4f0b8d516d70ddcb3445b39ba4a8f7
  source_range:
    end: 15887542
    part: 4
    start: 0
  summary: DP1 冷算及命中 130560 tokens 的复用均与冷算一致，但 canonical warmup 在 DP1、0 prefix hit 时也输出 `<|close|>`，否定了请求槽残留和 prefix-cache 恢复作为主要根因。3 槽、10 槽及 10 槽跨物理块 memcpy 的算子级 prefill 回归逐元素一致，因而 KDA 卷积和 chunk 状态迁移假设也未获支持。当前最强线索是启用 DSpark 后目标模型首个 prompt forward 已偏离，重点审计 `Qwen3DSparkModel` 的 materialized GQA auxiliary stream、QuaRot 对共享 `embedding/lm_head` 的反旋转，以及可能的 NPU storage 别名；尚无最终修复或验证成功。
  title: KDA 布局假设被否定，转向 DSpark/QuaRot 别名审计
- block_id: 0d67150304056716fd731bd81e89b75ad21d0fb3cf9b9771088b9f375209e069
  indexed: true
  sha256: baf06840691f4c7be44adf4b7db77d9cfab773312228e1719ae22785a572dd0d
  source_range:
    end: 32665112
    part: 0
    start: 15887542
  summary: 已确认单请求及冷/热128K结果可稳定复现，但128K并发2在高prefix命中、partial-tail/COW路径仍出现一个请求全`!`或输出漂移；单行复用、批量memcpy和partial-hash防护均不足。诊断服务曾因单机EP压缩导致NPU OOM，四机拓扑可启动；物理block-ID guard及相关单测通过，但r7第四轮再次出现同命中条件下采信和输出变化，说明仍不能视为修复成功。r8部署前的清单校验、pre-commit及gitleaks检查也记录了shell误解析、平台架构不兼容和既有误报等环境限制。
  title: r5–r7复现与物理块防护仍未解决并发污染
- block_id: 9050fd2ca4abc3f738a879f86b4fdca33c2ebec62724622cb31a8459fd2ddccf
  indexed: true
  sha256: cf3826dfd4d865afd4ca98e8ea5f89811f5c9893264c167127bcd5edce3143ae
  source_range:
    end: 32665112
    part: 1
    start: 15887542
  summary: r7四机TP16×DP4×EP64启动、health=200、8K基线、128K冷/热单请求及CPU/NPU测试均通过；单请求在99.609375% prefix命中时仍保持`784/1680`和冷态输出一致。并发2首轮正常，但后续相同99.609375%命中条件下出现输出SHA漂移、采信由`29.0786%`变为`29.9978%`，其中一次以控制标记开头，表明物理block-ID防护只消除了部分竞态。进一步审计发现partial-tail alias和后续COW目标块生命周期仍可能绕过保护；新增COW/边界测试`23 passed`，随后生成r8并完成四机清单校验。
  title: r7成功启动但高命中并发复现新一轮漂移
- block_id: bc6f92adf83b5e28f737eedcd52b0eb46dd50484f638e4ebfab1fff6fdaa5622
  indexed: true
  sha256: de2246e4ad37eb96809057ff61a4221d78087516e8ce16fcc88b73424359d875
  source_range:
    end: 32665112
    part: 2
    start: 15887542
  summary: r8的健康检查、模型身份、8K基线及128K冷/热单请求均通过，但并发第二轮在99.609375%命中时再次发生输出漂移和`<|close|>`开头，采信为`1388/4627=29.9978%`；因此COW物理块guard和边界校验仍未解决根因。随后r9追踪复现了第三轮降至27.41%，而生命周期埋点始终未触发；审计确认Ascend manager可能在补丁生效前被vLLM manager registry缓存，显式路由也只覆盖部分路径。r9的一次健康复现不能证明修复，调试RPC还因不可序列化的`torch.dtype`使服务失去后续证据资格；r10则在launcher预检阶段退出，尚未启动。
  title: r8失败后转向manager注册与实际运行路径审计
- block_id: fd06df3eb6ec86c579b8411f6f0bf7c3e64737b24d0698f2c21869ea6f1f74af
  indexed: true
  sha256: 8d46a0c6e8d63ee28bc0dcd04c46c6f27e9eb13482cb5f1a4b80c1117f6347d9
  source_range:
    end: 32665112
    part: 3
    start: 15887542
  summary: 确认 registry 路由曾绕过修复 manager；补齐 Mamba/KDA 新 state block 记录、worker 清零和 DSpark zeroer 初始化后，单卡清零测试及 CPU 回归通过，r12 的 zero-plan 覆盖 112 个 Mamba state block 且 post-state 无 NaN。四机连续 128K 测试中首轮约 45.76% 采信率正常，但重复高命中轮仍降至约 29.4%–29.5%，并出现异常续写，说明漏清零是真问题但不是唯一根因。清空 prefix cache 后同一 131072→1024 请求又出现 100% 采信率但输出全为 `!`，问题进一步指向 prefill 或缓存状态边界，而非已验证解决。
  title: r10–r12：Mamba 清零修复部分有效但未闭环
- block_id: 869453e9d4d85ceb0c608554c9e12b720da413e36c7875ac07283ca6cf3e6e39
  indexed: true
  sha256: 11db73e22fbe06d3e68f942e5de5d106ffebeb2858b5ff67a82603f4d3df0c32
  source_range:
    end: 32665112
    part: 4
    start: 15887542
  summary: r13 插桩显示，原路径可在同一 128K×1 请求中出现 100% 采信和 1024 个 `!`，首个非有限 recurrent state 位于 KDA layer 9，但同输入并非每次都复现。加入 layer 9/10 边界同步后，fresh 与 99.609% prefix-hit repeat 均稳定为 46.67%，输出 hash 一致，形成跨 stream/collective 可见性竞态的强反事实证据；这不是最终修复。单卡生产形状测试中，AscendC 在未补齐控制变量时曾全为非有限，但加入 L2-normalize 后连续 20 次有限且逐 bit 一致；因此尚不能归因于长序列算子溢出，当前转向审计 QuaRot 共享权重及 attention stream 边界，并计划以最小同步点进行四机 A/B。
  title: r13：同步诊断指向 Attention/KDA 就绪竞态
- block_id: 101de1ab9ab9d8ebb3f5dc2cbe224173119812e31963525f772c274b179d481c
  indexed: true
  sha256: 25f8145b41074cfe9fd346e79f9756868165bc43efee16d47cc8534de93f44ca
  source_range:
    end: 44875631
    part: 0
    start: 32665112
  summary: 修正层号后，index 8 的 KDA 边界在 r14 真实 128K/1K 请求中可通过局部执行边界恢复稳定结果；全 Triton r15 的单请求 fresh、prefix repeat 和 reset-fresh 均稳定，但 128K×2 仍退化为全 `!`。无共享 prefix 的双请求也失败，进一步将触发条件收窄为 prefill 与 speculative decode 混合批次，而非单纯 prefix cache/CoW；后续独立算子控制通过，完整 vLLM 调度与状态迁移仍未解决。
  title: 混合批次触发 KDA 状态污染
- block_id: e5ca4253b20954a243f775b0977cc997be134e75992c9927f64bc748a2e171e0
  indexed: true
  sha256: b4be7a643a571796a711c2b08e2d70ab198b1f94fb7d0fba64a6d47f477b1f8d
  source_range:
    end: 44875631
    part: 1
    start: 32665112
  summary: r16/r17 的 Kimi K3 + DSpark phase isolation 使两轮 128K×2 恢复正常，但之后 128K×1 仍出现 100% acceptance 和全 `!`，说明该修复不是充分条件。r18 的条件性 completion barrier 仍被反例否定：长 prefill 末段从 recurrent group 15–19 或 12–19 出现 NaN，提示 state block 迁移后的可见性、zeroing 或生命周期问题。此前关于单一双缓冲、全 Triton 或同步屏障的判断均只能解释部分现象，尚无可交付修复。
  title: phase isolation 后暴露 state-copy 竞态
- block_id: 9dc977599eb30f0075877b19c2100d3ea92cf9dabc51c290b09a2a6be301524a
  indexed: true
  sha256: 6be8484274a1ff91bd4a9536fdcb9a74ca30453207416793e87f4580118a22f0
  source_range:
    end: 44875631
    part: 2
    start: 32665112
  summary: r19 的两轮 128K×2 均通过，输出 hash 与正常样本一致、采信率约 60.3%、recurrent NaN 为 0；但随后相同顺序的 128K×1 在 `computed=122880` 再次出现 NaN 和全 `!`。因此 Ascend KV/Mamba block zeroer 复用异步 ID 缓冲虽是真实并发缺陷，按并发数轮转仍不足以解释根因；当前继续区分 layer 7 QuaRot Attention 输出异常与 layer 8 命中坏 `prev_state_idx`，r20 仅增加诊断探针，不能视为修复。
  title: zeroer 轮转修复仍未闭环
- block_id: b8efff81c3605db8f17d221e3abd1c1c8d86ad4df9987361bf95fb38c729a18e
  indexed: true
  sha256: d7635de731b419bdcf76104044c54d92357ccaf2b6cb501d652ed98b4622d90f
  source_range:
    end: 44875631
    part: 3
    start: 32665112
  summary: 日志确认 v0.26 为匹配 Mamba page size 将 attention block size 固定为 384，因此 `384+1` 的乱码/异常很可能发生在 Attention block 与 Mamba state block 换页时。新线索显示不带 DSpark 也可复现、A5 与 0.23 私仓未复现，故 QuaRot 和 DSpark 更像放大器；上游 `8472b0161` 的边界缓存修复高度相关，但此前等价逻辑仍未充分解决长上下文问题。当前应以纯 `0.26.0rc`、无 DSpark 的 `383/384/385/386` 冷缓存与热 prefix 矩阵，对照 Mamba state 对齐、copy/restore 和 hybrid cache manager 逻辑；r20/基线尚未给出最终验证结果。
  title: 384→385 页边界成为首要嫌疑
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8db58966cd27b59d5d669539bf2a249b5b2b3023d511deb3ca4caffd4fde2c46
  kind: experience
  material_digest: fad1d9d781f14a852fa2f23b11f9bfc4dc75fe8f08b2369a30c03a24b468618a
  revision: a2e3fc87e57cfe9900df415d5fee612463e4da54e6924903f8588a0dc367cc85
  schema: mindie-entry/3
  summary: 此前的高 prefix-hit、重复波次和 128K 长上下文测试持续出现 hash 漂移、全 `!`、异常 acceptance 与 NaN；局部同步、全 Triton、phase isolation、state-copy barrier 及 zeroer 轮转都曾改善部分波次，但均被后续 c1 或混合批次反例否定，不能视为修复成功。最新最强线索是 v0.26.0rc 在 attention page size=384 时跨到第 385 个 token 的 Attention/Mamba page 边界语义或 state 对齐/恢复错误；不带 DSpark 也可疑似复现，而 A5 与 0.23 私仓未复现。下一步是完成纯 0.26.0rc、无 DSpark 的 383/384/385/386 冷/热 prefix 边界矩阵，并对照 0.23 与 0.26 的 `patch_mamba_config`、`MambaManager`、`preprocess_mamba`、state copy/restore 和 cache 生命周期；当前仍保留多个竞争假设与未完成结果。
  title: 当前任务导航：v0.26.0rc 的 384→385 Mamba/Attention cache 边界回归
navigation: 此前的高 prefix-hit、重复波次和 128K 长上下文测试持续出现 hash 漂移、全 `!`、异常 acceptance 与 NaN；局部同步、全 Triton、phase isolation、state-copy barrier 及 zeroer 轮转都曾改善部分波次，但均被后续 c1 或混合批次反例否定，不能视为修复成功。最新最强线索是 v0.26.0rc 在 attention page size=384 时跨到第 385 个 token 的 Attention/Mamba page 边界语义或 state 对齐/恢复错误；不带 DSpark 也可疑似复现，而 A5 与 0.23 私仓未复现。下一步是完成纯 0.26.0rc、无 DSpark 的 383/384/385/386 冷/热 prefix 边界矩阵，并对照 0.23 与 0.26 的 `patch_mamba_config`、`MambaManager`、`preprocess_mamba`、state copy/restore 和 cache 生命周期；当前仍保留多个竞争假设与未完成结果。
schema: mindie-material-task/1
status: complete
task_id: 8db58966cd27b59d5d669539bf2a249b5b2b3023d511deb3ca4caffd4fde2c46
---

# 当前任务导航：v0.26.0rc 的 384→385 Mamba/Attention cache 边界回归

此前的高 prefix-hit、重复波次和 128K 长上下文测试持续出现 hash 漂移、全 `!`、异常 acceptance 与 NaN；局部同步、全 Triton、phase isolation、state-copy barrier 及 zeroer 轮转都曾改善部分波次，但均被后续 c1 或混合批次反例否定，不能视为修复成功。最新最强线索是 v0.26.0rc 在 attention page size=384 时跨到第 385 个 token 的 Attention/Mamba page 边界语义或 state 对齐/恢复错误；不带 DSpark 也可疑似复现，而 A5 与 0.23 私仓未复现。下一步是完成纯 0.26.0rc、无 DSpark 的 383/384/385/386 冷/热 prefix 边界矩阵，并对照 0.23 与 0.26 的 `patch_mamba_config`、`MambaManager`、`preprocess_mamba`、state copy/restore 和 cache 生命周期；当前仍保留多个竞争假设与未完成结果。

## Materials

- [定位 QuaRot 长上下文异常与初步根因](blocks/55e20bf40e96152930ad859ccaa62f8834ba3124be3e67b69e0ba2e6985bb6fe.md): 围绕 PR #13327、#13407 与指定 QuaRot 权重，完成四机环境、权重哈希、进程归属及组合分支核验；复现了 128K/1K 下 token 0/“!”、NaN logits 和 100% 假采信，且 DP0/DP1 一致。定位过程中排除了权重缺失、KDA 长度本身溢出及 draft 首因，初步发现 speculative conv state 的新增槽未初始化，并修复了 #13327 warmup 的 `BLOCK_VERIFY` 参数兼容问题。
- [修复 speculative state 尾槽并发现 prefix-cache 残余故障](blocks/a65a69bf696c57669f9bf46eada18e8789b73e997bdb12aed911e444b447a5c9.md): 同步探针会改变时序，不能作为修复证据；target-only 四 DP 的 128K 请求均正常，证明 QuaRot 权重和 target prefill 并非必然失效。单卡生产形状测试确认 speculative `state_len=10` 的新增 7 个槽未初始化，清零修复后无缓存路径通过，但 prefix hit 98.73% 时仍出现 `!`×1024 与 100% 采信；最终运行时清单、静态检查和针对性测试已通过。
- [追踪跨请求状态污染与缓存边界缺口](blocks/3b430ca752d199da20d94740b4be441b0568fbba86d8aea287a7cfd320bbf72d.md): target-only 在完整 128K/1K 请求后连续前缀复用仍正常，确认故障来自 speculative 跨请求状态生命周期，而非旋转量化权重。修复 accepted-token 异步回写后，主 128K/1K 请求恢复正常的 46.6667% 采信率，但相同 prompt 的第二次 warmup 仍返回 token 0/“!”，指向 exact-boundary recurrent state cache admission 缺口；当前正参考上游 PR #50457 做 v0.26 兼容的最小 backport，尚未完成验证。
- [r4 工厂路由修复后仍复现 warmup 异常](blocks/143d5afdea17b84e3112cdc8c55c4d380170080089245cf988717ced6c20beaa.md): 边界 admission 单测、r3/r4 部署校验及完整 22-test 回归最终通过；两次未加载 CANN `acl` 的测试收集失败被保留为环境配置问题。r4 四机 health=200，冷启动 128K/1K 与基线一致（`fd1aee0…`、采信 784/1680=46.67%、prefix hit 0），但两次 1-token warmup 仍输出 `<|close|>` 与 `!`，因此显式 `AscendMambaManager` 工厂路由并非完整根因。进一步实验显示 129792 边界缓存恢复与冷算首 token 都为 ` It`，异常集中在边界先缓存后继续运行的路径，当前怀疑 KDA 持久状态在后续续算/解码中被覆盖；任务仍未完成。
- [KDA 布局假设被否定，转向 DSpark/QuaRot 别名审计](blocks/4aa6356182ec6f99e2bf4963f3ac59a0f53b73c858c0042fe318b0d800b05da2.md): DP1 冷算及命中 130560 tokens 的复用均与冷算一致，但 canonical warmup 在 DP1、0 prefix hit 时也输出 `<|close|>`，否定了请求槽残留和 prefix-cache 恢复作为主要根因。3 槽、10 槽及 10 槽跨物理块 memcpy 的算子级 prefill 回归逐元素一致，因而 KDA 卷积和 chunk 状态迁移假设也未获支持。当前最强线索是启用 DSpark 后目标模型首个 prompt forward 已偏离，重点审计 `Qwen3DSparkModel` 的 materialized GQA auxiliary stream、QuaRot 对共享 `embedding/lm_head` 的反旋转，以及可能的 NPU storage 别名；尚无最终修复或验证成功。
- [r5–r7复现与物理块防护仍未解决并发污染](blocks/0d67150304056716fd731bd81e89b75ad21d0fb3cf9b9771088b9f375209e069.md): 已确认单请求及冷/热128K结果可稳定复现，但128K并发2在高prefix命中、partial-tail/COW路径仍出现一个请求全`!`或输出漂移；单行复用、批量memcpy和partial-hash防护均不足。诊断服务曾因单机EP压缩导致NPU OOM，四机拓扑可启动；物理block-ID guard及相关单测通过，但r7第四轮再次出现同命中条件下采信和输出变化，说明仍不能视为修复成功。r8部署前的清单校验、pre-commit及gitleaks检查也记录了shell误解析、平台架构不兼容和既有误报等环境限制。
- [r7成功启动但高命中并发复现新一轮漂移](blocks/9050fd2ca4abc3f738a879f86b4fdca33c2ebec62724622cb31a8459fd2ddccf.md): r7四机TP16×DP4×EP64启动、health=200、8K基线、128K冷/热单请求及CPU/NPU测试均通过；单请求在99.609375% prefix命中时仍保持`784/1680`和冷态输出一致。并发2首轮正常，但后续相同99.609375%命中条件下出现输出SHA漂移、采信由`29.0786%`变为`29.9978%`，其中一次以控制标记开头，表明物理block-ID防护只消除了部分竞态。进一步审计发现partial-tail alias和后续COW目标块生命周期仍可能绕过保护；新增COW/边界测试`23 passed`，随后生成r8并完成四机清单校验。
- [r8失败后转向manager注册与实际运行路径审计](blocks/bc6f92adf83b5e28f737eedcd52b0eb46dd50484f638e4ebfab1fff6fdaa5622.md): r8的健康检查、模型身份、8K基线及128K冷/热单请求均通过，但并发第二轮在99.609375%命中时再次发生输出漂移和`<|close|>`开头，采信为`1388/4627=29.9978%`；因此COW物理块guard和边界校验仍未解决根因。随后r9追踪复现了第三轮降至27.41%，而生命周期埋点始终未触发；审计确认Ascend manager可能在补丁生效前被vLLM manager registry缓存，显式路由也只覆盖部分路径。r9的一次健康复现不能证明修复，调试RPC还因不可序列化的`torch.dtype`使服务失去后续证据资格；r10则在launcher预检阶段退出，尚未启动。
- [r10–r12：Mamba 清零修复部分有效但未闭环](blocks/fd06df3eb6ec86c579b8411f6f0bf7c3e64737b24d0698f2c21869ea6f1f74af.md): 确认 registry 路由曾绕过修复 manager；补齐 Mamba/KDA 新 state block 记录、worker 清零和 DSpark zeroer 初始化后，单卡清零测试及 CPU 回归通过，r12 的 zero-plan 覆盖 112 个 Mamba state block 且 post-state 无 NaN。四机连续 128K 测试中首轮约 45.76% 采信率正常，但重复高命中轮仍降至约 29.4%–29.5%，并出现异常续写，说明漏清零是真问题但不是唯一根因。清空 prefix cache 后同一 131072→1024 请求又出现 100% 采信率但输出全为 `!`，问题进一步指向 prefill 或缓存状态边界，而非已验证解决。
- [r13：同步诊断指向 Attention/KDA 就绪竞态](blocks/869453e9d4d85ceb0c608554c9e12b720da413e36c7875ac07283ca6cf3e6e39.md): r13 插桩显示，原路径可在同一 128K×1 请求中出现 100% 采信和 1024 个 `!`，首个非有限 recurrent state 位于 KDA layer 9，但同输入并非每次都复现。加入 layer 9/10 边界同步后，fresh 与 99.609% prefix-hit repeat 均稳定为 46.67%，输出 hash 一致，形成跨 stream/collective 可见性竞态的强反事实证据；这不是最终修复。单卡生产形状测试中，AscendC 在未补齐控制变量时曾全为非有限，但加入 L2-normalize 后连续 20 次有限且逐 bit 一致；因此尚不能归因于长序列算子溢出，当前转向审计 QuaRot 共享权重及 attention stream 边界，并计划以最小同步点进行四机 A/B。
- [混合批次触发 KDA 状态污染](blocks/101de1ab9ab9d8ebb3f5dc2cbe224173119812e31963525f772c274b179d481c.md): 修正层号后，index 8 的 KDA 边界在 r14 真实 128K/1K 请求中可通过局部执行边界恢复稳定结果；全 Triton r15 的单请求 fresh、prefix repeat 和 reset-fresh 均稳定，但 128K×2 仍退化为全 `!`。无共享 prefix 的双请求也失败，进一步将触发条件收窄为 prefill 与 speculative decode 混合批次，而非单纯 prefix cache/CoW；后续独立算子控制通过，完整 vLLM 调度与状态迁移仍未解决。
- [phase isolation 后暴露 state-copy 竞态](blocks/e5ca4253b20954a243f775b0977cc997be134e75992c9927f64bc748a2e171e0.md): r16/r17 的 Kimi K3 + DSpark phase isolation 使两轮 128K×2 恢复正常，但之后 128K×1 仍出现 100% acceptance 和全 `!`，说明该修复不是充分条件。r18 的条件性 completion barrier 仍被反例否定：长 prefill 末段从 recurrent group 15–19 或 12–19 出现 NaN，提示 state block 迁移后的可见性、zeroing 或生命周期问题。此前关于单一双缓冲、全 Triton 或同步屏障的判断均只能解释部分现象，尚无可交付修复。
- [zeroer 轮转修复仍未闭环](blocks/9dc977599eb30f0075877b19c2100d3ea92cf9dabc51c290b09a2a6be301524a.md): r19 的两轮 128K×2 均通过，输出 hash 与正常样本一致、采信率约 60.3%、recurrent NaN 为 0；但随后相同顺序的 128K×1 在 `computed=122880` 再次出现 NaN 和全 `!`。因此 Ascend KV/Mamba block zeroer 复用异步 ID 缓冲虽是真实并发缺陷，按并发数轮转仍不足以解释根因；当前继续区分 layer 7 QuaRot Attention 输出异常与 layer 8 命中坏 `prev_state_idx`，r20 仅增加诊断探针，不能视为修复。
- [384→385 页边界成为首要嫌疑](blocks/b8efff81c3605db8f17d221e3abd1c1c8d86ad4df9987361bf95fb38c729a18e.md): 日志确认 v0.26 为匹配 Mamba page size 将 attention block size 固定为 384，因此 `384+1` 的乱码/异常很可能发生在 Attention block 与 Mamba state block 换页时。新线索显示不带 DSpark 也可复现、A5 与 0.23 私仓未复现，故 QuaRot 和 DSpark 更像放大器；上游 `8472b0161` 的边界缓存修复高度相关，但此前等价逻辑仍未充分解决长上下文问题。当前应以纯 `0.26.0rc`、无 DSpark 的 `383/384/385/386` 冷缓存与热 prefix 矩阵，对照 Mamba state 对齐、copy/restore 和 hybrid cache manager 逻辑；r20/基线尚未给出最终验证结果。

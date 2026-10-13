---
blocks:
- block_id: 83aa51870966b35140e0954d6a841ff03bddddd45d704628ee3268441cd09262
  indexed: true
  sha256: 47641f7889601f6c88002d10f1794f3af5b882d0f2029e509d8b6ea78d508923
  source_range:
    end: 9118634
    part: 0
    start: 0
  summary: PR 修复将单张量 V2 路径恢复为累积边界，并新增覆盖 MC2/非 MC2、count/cumsum 的回归测试；本地因缺少 `torch/torch_npu` 未能执行硬件单测，但语法、格式和远端 mock/CPU 测试通过。A2 关键分片部分通过，多个失败后来确认为 ModelScope 对 `Qwen/Qwen3-8B` 返回 403，A3 graph-mode 长期因四卡 runner 排队未启动，不能据此宣称全量硬件验证成功。
  title: 初始修复与 CI 外部阻塞
- block_id: e9bb5ee5b916264216a27b1852db36f1c85301309b27580b34cc120e44e11a4e
  indexed: true
  sha256: 67cc97e8d27e455c174d8646c33c917a1a556ba76c31798cf834c438728af63a
  source_range:
    end: 9118634
    part: 1
    start: 0
  summary: A2 token-dispatcher 分片在真实设备上通过，但 A3 多张量对照最初因旧的累计边界调用失败，改用生产契约的 count 模式后边界等价测试通过。远端四卡 graph-mode 先后受 Hugging Face 连接重置、vLLM 版本不匹配、缺少 `acl` 模块、HCCL 超时及端口 `16666` 冲突影响，未形成有效的完整模型结论。测试契约修正已推送，新一轮 cache 构建完成并展开 41 个 job，后续 A2/A3 硬件结果仍待验证。
  title: 远端 A3 排障与新 CI 展开
- block_id: 2a3159e3297042c8842ae0cb916d2b8afdfe65ec8cd3d1393ca1175f4c3a0f2e
  indexed: true
  sha256: 1dab02dd552bbb0c115864940091583a3a9d0da4f05e80b3c8456db8d84aa517
  source_range:
    end: 9118634
    part: 2
    start: 0
  summary: v0.27.1 A2 part 5 暴露 DeepSeek 多流 shared-expert eager 输出不一致，定位为单张量 packed-weight V2 路径的并发完成顺序问题；历史记录显示该问题自 V1→V2 迁移起存在。新增 A2 routed GMM1 完成屏障及测试夹具修正已推送，远端定向 UT 以 2/2 通过，最终 head 的 A3 count/cumsum 等价和 packed 单张量算子测试也通过。当前两套 A2 part 5 已进入真实设备测试但尚无最终结果，两套 A3 四卡仍排队，因此代码修复与全量 CI 成功均未最终确认。
  title: A2 多流屏障修复与待定硬件验证
- block_id: c2d83a2f444dd020d98d65ad9f80e5f9002248c766a47cdde83c38e6e67fbfee
  indexed: true
  sha256: 84de371ed13f2c205e507a1d6a8f35ebdef966a748705aa642fd693a46fa8c91
  source_range:
    end: 13695805
    part: 0
    start: 9118634
  summary: 旧 head 的 A2 part5 在多流输出差异上失败；先后确认共享流重叠、默认流修复后 ACLGraph replay 仍错，且 main 与 v0.27.1 均复现。A3 最小 NPUGraph 捕获/动态重放通过，范围收窄到 A2 910B 的打包 W8A8 TensorList/单 Tensor 调用形态；随后采用 torch-npu 已验证的 V2 绑定，保留 A3 及非零 SwiGLU limit 场景的自定义 V2。相关定向 UT、格式检查和 A3 捕获/重放验证被报告为通过，但新 CI 的完整硬件结果仍未完成。
  title: A2 ACLGraph 重放差异与 V2 绑定修正
- block_id: a0cff5a3185e77d984fb0f5be1d34a1cddc3a5dad1a9b39efdde41a2baf68ce9
  indexed: true
  sha256: e60667b96b55bf82cc3893871f8504e10856c29d130f92eaea9e4ae763e4d615
  source_range:
    end: 13695805
    part: 1
    start: 9118634
  summary: 提交 963205d71 后，A2 eager/multistream-eager 已相等，但 full-model ACLGraph replay 仍失败；两套日志一致，问题定位为 A2 图捕获下 V2 动态参数绑定。最终改为仅在 A2+W8A8+ACLGraph 捕获时使用拆分 kernel，eager 与 A3 继续走 V2；CPU mock 修复提交 5140a857f，分支回归、37 项关联 UT 中的新增测试、A3 实机 packed V2/TensorList 用例及格式检查被报告为通过，但已有 tiny-shape SwiGLUStep CPU/Triton fixture 仍失败。最新 E2E 已通过 coverage、pre-commit 和选例阶段，尚待 hitest/cache 后的 A2 part5 与 A3 四卡 graph-mode 正式结果。
  title: A2 图捕获 fallback 修复与新一轮 CI
- block_id: 18ff6550182ea62cfd6241d1234fdb9190ccfa963d39a5a4b31b799a68de385c
  indexed: true
  sha256: 2d63fc10b1a556c2e280fa83fcf2321ee396acb42b3265b10e83f20a7af16986
  source_range:
    end: 24357822
    part: 0
    start: 13695805
  summary: A2 shared-expert ACLGraph 曾重复出现输出不一致：先后定位到 `torch.compile` 时序固化、动态编译标志不可靠，随后将 A2 W8A8 静态固定到 replay-safe split kernel，并推送多个 PR head。用户进一步要求所有 `gmmswigluquant` 统一走仓库自定义 V2，助手确认存在 A2 特判、A5 `torch_npu` 旁路和 MoE 入口混用，开始收敛为统一封装并扩充单算子测试；此时真实统一后的完整 CI 结论尚未给出。
  title: A2 图捕获修复迭代与统一 V2 方向
- block_id: 3d8aeda7167ac05962324422586bfd087a7971a7ae9e0dec81620908bcfe99c1
  indexed: true
  sha256: d510312a9469a93e53e59a31d0da0823a0bd3cd486097f598cf615609613d77d
  source_range:
    end: 24357822
    part: 1
    start: 13695805
  summary: 围绕 A2/A3/A5 通用规格，列出了 A8W8、A8W4、A4W4、A5 per-token 与 MX 的布局、量化、group-list、scale 和图重放条件，并发现 Weight-NZ 桥错误地用 W4 物理形状重写 A8W8。A3 新增 9 项矩阵、真实 W4A8/W8A8 大规格和定向路由测试曾报告通过，A5 仅完成绑定/构建级接入，A5 实机未验证；同时发现并修复 A2 扁平 NZ storage 兼容、mypy 签名及线性 rebase/选测问题。后续 CI 曾暴露依赖 502、A3 四卡 DeepSeek ACLGraph 数值偏差等新问题，因此这些局部通过不能代表全量成功。
  title: 规格矩阵、V2 源码修复与 A2/A3 验证
- block_id: 8c4c532629bc3210afbb4865a61c9d85238beb1dce818325d0d5ccbf77006879
  indexed: true
  sha256: d6110a8dbd97a0ce16121886c39f30221b1d6c75b298f6c86fc09b7d391aee45
  source_range:
    end: 24357822
    part: 2
    start: 13695805
  summary: A2/A3 csrc 缓存均成功构建；A2 v0.27.1 part 5 的新增 GMM SwiGLU V2 文件报告为 9/9 通过，覆盖 W8A8/W4A8、单/多 Tensor、count/cumsum、graph replay、per-channel/per-group 与 clamp，但另一套 current-vLLM part 5 当时仍在运行。统一到自定义 V2 后，旧 CPU 测试关于 A2 W8A8 必须关闭多流的断言失效，改为 A2/A3 均启用多流，定点回归报告 13/13 通过，且两套 CPU 全量 UT 后续报告通过。随后 CI 仍出现 A3 四卡真实 DeepSeek-V2-Lite-W8A8 ACLGraph logprob 偏差、依赖镜像 502 等问题，不能将单算子结果推广为模型级验证。
  title: A2 官方单算子通过与后续 CPU 契约回归
- block_id: ad204522cc8fb16f6fb26a864e4e3051b8fcf0e415de3259b237215820bf9c0b
  indexed: true
  sha256: 5b8674d57854c0392f686f5956a8ed271db5b8f5bb5e08d0064d57e2a56fe577
  source_range:
    end: 24357822
    part: 3
    start: 13695805
  summary: 针对 A3 四卡 DeepSeek-V2-Lite-W8A8 ACLGraph 偏差，隔离复现先后被 Gloo hostname、缺失 CANN `acl` Python 路径、旧 vLLM 缺少 `_moe_C.topk_softmax`、MoE factory 导入时序以及临时算子包路径不一致阻塞，均未形成目标算子失败结论。切换到 PR 锁定的 vLLM `v0.27.1`、补齐 CANN 路径并调整隔离诊断副本后，测试曾越过权重加载，但最新一次仍未启动到 NPU；因此 A3 真实模型 ACLGraph 数值问题的根因和修复状态仍未确认。
  title: A3 四卡模型复现仍受环境阻塞
- block_id: 563c1a45a8444bea5c315bc5e1b9ff4cbb867589cb4c2f78736ce7f48c855bc0
  indexed: true
  sha256: 2cb5a3f6b62bec9b24121f8cd2c9773030d138f1fdccdb2deb7814dd96f95729
  source_range:
    end: 36103379
    part: 0
    start: 24357822
  summary: 真实四卡 eager 与 ACLGraph 均复现过 logprob 偏差，排除了单纯 graph replay 和部分环境因素；诊断先发现 V2 与旧 V1 在取整、乘法顺序、token scale 读取及 GM→UB 同步上的差异，后又确认 count/cumsum attr 不统一会触发同 shape 图状态复用。将 C++ V2 入口统一归一到 type=0 并修正 graph-safe workspace/scale 处理后，A3 单算子矩阵达到 16/16、随后 17/17，通过真实大规格 CPU golden 与真实 DeepSeek 双图逐位对照；四卡固定 golden 仍有约 0.1468 偏差，不能作为 V2 失败的单独证明。
  title: A3 V2 数值与 ACLGraph 生命周期诊断
- block_id: 6ecde51b804b172356dc28403eb77b66e53b24e763b367e8fb89996dcaf8ae21
  indexed: true
  sha256: 1be08acb794eb233f104dacf511dfd8a9f94d2c32062ad0c34abaf23673a54de
  source_range:
    end: 36103379
    part: 1
    start: 24357822
  summary: 修复提交 `51c513e23` 推送后，PR 重放到最新 main，保留主干 shared-expert 并行逻辑并移除过时 workaround；重放暴露的 `moe_config.tp_group` 和 Triton 激活隔离问题属于测试夹具适配，45 个定向 CPU 回归通过。A2/A3/310P/CPU 选测随后展开，A3 首轮 cache builder 因自动加载 `torch_npu` 导致 Triton 无 driver 而失败，但 A3 NPU fallback 能现场编译并开始测试；设置 `TORCH_DEVICE_BACKEND_AUTOLOAD=0` 的环境补丁已验证并推送，旧 run 因并发策略被收掉前未出现新的代码类失败。
  title: 修复提交后的 rebase、测试与 CI 构建
- block_id: e1875b98dc8c5e83a66459dee024cef77fbc2980814ab4ebb026fb75be24b18d
  indexed: true
  sha256: c3a7a0f7a60ea9217907ab4f3ac47fc4c5cb209583bcac9d32fe32cef6c2fd2d
  source_range:
    end: 36103379
    part: 2
    start: 24357822
  summary: 最终 head 的 A3 缓存构建已越过此前 `Install vllm-ascend` 失败点，产物校验和缓存保存均通过，确认禁用 `torch_npu/triton` 自动加载的 CI 修复有效；选测矩阵展开为 32 个分片，起始时为 0 失败。A2 两个关键 `part 3/5` 分片已进入真实 NPU 测试并包含新增单算子文件，整体曾达到 11 项成功、0 失败；A3 硬件分片仍在等待 runner，因此 A2 新增用例和整轮 CI 尚未给出最终完成结论。
  title: 最终 CI 的 A3 缓存修复与 A2 测试进行中
- block_id: 5f10b6c5c0a9ffbef2b78d76760b6eef13cb6f5b09ca6e9ac6143e86fb12cee3
  indexed: true
  sha256: 12e9fda143cd1190d629287136a5825ed6a8ca5175ed0f4d2aacd427060efc6e
  source_range:
    end: 43647210
    part: 0
    start: 36103379
  summary: A2 两套版本的新增单算子此前均报告真实 NPU 17/17 通过，A3 单算子随后在修复后报告 18/18 通过；但这些结果不等同于模型级成功。调查先后排除了 group-list、尾部 scale 未初始化及普通 eager/直接图误差，最终定位到真实约 25K token 规格下 V2 与旧兼容算子的 int8 舍入差异：逐元素除 scale 与标量求倒数后逐行相乘的运算顺序不同，修复后长序列单算子逐元素一致。四卡修复后输出回到主干/旧算子结果，但主干在该 A3 机器上也会被固定 golden 拒绝，因此 PR 根因仍需结合 CI；新 head 为 `7d5d5097f`，本地检查大体通过，gitleaks 因缺少 `wget` 未执行。
  title: 修复提交与长序列量化差异
- block_id: e44e7cfcedb93f0f9c70abc064716d05aef28a5982d5401b130f160745996843
  indexed: true
  sha256: 219f0b6570b00485adb8b3975a936271208c46ab76e2d307ca4bb405fc070186
  source_range:
    end: 43647210
    part: 1
    start: 36103379
  summary: 新 head 的预提交、select-tests 及 A2/A3/310P 自定义算子缓存构建均报告通过，且选测日志确认新增 `test_grouped_matmul_swiglu_quant_v2.py` 与此前失败的 A3 四卡 `test_graph_mode.py` 被纳入。主线与 `v0.27.1` 的 A2 关键分片后来分别确认新增单算子文件为 18 passed，并包含 24,972-token 真实容量回归；这只确认 A2 目标，不代表全矩阵完成。A3 两个关键四卡分片长时间保持运行、没有中间 failure，但在本块结束时尚未给出完成态，已接近或达到 120 分钟硬超时边界，不能据此宣称通过。
  title: 新 head 的缓存与 A2/A3 测试展开
- block_id: 75002cbfc2a0700c8566240d5ff7f3b3afccc92524dc8f910a9e54beefb5cc99
  indexed: true
  sha256: 0adaa6afcdbd8059e93777c80dedf69ab27585d1555627e4990586e5183a1c95
  source_range:
    end: 43647210
    part: 2
    start: 36103379
  summary: 两个关键 A3 四卡 job 最终在 120 分钟边界被 GitHub 取消；后续日志与全矩阵状态报告 14 个 A3 分片均沿同一路径超时，关键四卡任务卡在首个无关模型初始化阶段，未运行到 `test_graph_mode.py`，因此当前证据指向 A3 runner/集群系统性异常，而非 pytest 断言失败，但仍未验证 A3 代码结果。A2 两个版本保持通过，原生失败集合重跑 API 因 fork 缺少上游 Actions 管理权限而被拒绝；随后通过 PR `/rerun` 仅部分重启，并改用 `/e2e tests/e2e/pull_request/four_card/test_graph_mode.py` 定向触发运行 `31956523094`。定向运行的 A3 cache 查询起初被更正为 miss，需重新编译；截至块末缓存仍在正常构建，两个精确 graph-mode job 尚未启动。
  title: A3 全矩阵超时及定向重跑
- block_id: 7a8ed4b6c330ad3ce9a8d5fe845ae0caa58efe9803896d8b4e9a1768bc190bcc
  indexed: true
  sha256: 75b8b4c743cdf61c5f008150038aab46d91be632fb0ff0d849604335ef0206f2
  source_range:
    end: 49338441
    part: 0
    start: 43647210
  summary: A3 csrc 冷构建和产物校验成功后，两个精确四卡 graph-mode job 经长时间运行最终在约 120 分钟被取消；日志显示 cur_case0 通过，但 cur_case1 在模型初始化、权重读取前发生 worker 超时/HCCL 通信失败，cur_case2 又因残留 HCCL 监听端口冲突级联失败，清理阶段继续阻塞。未出现量化数值断言失败；该结果支持 A3 runner、存储或分布式环境故障的判断，但不构成模型级测试成功。
  title: 定向 A3 四卡测试确认权重前分布式阻塞
- block_id: ec22ede68c1b242d1f10a178db5ee3f02457b4d8d3e26d2b450a03da8720d062
  indexed: true
  sha256: e6ce2598046f0523ad1a6ebfe9958a2aeeb2f500f7917b4832845381097f303f
  source_range:
    end: 49338441
    part: 1
    start: 43647210
  summary: 确认 vllm_ascend/utils.py 的 47 行 A5 专属 custom-op 绕过逻辑与 A2/A3 无关，已删除并让 A5 恢复 torch_npu 路径；首次隔离测试因遗漏 tests/ut/conftest.py 未执行有效 UT，随后发现 CPU stub 缺少 V2 属性导致两个新增 A5 路由单测失败。通过 mock 的 create=True 修复后，签署提交 c4a8dd752 推送；匹配版本 CPU 全量分片报告 2404 passed、22 skipped，A2 10/10 也随后通过，但 A3 仍全部排队，尚未得到绿色结论。
  title: 移除无必要 utils 特例并修复 CPU mock 缺陷
- block_id: 0dc14ae20d9ff96720cd83e6bdf1dd6ff98106c75d339d31560ce5eb6bea695c
  indexed: true
  sha256: 417d583fe51d7abe52da9650094c103b38ac019c0ddea18f3d221532060aeb8a
  source_range:
    end: 49338441
    part: 2
    start: 43647210
  summary: 在多次 heartbeat 检查中，live head 未变化，CPU、A2、310P、upstream 及门禁均报告通过；A2 最终为 10/10，其他平台也完成。14 个 A3 分片持续处于 queued、尚未获得 runner，没有新的失败日志或可执行代码修复；排队状态仍只是 pending，不能视为 A3 测试通过或失败。
  title: 非 A3 检查全绿，A3 持续等待 runner
- block_id: 27a9ebb630d051e4ec56a2ae7c1d70a98991954fafe6b01a44747746fc764d9a
  indexed: true
  sha256: f6bdeaf2988e347665b28e8c132c36e77d18589775f59db9b0a6d95a564b5f9a
  source_range:
    end: 49338441
    part: 3
    start: 43647210
  summary: 监控记录显示 latest head 与本地一致，非 A3 检查曾保持 23 项成功、2 项预期跳过；A3 的 14 个分片从全数排队逐步获得 runner，随后报告 11/14 通过、3 个运行中，期间未报告失败。后续讨论对 gmmswigluquantv2 的改动进行了复核：删除旧实现规模较大，但 A2/A3 的 NZ shape、group-list 和量化一致性改动被认为有必要；A5/ND/MX 扩展被认为可能过度。随后更正确认 V2 原生支持 group_list_type=0/1，adapter 中的 count→cumsum 可能是重复且造成假覆盖；计划移除隐式转换并清理相关测试与 A5 分支，但这些改动在该材料中尚未给出完成结果。
  title: 'PR #12894 A3 分片逐步调度'
- block_id: 9c2ca1a8ed0cfb3e90adab5f72b01dea2426f77b800f33dc7499925306253b26
  indexed: true
  sha256: 157953648caccc605799d8356c9a4b10ecd1f91a23e92e48fca97a9d5e2101da
  source_range:
    end: 49338441
    part: 4
    start: 43647210
  summary: '监控最终报告 PR #12894 最新 head 的 A2/A3（含 14 个 A3 分片）、CPU、310P、upstream 和质量门禁均通过，并称监控已停止；这是来源中的报告，不能替代独立验证。随后用户指出 group_list_type=1 的语义问题，回复更正为 V2 内核原生支持 type 0/1，并承认 Python/C++ adapter 及部分 moe_mlp 可能存在重复 count→cumsum 转换，现有 type 1 测试可能只是“假覆盖”。用户同意按端到端原样传递语义，计划移除隐式转换、清理 A5 自定义分支并补充契约测试；材料只记录了复核和计划，未记录实施或验证完成。'
  title: 'PR #12894 报告全绿及语义修正计划'
- block_id: 32a4f42bbfc0b72d0ec7da789b13946af4fc17c8188201169fa96d79e4b0102b
  indexed: true
  sha256: ac61eaa44c5055de166189429e191bcaaec6d92e953aab8a18f5f5cba95d241e
  source_range:
    end: 50235285
    part: 0
    start: 49338441
  summary: 代码已删除 Python/C++ 层的 count→cumsum 隐式转换，保留生产调用所需的 `dequant_mode`，并报告 A3 单算子矩阵 16/16、CPU 契约 UT 43/43 通过；其中一次 type=0 replay 失败后来被判断为同一 pytest 进程中的 NPUGraph 资源干扰，未保留同步 teardown 改动。签名提交 `659448572` 已推送，另以 `d1f73c35a` 重触发 CI，但连续两轮 CPU runner 分别因 `Disk quota exceeded`、0 字节 action 包和 `gzip EOF` 在 checkout/测试前失败；A2 真实硬件测试未执行，因此不能宣称 A2 或 PR checks 已验证通过。
  title: group-list 原样透传已提交，但 A2 CI 受基础设施阻塞
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7513176008ab16b1ba604e0f2b9c1f6be9bb982980219685b2cd267582821d2d
  kind: experience
  material_digest: 355a4c149faa4fb234577d1eb0a0fb130284b42afb2980c26aa45bfae08b5021
  revision: c2f2a02af3fe48c684b62953f5a5d00bd9d14a77971c18aee6d39246b13881b0
  schema: mindie-entry/3
  summary: 新增材料报告 V2 已在 adaptor/C++ 中原样透传 `group_list_type=0/1`，A3 新扩展矩阵 16/16、CPU 契约 43/43 通过，且保留了 type=0 replay 顺序干扰这一失败与修正边界；这些是来源中的测试报告，不等同于所有环境验证。PR 已有提交并重触发 CI，但连续两轮 runner 在 checkout 前因磁盘配额、action 下载 0 字节和 `gzip EOF` 失败，ci-gate 也未执行代码测试；此前“全部 required checks 通过”的监控声明与这些后续基础设施失败并列保留，A2 及最终 CI 状态仍未完成验证。
  title: 当前任务导航：group-list 修正已本地/ A3 验证并提交，A2 CI 仍未执行
navigation: 新增材料报告 V2 已在 adaptor/C++ 中原样透传 `group_list_type=0/1`，A3 新扩展矩阵 16/16、CPU 契约 43/43 通过，且保留了 type=0 replay 顺序干扰这一失败与修正边界；这些是来源中的测试报告，不等同于所有环境验证。PR 已有提交并重触发 CI，但连续两轮 runner 在 checkout 前因磁盘配额、action 下载 0 字节和 `gzip EOF` 失败，ci-gate 也未执行代码测试；此前“全部 required checks 通过”的监控声明与这些后续基础设施失败并列保留，A2 及最终 CI 状态仍未完成验证。
schema: mindie-material-task/1
status: complete
task_id: 7513176008ab16b1ba604e0f2b9c1f6be9bb982980219685b2cd267582821d2d
---

# 当前任务导航：group-list 修正已本地/ A3 验证并提交，A2 CI 仍未执行

新增材料报告 V2 已在 adaptor/C++ 中原样透传 `group_list_type=0/1`，A3 新扩展矩阵 16/16、CPU 契约 43/43 通过，且保留了 type=0 replay 顺序干扰这一失败与修正边界；这些是来源中的测试报告，不等同于所有环境验证。PR 已有提交并重触发 CI，但连续两轮 runner 在 checkout 前因磁盘配额、action 下载 0 字节和 `gzip EOF` 失败，ci-gate 也未执行代码测试；此前“全部 required checks 通过”的监控声明与这些后续基础设施失败并列保留，A2 及最终 CI 状态仍未完成验证。

## Materials

- [初始修复与 CI 外部阻塞](blocks/83aa51870966b35140e0954d6a841ff03bddddd45d704628ee3268441cd09262.md): PR 修复将单张量 V2 路径恢复为累积边界，并新增覆盖 MC2/非 MC2、count/cumsum 的回归测试；本地因缺少 `torch/torch_npu` 未能执行硬件单测，但语法、格式和远端 mock/CPU 测试通过。A2 关键分片部分通过，多个失败后来确认为 ModelScope 对 `Qwen/Qwen3-8B` 返回 403，A3 graph-mode 长期因四卡 runner 排队未启动，不能据此宣称全量硬件验证成功。
- [远端 A3 排障与新 CI 展开](blocks/e9bb5ee5b916264216a27b1852db36f1c85301309b27580b34cc120e44e11a4e.md): A2 token-dispatcher 分片在真实设备上通过，但 A3 多张量对照最初因旧的累计边界调用失败，改用生产契约的 count 模式后边界等价测试通过。远端四卡 graph-mode 先后受 Hugging Face 连接重置、vLLM 版本不匹配、缺少 `acl` 模块、HCCL 超时及端口 `16666` 冲突影响，未形成有效的完整模型结论。测试契约修正已推送，新一轮 cache 构建完成并展开 41 个 job，后续 A2/A3 硬件结果仍待验证。
- [A2 多流屏障修复与待定硬件验证](blocks/2a3159e3297042c8842ae0cb916d2b8afdfe65ec8cd3d1393ca1175f4c3a0f2e.md): v0.27.1 A2 part 5 暴露 DeepSeek 多流 shared-expert eager 输出不一致，定位为单张量 packed-weight V2 路径的并发完成顺序问题；历史记录显示该问题自 V1→V2 迁移起存在。新增 A2 routed GMM1 完成屏障及测试夹具修正已推送，远端定向 UT 以 2/2 通过，最终 head 的 A3 count/cumsum 等价和 packed 单张量算子测试也通过。当前两套 A2 part 5 已进入真实设备测试但尚无最终结果，两套 A3 四卡仍排队，因此代码修复与全量 CI 成功均未最终确认。
- [A2 ACLGraph 重放差异与 V2 绑定修正](blocks/c2d83a2f444dd020d98d65ad9f80e5f9002248c766a47cdde83c38e6e67fbfee.md): 旧 head 的 A2 part5 在多流输出差异上失败；先后确认共享流重叠、默认流修复后 ACLGraph replay 仍错，且 main 与 v0.27.1 均复现。A3 最小 NPUGraph 捕获/动态重放通过，范围收窄到 A2 910B 的打包 W8A8 TensorList/单 Tensor 调用形态；随后采用 torch-npu 已验证的 V2 绑定，保留 A3 及非零 SwiGLU limit 场景的自定义 V2。相关定向 UT、格式检查和 A3 捕获/重放验证被报告为通过，但新 CI 的完整硬件结果仍未完成。
- [A2 图捕获 fallback 修复与新一轮 CI](blocks/a0cff5a3185e77d984fb0f5be1d34a1cddc3a5dad1a9b39efdde41a2baf68ce9.md): 提交 963205d71 后，A2 eager/multistream-eager 已相等，但 full-model ACLGraph replay 仍失败；两套日志一致，问题定位为 A2 图捕获下 V2 动态参数绑定。最终改为仅在 A2+W8A8+ACLGraph 捕获时使用拆分 kernel，eager 与 A3 继续走 V2；CPU mock 修复提交 5140a857f，分支回归、37 项关联 UT 中的新增测试、A3 实机 packed V2/TensorList 用例及格式检查被报告为通过，但已有 tiny-shape SwiGLUStep CPU/Triton fixture 仍失败。最新 E2E 已通过 coverage、pre-commit 和选例阶段，尚待 hitest/cache 后的 A2 part5 与 A3 四卡 graph-mode 正式结果。
- [A2 图捕获修复迭代与统一 V2 方向](blocks/18ff6550182ea62cfd6241d1234fdb9190ccfa963d39a5a4b31b799a68de385c.md): A2 shared-expert ACLGraph 曾重复出现输出不一致：先后定位到 `torch.compile` 时序固化、动态编译标志不可靠，随后将 A2 W8A8 静态固定到 replay-safe split kernel，并推送多个 PR head。用户进一步要求所有 `gmmswigluquant` 统一走仓库自定义 V2，助手确认存在 A2 特判、A5 `torch_npu` 旁路和 MoE 入口混用，开始收敛为统一封装并扩充单算子测试；此时真实统一后的完整 CI 结论尚未给出。
- [规格矩阵、V2 源码修复与 A2/A3 验证](blocks/3d8aeda7167ac05962324422586bfd087a7971a7ae9e0dec81620908bcfe99c1.md): 围绕 A2/A3/A5 通用规格，列出了 A8W8、A8W4、A4W4、A5 per-token 与 MX 的布局、量化、group-list、scale 和图重放条件，并发现 Weight-NZ 桥错误地用 W4 物理形状重写 A8W8。A3 新增 9 项矩阵、真实 W4A8/W8A8 大规格和定向路由测试曾报告通过，A5 仅完成绑定/构建级接入，A5 实机未验证；同时发现并修复 A2 扁平 NZ storage 兼容、mypy 签名及线性 rebase/选测问题。后续 CI 曾暴露依赖 502、A3 四卡 DeepSeek ACLGraph 数值偏差等新问题，因此这些局部通过不能代表全量成功。
- [A2 官方单算子通过与后续 CPU 契约回归](blocks/8c4c532629bc3210afbb4865a61c9d85238beb1dce818325d0d5ccbf77006879.md): A2/A3 csrc 缓存均成功构建；A2 v0.27.1 part 5 的新增 GMM SwiGLU V2 文件报告为 9/9 通过，覆盖 W8A8/W4A8、单/多 Tensor、count/cumsum、graph replay、per-channel/per-group 与 clamp，但另一套 current-vLLM part 5 当时仍在运行。统一到自定义 V2 后，旧 CPU 测试关于 A2 W8A8 必须关闭多流的断言失效，改为 A2/A3 均启用多流，定点回归报告 13/13 通过，且两套 CPU 全量 UT 后续报告通过。随后 CI 仍出现 A3 四卡真实 DeepSeek-V2-Lite-W8A8 ACLGraph logprob 偏差、依赖镜像 502 等问题，不能将单算子结果推广为模型级验证。
- [A3 四卡模型复现仍受环境阻塞](blocks/ad204522cc8fb16f6fb26a864e4e3051b8fcf0e415de3259b237215820bf9c0b.md): 针对 A3 四卡 DeepSeek-V2-Lite-W8A8 ACLGraph 偏差，隔离复现先后被 Gloo hostname、缺失 CANN `acl` Python 路径、旧 vLLM 缺少 `_moe_C.topk_softmax`、MoE factory 导入时序以及临时算子包路径不一致阻塞，均未形成目标算子失败结论。切换到 PR 锁定的 vLLM `v0.27.1`、补齐 CANN 路径并调整隔离诊断副本后，测试曾越过权重加载，但最新一次仍未启动到 NPU；因此 A3 真实模型 ACLGraph 数值问题的根因和修复状态仍未确认。
- [A3 V2 数值与 ACLGraph 生命周期诊断](blocks/563c1a45a8444bea5c315bc5e1b9ff4cbb867589cb4c2f78736ce7f48c855bc0.md): 真实四卡 eager 与 ACLGraph 均复现过 logprob 偏差，排除了单纯 graph replay 和部分环境因素；诊断先发现 V2 与旧 V1 在取整、乘法顺序、token scale 读取及 GM→UB 同步上的差异，后又确认 count/cumsum attr 不统一会触发同 shape 图状态复用。将 C++ V2 入口统一归一到 type=0 并修正 graph-safe workspace/scale 处理后，A3 单算子矩阵达到 16/16、随后 17/17，通过真实大规格 CPU golden 与真实 DeepSeek 双图逐位对照；四卡固定 golden 仍有约 0.1468 偏差，不能作为 V2 失败的单独证明。
- [修复提交后的 rebase、测试与 CI 构建](blocks/6ecde51b804b172356dc28403eb77b66e53b24e763b367e8fb89996dcaf8ae21.md): 修复提交 `51c513e23` 推送后，PR 重放到最新 main，保留主干 shared-expert 并行逻辑并移除过时 workaround；重放暴露的 `moe_config.tp_group` 和 Triton 激活隔离问题属于测试夹具适配，45 个定向 CPU 回归通过。A2/A3/310P/CPU 选测随后展开，A3 首轮 cache builder 因自动加载 `torch_npu` 导致 Triton 无 driver 而失败，但 A3 NPU fallback 能现场编译并开始测试；设置 `TORCH_DEVICE_BACKEND_AUTOLOAD=0` 的环境补丁已验证并推送，旧 run 因并发策略被收掉前未出现新的代码类失败。
- [最终 CI 的 A3 缓存修复与 A2 测试进行中](blocks/e1875b98dc8c5e83a66459dee024cef77fbc2980814ab4ebb026fb75be24b18d.md): 最终 head 的 A3 缓存构建已越过此前 `Install vllm-ascend` 失败点，产物校验和缓存保存均通过，确认禁用 `torch_npu/triton` 自动加载的 CI 修复有效；选测矩阵展开为 32 个分片，起始时为 0 失败。A2 两个关键 `part 3/5` 分片已进入真实 NPU 测试并包含新增单算子文件，整体曾达到 11 项成功、0 失败；A3 硬件分片仍在等待 runner，因此 A2 新增用例和整轮 CI 尚未给出最终完成结论。
- [修复提交与长序列量化差异](blocks/5f10b6c5c0a9ffbef2b78d76760b6eef13cb6f5b09ca6e9ac6143e86fb12cee3.md): A2 两套版本的新增单算子此前均报告真实 NPU 17/17 通过，A3 单算子随后在修复后报告 18/18 通过；但这些结果不等同于模型级成功。调查先后排除了 group-list、尾部 scale 未初始化及普通 eager/直接图误差，最终定位到真实约 25K token 规格下 V2 与旧兼容算子的 int8 舍入差异：逐元素除 scale 与标量求倒数后逐行相乘的运算顺序不同，修复后长序列单算子逐元素一致。四卡修复后输出回到主干/旧算子结果，但主干在该 A3 机器上也会被固定 golden 拒绝，因此 PR 根因仍需结合 CI；新 head 为 `7d5d5097f`，本地检查大体通过，gitleaks 因缺少 `wget` 未执行。
- [新 head 的缓存与 A2/A3 测试展开](blocks/e44e7cfcedb93f0f9c70abc064716d05aef28a5982d5401b130f160745996843.md): 新 head 的预提交、select-tests 及 A2/A3/310P 自定义算子缓存构建均报告通过，且选测日志确认新增 `test_grouped_matmul_swiglu_quant_v2.py` 与此前失败的 A3 四卡 `test_graph_mode.py` 被纳入。主线与 `v0.27.1` 的 A2 关键分片后来分别确认新增单算子文件为 18 passed，并包含 24,972-token 真实容量回归；这只确认 A2 目标，不代表全矩阵完成。A3 两个关键四卡分片长时间保持运行、没有中间 failure，但在本块结束时尚未给出完成态，已接近或达到 120 分钟硬超时边界，不能据此宣称通过。
- [A3 全矩阵超时及定向重跑](blocks/75002cbfc2a0700c8566240d5ff7f3b3afccc92524dc8f910a9e54beefb5cc99.md): 两个关键 A3 四卡 job 最终在 120 分钟边界被 GitHub 取消；后续日志与全矩阵状态报告 14 个 A3 分片均沿同一路径超时，关键四卡任务卡在首个无关模型初始化阶段，未运行到 `test_graph_mode.py`，因此当前证据指向 A3 runner/集群系统性异常，而非 pytest 断言失败，但仍未验证 A3 代码结果。A2 两个版本保持通过，原生失败集合重跑 API 因 fork 缺少上游 Actions 管理权限而被拒绝；随后通过 PR `/rerun` 仅部分重启，并改用 `/e2e tests/e2e/pull_request/four_card/test_graph_mode.py` 定向触发运行 `31956523094`。定向运行的 A3 cache 查询起初被更正为 miss，需重新编译；截至块末缓存仍在正常构建，两个精确 graph-mode job 尚未启动。
- [定向 A3 四卡测试确认权重前分布式阻塞](blocks/7a8ed4b6c330ad3ce9a8d5fe845ae0caa58efe9803896d8b4e9a1768bc190bcc.md): A3 csrc 冷构建和产物校验成功后，两个精确四卡 graph-mode job 经长时间运行最终在约 120 分钟被取消；日志显示 cur_case0 通过，但 cur_case1 在模型初始化、权重读取前发生 worker 超时/HCCL 通信失败，cur_case2 又因残留 HCCL 监听端口冲突级联失败，清理阶段继续阻塞。未出现量化数值断言失败；该结果支持 A3 runner、存储或分布式环境故障的判断，但不构成模型级测试成功。
- [移除无必要 utils 特例并修复 CPU mock 缺陷](blocks/ec22ede68c1b242d1f10a178db5ee3f02457b4d8d3e26d2b450a03da8720d062.md): 确认 vllm_ascend/utils.py 的 47 行 A5 专属 custom-op 绕过逻辑与 A2/A3 无关，已删除并让 A5 恢复 torch_npu 路径；首次隔离测试因遗漏 tests/ut/conftest.py 未执行有效 UT，随后发现 CPU stub 缺少 V2 属性导致两个新增 A5 路由单测失败。通过 mock 的 create=True 修复后，签署提交 c4a8dd752 推送；匹配版本 CPU 全量分片报告 2404 passed、22 skipped，A2 10/10 也随后通过，但 A3 仍全部排队，尚未得到绿色结论。
- [非 A3 检查全绿，A3 持续等待 runner](blocks/0dc14ae20d9ff96720cd83e6bdf1dd6ff98106c75d339d31560ce5eb6bea695c.md): 在多次 heartbeat 检查中，live head 未变化，CPU、A2、310P、upstream 及门禁均报告通过；A2 最终为 10/10，其他平台也完成。14 个 A3 分片持续处于 queued、尚未获得 runner，没有新的失败日志或可执行代码修复；排队状态仍只是 pending，不能视为 A3 测试通过或失败。
- [PR #12894 A3 分片逐步调度](blocks/27a9ebb630d051e4ec56a2ae7c1d70a98991954fafe6b01a44747746fc764d9a.md): 监控记录显示 latest head 与本地一致，非 A3 检查曾保持 23 项成功、2 项预期跳过；A3 的 14 个分片从全数排队逐步获得 runner，随后报告 11/14 通过、3 个运行中，期间未报告失败。后续讨论对 gmmswigluquantv2 的改动进行了复核：删除旧实现规模较大，但 A2/A3 的 NZ shape、group-list 和量化一致性改动被认为有必要；A5/ND/MX 扩展被认为可能过度。随后更正确认 V2 原生支持 group_list_type=0/1，adapter 中的 count→cumsum 可能是重复且造成假覆盖；计划移除隐式转换并清理相关测试与 A5 分支，但这些改动在该材料中尚未给出完成结果。
- [PR #12894 报告全绿及语义修正计划](blocks/9c2ca1a8ed0cfb3e90adab5f72b01dea2426f77b800f33dc7499925306253b26.md): 监控最终报告 PR #12894 最新 head 的 A2/A3（含 14 个 A3 分片）、CPU、310P、upstream 和质量门禁均通过，并称监控已停止；这是来源中的报告，不能替代独立验证。随后用户指出 group_list_type=1 的语义问题，回复更正为 V2 内核原生支持 type 0/1，并承认 Python/C++ adapter 及部分 moe_mlp 可能存在重复 count→cumsum 转换，现有 type 1 测试可能只是“假覆盖”。用户同意按端到端原样传递语义，计划移除隐式转换、清理 A5 自定义分支并补充契约测试；材料只记录了复核和计划，未记录实施或验证完成。
- [group-list 原样透传已提交，但 A2 CI 受基础设施阻塞](blocks/32a4f42bbfc0b72d0ec7da789b13946af4fc17c8188201169fa96d79e4b0102b.md): 代码已删除 Python/C++ 层的 count→cumsum 隐式转换，保留生产调用所需的 `dequant_mode`，并报告 A3 单算子矩阵 16/16、CPU 契约 UT 43/43 通过；其中一次 type=0 replay 失败后来被判断为同一 pytest 进程中的 NPUGraph 资源干扰，未保留同步 teardown 改动。签名提交 `659448572` 已推送，另以 `d1f73c35a` 重触发 CI，但连续两轮 CPU runner 分别因 `Disk quota exceeded`、0 字节 action 包和 `gzip EOF` 在 checkout/测试前失败；A2 真实硬件测试未执行，因此不能宣称 A2 或 PR checks 已验证通过。

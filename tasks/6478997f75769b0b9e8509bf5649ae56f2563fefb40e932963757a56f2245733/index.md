---
blocks:
- block_id: 694f6240bdddf50b4d45782f3e8aa32e3e74e610beda456da16a9984acd68f51
  indexed: true
  sha256: 1c41da72834ff0df361140cadd3c814e275ce7ff31413b7efdc30e6bb02a2322
  source_range:
    end: 14634047
    part: 0
    start: 0
  summary: 围绕将 `maoxx241:releases/v0.23.0_new` 准备为面向 `vllm-project/vllm-ascend:main` 的 Kimi K3 PR 分支，记录了从整线迁移、删除 `.github`/过程性 UT，到最终从 upstream main 逐项移植 K3 生产代码的多次修正。最终报告称分支以 `522c0f6d7` 为基线、提交为 `71d9d73c9`，移除了 0.23 专属实现和主线已删除 API 的调用；但完整 pytest 与 NPU 验证因本机缺少环境未执行，且这些内容属于此前操作报告而非独立验证。
  title: Kimi K3 分支重建与清理记录
- block_id: 8452a7e76aebde82c90d87be6dc488a57ba6bb18b3020065ea3899a236c50488
  indexed: true
  sha256: 8d69080fdb64f78368c4edba6c024a39a62a130cd49ef3eeaa4cdd99b29c4b72
  source_range:
    end: 14634047
    part: 1
    start: 0
  summary: 用户要求基于 Kimi K3 分支重构 Python 组网和特性注册，明确不修改 C++/AscendC 算子；只读审计识别出全局 monkey patch、`REGISTERED_ASCEND_OPS` 补丁式注册、SiTU 参数污染 upstream `FusedMoE`、动态 MLA 属性和多处防御性兼容。报告指出 parser 漏传 `is_harmony`/`mm_token_counts`、K3 MoE 真实 factory 契约不成立，并建议改用显式 runner、typed contract、局部组网和统一注册入口；后续工作报告称部分重构已完成，但相关 UT 因缺少 `torch`/`torch_npu`/`vllm` 未实际执行。
  title: Python 层架构审计与重构范围
- block_id: 0088f84d725e4f500f25e0396ea11ec800ef13e2f45f9c79d4685914ed0c50ec
  indexed: true
  sha256: f060abaa9e15f1cbfb714ab5f14149ce5c6b06567978c87d24d8332bd9489918
  source_range:
    end: 14634047
    part: 2
    start: 0
  summary: 审计进一步列出应删除的 serving/parser monkey patch、静默 fail-soft、防御性 `getattr/hasattr`、运行期 ABI 探测和会被 skip 掩盖的 NPU 测试，并区分必须保留的输入、权重、协议和 ABI 边界校验。后续报告称已完成 `AscendMoERunner`/SiTU ABI adapter、K3 直接构造 KDA、typed MLA contract，以及将 K3 serving/parser/renderer/XTML 移至 `vllm_ascend/serving/`，删除大部分 OpenAI serving 私有方法补丁。仍有 `OnlineRenderer.render_chat` 的窄 adapter 和 `AscendMLAMetadataBuilder` 中的 `getattr(..., "mla_use_rope", True)` 遗留；静态检查据报通过，但 UT 仍未因缺少 `torch` 而运行。
  title: Serving、MLA 与 SiTU 重构结果及遗留项
- block_id: c26a193e19121a371128df423c54d1d2330d8c026aae1b077be7190b554632c2
  indexed: true
  sha256: fc3f67ab5b5ba0e463786f1399941a0fa04aa6281de7d3a110a117d726f82594
  source_range:
    end: 14634047
    part: 3
    start: 0
  summary: 材料记录了 KDA 从临时低秩 gate 替换为显式 full-rank K3 拓扑、shared expert/TP 布局、KDA/MLA 通信所有权及 serving 注册路径的多轮调整；也保留了 renderer、parser、pre-render hook 不足和全局 monkey-patch 风险等未决判断。静态检查曾报告 compileall、ruff、format、git diff --check 等通过，但不同阶段因缺少 torch、vLLM、pytest 等依赖而未完成运行时测试；部分“完成”结论仅属于源码审计或计划。后续审计建议保留配置注册幂等、decoder 组网矩阵、request 行为和 parser 契约等测试缺口，不把静态通过升级为运行时成功。
  title: K3 Python 组网与 serving 审计
- block_id: dc6025ea8db39f244d549421a1b46d4a8062bc8f84da0983ea2392d6ac259d34
  indexed: true
  sha256: 842aa581ece4cf05ba89940a6b00016f37f1b31f2003d0dead02cbdfa183afb8
  source_range:
    end: 14634047
    part: 4
    start: 0
  summary: 该批材料记录了 routed experts 使用 latent hidden、shared experts 使用原始 full hidden 的数据流修正，以及 packed MXFP4 checkpoint ABI（*_weight_packed）向标准运行时字段转换的约定。随后对 g_proj 重复 all-gather、TP/DP shared expert、Sequence Parallel 归约和 W4A16-MXFP SiTU 专用路径进行了多轮修订；审计还建议删除过程型、弱校验及私有实现导向的 UT，同时保留核心组网、checkpoint ABI、数学和算子覆盖。报告的 ruff、format、AST、py_compile 与 diff 检查不等同于 pytest/NPU 验证，本机缺少 torch、torch_npu、vLLM 或 pytest 的事实需随后续索引保留。
  title: MoE、量化 ABI 与通信契约修正
- block_id: 993d3a0ec2c782051a9f5ec0b2e1c7d5e41e97ac16fbbd9481ae08f0c483dc2a
  indexed: true
  sha256: eafe222844aa4b1b0a8e71b03c584b73b97a465ce983604ab7aae4886b6792ae
  source_range:
    end: 14634047
    part: 5
    start: 0
  summary: 材料称最终 Python diff 清除了 K3 全局 op registry 注入、过程型测试及 parser/patch 残留，完成 SP、TP/DP shared expert、KDA metadata tuple 和 W4A16MXFP SiTU 契约的静态收口；未修改 .cpp/.h，pytest/NPU runtime 仍未运行。gitleaks 先后出现 wget 缺失、Linux 二进制在 macOS 上 exec format error，后改用 macOS 构建并命中 tokenBatch、tilingKey、per_token_scale_dtype 三个被判断为标识符误报的规则；这不是零命中扫描。后续材料报告提交从 61ec3b12 重放为 219c7b436、目标分支与官方 main 为 0 behind/1 ahead，并拒绝 cherry-pick ci-new 的 0f6aee5b，因为目标树已删除相关 protocol patch；这些发布状态来自报告内容，仍应与独立记录区分。
  title: 静态收口、提交发布与 ci-new 回退同步
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 6478997f75769b0b9e8509bf5649ae56f2563fefb40e932963757a56f2245733
  kind: experience
  material_digest: 3c0ef458a71a81e1ae365da5586ce88268573b91b3bba04eb86d3249d4af792e
  revision: a6d2c414d23ebf187adebeaa3ff5bf530b23a1afd7ed88c023097f216a54839e
  schema: mindie-entry/3
  summary: 现有材料覆盖 K3-only Python 重构、KDA/MLA 与 MoE 通信布局、shared expert 和 SiTU/量化 ABI、测试删改，以及从 61ec3b12 重放到 219c7b436 的发布与 ci-new 回退对照。当前最重要的未验证边界仍是完整 pytest、torch/torch_npu/vLLM 环境下的 NPU runtime、decoder/注册/request/parser 契约测试，以及静态检查与实际行为之间的差异；报告中的“通过”不得视为运行时成功。另需注意 gitleaks 曾因工具/架构问题更换执行方式并产生三个标识符误报，且 ci-new 回退被判断为语义已覆盖而未移植；后续查找应保留这些失败尝试、条件判断和远端状态假设。C++/header 与 AscendC 算子实现持续被列为未修改范围。
  title: 当前任务导航：Kimi K3 Python 重构、量化与发布状态索引
navigation: 现有材料覆盖 K3-only Python 重构、KDA/MLA 与 MoE 通信布局、shared expert 和 SiTU/量化 ABI、测试删改，以及从 61ec3b12 重放到 219c7b436 的发布与 ci-new 回退对照。当前最重要的未验证边界仍是完整 pytest、torch/torch_npu/vLLM 环境下的 NPU runtime、decoder/注册/request/parser 契约测试，以及静态检查与实际行为之间的差异；报告中的“通过”不得视为运行时成功。另需注意 gitleaks 曾因工具/架构问题更换执行方式并产生三个标识符误报，且 ci-new 回退被判断为语义已覆盖而未移植；后续查找应保留这些失败尝试、条件判断和远端状态假设。C++/header 与 AscendC 算子实现持续被列为未修改范围。
schema: mindie-material-task/1
status: complete
task_id: 6478997f75769b0b9e8509bf5649ae56f2563fefb40e932963757a56f2245733
---

# 当前任务导航：Kimi K3 Python 重构、量化与发布状态索引

现有材料覆盖 K3-only Python 重构、KDA/MLA 与 MoE 通信布局、shared expert 和 SiTU/量化 ABI、测试删改，以及从 61ec3b12 重放到 219c7b436 的发布与 ci-new 回退对照。当前最重要的未验证边界仍是完整 pytest、torch/torch_npu/vLLM 环境下的 NPU runtime、decoder/注册/request/parser 契约测试，以及静态检查与实际行为之间的差异；报告中的“通过”不得视为运行时成功。另需注意 gitleaks 曾因工具/架构问题更换执行方式并产生三个标识符误报，且 ci-new 回退被判断为语义已覆盖而未移植；后续查找应保留这些失败尝试、条件判断和远端状态假设。C++/header 与 AscendC 算子实现持续被列为未修改范围。

## Materials

- [Kimi K3 分支重建与清理记录](blocks/694f6240bdddf50b4d45782f3e8aa32e3e74e610beda456da16a9984acd68f51.md): 围绕将 `maoxx241:releases/v0.23.0_new` 准备为面向 `vllm-project/vllm-ascend:main` 的 Kimi K3 PR 分支，记录了从整线迁移、删除 `.github`/过程性 UT，到最终从 upstream main 逐项移植 K3 生产代码的多次修正。最终报告称分支以 `522c0f6d7` 为基线、提交为 `71d9d73c9`，移除了 0.23 专属实现和主线已删除 API 的调用；但完整 pytest 与 NPU 验证因本机缺少环境未执行，且这些内容属于此前操作报告而非独立验证。
- [Python 层架构审计与重构范围](blocks/8452a7e76aebde82c90d87be6dc488a57ba6bb18b3020065ea3899a236c50488.md): 用户要求基于 Kimi K3 分支重构 Python 组网和特性注册，明确不修改 C++/AscendC 算子；只读审计识别出全局 monkey patch、`REGISTERED_ASCEND_OPS` 补丁式注册、SiTU 参数污染 upstream `FusedMoE`、动态 MLA 属性和多处防御性兼容。报告指出 parser 漏传 `is_harmony`/`mm_token_counts`、K3 MoE 真实 factory 契约不成立，并建议改用显式 runner、typed contract、局部组网和统一注册入口；后续工作报告称部分重构已完成，但相关 UT 因缺少 `torch`/`torch_npu`/`vllm` 未实际执行。
- [Serving、MLA 与 SiTU 重构结果及遗留项](blocks/0088f84d725e4f500f25e0396ea11ec800ef13e2f45f9c79d4685914ed0c50ec.md): 审计进一步列出应删除的 serving/parser monkey patch、静默 fail-soft、防御性 `getattr/hasattr`、运行期 ABI 探测和会被 skip 掩盖的 NPU 测试，并区分必须保留的输入、权重、协议和 ABI 边界校验。后续报告称已完成 `AscendMoERunner`/SiTU ABI adapter、K3 直接构造 KDA、typed MLA contract，以及将 K3 serving/parser/renderer/XTML 移至 `vllm_ascend/serving/`，删除大部分 OpenAI serving 私有方法补丁。仍有 `OnlineRenderer.render_chat` 的窄 adapter 和 `AscendMLAMetadataBuilder` 中的 `getattr(..., "mla_use_rope", True)` 遗留；静态检查据报通过，但 UT 仍未因缺少 `torch` 而运行。
- [K3 Python 组网与 serving 审计](blocks/c26a193e19121a371128df423c54d1d2330d8c026aae1b077be7190b554632c2.md): 材料记录了 KDA 从临时低秩 gate 替换为显式 full-rank K3 拓扑、shared expert/TP 布局、KDA/MLA 通信所有权及 serving 注册路径的多轮调整；也保留了 renderer、parser、pre-render hook 不足和全局 monkey-patch 风险等未决判断。静态检查曾报告 compileall、ruff、format、git diff --check 等通过，但不同阶段因缺少 torch、vLLM、pytest 等依赖而未完成运行时测试；部分“完成”结论仅属于源码审计或计划。后续审计建议保留配置注册幂等、decoder 组网矩阵、request 行为和 parser 契约等测试缺口，不把静态通过升级为运行时成功。
- [MoE、量化 ABI 与通信契约修正](blocks/dc6025ea8db39f244d549421a1b46d4a8062bc8f84da0983ea2392d6ac259d34.md): 该批材料记录了 routed experts 使用 latent hidden、shared experts 使用原始 full hidden 的数据流修正，以及 packed MXFP4 checkpoint ABI（*_weight_packed）向标准运行时字段转换的约定。随后对 g_proj 重复 all-gather、TP/DP shared expert、Sequence Parallel 归约和 W4A16-MXFP SiTU 专用路径进行了多轮修订；审计还建议删除过程型、弱校验及私有实现导向的 UT，同时保留核心组网、checkpoint ABI、数学和算子覆盖。报告的 ruff、format、AST、py_compile 与 diff 检查不等同于 pytest/NPU 验证，本机缺少 torch、torch_npu、vLLM 或 pytest 的事实需随后续索引保留。
- [静态收口、提交发布与 ci-new 回退同步](blocks/993d3a0ec2c782051a9f5ec0b2e1c7d5e41e97ac16fbbd9481ae08f0c483dc2a.md): 材料称最终 Python diff 清除了 K3 全局 op registry 注入、过程型测试及 parser/patch 残留，完成 SP、TP/DP shared expert、KDA metadata tuple 和 W4A16MXFP SiTU 契约的静态收口；未修改 .cpp/.h，pytest/NPU runtime 仍未运行。gitleaks 先后出现 wget 缺失、Linux 二进制在 macOS 上 exec format error，后改用 macOS 构建并命中 tokenBatch、tilingKey、per_token_scale_dtype 三个被判断为标识符误报的规则；这不是零命中扫描。后续材料报告提交从 61ec3b12 重放为 219c7b436、目标分支与官方 main 为 0 behind/1 ahead，并拒绝 cherry-pick ci-new 的 0f6aee5b，因为目标树已删除相关 protocol patch；这些发布状态来自报告内容，仍应与独立记录区分。

---
blocks:
- block_id: aea10b647d2136e1fbfc50e94a48061d082610f1ec997413062a61ef6eac598b
  indexed: true
  sha256: f4ef6458e699c34be2df7fa39e68871589b66c456c281266cb792edb9997e334
  source_range:
    end: 2784923
    part: 0
    start: 0
  summary: 完成了 GitHub CLI 登录和两台旧机器的本地状态清理，但远端容器删除因 SSH 22 端口超时未获确认；随后以最新 rc 镜像配置 A3 新机器，类型为 A3，容器为 `vaws-maoxx241`，状态报告为 ready。围绕 causal conv1d update，分析转向 GDN 业务路径，重点关注 metadata 转换、Triton/AscendC 入参布局差异、`num_accepted_tokens` 偏移和 `mtp=3` 场景；此前关于越界的判断仍属待验证假设。
  title: 仓库与 A3 新机器配置及业务复现方向
- block_id: 30de5f0fd5898547ac917559c582a58dc8edfcbef32d5ee8156f85732f3465cb
  indexed: true
  sha256: 53bf1003686a054ed9146a0321353ea53d8ecbbd26d76790aba9d73012b76c1b
  source_range:
    end: 2784923
    part: 1
    start: 0
  summary: 按要求只依据业务代码构造 `GDNAttentionMetadataBuilder`/Ascend builder 相关 metadata，默认 `mtp=3`，模拟 spec decode 的 4 个候选 token，并分别准备 Triton `causal_conv1d_update_npu` 与 AscendC `npu_causal_conv1d_custom` 对照调用。验证脚本 `tools/causal_conv1d_business_repro.py` 已完成语法检查，但尚未得到远端运行结果；A3 同步一度需要 `local` 模式及本地 vLLM/vLLM-Ascend 替换，后续配套已切到 upstream CI verified commits。`num_accepted_tokens=0` 被标记为可能不是默认合法 GDN 值，不能当作已确认复现。
  title: 基于 GDN metadata 的业务复现脚本
- block_id: 76ed4f4f540a7fa83cdb78e0db193888d3021526d62e6f47eb557f30f4390106
  indexed: true
  sha256: 2cc8c277e2108aa428c04ee00a05c932f8e7bfc108933ef4b2aec290c2940c25
  source_range:
    end: 2784923
    part: 2
    start: 0
  summary: parity 同步先后遇到 shallow Git mirror、SSH timeout、stale lock、`pip install uv` 长时间卡住、清华源 403、缺少 `setuptools_rust`、Python 3.10/3.12 混用及 CMake generator 缓存冲突等问题，之后改用华为云主源、PyPI fallback 和 no-deps editable install。vLLM 已报告安装成功，vLLM-Ascend 正在 A3 上进行 CANN 自定义算子编译，进度曾到约 619/654，`causal_conv1d` 已进入编译列表，但尚未报告编译完成、import smoke 或复现脚本的 Triton/AscendC 对照结果。
  title: A3 本地包安装与编译仍未完成
- block_id: ff42f5c4ccdf50360b84413a57c9931fbf284e33a2b0ddb7399d68cbd0de1b28
  indexed: true
  sha256: 181b0c452ef67a9c88a106c6d46c05e6d3ee5f42d6a0dfa6163934c46e6693b6
  source_range:
    end: 5199282
    part: 0
    start: 2784923
  summary: A3 上完成 vLLM/vllm-ascend editable 安装并通过 import smoke；此前源码路径绕过安装不可行，因为缺少 `vllm_ascend_C` 注册。按 `AscendGDNAttentionMetadataBuilder` 构造业务 metadata 后，AscendC 与 Triton 的最小及多 batch 用例均可运行；未稳定复现 Triton 越界、报错或 hang。未 source CANN 时曾出现 `libhccl.so` 缺失，显式 source `/usr/local/Ascend/cann-9.0.0/set_env.sh` 后，先前 20 秒级慢路径未再复现；Triton spec decode 的状态偏移仍只是待验证风险点。
  title: A3 安装完成与基础业务路径验证
- block_id: 6e453bd06ef01806e1899e83ef3791d3730103d59743a415a17a7e54f1d6c362
  indexed: true
  sha256: bdfeb69bdd1293f1e340de9068b386972ef114f66055e76a9303a5f91fb47f24
  source_range:
    end: 5199282
    part: 1
    start: 2784923
  summary: 脚本加入 Qwen3.5-397B-A17B 模拟配置：key/value heads 为 16/64、head dim 为 128、kernel width 为 4，global conv dim 为 12288，并设置 `max_model_len=262144`、`max_num_seqs=128`、`mtp=3`；TP=1/2/4/8/16 对应本地 dim 12288/6144/3072/1536/768。`batch=128` 的 13 种 token pattern、5 种 TP，以及 28 个 batch/pattern 组合均为 0 failure，未构造出 AscendC 成功而 Triton 越界或报错的合法用例。观察到新 shape 的 Triton 首次 JIT/编译尖峰：部分 case 约 25.6–25.8 秒，batch=64 新 token 数约 7.6 秒；暖缓存后约 120–150ms，这属于业务首请求卡顿/超时风险而非已证实的算子错误。当前结论限定于 `mamba_cache_mode=none` 和所构造的 Qwen GDN metadata；真实错误场景的 cache index、graph capture 或其他配置仍未验证。
  title: Qwen3.5-397B shape 扫描与 Triton 冷编译尖峰
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 88840079a5ad8c984a9dd7bb094c84a55b4add1150cde42722200d6abdbef93f
  kind: experience
  material_digest: 3d0a04f0c4749b4ec07832c41a34d89f5bd4b8541c004bb6d78a46e425f6c063
  revision: ebda506feb154b5c90db333cd058b38dbf33b9cc27ea1658aebbafeb48e8fe6a
  schema: mindie-entry/3
  summary: A3 安装、CANN 环境和业务 metadata 复现链路已完成；旧导航中“编译阻塞、尚待运行”的状态已被后续结果修正。基于 Qwen3.5-397B-A17B shape、`max_model_len=262144`、`max_num_seqs=128`，已完成 pattern、TP 和 batch 边界扫描，当前范围内 AscendC/Triton 均未出现 failure 或稳定 OOB；此前无 CANN 环境导致的 `libhccl.so` 错误不是算子失败。应保留两个未决方向：Triton 新 shape 的约 7.6–25.8 秒首次编译尖峰可能造成业务超时，以及真实问题中的 `mamba_cache_mode`、cache index 分布和 graph capture 参数尚未覆盖；原始 blocks 仍可独立检索，不能将这些线索提升为已验证成功或已证实缺陷。
  title: 当前任务导航：Qwen3.5-397B 业务复现未发现 Triton 越界，保留冷编译风险
navigation: A3 安装、CANN 环境和业务 metadata 复现链路已完成；旧导航中“编译阻塞、尚待运行”的状态已被后续结果修正。基于 Qwen3.5-397B-A17B shape、`max_model_len=262144`、`max_num_seqs=128`，已完成 pattern、TP 和 batch 边界扫描，当前范围内 AscendC/Triton 均未出现 failure 或稳定 OOB；此前无 CANN 环境导致的 `libhccl.so` 错误不是算子失败。应保留两个未决方向：Triton 新 shape 的约 7.6–25.8 秒首次编译尖峰可能造成业务超时，以及真实问题中的 `mamba_cache_mode`、cache index 分布和 graph capture 参数尚未覆盖；原始 blocks 仍可独立检索，不能将这些线索提升为已验证成功或已证实缺陷。
schema: mindie-material-task/1
status: complete
task_id: 88840079a5ad8c984a9dd7bb094c84a55b4add1150cde42722200d6abdbef93f
---

# 当前任务导航：Qwen3.5-397B 业务复现未发现 Triton 越界，保留冷编译风险

A3 安装、CANN 环境和业务 metadata 复现链路已完成；旧导航中“编译阻塞、尚待运行”的状态已被后续结果修正。基于 Qwen3.5-397B-A17B shape、`max_model_len=262144`、`max_num_seqs=128`，已完成 pattern、TP 和 batch 边界扫描，当前范围内 AscendC/Triton 均未出现 failure 或稳定 OOB；此前无 CANN 环境导致的 `libhccl.so` 错误不是算子失败。应保留两个未决方向：Triton 新 shape 的约 7.6–25.8 秒首次编译尖峰可能造成业务超时，以及真实问题中的 `mamba_cache_mode`、cache index 分布和 graph capture 参数尚未覆盖；原始 blocks 仍可独立检索，不能将这些线索提升为已验证成功或已证实缺陷。

## Materials

- [仓库与 A3 新机器配置及业务复现方向](blocks/aea10b647d2136e1fbfc50e94a48061d082610f1ec997413062a61ef6eac598b.md): 完成了 GitHub CLI 登录和两台旧机器的本地状态清理，但远端容器删除因 SSH 22 端口超时未获确认；随后以最新 rc 镜像配置 A3 新机器，类型为 A3，容器为 `vaws-maoxx241`，状态报告为 ready。围绕 causal conv1d update，分析转向 GDN 业务路径，重点关注 metadata 转换、Triton/AscendC 入参布局差异、`num_accepted_tokens` 偏移和 `mtp=3` 场景；此前关于越界的判断仍属待验证假设。
- [基于 GDN metadata 的业务复现脚本](blocks/30de5f0fd5898547ac917559c582a58dc8edfcbef32d5ee8156f85732f3465cb.md): 按要求只依据业务代码构造 `GDNAttentionMetadataBuilder`/Ascend builder 相关 metadata，默认 `mtp=3`，模拟 spec decode 的 4 个候选 token，并分别准备 Triton `causal_conv1d_update_npu` 与 AscendC `npu_causal_conv1d_custom` 对照调用。验证脚本 `tools/causal_conv1d_business_repro.py` 已完成语法检查，但尚未得到远端运行结果；A3 同步一度需要 `local` 模式及本地 vLLM/vLLM-Ascend 替换，后续配套已切到 upstream CI verified commits。`num_accepted_tokens=0` 被标记为可能不是默认合法 GDN 值，不能当作已确认复现。
- [A3 本地包安装与编译仍未完成](blocks/76ed4f4f540a7fa83cdb78e0db193888d3021526d62e6f47eb557f30f4390106.md): parity 同步先后遇到 shallow Git mirror、SSH timeout、stale lock、`pip install uv` 长时间卡住、清华源 403、缺少 `setuptools_rust`、Python 3.10/3.12 混用及 CMake generator 缓存冲突等问题，之后改用华为云主源、PyPI fallback 和 no-deps editable install。vLLM 已报告安装成功，vLLM-Ascend 正在 A3 上进行 CANN 自定义算子编译，进度曾到约 619/654，`causal_conv1d` 已进入编译列表，但尚未报告编译完成、import smoke 或复现脚本的 Triton/AscendC 对照结果。
- [A3 安装完成与基础业务路径验证](blocks/ff42f5c4ccdf50360b84413a57c9931fbf284e33a2b0ddb7399d68cbd0de1b28.md): A3 上完成 vLLM/vllm-ascend editable 安装并通过 import smoke；此前源码路径绕过安装不可行，因为缺少 `vllm_ascend_C` 注册。按 `AscendGDNAttentionMetadataBuilder` 构造业务 metadata 后，AscendC 与 Triton 的最小及多 batch 用例均可运行；未稳定复现 Triton 越界、报错或 hang。未 source CANN 时曾出现 `libhccl.so` 缺失，显式 source `/usr/local/Ascend/cann-9.0.0/set_env.sh` 后，先前 20 秒级慢路径未再复现；Triton spec decode 的状态偏移仍只是待验证风险点。
- [Qwen3.5-397B shape 扫描与 Triton 冷编译尖峰](blocks/6e453bd06ef01806e1899e83ef3791d3730103d59743a415a17a7e54f1d6c362.md): 脚本加入 Qwen3.5-397B-A17B 模拟配置：key/value heads 为 16/64、head dim 为 128、kernel width 为 4，global conv dim 为 12288，并设置 `max_model_len=262144`、`max_num_seqs=128`、`mtp=3`；TP=1/2/4/8/16 对应本地 dim 12288/6144/3072/1536/768。`batch=128` 的 13 种 token pattern、5 种 TP，以及 28 个 batch/pattern 组合均为 0 failure，未构造出 AscendC 成功而 Triton 越界或报错的合法用例。观察到新 shape 的 Triton 首次 JIT/编译尖峰：部分 case 约 25.6–25.8 秒，batch=64 新 token 数约 7.6 秒；暖缓存后约 120–150ms，这属于业务首请求卡顿/超时风险而非已证实的算子错误。当前结论限定于 `mamba_cache_mode=none` 和所构造的 Qwen GDN metadata；真实错误场景的 cache index、graph capture 或其他配置仍未验证。

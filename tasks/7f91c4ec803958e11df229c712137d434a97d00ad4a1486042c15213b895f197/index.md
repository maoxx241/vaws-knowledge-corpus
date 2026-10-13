---
blocks:
- block_id: 561dcf31e6493bbd451517d67f5fd53ada8ccabe3646ec504dd40b3ad6fd2311
  indexed: true
  sha256: afdc199439c4ba14c2ffc990b96e184b2b2c169d5ef6a872537c0c61d9a25fb2
  source_range:
    end: 980937
    part: 0
    start: 0
  summary: 用户要求定位 Kimi K3 在固定 max_num_batched_tokens 下峰值激活随 max_model_len 升高的问题，并限定为纯语言、关闭 ViT；记录中的 156 节点扫点采用 5 层、TP8 配置，未执行真实 generate 或请求 forward。8192 首次 profile 因旧 `_C_ascend` 扩展缺少 `dequant_situ_quant` 绑定而失败，随后据称复用匹配扩展重跑；记录报告 8192 与 32768 的 peak activation 均为 1.97 GiB，但这只是该次环境和配置下的观察，不能据此确认根因或普遍修复。
  title: Kimi K3 显存峰值扫点与 profile 扩展不匹配
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7f91c4ec803958e11df229c712137d434a97d00ad4a1486042c15213b895f197
  kind: experience
  material_digest: 6a95cb14be86c0608ae49f074e8d426e5940499dd9b4861b22d68928edc25dc5
  revision: b91c444925357c4508afae2e87d2ba76d30f478d05a59928f9fd7ce475f3560f
  schema: mindie-entry/3
  summary: 当前材料聚焦固定 8192 batched tokens、5 层纯语言模型在 max_model_len=8192/32768 下的 profile 显存观察，以及旧扩展缺少 `dequant_situ_quant` 导致的首次失败和后续扩展复用。记录称两种长度的峰值激活相同、进程退出后 NPU 为空，但仍应保留首次失败、环境残留僵尸进程、构建产物不匹配和未完成更高长度/4 机完整权重验证等不确定性；原始材料仍可独立检索。
  title: Kimi K3 显存问题：156 节点 profile 扫点记录
navigation: 当前材料聚焦固定 8192 batched tokens、5 层纯语言模型在 max_model_len=8192/32768 下的 profile 显存观察，以及旧扩展缺少 `dequant_situ_quant` 导致的首次失败和后续扩展复用。记录称两种长度的峰值激活相同、进程退出后 NPU 为空，但仍应保留首次失败、环境残留僵尸进程、构建产物不匹配和未完成更高长度/4 机完整权重验证等不确定性；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 7f91c4ec803958e11df229c712137d434a97d00ad4a1486042c15213b895f197
---

# Kimi K3 显存问题：156 节点 profile 扫点记录

当前材料聚焦固定 8192 batched tokens、5 层纯语言模型在 max_model_len=8192/32768 下的 profile 显存观察，以及旧扩展缺少 `dequant_situ_quant` 导致的首次失败和后续扩展复用。记录称两种长度的峰值激活相同、进程退出后 NPU 为空，但仍应保留首次失败、环境残留僵尸进程、构建产物不匹配和未完成更高长度/4 机完整权重验证等不确定性；原始材料仍可独立检索。

## Materials

- [Kimi K3 显存峰值扫点与 profile 扩展不匹配](blocks/561dcf31e6493bbd451517d67f5fd53ada8ccabe3646ec504dd40b3ad6fd2311.md): 用户要求定位 Kimi K3 在固定 max_num_batched_tokens 下峰值激活随 max_model_len 升高的问题，并限定为纯语言、关闭 ViT；记录中的 156 节点扫点采用 5 层、TP8 配置，未执行真实 generate 或请求 forward。8192 首次 profile 因旧 `_C_ascend` 扩展缺少 `dequant_situ_quant` 绑定而失败，随后据称复用匹配扩展重跑；记录报告 8192 与 32768 的 peak activation 均为 1.97 GiB，但这只是该次环境和配置下的观察，不能据此确认根因或普遍修复。

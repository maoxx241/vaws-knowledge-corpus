---
blocks:
- block_id: 3873a605e1a1bf99e0e7ba1993888a8eed243b659d85bb137736859633f15534
  indexed: true
  sha256: 5e5c50477c86d73572b963202d88631859de926fd0580d92162fc6e6ba568b20
  source_range:
    end: 1024230
    part: 0
    start: 0
  summary: 已完成四机 SGLang oracle 的只读准备，确认镜像 ID/digest 一致，但镜像内实际源码 SHA 为 `02a924b4...`，不同于当前 PR 头；数据网卡为 `data0.173`，本地 target/draft 配置、tokenizer 与 chat template 哈希一致，完整权重尚未逐文件校验。启动时需锁定镜像 digest，并补充 `--chat-template`、`--served-model-name`、`--decode-log-interval 1` 等参数；SGLang 支持 `X-Data-Parallel-Rank`，且 `block_size=7` 对应 verify width 8。指标方面，`accept_length = accepted_drafts + 1`、`accept_rate=(AL-1)/7`，`/metrics` 的 Gauge 不适合作为 vLLM counter delta，建议 warmup 后通过 `/set_internal_state` 重置并从 `/server_info` 的 DP state 读取 `avg_spec_accept_length`；HF 的 `AL=5.6660` 仅为背景参考，尚未验证适用于当前 W4A8、64 样本 oracle。
  title: SGLang 四机 oracle 只读准备与指标口径
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8d4a8b34e1e248e3609a3290dc5a3adb004873846c54a6c3447a4779df56366b
  kind: experience
  material_digest: 277fbde914f8b8cf1a39602caa2d5a70eb69d0b2edcfe20a260cb2acf733a11c
  revision: b0a976ac45b48c62e9b9a43ab9cf89609f626cf532581303615f9d1ecfecc0c0
  schema: mindie-entry/3
  summary: 当前批次记录了四机镜像、源码版本、网络接口、权重与 tokenizer 一致性、请求兼容性、speculative decoding 指标口径及最小启动差异。关键未完成项包括完整 1.5TB 权重逐文件校验，以及在锁定镜像和本地 W4A8 target/draft 权重上实际测量 AL 与精度；此前仅完成只读检查，未启动容器或占用 NPU。
  title: SGLang 四机 oracle 准备索引
navigation: 当前批次记录了四机镜像、源码版本、网络接口、权重与 tokenizer 一致性、请求兼容性、speculative decoding 指标口径及最小启动差异。关键未完成项包括完整 1.5TB 权重逐文件校验，以及在锁定镜像和本地 W4A8 target/draft 权重上实际测量 AL 与精度；此前仅完成只读检查，未启动容器或占用 NPU。
schema: mindie-material-task/1
status: complete
task_id: 8d4a8b34e1e248e3609a3290dc5a3adb004873846c54a6c3447a4779df56366b
---

# SGLang 四机 oracle 准备索引

当前批次记录了四机镜像、源码版本、网络接口、权重与 tokenizer 一致性、请求兼容性、speculative decoding 指标口径及最小启动差异。关键未完成项包括完整 1.5TB 权重逐文件校验，以及在锁定镜像和本地 W4A8 target/draft 权重上实际测量 AL 与精度；此前仅完成只读检查，未启动容器或占用 NPU。

## Materials

- [SGLang 四机 oracle 只读准备与指标口径](blocks/3873a605e1a1bf99e0e7ba1993888a8eed243b659d85bb137736859633f15534.md): 已完成四机 SGLang oracle 的只读准备，确认镜像 ID/digest 一致，但镜像内实际源码 SHA 为 `02a924b4...`，不同于当前 PR 头；数据网卡为 `data0.173`，本地 target/draft 配置、tokenizer 与 chat template 哈希一致，完整权重尚未逐文件校验。启动时需锁定镜像 digest，并补充 `--chat-template`、`--served-model-name`、`--decode-log-interval 1` 等参数；SGLang 支持 `X-Data-Parallel-Rank`，且 `block_size=7` 对应 verify width 8。指标方面，`accept_length = accepted_drafts + 1`、`accept_rate=(AL-1)/7`，`/metrics` 的 Gauge 不适合作为 vLLM counter delta，建议 warmup 后通过 `/set_internal_state` 重置并从 `/server_info` 的 DP state 读取 `avg_spec_accept_length`；HF 的 `AL=5.6660` 仅为背景参考，尚未验证适用于当前 W4A8、64 样本 oracle。

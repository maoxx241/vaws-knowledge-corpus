---
blocks:
- block_id: a503a7ef61ea850ba57bd4c6401a0256618be0b127f352c165da3e310b5b92a0
  indexed: true
  sha256: 08d00ade735573fc20e1ab09a7d6038199a98e7f356229d3490f4dfc3082b5ec
  source_range:
    end: 1775784
    part: 0
    start: 0
  summary: 本地只读审计认为 KDA/conv 源码与 vendor OPP 更可能是排除项，#13376 的 mla_preprocess 在 standalone launcher 中未实际入栈；若做 A/B，extension 与 direct-kernel `.so` 需成对替换。Python 路径大多等价或 byte-identical，精确 128K×2、temperature=0、spec=7 走动态 vec_len 的 greedy rejection kernel，因此 r26 的 random/spec_len=1 动态标记未必影响该压测。建议移植 r9 的 9 个文件并复用 warmup 测试，但 raw `ec56b6b2` 引用缺失的 `dsa_triton_warmup` 且没有 hook，不能直接合入；warmup 仍需由各 rank 日志中的 `rejection_sampler Triton warmup complete` 验证，fail-open 可能导致服务启动但候选未生效。
  title: r26/r9 native 与 Triton release-port 审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 25d9f262b95bf152908ef519d56d06eb39d6d371caa10d479c5841bcf03a3e58
  kind: experience
  material_digest: 61d607e1281b955d01d7eef9ed35853d49181a7dfb6a5fe1168ead0ab744b7a6
  revision: e8fafb80c96bd44368d2c9174d5f8263e0f112941224d7fc564a1e4ea1b03355
  schema: mindie-entry/3
  summary: 已完成一轮本地静态审计，未修改工作树、未接触集群或服务。当前重点是 9 文件 Triton warmup/rejection-sampling port、`_dummy_run()` 后至 `capture_model()` 前的 hook 顺序、逐 family fail-open、kernel API 兼容性及 NPU 回归；native `#13376` 的实际触发与 ABI 配对仍需按运行日志或 A/B 进一步确认，不能将计划或 reported check 视为验证成功。
  title: 当前任务导航：r26/r9 release-port 候选与 native 路径审计
navigation: 已完成一轮本地静态审计，未修改工作树、未接触集群或服务。当前重点是 9 文件 Triton warmup/rejection-sampling port、`_dummy_run()` 后至 `capture_model()` 前的 hook 顺序、逐 family fail-open、kernel API 兼容性及 NPU 回归；native `#13376` 的实际触发与 ABI 配对仍需按运行日志或 A/B 进一步确认，不能将计划或 reported check 视为验证成功。
schema: mindie-material-task/1
status: complete
task_id: 25d9f262b95bf152908ef519d56d06eb39d6d371caa10d479c5841bcf03a3e58
---

# 当前任务导航：r26/r9 release-port 候选与 native 路径审计

已完成一轮本地静态审计，未修改工作树、未接触集群或服务。当前重点是 9 文件 Triton warmup/rejection-sampling port、`_dummy_run()` 后至 `capture_model()` 前的 hook 顺序、逐 family fail-open、kernel API 兼容性及 NPU 回归；native `#13376` 的实际触发与 ABI 配对仍需按运行日志或 A/B 进一步确认，不能将计划或 reported check 视为验证成功。

## Materials

- [r26/r9 native 与 Triton release-port 审计](blocks/a503a7ef61ea850ba57bd4c6401a0256618be0b127f352c165da3e310b5b92a0.md): 本地只读审计认为 KDA/conv 源码与 vendor OPP 更可能是排除项，#13376 的 mla_preprocess 在 standalone launcher 中未实际入栈；若做 A/B，extension 与 direct-kernel `.so` 需成对替换。Python 路径大多等价或 byte-identical，精确 128K×2、temperature=0、spec=7 走动态 vec_len 的 greedy rejection kernel，因此 r26 的 random/spec_len=1 动态标记未必影响该压测。建议移植 r9 的 9 个文件并复用 warmup 测试，但 raw `ec56b6b2` 引用缺失的 `dsa_triton_warmup` 且没有 hook，不能直接合入；warmup 仍需由各 rank 日志中的 `rejection_sampler Triton warmup complete` 验证，fail-open 可能导致服务启动但候选未生效。

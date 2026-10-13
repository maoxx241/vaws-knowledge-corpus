---
blocks:
- block_id: b5a8f978eb4a7b96a1743d33b97843284d4699ad5e903572d964e987c67a6ab3
  indexed: true
  sha256: d9be4b872e823c5a7cd975b812181060ed183a0be1b0d7b61f35811927e4a9d3
  source_range:
    end: 16780203
    part: 0
    start: 0
  summary: 在 K3 GQA DSpark、关闭 prefix cache 的 TP8×DP2 与 TP16 测试中，基础请求未复现“全是 !”或崩溃；TP16 扩大到 max_num_seqs=128 后，采样器完整词表排序因临时显存不足触发 NPU OOM，128 个请求全部 HTTP 500。将采样排序改为分批处理后，临时显存约从 5.04 GiB 降至 574 MiB，修复提交为 `2dbb2b080`；修复后的 128 并发通过，但 256 并发仅 99/256 成功、157 条超过 600 秒，排队延迟仍未解决。关于 `valid sampled tokens count=0`，代码和构造 NPU 测试表明分块 prefill 中为 0 可正常，异常 decode 中为 0 会造成危险索引；当时尚未在真实请求中确认该异常。
  title: TP16 高并发采样器 OOM 复现与修复回放
- block_id: 12b0143245219e443b4013315e4805370e14ab09325bc9ad5896f2a5b112ba25
  indexed: true
  sha256: 9d5dab79f3b28781583e6eba04ce2941f6ea6367df7db46ac6dca5491f1c47df
  source_range:
    end: 16780203
    part: 1
    start: 0
  summary: 在 max_num_seqs=256、128 持续并发、关闭 prefix cache 的真实压测中，未被 discard 的 decode 行出现 `valid_sampled_tokens_count=0`，采样 ID 全为 `-1`，部分请求因 NaN 返回 HTTP 400；不请求 logprobs 时虽有 HTTP 200，但出现异常文本和 token ID 0，因此不能视为成功。采样前 logits、hidden states 及更早的 KDA 输出已含 NaN；独立 `chunk_kda_fwd` 测试进一步复现 127/128 条短序列触发 NaN，根因定位为 KDA W 临时区按补齐长度寻址、却按未补齐 token 总数分配，导致越界写入。修正容量后同一失败输入的 12 个张量保持有限，并通过 8 组 NPU 数值回归及 CPU 参考对照；修复提交为 `c2b476cf7`，完整服务端到端复跑仍在进行，尚不能宣称真实故障已最终解决。
  title: Decode count=0 追踪至 KDA 批处理越界
- block_id: 92810bf1958d4b0a8f60780d1f5f865d09a094f962e0370268b6c08bb8e2b34b
  indexed: true
  sha256: 27d9e8a6e50559aa398347faa184d8f4b43ac8e40079f3dd9891f748ba81d97b
  source_range:
    end: 18650306
    part: 0
    start: 16780203
  summary: 'KDA workspace 容量修复及回归测试已提交 PR #16087（目标分支 releases/v0.27.1rc），TP16、DSpark 7 token、prefix cache 关闭配置下，三轮共 896/896 请求成功，16 个 rank 的 count 为 1–8，未见 count=0、非有限 logprob 或服务错误；移除诊断采集后的 256 个请求也成功，健康检查返回 200。首轮 CI 曾因新增测试缺少对可空 final_state 的显式类型断言而失败，断言已补齐并推送，但新一轮 CI 的最终状态在该材料中仍未确认；验证报告已贴入 PR 评论，且移除了内部路径和机器信息。'
  title: KDA 容量修复 PR 与服务验证
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: bd76c09b015265e14e93da7611506d3566ca73c59a0e66c59d1555e41e400a6d
  kind: experience
  material_digest: 3e4b8f993fc6fca0b14d704fb18d379d1017a5812e7a83c2f1f3a12d335b4b15
  revision: bf9238774531c054614bc46608e1872279e3476af811bcef96ecc1aa852cd3ed
  schema: mindie-entry/3
  summary: '当前主线仍区分采样器排序 OOM、256 并发排队超时、尚未独立复现的“全是 !”，以及已追溯到 KDA 127/128 短序列批处理中 W 临时区分配越界的 valid_sampled_tokens_count=0、前向 NaN、采样 ID 全 -1 和后续 count=0。新增证据显示容量修复已完成独立算子回归，并在完整 TP16 服务验证中通过 896/896 请求及移除诊断采集后的 256 请求；但首轮 CI 曾因 final_state 可空类型检查失败，虽已修正并重新推送，新一轮 CI 结果仍未在材料中确认。PR #16087 与验证报告评论已建立，但这些结果仍属于该批次报告中的观察，不能替代对所有历史假设、未完成结果和后续 CI 状态的独立检索。'
  title: K3 GQA DSpark 关闭 prefix cache 故障定位与修复验证导航
navigation: '当前主线仍区分采样器排序 OOM、256 并发排队超时、尚未独立复现的“全是 !”，以及已追溯到 KDA 127/128 短序列批处理中 W 临时区分配越界的 valid_sampled_tokens_count=0、前向 NaN、采样 ID 全 -1 和后续 count=0。新增证据显示容量修复已完成独立算子回归，并在完整 TP16 服务验证中通过 896/896 请求及移除诊断采集后的 256 请求；但首轮 CI 曾因 final_state 可空类型检查失败，虽已修正并重新推送，新一轮 CI 结果仍未在材料中确认。PR #16087 与验证报告评论已建立，但这些结果仍属于该批次报告中的观察，不能替代对所有历史假设、未完成结果和后续 CI 状态的独立检索。'
schema: mindie-material-task/1
status: complete
task_id: bd76c09b015265e14e93da7611506d3566ca73c59a0e66c59d1555e41e400a6d
---

# K3 GQA DSpark 关闭 prefix cache 故障定位与修复验证导航

当前主线仍区分采样器排序 OOM、256 并发排队超时、尚未独立复现的“全是 !”，以及已追溯到 KDA 127/128 短序列批处理中 W 临时区分配越界的 valid_sampled_tokens_count=0、前向 NaN、采样 ID 全 -1 和后续 count=0。新增证据显示容量修复已完成独立算子回归，并在完整 TP16 服务验证中通过 896/896 请求及移除诊断采集后的 256 请求；但首轮 CI 曾因 final_state 可空类型检查失败，虽已修正并重新推送，新一轮 CI 结果仍未在材料中确认。PR #16087 与验证报告评论已建立，但这些结果仍属于该批次报告中的观察，不能替代对所有历史假设、未完成结果和后续 CI 状态的独立检索。

## Materials

- [TP16 高并发采样器 OOM 复现与修复回放](blocks/b5a8f978eb4a7b96a1743d33b97843284d4699ad5e903572d964e987c67a6ab3.md): 在 K3 GQA DSpark、关闭 prefix cache 的 TP8×DP2 与 TP16 测试中，基础请求未复现“全是 !”或崩溃；TP16 扩大到 max_num_seqs=128 后，采样器完整词表排序因临时显存不足触发 NPU OOM，128 个请求全部 HTTP 500。将采样排序改为分批处理后，临时显存约从 5.04 GiB 降至 574 MiB，修复提交为 `2dbb2b080`；修复后的 128 并发通过，但 256 并发仅 99/256 成功、157 条超过 600 秒，排队延迟仍未解决。关于 `valid sampled tokens count=0`，代码和构造 NPU 测试表明分块 prefill 中为 0 可正常，异常 decode 中为 0 会造成危险索引；当时尚未在真实请求中确认该异常。
- [Decode count=0 追踪至 KDA 批处理越界](blocks/12b0143245219e443b4013315e4805370e14ab09325bc9ad5896f2a5b112ba25.md): 在 max_num_seqs=256、128 持续并发、关闭 prefix cache 的真实压测中，未被 discard 的 decode 行出现 `valid_sampled_tokens_count=0`，采样 ID 全为 `-1`，部分请求因 NaN 返回 HTTP 400；不请求 logprobs 时虽有 HTTP 200，但出现异常文本和 token ID 0，因此不能视为成功。采样前 logits、hidden states 及更早的 KDA 输出已含 NaN；独立 `chunk_kda_fwd` 测试进一步复现 127/128 条短序列触发 NaN，根因定位为 KDA W 临时区按补齐长度寻址、却按未补齐 token 总数分配，导致越界写入。修正容量后同一失败输入的 12 个张量保持有限，并通过 8 组 NPU 数值回归及 CPU 参考对照；修复提交为 `c2b476cf7`，完整服务端到端复跑仍在进行，尚不能宣称真实故障已最终解决。
- [KDA 容量修复 PR 与服务验证](blocks/92810bf1958d4b0a8f60780d1f5f865d09a094f962e0370268b6c08bb8e2b34b.md): KDA workspace 容量修复及回归测试已提交 PR #16087（目标分支 releases/v0.27.1rc），TP16、DSpark 7 token、prefix cache 关闭配置下，三轮共 896/896 请求成功，16 个 rank 的 count 为 1–8，未见 count=0、非有限 logprob 或服务错误；移除诊断采集后的 256 个请求也成功，健康检查返回 200。首轮 CI 曾因新增测试缺少对可空 final_state 的显式类型断言而失败，断言已补齐并推送，但新一轮 CI 的最终状态在该材料中仍未确认；验证报告已贴入 PR 评论，且移除了内部路径和机器信息。

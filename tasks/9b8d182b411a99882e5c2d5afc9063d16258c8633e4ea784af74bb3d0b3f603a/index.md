---
blocks:
- block_id: 4c3885c42e2bd04ccbe982c430a72fa87589459f3f36a2ee337832b2ca353302
  indexed: true
  sha256: ba946ba6ff2d3989c4d5b64ffdc1b836ca1008be9573e4275618d05d4dc48b68
  source_range:
    end: 849842
    part: 0
    start: 0
  summary: 针对用户提出的“ci-new 分支问题似乎与 DSpark 无关”，此前助手给出了一份只读复核后的排查与执行建议，但内容仍是待验证参考，不能视为成功案例或已完成验证。记录的环境/API/方法线索包括 tokenizer 缺少 `tiktoken.model`、5 层模型的 `max_model_len=64` 可能不足、P/D wrapper 的 launcher 路径缺失、健康端点为 `/healthcheck`，以及 full-weight、5 层 aggregate、P/D 拓扑和 prefix/KV smoke 的分阶段检查；同时明确保留失败日志、原始 metrics、不确定性和 5 层 workaround 不能证明精度。
  title: ci-new 与 DSpark 相关问题的未验证复核建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9b8d182b411a99882e5c2d5afc9063d16258c8633e4ea784af74bb3d0b3f603a
  kind: experience
  material_digest: f36ccf38e8adb53b8ab8f53c1847dd257512a386dbd1da797a93d7d8d5f86cd9
  revision: cd0728d17387cd66f2f80917dac80fa2961fa48a665c3f6ca6f64d91c2b2587c
  schema: mindie-entry/3
  summary: 当前仅有一个未验证参考块，主题是 ci-new 分支相关问题的环境阻塞、DSpark/P-D/KV smoke 排查建议及证据保留限制。原文包含 tokenizer、模型长度、launcher 路径、健康端点、验证顺序和断言条件；这些均为报告的检查项或建议，不应当作已验证成功，失败场景的结构化证据可能不完整。
  title: 当前任务导航
navigation: 当前仅有一个未验证参考块，主题是 ci-new 分支相关问题的环境阻塞、DSpark/P-D/KV smoke 排查建议及证据保留限制。原文包含 tokenizer、模型长度、launcher 路径、健康端点、验证顺序和断言条件；这些均为报告的检查项或建议，不应当作已验证成功，失败场景的结构化证据可能不完整。
schema: mindie-material-task/1
status: complete
task_id: 9b8d182b411a99882e5c2d5afc9063d16258c8633e4ea784af74bb3d0b3f603a
---

# 当前任务导航

当前仅有一个未验证参考块，主题是 ci-new 分支相关问题的环境阻塞、DSpark/P-D/KV smoke 排查建议及证据保留限制。原文包含 tokenizer、模型长度、launcher 路径、健康端点、验证顺序和断言条件；这些均为报告的检查项或建议，不应当作已验证成功，失败场景的结构化证据可能不完整。

## Materials

- [ci-new 与 DSpark 相关问题的未验证复核建议](blocks/4c3885c42e2bd04ccbe982c430a72fa87589459f3f36a2ee337832b2ca353302.md): 针对用户提出的“ci-new 分支问题似乎与 DSpark 无关”，此前助手给出了一份只读复核后的排查与执行建议，但内容仍是待验证参考，不能视为成功案例或已完成验证。记录的环境/API/方法线索包括 tokenizer 缺少 `tiktoken.model`、5 层模型的 `max_model_len=64` 可能不足、P/D wrapper 的 launcher 路径缺失、健康端点为 `/healthcheck`，以及 full-weight、5 层 aggregate、P/D 拓扑和 prefix/KV smoke 的分阶段检查；同时明确保留失败日志、原始 metrics、不确定性和 5 层 workaround 不能证明精度。

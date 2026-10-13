---
blocks:
- block_id: 16ff8460591ef268b96abf4a58cc93763a005c43124977ef1b1b2334a7f35618
  indexed: true
  sha256: 551736cc4413a0ff77781c2e78496831cf88bbd4af8526708593bc8e4a52a15d
  source_range:
    end: 774002
    part: 0
    start: 0
  summary: 只读盘点认为当前没有可直接部署的 completion-only runtime/launcher；建议从自洽的 `b2-async-off-ab-r17` 抽取 B2 请求序列、golden、客户端契约和 P1 verifier，另建无 DFX 的 completion closure。记录的协议包含 reset、miss/warm/hit 请求、SSE/HTTP 200/`[DONE]`/请求 ID/ token 数硬校验，以及 q/h、四 DP preemption delta 和仅 DP0 变化门禁；B2 通过仍不能自动等同于 P1 成功，P1 需显式 `RUN_P1_AFTER_B2=1` 且满足 `all_passed=true && experiment_valid=true`。必须移除 DFX manifest、`dfx-verdict.json`、相关校验和 `k3-dfx-*` ID，并拒绝 DFX 环境变量；`early-postprocess-wait-ab-r17` 存在缺失 pin、待定 SHA、错误 launcher/verifier SHA、残留 DFX 导出和 scheduler mode 泄漏，因此只能作结构参考，不能作为 source/runtime/launcher 输入；本轮未执行 pytest。
  title: B2 completion-only 闭包与 P1 门禁审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1064002351782dc84ae327b7062bd277aab6dec581247b55c88a4de533668f32
  kind: experience
  material_digest: 5d4a6ab975f9d842c204d0afa683d4b1e3637a5ed933978f6355b31f376f635c
  revision: 34f78a896a5195c98bb9b91823ccdec4cbfbc58afeb065065e936650362e2ac3
  schema: mindie-entry/3
  summary: 已有材料聚焦 B2 async-off 协议、golden/contract/P1 独立门禁，以及 completion source 的 commit、SHA 和排除 pin；当前结论是不能直接部署，需新建并完成无 DFX、固定 `--no-async-scheduling`、拒绝相关环境变量且不继承 r18/full-chain 路径的 completion closure。后续检索应优先核对 B2 硬门禁、completion fence UT、source/model-runner/patch/postprocess SHA 和 launcher wrapper 一致性，再单独判断显式 P1；保留“未执行 pytest”、缺失日志和现有候选闭包不可复用等未完成或不确定状态，不将只读检查或计划提升为验证成功。
  title: 当前任务导航：completion-only runtime/launcher 检索索引
navigation: 已有材料聚焦 B2 async-off 协议、golden/contract/P1 独立门禁，以及 completion source 的 commit、SHA 和排除 pin；当前结论是不能直接部署，需新建并完成无 DFX、固定 `--no-async-scheduling`、拒绝相关环境变量且不继承 r18/full-chain 路径的 completion closure。后续检索应优先核对 B2 硬门禁、completion fence UT、source/model-runner/patch/postprocess SHA 和 launcher wrapper 一致性，再单独判断显式 P1；保留“未执行 pytest”、缺失日志和现有候选闭包不可复用等未完成或不确定状态，不将只读检查或计划提升为验证成功。
schema: mindie-material-task/1
status: complete
task_id: 1064002351782dc84ae327b7062bd277aab6dec581247b55c88a4de533668f32
---

# 当前任务导航：completion-only runtime/launcher 检索索引

已有材料聚焦 B2 async-off 协议、golden/contract/P1 独立门禁，以及 completion source 的 commit、SHA 和排除 pin；当前结论是不能直接部署，需新建并完成无 DFX、固定 `--no-async-scheduling`、拒绝相关环境变量且不继承 r18/full-chain 路径的 completion closure。后续检索应优先核对 B2 硬门禁、completion fence UT、source/model-runner/patch/postprocess SHA 和 launcher wrapper 一致性，再单独判断显式 P1；保留“未执行 pytest”、缺失日志和现有候选闭包不可复用等未完成或不确定状态，不将只读检查或计划提升为验证成功。

## Materials

- [B2 completion-only 闭包与 P1 门禁审计](blocks/16ff8460591ef268b96abf4a58cc93763a005c43124977ef1b1b2334a7f35618.md): 只读盘点认为当前没有可直接部署的 completion-only runtime/launcher；建议从自洽的 `b2-async-off-ab-r17` 抽取 B2 请求序列、golden、客户端契约和 P1 verifier，另建无 DFX 的 completion closure。记录的协议包含 reset、miss/warm/hit 请求、SSE/HTTP 200/`[DONE]`/请求 ID/ token 数硬校验，以及 q/h、四 DP preemption delta 和仅 DP0 变化门禁；B2 通过仍不能自动等同于 P1 成功，P1 需显式 `RUN_P1_AFTER_B2=1` 且满足 `all_passed=true && experiment_valid=true`。必须移除 DFX manifest、`dfx-verdict.json`、相关校验和 `k3-dfx-*` ID，并拒绝 DFX 环境变量；`early-postprocess-wait-ab-r17` 存在缺失 pin、待定 SHA、错误 launcher/verifier SHA、残留 DFX 导出和 scheduler mode 泄漏，因此只能作结构参考，不能作为 source/runtime/launcher 输入；本轮未执行 pytest。

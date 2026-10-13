---
blocks:
- block_id: 14bbf012578c18258a31964647e2d3547fc61d82bb0646fedaeb948657d88b11
  indexed: true
  sha256: 87f3e934532475cc4011cd28af9e2e864d94f42f1bb0364e8925b66d21eba991
  source_range:
    end: 956545
    part: 0
    start: 0
  summary: 记录称已完成并部署 128K runner、comparator 与三轮编排脚本，但明确未发送推理、tokenize 或 reset 请求，因此尚未验证真实服务行为。静态审计与离线合成验收声称通过 141 项检查，涉及 DP0–3 精确路由、129792-token shared prefix、miss/hit warmup 规则、token/hash、Prometheus 指标、采信率与 AL 公式；这些仍属于报告的检查结果，不能视为线上成功。后续任务仍需在显式设置 `RUN_R12_128K_ACCEPTANCE=1` 后执行，且应保留三轮、不重启与 reset 第三轮等未完成的真实验证。
  title: 128K 接受测试准备与部署记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7468d2e407006e8d4e913cf7efbf35ba8340af42ed8ecb0d7d76d382e7791b02
  kind: experience
  material_digest: 4b80d97fc047b445d41d528a2e70e62f688ee65ca7f39f3de2239722c1c7bc67
  revision: 216d45aae0a1a4d7aa7b6a398ef5d1adc74bff64ca21bdedde9cf8e55a966c57
  schema: mindie-entry/3
  summary: 当前唯一材料集中于 128K runner/comparator 的离线门禁、部署与真实执行前准备。已报告的 141 项合成检查和远端语法检查不可替代线上请求验证；推理、tokenize、reset 及三轮稳定性仍是未完成或未证实事项。
  title: 当前任务导航：128K 接受测试索引
navigation: 当前唯一材料集中于 128K runner/comparator 的离线门禁、部署与真实执行前准备。已报告的 141 项合成检查和远端语法检查不可替代线上请求验证；推理、tokenize、reset 及三轮稳定性仍是未完成或未证实事项。
schema: mindie-material-task/1
status: complete
task_id: 7468d2e407006e8d4e913cf7efbf35ba8340af42ed8ecb0d7d76d382e7791b02
---

# 当前任务导航：128K 接受测试索引

当前唯一材料集中于 128K runner/comparator 的离线门禁、部署与真实执行前准备。已报告的 141 项合成检查和远端语法检查不可替代线上请求验证；推理、tokenize、reset 及三轮稳定性仍是未完成或未证实事项。

## Materials

- [128K 接受测试准备与部署记录](blocks/14bbf012578c18258a31964647e2d3547fc61d82bb0646fedaeb948657d88b11.md): 记录称已完成并部署 128K runner、comparator 与三轮编排脚本，但明确未发送推理、tokenize 或 reset 请求，因此尚未验证真实服务行为。静态审计与离线合成验收声称通过 141 项检查，涉及 DP0–3 精确路由、129792-token shared prefix、miss/hit warmup 规则、token/hash、Prometheus 指标、采信率与 AL 公式；这些仍属于报告的检查结果，不能视为线上成功。后续任务仍需在显式设置 `RUN_R12_128K_ACCEPTANCE=1` 后执行，且应保留三轮、不重启与 reset 第三轮等未完成的真实验证。

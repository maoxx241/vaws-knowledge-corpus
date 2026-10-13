---
blocks:
- block_id: 731bcf07f23adab8984719173977016952ca1bd333533abdf715a155d6403ba8
  indexed: true
  sha256: 6f5cc2cb5ab4e02aee0e305f425cb0449edf74ed994741b69ecaf41c795801c9
  source_range:
    end: 665015
    part: 0
    start: 0
  summary: 只读审查认为当前 diff 未发现阻断两机冒烟或四机验证的静态问题，但真实 5 层权重的 AutoWeightsLoader、DP2/DP4 uneven/idle-rank、NPU runtime、接受率、精度及特性叠加仍未完成实际验收；Mooncake coverage marker 也不能证明接受率。记录称普通 prefix-cache、KV group 传输、verifier reorder 边界、draft-vocab 映射和初始化 block-size 传递已按预期修改，但这些结论不应替代后续运行验证。后续只读核对发现验证脚本存在假阳性风险：零命中或 reset 无效、accepted_tokens 为 0、非可靠的 content + reasoning_content 比较、失败 aggregate 仍写出 PD reference，以及未强制 warm local prefix hit；本地 0.23 API、Prometheus Counter `_total` 解析和 P/D proxy 字段转发据报告核对正常。
  title: DSpark/Mooncake 静态审查与验证脚本风险
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e66b3cec736044198c042cdb15d11eb1550e2a3792a6dff940f853d5c9f08af5
  kind: experience
  material_digest: e2557878b62426f737bedc17d6eb721b56291d5d9628480a6ae93b68f61c9ca0
  revision: ebfdb426898853fbbebf273b904c3fd16a126de5cdc8c8a48c32c127eec849ca
  schema: mindie-entry/3
  summary: 已有静态审查记录及未完成的运行验收项；重点追踪 AutoWeightsLoader、DP uneven/idle-rank、接受率、精度和 NPU runtime。验证脚本存在可能掩盖 prefix-cache、draft KV、PD 路径或全拒收问题的假阳性，后续应保留这些不确定性并分别核验命中、accepted_tokens、reference 生成和输出比较。
  title: 当前任务导航：DSpark/Mooncake 索引与验证风险
navigation: 已有静态审查记录及未完成的运行验收项；重点追踪 AutoWeightsLoader、DP uneven/idle-rank、接受率、精度和 NPU runtime。验证脚本存在可能掩盖 prefix-cache、draft KV、PD 路径或全拒收问题的假阳性，后续应保留这些不确定性并分别核验命中、accepted_tokens、reference 生成和输出比较。
schema: mindie-material-task/1
status: complete
task_id: e66b3cec736044198c042cdb15d11eb1550e2a3792a6dff940f853d5c9f08af5
---

# 当前任务导航：DSpark/Mooncake 索引与验证风险

已有静态审查记录及未完成的运行验收项；重点追踪 AutoWeightsLoader、DP uneven/idle-rank、接受率、精度和 NPU runtime。验证脚本存在可能掩盖 prefix-cache、draft KV、PD 路径或全拒收问题的假阳性，后续应保留这些不确定性并分别核验命中、accepted_tokens、reference 生成和输出比较。

## Materials

- [DSpark/Mooncake 静态审查与验证脚本风险](blocks/731bcf07f23adab8984719173977016952ca1bd333533abdf715a155d6403ba8.md): 只读审查认为当前 diff 未发现阻断两机冒烟或四机验证的静态问题，但真实 5 层权重的 AutoWeightsLoader、DP2/DP4 uneven/idle-rank、NPU runtime、接受率、精度及特性叠加仍未完成实际验收；Mooncake coverage marker 也不能证明接受率。记录称普通 prefix-cache、KV group 传输、verifier reorder 边界、draft-vocab 映射和初始化 block-size 传递已按预期修改，但这些结论不应替代后续运行验证。后续只读核对发现验证脚本存在假阳性风险：零命中或 reset 无效、accepted_tokens 为 0、非可靠的 content + reasoning_content 比较、失败 aggregate 仍写出 PD reference，以及未强制 warm local prefix hit；本地 0.23 API、Prometheus Counter `_total` 解析和 P/D proxy 字段转发据报告核对正常。

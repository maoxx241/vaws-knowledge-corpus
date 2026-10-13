---
blocks:
- block_id: f1ef286d1bf461e47317d4d2390aef551666ae371cd4176ce5e66aab401c107a
  indexed: true
  sha256: f19a41c245f64a4d29846642ada8445cd7d54ab6dc54ded254a89cf37c9da1f3
  source_range:
    end: 796467
    part: 0
    start: 0
  summary: 只读检查指出现有脚本串行运行、缺少 X-data-parallel-rank、不统计 DSpark，且 cars.jpg 超出 draft 长度限制；建议仅使用 tiger.jpeg，并按实际 vLLM spec_decode 指标做差分。提出了四 rank 的 ViT、重复请求、chat 和 tool-call 验证顺序，以及 HTTP、输出一致性、tool_calls、prefix cache 和 DSpark 的判定标准，但这些是待执行门禁，不能视为已验证成功。旧服务的累计接收率 3353/5932（56.5239%）和平均接受长度 4.8853 被报告为与历史基线一致；正式 GSM8K runner、数据集哈希和 jq 阈值也被给出，但 runner 退出状态不会将 accuracy 自动视为硬失败，仍需额外检查。
  title: 多模态 DP 四 Rank 门禁与 DSpark 指标核对
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a2475894b7ac94deae46f6e0b06179ebe6a9440e69cfda3e4da40f4585f72aa2
  kind: experience
  material_digest: b2b169b9680b601375fcd2181f8bdc1d49abae5cfd4c96318f6f678edb34b60b
  revision: 837be50b7c456c993a3d68c89c36d42f1776e540f2fc5a8bd0f5094a8f35d514
  schema: mindie-entry/3
  summary: 已索引 1 个新材料块，主题为多模态服务的四 DP rank 验证、实际 spec_decode 指标、DSpark/prefix cache 门禁及正式接收率回归检查。材料包含已观察的脚本与旧服务数据，也包含尚待执行的命令和判定标准；后续检索时应区分报告值、建议方案与尚未验证的结果。
  title: 当前任务：为新材料建立检索索引并维护短导航
navigation: 已索引 1 个新材料块，主题为多模态服务的四 DP rank 验证、实际 spec_decode 指标、DSpark/prefix cache 门禁及正式接收率回归检查。材料包含已观察的脚本与旧服务数据，也包含尚待执行的命令和判定标准；后续检索时应区分报告值、建议方案与尚未验证的结果。
schema: mindie-material-task/1
status: complete
task_id: a2475894b7ac94deae46f6e0b06179ebe6a9440e69cfda3e4da40f4585f72aa2
---

# 当前任务：为新材料建立检索索引并维护短导航

已索引 1 个新材料块，主题为多模态服务的四 DP rank 验证、实际 spec_decode 指标、DSpark/prefix cache 门禁及正式接收率回归检查。材料包含已观察的脚本与旧服务数据，也包含尚待执行的命令和判定标准；后续检索时应区分报告值、建议方案与尚未验证的结果。

## Materials

- [多模态 DP 四 Rank 门禁与 DSpark 指标核对](blocks/f1ef286d1bf461e47317d4d2390aef551666ae371cd4176ce5e66aab401c107a.md): 只读检查指出现有脚本串行运行、缺少 X-data-parallel-rank、不统计 DSpark，且 cars.jpg 超出 draft 长度限制；建议仅使用 tiger.jpeg，并按实际 vLLM spec_decode 指标做差分。提出了四 rank 的 ViT、重复请求、chat 和 tool-call 验证顺序，以及 HTTP、输出一致性、tool_calls、prefix cache 和 DSpark 的判定标准，但这些是待执行门禁，不能视为已验证成功。旧服务的累计接收率 3353/5932（56.5239%）和平均接受长度 4.8853 被报告为与历史基线一致；正式 GSM8K runner、数据集哈希和 jq 阈值也被给出，但 runner 退出状态不会将 accuracy 自动视为硬失败，仍需额外检查。

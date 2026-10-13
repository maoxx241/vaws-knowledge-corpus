---
blocks:
- block_id: c9c47b9682ce21d5dcd9ec9aab167a3d8783aa2032228fa56a3886421b043d9f
  indexed: true
  sha256: 0c0b453b4d34a608bdea5a1b8c10c3edadf2f587a36611fa0cc1a7cdf615744e
  source_range:
    end: 1206688
    part: 0
    start: 0
  summary: 完成只读 PR 审查，识别并确认若干共性修复方向：移除固定 Qwen3 DSpark block_size=7、直接使用 per-group block sizes、保留 KDA dummy row 清零，并收缩模型 UT 的非关键断言；未修改文件。审查认为普通 Qwen3-8B 不受 materialized aux stream 影响，parser/tool/reasoning 与上游 Kimi K3 核心语义基本对齐，但 vLLM v0.26.0 缺少原生 K3 支持，不能直接替换当前 release bridge。四机 ViT 验证尚未执行，建议覆盖 TP16 下 12-head DP fallback、图模式 decode、DSpark、prefix cache，以及图像与纯文本共存，并移除 `--language-model-only`、开启 image limit 与 profiling。
  title: PR 检视意见与 ViT 四机验证建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 54673f4b3c79023931b40aacc19a97604dd3297f2cc7b3020cff987a3cdd6321
  kind: experience
  material_digest: 3fe70996910b90e217066081c577dd17c290ab7666737d78eced4bd67cbc6213
  revision: e583d9c338600ff4641d4aaefbe15aec6c5871d05717488e437cdf191bf6ab99
  schema: mindie-entry/3
  summary: 当前材料记录了一次未改文件的只读审查及修复建议，重点涉及 Qwen3 DSpark block_size 契约、proposer block-table 异常探测、KDA dummy 输出清零、模型 UT 组织，以及 Kimi K3/vLLM v0.26 兼容性。建议后续先按 review 意见更新 PR，再运行覆盖多模态与纯文本共存场景的四机 ViT case；该四机验证在本材料中仍未完成，不能视为已验证成功。
  title: PR 修复与 ViT 四机验证
navigation: 当前材料记录了一次未改文件的只读审查及修复建议，重点涉及 Qwen3 DSpark block_size 契约、proposer block-table 异常探测、KDA dummy 输出清零、模型 UT 组织，以及 Kimi K3/vLLM v0.26 兼容性。建议后续先按 review 意见更新 PR，再运行覆盖多模态与纯文本共存场景的四机 ViT case；该四机验证在本材料中仍未完成，不能视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 54673f4b3c79023931b40aacc19a97604dd3297f2cc7b3020cff987a3cdd6321
---

# PR 修复与 ViT 四机验证

当前材料记录了一次未改文件的只读审查及修复建议，重点涉及 Qwen3 DSpark block_size 契约、proposer block-table 异常探测、KDA dummy 输出清零、模型 UT 组织，以及 Kimi K3/vLLM v0.26 兼容性。建议后续先按 review 意见更新 PR，再运行覆盖多模态与纯文本共存场景的四机 ViT case；该四机验证在本材料中仍未完成，不能视为已验证成功。

## Materials

- [PR 检视意见与 ViT 四机验证建议](blocks/c9c47b9682ce21d5dcd9ec9aab167a3d8783aa2032228fa56a3886421b043d9f.md): 完成只读 PR 审查，识别并确认若干共性修复方向：移除固定 Qwen3 DSpark block_size=7、直接使用 per-group block sizes、保留 KDA dummy row 清零，并收缩模型 UT 的非关键断言；未修改文件。审查认为普通 Qwen3-8B 不受 materialized aux stream 影响，parser/tool/reasoning 与上游 Kimi K3 核心语义基本对齐，但 vLLM v0.26.0 缺少原生 K3 支持，不能直接替换当前 release bridge。四机 ViT 验证尚未执行，建议覆盖 TP16 下 12-head DP fallback、图模式 decode、DSpark、prefix cache，以及图像与纯文本共存，并移除 `--language-model-only`、开启 image limit 与 profiling。

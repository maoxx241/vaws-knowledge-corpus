---
blocks:
- block_id: 538cafd0c4168a3b7ddd755d3f80d1380f7e9a6dd32556badcb755c45196264d
  indexed: true
  sha256: 1b10ed2e1aec076ab9ba99cd0fa6e6419191d0237cea1bd8f5228e4e4500bc8a
  source_range:
    end: 1170427
    part: 0
    start: 0
  summary: 只读核对记录指出，当前四机服务使用 `--language-model-only` 与 `--limit-mm-per-prompt '{"image":0}'，因此不能直接验证 ViT；建议替换为移除前者、将图片限制设为 1，并保留既有多模态相关参数。方案建议使用标准 OpenAI `image_url`、原生 `kimi_k3` renderer、Tiger 图片及四个 DP rank 的重复请求，验收项包括 HTTP 200、`finish_reason=stop`、语义关键词、日志错误排查和 metrics 变化；这些是待执行条件，不是已验证成功。历史 `.85` 显存配置曾在节点 155 失败，当前 `.82` 接近边界，建议尝试 `.80`；原始 `cars.jpg` 的视觉 token 可能超过 draft 上限，需缩放后再测。记录还保留了预期 warning/eager 路径、源码干净与健康检查 200 等观察，但后续仍需在 review 修复后精准替换服务并执行门禁。
  title: 四机 ViT 验证方案与只读现状
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1a8fa8e5c133f7e833dbf8699f56c1639b0600854cacc4d62b2cfbf24ff74f30
  kind: experience
  material_digest: 810e8a6eafa581a844695a14c135386d526215a59bdb7a5c2b6a95bb1a54ccd4
  revision: b206af1134a878cef9227eb391fb58ad78280cfdfe99e1090e964d3d0718a2c2
  schema: mindie-entry/3
  summary: 唯一新材料块集中记录了四机 ViT 验证的只读核对结果、启动参数差异、输入素材限制、建议验收条件及未完成的后续执行。当前服务尚未完成该 ViT 门禁验证；显存边界、视觉 token 上限、预期 warning 与潜在 OOM/placeholder mismatch 等不确定性和失败风险仍需保留并在后续结果中修正。
  title: 当前任务导航：四机 ViT 多模态验证准备
navigation: 唯一新材料块集中记录了四机 ViT 验证的只读核对结果、启动参数差异、输入素材限制、建议验收条件及未完成的后续执行。当前服务尚未完成该 ViT 门禁验证；显存边界、视觉 token 上限、预期 warning 与潜在 OOM/placeholder mismatch 等不确定性和失败风险仍需保留并在后续结果中修正。
schema: mindie-material-task/1
status: complete
task_id: 1a8fa8e5c133f7e833dbf8699f56c1639b0600854cacc4d62b2cfbf24ff74f30
---

# 当前任务导航：四机 ViT 多模态验证准备

唯一新材料块集中记录了四机 ViT 验证的只读核对结果、启动参数差异、输入素材限制、建议验收条件及未完成的后续执行。当前服务尚未完成该 ViT 门禁验证；显存边界、视觉 token 上限、预期 warning 与潜在 OOM/placeholder mismatch 等不确定性和失败风险仍需保留并在后续结果中修正。

## Materials

- [四机 ViT 验证方案与只读现状](blocks/538cafd0c4168a3b7ddd755d3f80d1380f7e9a6dd32556badcb755c45196264d.md): 只读核对记录指出，当前四机服务使用 `--language-model-only` 与 `--limit-mm-per-prompt '{"image":0}'，因此不能直接验证 ViT；建议替换为移除前者、将图片限制设为 1，并保留既有多模态相关参数。方案建议使用标准 OpenAI `image_url`、原生 `kimi_k3` renderer、Tiger 图片及四个 DP rank 的重复请求，验收项包括 HTTP 200、`finish_reason=stop`、语义关键词、日志错误排查和 metrics 变化；这些是待执行条件，不是已验证成功。历史 `.85` 显存配置曾在节点 155 失败，当前 `.82` 接近边界，建议尝试 `.80`；原始 `cars.jpg` 的视觉 token 可能超过 draft 上限，需缩放后再测。记录还保留了预期 warning/eager 路径、源码干净与健康检查 200 等观察，但后续仍需在 review 修复后精准替换服务并执行门禁。

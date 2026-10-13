---
blocks:
- block_id: c8c0d4d83bae05fbed7dc1cf980ae52f0dfd9f36aa59f5bc72bec45245dff669
  indexed: true
  sha256: b4df0a85931d0f7b913e6b74a76fe2ff180aa9e5d6bd94a9a813c83556eb1a87
  source_range:
    end: 5275362
    part: 0
    start: 0
  summary: '围绕主线 GQA DSpark 采信率异常，对比了 PR #14454 的已测基线：SHA `3053909a` 在四机 GQA、7 个 draft token、8K/1K、每 DP 并发 1 条件下为 76.22%，并在 153+154 进行 TP16 × DP2 × EP32 双机验证。代码审计确认 draft 阶段复用序列长度会导致 MLA KV 长度多算被拒绝 token，但 GQA 会覆盖为 device 上的正确长度，因此该缺陷不能解释当前 GQA 反馈，仍需检查 target 辅助 hidden states、KDA 状态更新及 MarkovHead／采样链路。验证尚未完成；启动时另遇到普通 W4A8 checkpoint 的 `mm_projector.rot_proj` 权重加载兼容错误，已保留失败日志并计划用纯文本模式排除该独立阻碍。'
  title: GQA DSpark 采信率回归排查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 42777002a810f8f811f82aea9286d0c1bdcff187fdbd8435d78e70aa2bb7a9c0
  kind: experience
  material_digest: c61e7132ca16bdb2cfc093f2bc9e2d024749ec7f12b2ecad71d98dad2c80cd94
  revision: e60caad57257decf81db864b2686dfd0b36c1c40e479ed3da1d64ced9664edc4
  schema: mindie-entry/3
  summary: '当前重点是定位主线 GQA DSpark 采信率问题，参考 #14454 的 `3053909a` 基线并在 153+154 做双机对照。MLA 序列长度缺陷已确认但不适用于 GQA，不能作为本次根因；GQA 核心代码合入后未再改，需继续追查 #14454 最终合入前后的辅助 hidden states、KDA 与采样链路。真实请求验证尚未完成，且曾被 `mm_projector.rot_proj` checkpoint 加载兼容问题阻断。'
  title: DSpark GQA 采信率回归排查
navigation: '当前重点是定位主线 GQA DSpark 采信率问题，参考 #14454 的 `3053909a` 基线并在 153+154 做双机对照。MLA 序列长度缺陷已确认但不适用于 GQA，不能作为本次根因；GQA 核心代码合入后未再改，需继续追查 #14454 最终合入前后的辅助 hidden states、KDA 与采样链路。真实请求验证尚未完成，且曾被 `mm_projector.rot_proj` checkpoint 加载兼容问题阻断。'
schema: mindie-material-task/1
status: complete
task_id: 42777002a810f8f811f82aea9286d0c1bdcff187fdbd8435d78e70aa2bb7a9c0
---

# DSpark GQA 采信率回归排查

当前重点是定位主线 GQA DSpark 采信率问题，参考 #14454 的 `3053909a` 基线并在 153+154 做双机对照。MLA 序列长度缺陷已确认但不适用于 GQA，不能作为本次根因；GQA 核心代码合入后未再改，需继续追查 #14454 最终合入前后的辅助 hidden states、KDA 与采样链路。真实请求验证尚未完成，且曾被 `mm_projector.rot_proj` checkpoint 加载兼容问题阻断。

## Materials

- [GQA DSpark 采信率回归排查](blocks/c8c0d4d83bae05fbed7dc1cf980ae52f0dfd9f36aa59f5bc72bec45245dff669.md): 围绕主线 GQA DSpark 采信率异常，对比了 PR #14454 的已测基线：SHA `3053909a` 在四机 GQA、7 个 draft token、8K/1K、每 DP 并发 1 条件下为 76.22%，并在 153+154 进行 TP16 × DP2 × EP32 双机验证。代码审计确认 draft 阶段复用序列长度会导致 MLA KV 长度多算被拒绝 token，但 GQA 会覆盖为 device 上的正确长度，因此该缺陷不能解释当前 GQA 反馈，仍需检查 target 辅助 hidden states、KDA 状态更新及 MarkovHead／采样链路。验证尚未完成；启动时另遇到普通 W4A8 checkpoint 的 `mm_projector.rot_proj` 权重加载兼容错误，已保留失败日志并计划用纯文本模式排除该独立阻碍。

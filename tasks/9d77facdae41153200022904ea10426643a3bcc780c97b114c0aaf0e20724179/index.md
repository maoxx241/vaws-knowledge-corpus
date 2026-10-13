---
blocks:
- block_id: 95add889c824c7e27630d10bb73dcaca6eadcf5d5173be5e555ee948312d83a8
  indexed: true
  sha256: 4391d5d40dcfa9302e89e34163e4bc14cfe48bfbbfdc642af21e898c971c4c34
  source_range:
    end: 3775364
    part: 0
    start: 0
  summary: KVV 测试中发现非法 typed arguments 和空 tool name 导致严格 KimiK3 XTML parser 异常，异常经 serving 转为 APIError 并触发重试；EngineDeadError 后确认属于手动 kill。讨论后将方向调整为保留严格校验、仅对模型生成的非法 XTML 做 parser 顶层 fail-soft，避免输出 tool call，不改造 serving 的 finish_reason；请求配置错误和 parser 内部状态错误仍应显式失败。随后用户要求删除严格异常型 UT，并使用 4 机、multicards-0724 权重和 KVV 验证，但当时实际旧服务仍占用端口 18079，用户授权直接清理。
  title: K3 XTML 解析异常与 fail-soft 重构方向
- block_id: 03623f414fbeab2b21d03d56813eae3f79c1d2b55e8e5fc8f751752a72c990ef
  indexed: true
  sha256: b10cd53e78ebd72b58025fd37fcf276120f9fe1e1cf35176754d4767b58cdc67
  source_range:
    end: 3775364
    part: 1
    start: 0
  summary: 四个 0722/18079 服务组已 TERM 清理，隔离环境同步到 11ed9e97；parser 改动后的 UT 报告为 48 passed，四机均加载 multicards-0724，配置为 TP16×DP4，服务最终 health=200。KVV 113-case 测试已启动，前 35 个样本未出现 APIError、stream error 或 fail-soft warning，说明运行稳定性符合预期，但 scorer 暂为 13 C / 22 I（37.1%），低于上一轮相同样本的 22 C / 13 I（62.9%）。测试尚未完成，当前不能据此确认最终精度，且需考虑随机性与逐 case 差异。
  title: 0724 四机 KVV 验证进行中，稳定性与精度暂不一致
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 9d77facdae41153200022904ea10426643a3bcc780c97b114c0aaf0e20724179
  kind: experience
  material_digest: f59d2b7d539b67d247614128964f4c41472d4148462fa06ab629df8c7b0b863b
  revision: 9aac1a97ea8af4515e8dfa3721392a326df7799e56a9de30d0325521b604402e
  schema: mindie-entry/3
  summary: 当前任务聚焦 Kimi K3 XTML 非法工具输出的容错重构：严格 parser 保留校验，模型生成错误在 parser 顶层 fail-soft，避免 APIError、重试和非法 tool call，同时区分配置错误与内部状态错误。旧 0722 服务已清理，四机已用 11ed9e97 和 multicards-0724 启动并完成 48 项 UT；KVV 仍在运行，前 35 个样本服务稳定但 scorer 精度暂为 37.1%，低于上一轮 62.9%，最终结论待 113 个样本完成后再定。
  title: K3 parser fail-soft 重构与 0724 KVV 验证
navigation: 当前任务聚焦 Kimi K3 XTML 非法工具输出的容错重构：严格 parser 保留校验，模型生成错误在 parser 顶层 fail-soft，避免 APIError、重试和非法 tool call，同时区分配置错误与内部状态错误。旧 0722 服务已清理，四机已用 11ed9e97 和 multicards-0724 启动并完成 48 项 UT；KVV 仍在运行，前 35 个样本服务稳定但 scorer 精度暂为 37.1%，低于上一轮 62.9%，最终结论待 113 个样本完成后再定。
schema: mindie-material-task/1
status: complete
task_id: 9d77facdae41153200022904ea10426643a3bcc780c97b114c0aaf0e20724179
---

# K3 parser fail-soft 重构与 0724 KVV 验证

当前任务聚焦 Kimi K3 XTML 非法工具输出的容错重构：严格 parser 保留校验，模型生成错误在 parser 顶层 fail-soft，避免 APIError、重试和非法 tool call，同时区分配置错误与内部状态错误。旧 0722 服务已清理，四机已用 11ed9e97 和 multicards-0724 启动并完成 48 项 UT；KVV 仍在运行，前 35 个样本服务稳定但 scorer 精度暂为 37.1%，低于上一轮 62.9%，最终结论待 113 个样本完成后再定。

## Materials

- [K3 XTML 解析异常与 fail-soft 重构方向](blocks/95add889c824c7e27630d10bb73dcaca6eadcf5d5173be5e555ee948312d83a8.md): KVV 测试中发现非法 typed arguments 和空 tool name 导致严格 KimiK3 XTML parser 异常，异常经 serving 转为 APIError 并触发重试；EngineDeadError 后确认属于手动 kill。讨论后将方向调整为保留严格校验、仅对模型生成的非法 XTML 做 parser 顶层 fail-soft，避免输出 tool call，不改造 serving 的 finish_reason；请求配置错误和 parser 内部状态错误仍应显式失败。随后用户要求删除严格异常型 UT，并使用 4 机、multicards-0724 权重和 KVV 验证，但当时实际旧服务仍占用端口 18079，用户授权直接清理。
- [0724 四机 KVV 验证进行中，稳定性与精度暂不一致](blocks/03623f414fbeab2b21d03d56813eae3f79c1d2b55e8e5fc8f751752a72c990ef.md): 四个 0722/18079 服务组已 TERM 清理，隔离环境同步到 11ed9e97；parser 改动后的 UT 报告为 48 passed，四机均加载 multicards-0724，配置为 TP16×DP4，服务最终 health=200。KVV 113-case 测试已启动，前 35 个样本未出现 APIError、stream error 或 fail-soft warning，说明运行稳定性符合预期，但 scorer 暂为 13 C / 22 I（37.1%），低于上一轮相同样本的 22 C / 13 I（62.9%）。测试尚未完成，当前不能据此确认最终精度，且需考虑随机性与逐 case 差异。

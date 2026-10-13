---
blocks:
- block_id: 2debd60de2e6b597aecb1db8cfa2376118069e61c5fc389f2ec78d76f4602451
  indexed: true
  sha256: 1fa02dfbb175d4e3239b0aca85266bd6e7fb1aab25e1b6a2841d0555078d0e26
  source_range:
    end: 833234
    part: 0
    start: 0
  summary: 消息称已审查并修补验收脚本，收紧 Prefix hit 判定：128K 历史正确回放值应为每请求 129,408 tokens，并加入完整输出 token 证据、aggregate/逐 DP 采信率及跨轮漂移检查；但同一消息明确表示尚未发送远程推理请求、未修改生产源码，因此这些检查结果尚未验证。记录了通过 SSH/SCP 部署脚本、校验 SHA-256、运行三轮 miss/hit、并发 1/4、四 DP spread 及最终比较脚本的方法和依赖路径；执行结果、服务状态与后续未完成情况仍待确认。
  title: 验收脚本修补与四 DP 三轮矩阵准备
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b4d797ecee956e4718f332bdf23c4a7eb6a7064e5f9414255b409a3c7e78e6cd
  kind: experience
  material_digest: f70ee062402b025dd63d2683e7b7886d9ce89f788895fa1ab8132d853546e979
  revision: 8a97b4ade8e2e03a6051afe0bf5dc438228c5274905ee7ba284e67577d53ecd0
  schema: mindie-entry/3
  summary: 当前材料集中于未执行完成的验收准备：脚本修补、129792-token warmup、129408-token replay 强校验、完整输出证据，以及四 DP 三轮 miss/hit 对比。远程请求尚未发送，故任何 Prefix 命中、采信率、跨轮一致性或漂移结论都不能视为验证成功；后续应独立检索部署命令、依赖/哈希校验和最终 comparison 输出。
  title: 当前任务导航：验收脚本与四 DP Prefix 矩阵
navigation: 当前材料集中于未执行完成的验收准备：脚本修补、129792-token warmup、129408-token replay 强校验、完整输出证据，以及四 DP 三轮 miss/hit 对比。远程请求尚未发送，故任何 Prefix 命中、采信率、跨轮一致性或漂移结论都不能视为验证成功；后续应独立检索部署命令、依赖/哈希校验和最终 comparison 输出。
schema: mindie-material-task/1
status: complete
task_id: b4d797ecee956e4718f332bdf23c4a7eb6a7064e5f9414255b409a3c7e78e6cd
---

# 当前任务导航：验收脚本与四 DP Prefix 矩阵

当前材料集中于未执行完成的验收准备：脚本修补、129792-token warmup、129408-token replay 强校验、完整输出证据，以及四 DP 三轮 miss/hit 对比。远程请求尚未发送，故任何 Prefix 命中、采信率、跨轮一致性或漂移结论都不能视为验证成功；后续应独立检索部署命令、依赖/哈希校验和最终 comparison 输出。

## Materials

- [验收脚本修补与四 DP 三轮矩阵准备](blocks/2debd60de2e6b597aecb1db8cfa2376118069e61c5fc389f2ec78d76f4602451.md): 消息称已审查并修补验收脚本，收紧 Prefix hit 判定：128K 历史正确回放值应为每请求 129,408 tokens，并加入完整输出 token 证据、aggregate/逐 DP 采信率及跨轮漂移检查；但同一消息明确表示尚未发送远程推理请求、未修改生产源码，因此这些检查结果尚未验证。记录了通过 SSH/SCP 部署脚本、校验 SHA-256、运行三轮 miss/hit、并发 1/4、四 DP spread 及最终比较脚本的方法和依赖路径；执行结果、服务状态与后续未完成情况仍待确认。

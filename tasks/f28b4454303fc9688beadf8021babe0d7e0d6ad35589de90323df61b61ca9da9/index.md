---
blocks:
- block_id: df5d5f1af898e7dbd881eb551d8ff13bae1a2bbf67e019026473f0499d2a2b2f
  indexed: true
  sha256: 43848ca9d20add7b9a82ed94eb9d8ddda59003f73101664c1526681ddc28f9b6
  source_range:
    end: 2988426
    part: 0
    start: 0
  summary: 围绕 Kimi K3 峰值激活随 max_model_len 增长的疑问，记录了 5 层 INT4、TP8、固定 max_num_batched_tokens=8192 的多组 profile；纯语言及 EP/FlashComm 对照中，8192–327680 的峰值分别保持约 1.97 GiB 和 0.83 GiB，但完整 70K 轮次在权重加载阶段被停止，未取得 peak。曾误判 MLA dummy workspace 可能按 max_model_len 分配，后经 OOT wrapper 与空 metadata profile 分支复核排除；静态审计还指出 profile 指标实际是 Torch allocated 峰值增量，历史日志混用了 VIT、vision item、batch token、版本等变量，尚不能将观察到的差异归因于 max_model_len。期间发生 hostname/Gloo 初始化失败、旧扩展缺少 dequant_situ_quant、ASCEND_CUSTOM_OPP_PATH 未定义等环境/启动问题；多次精确清理 PGID 后资源释放，但未完成最终全量 A/B，且后续服务、GPQA 与显存 profile 之间存在暂停和占用边界。
  title: K3 显存 profile：减层实验、全量暂停与证据审计
- block_id: a38aeb9c837550cd832c8b5716752b7771a167de50e5ac3c5a7da9e89e06604a
  indexed: true
  sha256: 9fab305fb14956cc4c0957a071b811b832d07a1d6dad3a9128c89c245975241b
  source_range:
    end: 2988426
    part: 1
    start: 0
  summary: 四机 `qjj_k3` 显存 profile 曾按 PID/PGID 精确停止，153 的 PGID 500 需补发 KILL，其他节点 TERM 后退出；随后核验称 64/64 张卡释放、容器仍运行、日志保留，但该轮停在权重加载阶段，没有 peak 结果。FlashComm1/shared-expert-DP 服务和后续 ci-new GPQA 按精确 PGID 管理，期间要求保持显存 profile 暂停；GPQA 释放后又启动了纯语言、vision=0、8192 tokens、EP/FC1 的 70K 严格轮次，但最终用户要求停止，四个 PGID（48053、37405、37219、37263）被清理。最终状态是卡已释放、容器未停止、profile 端口已释放，本轮没有产生 peak activation，不能视为完成验证。
  title: 四机资源释放、服务暂停与未完成的 70K A/B
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f28b4454303fc9688beadf8021babe0d7e0d6ad35589de90323df61b61ca9da9
  kind: experience
  material_digest: 4bd9790be839342af98210ff61955cdbb36db4a57ddb32dd42612d5c8a8a3278
  revision: 7894ca0f621dba49c3086bececb141585f10298e085a8e743e4fd8ee3511f77f
  schema: mindie-entry/3
  summary: 可检索重点包括：固定 batch-token 下 5 层 profile 未观察到 max_model_len 依赖；历史 0.32→1.89 GiB 差异可能来自 language-only/full-MM、vision item、batch tokens 与版本混杂；MLA/KDA 空 metadata profile 分支、FlashComm/shared-expert-DP 解耦及指标口径均有审计记录。四机当前 profile 已停止且 64 张卡释放，但完整模型严格 A/B 未拿到 peak，后续任何恢复运行都需区分已完成证据、失败启动、暂停任务和未验证假设。
  title: 当前任务导航：K3 显存问题与四机资源状态
navigation: 可检索重点包括：固定 batch-token 下 5 层 profile 未观察到 max_model_len 依赖；历史 0.32→1.89 GiB 差异可能来自 language-only/full-MM、vision item、batch tokens 与版本混杂；MLA/KDA 空 metadata profile 分支、FlashComm/shared-expert-DP 解耦及指标口径均有审计记录。四机当前 profile 已停止且 64 张卡释放，但完整模型严格 A/B 未拿到 peak，后续任何恢复运行都需区分已完成证据、失败启动、暂停任务和未验证假设。
schema: mindie-material-task/1
status: complete
task_id: f28b4454303fc9688beadf8021babe0d7e0d6ad35589de90323df61b61ca9da9
---

# 当前任务导航：K3 显存问题与四机资源状态

可检索重点包括：固定 batch-token 下 5 层 profile 未观察到 max_model_len 依赖；历史 0.32→1.89 GiB 差异可能来自 language-only/full-MM、vision item、batch tokens 与版本混杂；MLA/KDA 空 metadata profile 分支、FlashComm/shared-expert-DP 解耦及指标口径均有审计记录。四机当前 profile 已停止且 64 张卡释放，但完整模型严格 A/B 未拿到 peak，后续任何恢复运行都需区分已完成证据、失败启动、暂停任务和未验证假设。

## Materials

- [K3 显存 profile：减层实验、全量暂停与证据审计](blocks/df5d5f1af898e7dbd881eb551d8ff13bae1a2bbf67e019026473f0499d2a2b2f.md): 围绕 Kimi K3 峰值激活随 max_model_len 增长的疑问，记录了 5 层 INT4、TP8、固定 max_num_batched_tokens=8192 的多组 profile；纯语言及 EP/FlashComm 对照中，8192–327680 的峰值分别保持约 1.97 GiB 和 0.83 GiB，但完整 70K 轮次在权重加载阶段被停止，未取得 peak。曾误判 MLA dummy workspace 可能按 max_model_len 分配，后经 OOT wrapper 与空 metadata profile 分支复核排除；静态审计还指出 profile 指标实际是 Torch allocated 峰值增量，历史日志混用了 VIT、vision item、batch token、版本等变量，尚不能将观察到的差异归因于 max_model_len。期间发生 hostname/Gloo 初始化失败、旧扩展缺少 dequant_situ_quant、ASCEND_CUSTOM_OPP_PATH 未定义等环境/启动问题；多次精确清理 PGID 后资源释放，但未完成最终全量 A/B，且后续服务、GPQA 与显存 profile 之间存在暂停和占用边界。
- [四机资源释放、服务暂停与未完成的 70K A/B](blocks/a38aeb9c837550cd832c8b5716752b7771a167de50e5ac3c5a7da9e89e06604a.md): 四机 `qjj_k3` 显存 profile 曾按 PID/PGID 精确停止，153 的 PGID 500 需补发 KILL，其他节点 TERM 后退出；随后核验称 64/64 张卡释放、容器仍运行、日志保留，但该轮停在权重加载阶段，没有 peak 结果。FlashComm1/shared-expert-DP 服务和后续 ci-new GPQA 按精确 PGID 管理，期间要求保持显存 profile 暂停；GPQA 释放后又启动了纯语言、vision=0、8192 tokens、EP/FC1 的 70K 严格轮次，但最终用户要求停止，四个 PGID（48053、37405、37219、37263）被清理。最终状态是卡已释放、容器未停止、profile 端口已释放，本轮没有产生 peak activation，不能视为完成验证。

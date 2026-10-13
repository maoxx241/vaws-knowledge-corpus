---
blocks:
- block_id: 3ac8a38779369baafa0d89a856822c9785dbce74b7457d66f466a189b11bba50
  indexed: true
  sha256: 35d22eccb9ceb4fc80fb1a5a5578388129e2d59a6a91e9520027f5c3caa009be
  source_range:
    end: 1026370
    part: 0
    start: 0
  summary: 静态审查认为 diff 未发现阻断两机冒烟或四机验证的问题，但真实 5 层权重的 AutoWeightsLoader、DP2/DP4 uneven/idle-rank、Mooncake 接受率、NPU runtime、精度及特性叠加仍未验证；保留了 fake loader、coverage marker 不等于接受率等不确定性。启动器新增识别出两个接机前 P0：未固定 VLLM_HOST_IP，以及无法按精确 PGID 安全清理；另有四机上下文长度、PD proxy、rank 映射和端口校验等 P1/P2 风险。已确认基础 DP/TP/EP、prefix-cache 参数、PD 映射、Mooncake producer/consumer 配置、K=7 capture、bash -n 和 shellcheck 通过，但未连接远端、启动服务或修改文件。
  title: DSpark 静态审查与启动器风险索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 639a7e30e32d52fce9aac7d8f788ac213375e6d887521a6c0f87b6e4d7f17fe0
  kind: experience
  material_digest: 32e8aa0a7d8fba63f9fee91d20aa43f5a6c3cd26ad8560863645e2fc567b5c8c
  revision: 8ad5301b577fe16be1613fa79fb5551c225ce5c56ae65802698b8908c963c614
  schema: mindie-entry/3
  summary: 已收录 DSpark 模型、spec decode、KV/Mooncake、prefix-cache 和 launcher 的静态审查结论。后续优先处理 VLLM_HOST_IP 固定、精确 PGID 生命周期管理、四机上下文长度、PD proxy 与 rank/IP 校验；同时保留权重加载、DP uneven、接受率、runtime、精度及组合特性尚未实测的状态，不能将静态确认视为运行成功。
  title: 当前任务导航：DSpark 变更与多机启动验证索引
navigation: 已收录 DSpark 模型、spec decode、KV/Mooncake、prefix-cache 和 launcher 的静态审查结论。后续优先处理 VLLM_HOST_IP 固定、精确 PGID 生命周期管理、四机上下文长度、PD proxy 与 rank/IP 校验；同时保留权重加载、DP uneven、接受率、runtime、精度及组合特性尚未实测的状态，不能将静态确认视为运行成功。
schema: mindie-material-task/1
status: complete
task_id: 639a7e30e32d52fce9aac7d8f788ac213375e6d887521a6c0f87b6e4d7f17fe0
---

# 当前任务导航：DSpark 变更与多机启动验证索引

已收录 DSpark 模型、spec decode、KV/Mooncake、prefix-cache 和 launcher 的静态审查结论。后续优先处理 VLLM_HOST_IP 固定、精确 PGID 生命周期管理、四机上下文长度、PD proxy 与 rank/IP 校验；同时保留权重加载、DP uneven、接受率、runtime、精度及组合特性尚未实测的状态，不能将静态确认视为运行成功。

## Materials

- [DSpark 静态审查与启动器风险索引](blocks/3ac8a38779369baafa0d89a856822c9785dbce74b7457d66f466a189b11bba50.md): 静态审查认为 diff 未发现阻断两机冒烟或四机验证的问题，但真实 5 层权重的 AutoWeightsLoader、DP2/DP4 uneven/idle-rank、Mooncake 接受率、NPU runtime、精度及特性叠加仍未验证；保留了 fake loader、coverage marker 不等于接受率等不确定性。启动器新增识别出两个接机前 P0：未固定 VLLM_HOST_IP，以及无法按精确 PGID 安全清理；另有四机上下文长度、PD proxy、rank 映射和端口校验等 P1/P2 风险。已确认基础 DP/TP/EP、prefix-cache 参数、PD 映射、Mooncake producer/consumer 配置、K=7 capture、bash -n 和 shellcheck 通过，但未连接远端、启动服务或修改文件。

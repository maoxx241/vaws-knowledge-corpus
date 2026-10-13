---
blocks:
- block_id: cb85585d4fd293b2be226c9985285021a35542f37e01e43c9b2409c015819b49
  indexed: true
  sha256: 875e7f02e562136ffac0936db76d9ac6388888c3d2bef2b850015267f0ac598c
  source_range:
    end: 633436
    part: 0
    start: 0
  summary: 用户要求读取已安装的 MindIE 手动入口 Skill，并按原生流程激活一次；要求不自行设置或伪造 CODEX_THREAD_ID，保持社区贡献关闭。助手报告已定位 Skill、确认使用 `bridge.py activate` 且不传身份参数，并表示正在执行激活；材料未提供激活结果，也未证明后续 `knowledge_query` 已调用或成功。
  title: MindIE 手动入口激活与查询前置
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: afc3a96fb1e630f37d89aa42aff45e0c244e52d655318b66940cd614cc0d74d2
  kind: experience
  material_digest: f13a1e2a1955ecf607e74e9c0d53ddd088cd8f6034371bcf67376ecd3ee59c40
  revision: 50a6443dce5abae1607f02ecae1dc74249d2369307c8bcf55c6f3338fa854a37
  schema: mindie-entry/3
  summary: 当前材料记录了 MindIE 手动入口 Skill 的定位及一次原生激活的准备/执行状态，涉及 `bridge.py activate`、CODEX_THREAD_ID、社区贡献开关和后续一次性 `knowledge_query` 查询。激活结果与查询结果尚未出现，不能视为成功；后续需独立检索原始块以确认未完成事项及任何更正。
  title: MindIE 安装验收任务导航
navigation: 当前材料记录了 MindIE 手动入口 Skill 的定位及一次原生激活的准备/执行状态，涉及 `bridge.py activate`、CODEX_THREAD_ID、社区贡献开关和后续一次性 `knowledge_query` 查询。激活结果与查询结果尚未出现，不能视为成功；后续需独立检索原始块以确认未完成事项及任何更正。
schema: mindie-material-task/1
status: complete
task_id: afc3a96fb1e630f37d89aa42aff45e0c244e52d655318b66940cd614cc0d74d2
---

# MindIE 安装验收任务导航

当前材料记录了 MindIE 手动入口 Skill 的定位及一次原生激活的准备/执行状态，涉及 `bridge.py activate`、CODEX_THREAD_ID、社区贡献开关和后续一次性 `knowledge_query` 查询。激活结果与查询结果尚未出现，不能视为成功；后续需独立检索原始块以确认未完成事项及任何更正。

## Materials

- [MindIE 手动入口激活与查询前置](blocks/cb85585d4fd293b2be226c9985285021a35542f37e01e43c9b2409c015819b49.md): 用户要求读取已安装的 MindIE 手动入口 Skill，并按原生流程激活一次；要求不自行设置或伪造 CODEX_THREAD_ID，保持社区贡献关闭。助手报告已定位 Skill、确认使用 `bridge.py activate` 且不传身份参数，并表示正在执行激活；材料未提供激活结果，也未证明后续 `knowledge_query` 已调用或成功。

---
blocks:
- block_id: 971152ab09953bcfc0d821e0e45212b22658f2f61f0ab8cd53be9c873c175077
  indexed: true
  sha256: b1f13ab1cd6f94bc0467e68fc425a23b2c78de908666f82a3db99f691c51a740
  source_range:
    end: 675891
    part: 0
    start: 0
  summary: 只读审计报告称 154 的 clean build 已完成并安装，相关进程退出，三项算子在最终清单中各有条目，且对应 `.json/.o` 文件存在；但此前构建期间源码目录中的旧 manifest 曾缺失这些算子，不能将中间清单视为最终结果。报告称 156 仍是旧坏包，三项算子缺失，建议先同步 154 新包；这些观察、哈希和构建成功结论均属于该报告的记录，未在此独立验证。另提供覆盖 `H=6,T=1` 等形状的 KDA 直调、e2e 测试和 Torch Meta 门禁命令，但这些是建议/待执行步骤，不应视为已通过。
  title: KDA 清单构建审计与验证建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2325b10f3ffbe3f9dbdf536d9006ab95e7c958d01ef12d74f08710e7c5074f60
  kind: experience
  material_digest: 50cf9fb14981dc7dadee34ea7bec8710e67d5eec4d119e7f1185aeb56850477e
  revision: 537e3f63900660a1dc79b5028bec261826eef0d372709770da4fbb90f6208935
  schema: mindie-entry/3
  summary: 本批唯一材料聚焦两台机器上的 KDA clean build、聚合 manifest、算子二进制及后续验证命令。可检索 154/156、`binary_info_config.json`、`aic-ascend910_93-ops-info.json`、Gate/Chunk/Layout 的 `3/7/3` 数量、旧坏包、SHA256、`.done`、compressor 编译，以及 `kda_gate_cumsum`、e2e 测试和 Torch Meta 门禁；构建审计被报告为完成，但建议命令的执行结果未提供。
  title: 当前任务导航：KDA 构建清单与测试索引
navigation: 本批唯一材料聚焦两台机器上的 KDA clean build、聚合 manifest、算子二进制及后续验证命令。可检索 154/156、`binary_info_config.json`、`aic-ascend910_93-ops-info.json`、Gate/Chunk/Layout 的 `3/7/3` 数量、旧坏包、SHA256、`.done`、compressor 编译，以及 `kda_gate_cumsum`、e2e 测试和 Torch Meta 门禁；构建审计被报告为完成，但建议命令的执行结果未提供。
schema: mindie-material-task/1
status: complete
task_id: 2325b10f3ffbe3f9dbdf536d9006ab95e7c958d01ef12d74f08710e7c5074f60
---

# 当前任务导航：KDA 构建清单与测试索引

本批唯一材料聚焦两台机器上的 KDA clean build、聚合 manifest、算子二进制及后续验证命令。可检索 154/156、`binary_info_config.json`、`aic-ascend910_93-ops-info.json`、Gate/Chunk/Layout 的 `3/7/3` 数量、旧坏包、SHA256、`.done`、compressor 编译，以及 `kda_gate_cumsum`、e2e 测试和 Torch Meta 门禁；构建审计被报告为完成，但建议命令的执行结果未提供。

## Materials

- [KDA 清单构建审计与验证建议](blocks/971152ab09953bcfc0d821e0e45212b22658f2f61f0ab8cd53be9c873c175077.md): 只读审计报告称 154 的 clean build 已完成并安装，相关进程退出，三项算子在最终清单中各有条目，且对应 `.json/.o` 文件存在；但此前构建期间源码目录中的旧 manifest 曾缺失这些算子，不能将中间清单视为最终结果。报告称 156 仍是旧坏包，三项算子缺失，建议先同步 154 新包；这些观察、哈希和构建成功结论均属于该报告的记录，未在此独立验证。另提供覆盖 `H=6,T=1` 等形状的 KDA 直调、e2e 测试和 Torch Meta 门禁命令，但这些是建议/待执行步骤，不应视为已通过。

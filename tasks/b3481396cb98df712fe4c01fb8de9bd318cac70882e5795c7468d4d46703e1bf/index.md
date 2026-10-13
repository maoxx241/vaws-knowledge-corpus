---
blocks:
- block_id: 17d798b0a1c65d17b522da2dacec3520b2e0068b144126a5de7092e188f04e35
  indexed: true
  sha256: 67c505d03f211ebed1a2b91a3396c1f5257b4cdaff0db9e4952ccbe6afb525c9
  source_range:
    end: 413000
    part: 0
    start: 0
  summary: 历史记录中先未找到远端验证启动命令，后来确认曾在已脱敏的远端容器中执行 TP8、5 层 INT4 模型的 eager 生成验证；命令包含 `--max-model-len 64`、`--max-tokens 2`、`--gpu-memory-utilization 0.85`、`--load-format auto` 和 `--quantization ascend`，路径、地址、容器命令及模型参数含脱敏或秘密占位符。该次脚本固定使用 `enforce_eager=True`，记录的生成 token 为 `[161095, 74143]`，耗时约 275.039 秒；此前提到的 ACLGraph、`FULL_DECODE_ONLY` 和 warmup=1 属于另一轮验证，不能与此次 eager 结果混同。
  title: 远端 TP8 生成验证命令与结果
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: b3481396cb98df712fe4c01fb8de9bd318cac70882e5795c7468d4d46703e1bf
  kind: experience
  material_digest: 2dc061302e9cfd8d83a376bbe48cb10c375d829796fb136cc727425e36576b6b
  revision: 77f76d6755eebff204170c3b3745adcb017197d24eb0a644ffa3d7c9d8f208d4
  schema: mindie-entry/3
  summary: 已收录 1 个新材料块：远端容器中的 TP8 eager 两 token 生成验证命令、环境参数、脱敏信息及观测结果。记录保留了先前未找到命令的结论、后续定位结果，以及 ACLGraph 验证与 eager 验证之间的更正和区分；原始块仍可独立检索。
  title: 当前任务导航：远端验证命令索引
navigation: 已收录 1 个新材料块：远端容器中的 TP8 eager 两 token 生成验证命令、环境参数、脱敏信息及观测结果。记录保留了先前未找到命令的结论、后续定位结果，以及 ACLGraph 验证与 eager 验证之间的更正和区分；原始块仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: b3481396cb98df712fe4c01fb8de9bd318cac70882e5795c7468d4d46703e1bf
---

# 当前任务导航：远端验证命令索引

已收录 1 个新材料块：远端容器中的 TP8 eager 两 token 生成验证命令、环境参数、脱敏信息及观测结果。记录保留了先前未找到命令的结论、后续定位结果，以及 ACLGraph 验证与 eager 验证之间的更正和区分；原始块仍可独立检索。

## Materials

- [远端 TP8 生成验证命令与结果](blocks/17d798b0a1c65d17b522da2dacec3520b2e0068b144126a5de7092e188f04e35.md): 历史记录中先未找到远端验证启动命令，后来确认曾在已脱敏的远端容器中执行 TP8、5 层 INT4 模型的 eager 生成验证；命令包含 `--max-model-len 64`、`--max-tokens 2`、`--gpu-memory-utilization 0.85`、`--load-format auto` 和 `--quantization ascend`，路径、地址、容器命令及模型参数含脱敏或秘密占位符。该次脚本固定使用 `enforce_eager=True`，记录的生成 token 为 `[161095, 74143]`，耗时约 275.039 秒；此前提到的 ACLGraph、`FULL_DECODE_ONLY` 和 warmup=1 属于另一轮验证，不能与此次 eager 结果混同。

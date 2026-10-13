---
blocks:
- block_id: 4bb6c3928b39eb94481d64eb84d4d31fdc5331abb63a78519211c312e060999e
  indexed: true
  sha256: a52e2c8639d9ebf3eb3ef1de03d0fc6bfbed163ab5778a89fa2ce0502125ce42
  source_range:
    end: 1096345
    part: 0
    start: 0
  summary: 只读核查显示，r5 target 使用 `enforce_eager=False + FULL_DECODE_ONLY`，draft 的 `speculative_config.enforce_eager=True`；日志中仅发现 target 的 6 个 decode FULL 图捕获，未发现 draft 的 `ACLGraphWrapper` 包装记录。该证据使“draft 误入图捕获”暂不能作为当前异常原因，但 target 图重放与 DSpark token 对齐仍未完成定位。快速验证进程已设置 `HCCL_BUFFSIZE=512`，未执行服务或请求操作。
  title: r5 图捕获与 token 对齐只读核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 532257306954b01ec905fa99092530ef2e0d57da78f898b35165d6d17fea2364
  kind: experience
  material_digest: 08d7d119e35ba9b94812f258eb424680c166a9a41a9a69e51852d352d8a74b0b
  revision: 2b30805da60f88c2528812e71d1edb8178b8851fd16e30ca5f8cb93ebc83a7dd
  schema: mindie-entry/3
  summary: 可检索重点：`enforce_eager`、`FULL_DECODE_ONLY`、`speculative_config`、`ACLGraphWrapper`、target decode FULL 图捕获、DSpark token 对齐、`HCCL_BUFFSIZE=512`。当前已知结果仅来自只读检查；draft 误入图捕获尚无日志支持，异常根因与 token 0 重复接受仍未验证，后续应保留这些未完成和不确定状态。
  title: 当前任务导航：r5 图捕获、draft 隔离与 token 对齐
navigation: 可检索重点：`enforce_eager`、`FULL_DECODE_ONLY`、`speculative_config`、`ACLGraphWrapper`、target decode FULL 图捕获、DSpark token 对齐、`HCCL_BUFFSIZE=512`。当前已知结果仅来自只读检查；draft 误入图捕获尚无日志支持，异常根因与 token 0 重复接受仍未验证，后续应保留这些未完成和不确定状态。
schema: mindie-material-task/1
status: complete
task_id: 532257306954b01ec905fa99092530ef2e0d57da78f898b35165d6d17fea2364
---

# 当前任务导航：r5 图捕获、draft 隔离与 token 对齐

可检索重点：`enforce_eager`、`FULL_DECODE_ONLY`、`speculative_config`、`ACLGraphWrapper`、target decode FULL 图捕获、DSpark token 对齐、`HCCL_BUFFSIZE=512`。当前已知结果仅来自只读检查；draft 误入图捕获尚无日志支持，异常根因与 token 0 重复接受仍未验证，后续应保留这些未完成和不确定状态。

## Materials

- [r5 图捕获与 token 对齐只读核查](blocks/4bb6c3928b39eb94481d64eb84d4d31fdc5331abb63a78519211c312e060999e.md): 只读核查显示，r5 target 使用 `enforce_eager=False + FULL_DECODE_ONLY`，draft 的 `speculative_config.enforce_eager=True`；日志中仅发现 target 的 6 个 decode FULL 图捕获，未发现 draft 的 `ACLGraphWrapper` 包装记录。该证据使“draft 误入图捕获”暂不能作为当前异常原因，但 target 图重放与 DSpark token 对齐仍未完成定位。快速验证进程已设置 `HCCL_BUFFSIZE=512`，未执行服务或请求操作。

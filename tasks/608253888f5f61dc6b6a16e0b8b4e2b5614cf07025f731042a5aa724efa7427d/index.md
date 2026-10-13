---
blocks:
- block_id: fd1b5c0d815799de6118cccd30e8d02e839fe09e2c83200711c3703b9704ac0e
  indexed: true
  sha256: ad79c5ddb94a07b9d4e9462ab896039c1a84614831459a3e10f64c7191d06ba3
  source_range:
    end: 670686
    part: 0
    start: 0
  summary: 消息记录称已新增 K3 prefix-cache CPU 回归，覆盖 align 模式下的 prefix-hit 与 cold prefill、KDA state block 选择，以及 `prefill_has_initial_state == [True, False]` 到 causal-conv metadata 的传递；并提到与既有 KDA 单测覆盖命中状态保留和未命中状态清零。记录同时说明本地未安装 vLLM，计划复用远端开发容器执行 CPU UT、不启动真实模型或占用 NPU；最终消息声称相关两个 UT 文件共 `32 passed`，但该结果仍应视为消息中的报告而非独立验证，且修改未提交、未推送。
  title: K3 prefix-cache CPU 回归测试
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 608253888f5f61dc6b6a16e0b8b4e2b5614cf07025f731042a5aa724efa7427d
  kind: experience
  material_digest: d8ac01ed1e707e36bfc77702f15f1b9e5108f8f1bba84f8d529aad21b2d02597
  revision: c12f2ccbe8f8af9994388231fcca665cebfdfd25417ead86a07e986990bc4577
  schema: mindie-entry/3
  summary: 本批次包含 1 个可检索材料块，主题为 K3 prefix-cache CPU 回归测试及其 KDA/causal-conv metadata 断言。保留的关键检索词包括 `align`、prefix-hit、cold prefill、`prefill_has_initial_state`、KDA state block、vLLM、CPU UT、`32 passed`、未提交未推送；通过结果属于消息报告，未独立核验。
  title: 当前任务导航
navigation: 本批次包含 1 个可检索材料块，主题为 K3 prefix-cache CPU 回归测试及其 KDA/causal-conv metadata 断言。保留的关键检索词包括 `align`、prefix-hit、cold prefill、`prefill_has_initial_state`、KDA state block、vLLM、CPU UT、`32 passed`、未提交未推送；通过结果属于消息报告，未独立核验。
schema: mindie-material-task/1
status: complete
task_id: 608253888f5f61dc6b6a16e0b8b4e2b5614cf07025f731042a5aa724efa7427d
---

# 当前任务导航

本批次包含 1 个可检索材料块，主题为 K3 prefix-cache CPU 回归测试及其 KDA/causal-conv metadata 断言。保留的关键检索词包括 `align`、prefix-hit、cold prefill、`prefill_has_initial_state`、KDA state block、vLLM、CPU UT、`32 passed`、未提交未推送；通过结果属于消息报告，未独立核验。

## Materials

- [K3 prefix-cache CPU 回归测试](blocks/fd1b5c0d815799de6118cccd30e8d02e839fe09e2c83200711c3703b9704ac0e.md): 消息记录称已新增 K3 prefix-cache CPU 回归，覆盖 align 模式下的 prefix-hit 与 cold prefill、KDA state block 选择，以及 `prefill_has_initial_state == [True, False]` 到 causal-conv metadata 的传递；并提到与既有 KDA 单测覆盖命中状态保留和未命中状态清零。记录同时说明本地未安装 vLLM，计划复用远端开发容器执行 CPU UT、不启动真实模型或占用 NPU；最终消息声称相关两个 UT 文件共 `32 passed`，但该结果仍应视为消息中的报告而非独立验证，且修改未提交、未推送。

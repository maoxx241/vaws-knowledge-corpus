---
blocks:
- block_id: a96793a1a19a52ec857556f148e4190f9e6fcff052e0f2c1d37257be410ed512
  indexed: true
  sha256: 5620248027ef2c4a7f38271ff6ec4f2ea2b4be55ca60f278cfa4b0ef5f10f33f
  source_range:
    end: 991074
    part: 0
    start: 0
  summary: r13切换前只读复核报告称，四机PID/PGID/SID、cmdline、served id及source环境一致，fatal count均为0；r14相关目标当时尚不存在，且未执行停止、启动或推理。随后报告的r14首次DFX终态显示，四机进程组均已退出，DP0在execute路径调用`get_current_vllm_config()`触发assertion，被标为唯一首因；DP1–DP3仅出现`Connection closed by peer`级联。Client/verifier已退出，DFX dump为空，结果明确标记为`invalid_experiment`；日志及invalid-result文件的终态大小和SHA256据称已回报，但未在材料中独立核验。
  title: r13复核与r14首次DFX终态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f6db1b14045356268e6ee41796de039dec0ecc5f464b3b83f502f8d8b0ae9e82
  kind: experience
  material_digest: 67e1dba3aea781f417dc721223b316021e939581ff8476a21907d904feaa5394
  revision: f6ae01e4373c7dfaee1eacf6e6d4d3e779487d984bbb65d5ed22f0878eed41e7
  schema: mindie-entry/3
  summary: 材料覆盖r13切换前只读复核，以及r14首次DFX运行的失败终态。后续检索应关注精确PID/PGID/SID与组成员检查、`get_current_vllm_config()` assertion、DP间`Connection closed by peer`级联、空DFX dump和`invalid_experiment`标记；不要将复核报告中的健康检查或已回报的哈希视为本索引已验证的成功。
  title: 当前任务导航
navigation: 材料覆盖r13切换前只读复核，以及r14首次DFX运行的失败终态。后续检索应关注精确PID/PGID/SID与组成员检查、`get_current_vllm_config()` assertion、DP间`Connection closed by peer`级联、空DFX dump和`invalid_experiment`标记；不要将复核报告中的健康检查或已回报的哈希视为本索引已验证的成功。
schema: mindie-material-task/1
status: complete
task_id: f6db1b14045356268e6ee41796de039dec0ecc5f464b3b83f502f8d8b0ae9e82
---

# 当前任务导航

材料覆盖r13切换前只读复核，以及r14首次DFX运行的失败终态。后续检索应关注精确PID/PGID/SID与组成员检查、`get_current_vllm_config()` assertion、DP间`Connection closed by peer`级联、空DFX dump和`invalid_experiment`标记；不要将复核报告中的健康检查或已回报的哈希视为本索引已验证的成功。

## Materials

- [r13复核与r14首次DFX终态](blocks/a96793a1a19a52ec857556f148e4190f9e6fcff052e0f2c1d37257be410ed512.md): r13切换前只读复核报告称，四机PID/PGID/SID、cmdline、served id及source环境一致，fatal count均为0；r14相关目标当时尚不存在，且未执行停止、启动或推理。随后报告的r14首次DFX终态显示，四机进程组均已退出，DP0在execute路径调用`get_current_vllm_config()`触发assertion，被标为唯一首因；DP1–DP3仅出现`Connection closed by peer`级联。Client/verifier已退出，DFX dump为空，结果明确标记为`invalid_experiment`；日志及invalid-result文件的终态大小和SHA256据称已回报，但未在材料中独立核验。

---
blocks:
- block_id: d8a17b40489055e52045d6869ea15e8428d56b0a5de92b931df09246a8acafbd
  indexed: true
  sha256: d55d1e7144f9f569125336b64210cb1f3d07c17f0663f45c059895211715fff0
  source_range:
    end: 634962
    part: 0
    start: 0
  summary: 对153–156进行只读核查时，154曾运行监听8098的Minimax H3 vLLM，后续释放；156随后发现运行监听8010的Qwen3.8-Flash-Next vLLM，HBM接近满载但多次采样（含约30秒窗口）AICore均为0%，期间一次采样脚本字段提取失败，不能视为有效结果。根据30秒持续为0%的条件，对精确PID 904181发送SIGTERM；后续报告称进程退出、8010关闭、HBM回落且无NPU运行进程，但该释放结论属于当时的检查报告，索引不将其提升为独立验证事实。
  title: 四机占用核查与156服务释放
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 34897a8e2c0a0098612ac9d5b7677e1a2cf3fb1cf4b803a086276e22c22eac89
  kind: experience
  material_digest: 315ea7f3535d7fb9c106c943ad6db34ee91091b3d65fcad7c376b2c0cc3797b1
  revision: bc9fda56c795e8746e93a2f4a80dd6fd27496967dd24e73b0c8f4b2582291518
  schema: mindie-entry/3
  summary: 记录153–156的服务、端口、NPU进程及AICore采样状态：154的Minimax服务曾占用资源，156的Qwen vLLM在AICore持续0%后按条件被终止。保留一次无效采样及“进程驻留但空闲”这类不确定性；最新报告称156已释放，但原始材料仍应独立检索，不能将报告性收尾自动视为已验证成功。
  title: 四机NPU资源占用与释放跟踪
navigation: 记录153–156的服务、端口、NPU进程及AICore采样状态：154的Minimax服务曾占用资源，156的Qwen vLLM在AICore持续0%后按条件被终止。保留一次无效采样及“进程驻留但空闲”这类不确定性；最新报告称156已释放，但原始材料仍应独立检索，不能将报告性收尾自动视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 34897a8e2c0a0098612ac9d5b7677e1a2cf3fb1cf4b803a086276e22c22eac89
---

# 四机NPU资源占用与释放跟踪

记录153–156的服务、端口、NPU进程及AICore采样状态：154的Minimax服务曾占用资源，156的Qwen vLLM在AICore持续0%后按条件被终止。保留一次无效采样及“进程驻留但空闲”这类不确定性；最新报告称156已释放，但原始材料仍应独立检索，不能将报告性收尾自动视为已验证成功。

## Materials

- [四机占用核查与156服务释放](blocks/d8a17b40489055e52045d6869ea15e8428d56b0a5de92b931df09246a8acafbd.md): 对153–156进行只读核查时，154曾运行监听8098的Minimax H3 vLLM，后续释放；156随后发现运行监听8010的Qwen3.8-Flash-Next vLLM，HBM接近满载但多次采样（含约30秒窗口）AICore均为0%，期间一次采样脚本字段提取失败，不能视为有效结果。根据30秒持续为0%的条件，对精确PID 904181发送SIGTERM；后续报告称进程退出、8010关闭、HBM回落且无NPU运行进程，但该释放结论属于当时的检查报告，索引不将其提升为独立验证事实。

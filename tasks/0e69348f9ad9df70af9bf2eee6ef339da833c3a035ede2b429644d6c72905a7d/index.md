---
blocks:
- block_id: eb68812b1b387891eb966c7acf1ba68b48b1806f85438e52ed775ad3406d6d58
  indexed: true
  sha256: edc1935fd9e086609d78962e829f43127a9ffea472910dca1cc8f22601fa0c2b
  source_range:
    end: 1520753
    part: 0
    start: 0
  summary: 复核认为 arch22 patch 的事件方向与顺序正确：V2 FP32 final-state 路径保留 E0 为 MTE3_MTE2，V1 需消费旧状态并恢复 V_MTE2；empty subblock 的 wait/set 顺序无需额外 MTE2_V 桥。task41 timeout、task42 tokens-per-sequence=2 pass 被作为机制佐证，但仍需按 task40/41/42、task80/96、output_final_state=False 及数值稳定性矩阵继续验收，不能视为完整验证成功。建议在隔离 worktree 和 fresh process 中做单算子 A/B；build_aclnn.sh 的第三参数、双 vendor overlay、ASCEND_CUSTOM_OPP_PATH/LD_LIBRARY_PATH、nm -D、/proc/self/maps 等属于实验方法与环境检查，最终服务仍应重建完整 custom-op 包；消息称未修改文件或远端状态。
  title: arch22 epilogue 同步与隔离 A/B 验证建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0e69348f9ad9df70af9bf2eee6ef339da833c3a035ede2b429644d6c72905a7d
  kind: experience
  material_digest: f05f022b17ce009885f0246b53d8987bbe87588ac591b059c38bfdda5e97591b
  revision: 2d2a1d19fcbd1c04921c96beaf6a8e9df5fc68c42fbd802374df1a52169ab39f
  schema: mindie-entry/3
  summary: 核心线索是 V1/V2 的 E0、V_MTE2 与 MTE2 pipeline 复用，以及 task41 timeout 与 task42 tokens-per-sequence=2 行为之间的关系。现有材料报告 patch 方向正确和部分机制佐证，但保留未完成的验收要求：隔离 fresh-process A/B、完整 task/参数矩阵、多次重复、h/v_new/final_state 数值对比、finite/误差/稳定性检查，以及 vendor 包和加载路径核验；不要将这些计划或检查项当作已验证成功。构建脚本第三参数和双 vendor overlay仅适合实验，最终方案需重建完整 custom-op 包。
  title: 当前任务导航：arch22 单算子同步修复的检验索引
navigation: 核心线索是 V1/V2 的 E0、V_MTE2 与 MTE2 pipeline 复用，以及 task41 timeout 与 task42 tokens-per-sequence=2 行为之间的关系。现有材料报告 patch 方向正确和部分机制佐证，但保留未完成的验收要求：隔离 fresh-process A/B、完整 task/参数矩阵、多次重复、h/v_new/final_state 数值对比、finite/误差/稳定性检查，以及 vendor 包和加载路径核验；不要将这些计划或检查项当作已验证成功。构建脚本第三参数和双 vendor overlay仅适合实验，最终方案需重建完整 custom-op 包。
schema: mindie-material-task/1
status: complete
task_id: 0e69348f9ad9df70af9bf2eee6ef339da833c3a035ede2b429644d6c72905a7d
---

# 当前任务导航：arch22 单算子同步修复的检验索引

核心线索是 V1/V2 的 E0、V_MTE2 与 MTE2 pipeline 复用，以及 task41 timeout 与 task42 tokens-per-sequence=2 行为之间的关系。现有材料报告 patch 方向正确和部分机制佐证，但保留未完成的验收要求：隔离 fresh-process A/B、完整 task/参数矩阵、多次重复、h/v_new/final_state 数值对比、finite/误差/稳定性检查，以及 vendor 包和加载路径核验；不要将这些计划或检查项当作已验证成功。构建脚本第三参数和双 vendor overlay仅适合实验，最终方案需重建完整 custom-op 包。

## Materials

- [arch22 epilogue 同步与隔离 A/B 验证建议](blocks/eb68812b1b387891eb966c7acf1ba68b48b1806f85438e52ed775ad3406d6d58.md): 复核认为 arch22 patch 的事件方向与顺序正确：V2 FP32 final-state 路径保留 E0 为 MTE3_MTE2，V1 需消费旧状态并恢复 V_MTE2；empty subblock 的 wait/set 顺序无需额外 MTE2_V 桥。task41 timeout、task42 tokens-per-sequence=2 pass 被作为机制佐证，但仍需按 task40/41/42、task80/96、output_final_state=False 及数值稳定性矩阵继续验收，不能视为完整验证成功。建议在隔离 worktree 和 fresh process 中做单算子 A/B；build_aclnn.sh 的第三参数、双 vendor overlay、ASCEND_CUSTOM_OPP_PATH/LD_LIBRARY_PATH、nm -D、/proc/self/maps 等属于实验方法与环境检查，最终服务仍应重建完整 custom-op 包；消息称未修改文件或远端状态。

---
blocks:
- block_id: 2cfa3fc60f82826475b250d00f2c28994870af29985850f463fb07da3e2f3a19
  indexed: true
  sha256: af401c71149cf76607b109c793e5b4b0bed03039dacf6b795bb01272e4d5b364
  source_range:
    end: 478612
    part: 0
    start: 0
  summary: 首轮 pytest 因 conftest 导入缺失的 `vllm_ascend._build_info` 退出，0 个测试被收集；Ruff 格式检查也曾发现 `tests/ut/models/test_kimi_k3.py` 需要格式化。随后通过进程内 mock 注入 `_build_info`、`torch_npu` 和 `torch.npu`，关键测试先后报告 93 个、再报告 94 个通过；K3 首轮还因 mock 缺少 `npu_rms_norm` 而失败，补充占位属性后重跑通过。记录称 Ruff、compileall、diff 检查及最终工作树均通过，但这些结果依赖特定 mock、环境变量、解释器和测试隔离设置，且未访问远端或 NPU。
  title: 本地测试与静态检查记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 2c183652a615b982ef0208b862c882640358db11ff688ca6b2fd702308d4a8a8
  kind: experience
  material_digest: 2d59180399d333e21c299067582dbafc3fd39f48f3208b030478989aa4be582a
  revision: 8fff3d5ca74ad9988f32af3dd56bf85ee38a0b35886d084c0353ff4db6beeb5e
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，集中记录本地只读验证、构建元数据缺失、mock harness 修正及后续测试结果。可检索 `vllm_ascend._build_info`、`conftest`、`npu_rms_norm`、`93 passed`、`94 passed`、Ruff、compileall、工作树和未访问远端/NPU等线索；早期阻断、格式问题及 harness 修正均保留，未将报告结果扩展为独立验证。
  title: 当前任务导航
navigation: 本批仅包含一个材料块，集中记录本地只读验证、构建元数据缺失、mock harness 修正及后续测试结果。可检索 `vllm_ascend._build_info`、`conftest`、`npu_rms_norm`、`93 passed`、`94 passed`、Ruff、compileall、工作树和未访问远端/NPU等线索；早期阻断、格式问题及 harness 修正均保留，未将报告结果扩展为独立验证。
schema: mindie-material-task/1
status: complete
task_id: 2c183652a615b982ef0208b862c882640358db11ff688ca6b2fd702308d4a8a8
---

# 当前任务导航

本批仅包含一个材料块，集中记录本地只读验证、构建元数据缺失、mock harness 修正及后续测试结果。可检索 `vllm_ascend._build_info`、`conftest`、`npu_rms_norm`、`93 passed`、`94 passed`、Ruff、compileall、工作树和未访问远端/NPU等线索；早期阻断、格式问题及 harness 修正均保留，未将报告结果扩展为独立验证。

## Materials

- [本地测试与静态检查记录](blocks/2cfa3fc60f82826475b250d00f2c28994870af29985850f463fb07da3e2f3a19.md): 首轮 pytest 因 conftest 导入缺失的 `vllm_ascend._build_info` 退出，0 个测试被收集；Ruff 格式检查也曾发现 `tests/ut/models/test_kimi_k3.py` 需要格式化。随后通过进程内 mock 注入 `_build_info`、`torch_npu` 和 `torch.npu`，关键测试先后报告 93 个、再报告 94 个通过；K3 首轮还因 mock 缺少 `npu_rms_norm` 而失败，补充占位属性后重跑通过。记录称 Ruff、compileall、diff 检查及最终工作树均通过，但这些结果依赖特定 mock、环境变量、解释器和测试隔离设置，且未访问远端或 NPU。

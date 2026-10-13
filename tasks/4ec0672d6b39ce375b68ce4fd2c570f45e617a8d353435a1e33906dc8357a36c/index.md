---
blocks:
- block_id: 73e5ff1173c744dcd5be6519ca338bd058e71850a55786d9d6938518f76374c2
  indexed: true
  sha256: c56eef3bd8974a6b7d42b71afdca0d3264c6ba670756f03b2ee4de0fcefab6f0
  source_range:
    end: 812463
    part: 0
    start: 0
  summary: 只读核对后建议 DP1 固定 rank=0、temperature=0、concurrency=1、关闭 prefix cache，并通过 127.0.0.1:18077 分阶段运行短请求及 GSM8K 1/8/64 条测试；脚本内置硬门槛仅覆盖输出结构和 Prometheus 计数不变量。提出的 parsed/correct、Mean Acceptance Length、fixed-width draft acceptance 等数值是本轮校准建议阈值，不是已验证结果；脚本退出 0 也不会自动检查数据 SHA、GSM8K accuracy、七个 position 或 AL。严格的 greedy 精度结论仍需与相同 prompt 的 target-only 配置对比，且文中命令含有已保留的路径与文件名脱敏占位符。
  title: DP1 GSM8K 分阶段校准与门槛
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4ec0672d6b39ce375b68ce4fd2c570f45e617a8d353435a1e33906dc8357a36c
  kind: experience
  material_digest: 39f53d8e4d869dd3cfca57d497975c10dc9e9dd0186234b006a36de63a571bd1
  revision: 0b10689aa65e142e43906d53a3ddeae755e1370fca4f2f55b2e8a211c218e51d
  schema: mindie-entry/3
  summary: 新增材料聚焦 DP1、18077 端口下的只读配置核对、分阶段 GSM8K 测试命令及建议硬阈值。应将脚本硬门槛与未验证的校准建议区分开，并保留协议烟测、小样本不足以证明精度、target-only 对照尚未完成等不确定性；原始材料仍可独立检索。
  title: 当前任务导航：DP1 GSM8K 校准索引
navigation: 新增材料聚焦 DP1、18077 端口下的只读配置核对、分阶段 GSM8K 测试命令及建议硬阈值。应将脚本硬门槛与未验证的校准建议区分开，并保留协议烟测、小样本不足以证明精度、target-only 对照尚未完成等不确定性；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 4ec0672d6b39ce375b68ce4fd2c570f45e617a8d353435a1e33906dc8357a36c
---

# 当前任务导航：DP1 GSM8K 校准索引

新增材料聚焦 DP1、18077 端口下的只读配置核对、分阶段 GSM8K 测试命令及建议硬阈值。应将脚本硬门槛与未验证的校准建议区分开，并保留协议烟测、小样本不足以证明精度、target-only 对照尚未完成等不确定性；原始材料仍可独立检索。

## Materials

- [DP1 GSM8K 分阶段校准与门槛](blocks/73e5ff1173c744dcd5be6519ca338bd058e71850a55786d9d6938518f76374c2.md): 只读核对后建议 DP1 固定 rank=0、temperature=0、concurrency=1、关闭 prefix cache，并通过 127.0.0.1:18077 分阶段运行短请求及 GSM8K 1/8/64 条测试；脚本内置硬门槛仅覆盖输出结构和 Prometheus 计数不变量。提出的 parsed/correct、Mean Acceptance Length、fixed-width draft acceptance 等数值是本轮校准建议阈值，不是已验证结果；脚本退出 0 也不会自动检查数据 SHA、GSM8K accuracy、七个 position 或 AL。严格的 greedy 精度结论仍需与相同 prompt 的 target-only 配置对比，且文中命令含有已保留的路径与文件名脱敏占位符。

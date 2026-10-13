---
blocks:
- block_id: 20c6eb7a3911dae92f080eebfd5348c077b4f937deb099b6fa19c8176fc9de37
  indexed: true
  sha256: 492d7ea815c0afb6b0162382829d8bfbcdc99c15c0ed47bcb5b2c86e7d22b92f
  source_range:
    end: 5011231
    part: 0
    start: 0
  summary: '该块记录了 K3 四台 A3 机器注册到 nightly 镜像的过程，以及 `npu-smi` 仅显示通用 `Ascend910` 导致的初始判型不确定性；后续更正为四机硬件信息一致，精确 board-chip 查询得到 `Ascend910_9362 / A3`，并发现旧 `SOC_VERSION=ascend910_9391` 元数据可能污染判断。随后更新了 machine-management skill：优先实时查询、支持 A2/A3/A5 分层探测、保留旧元数据冲突告警，并据称通过 7 项回归和 36 项主线测试；最终报告 PR #69 已合入，但这些成功结论应视为来源中的报告而非独立验证。'
  title: K3 四机注册与 Ascend SoC 探测修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e38986459e3725620b051b4acad0c770e330e77fd9bddfa4a431ba8138106291
  kind: experience
  material_digest: 370be1c143204d971fb9640f2933701c5b47a736de588d5426021da7a003a62f
  revision: 6a5e1cdfacfe77e04e5db08c149ede3d18a5c74a92e60d6619e5ff322d9a59fc
  schema: mindie-entry/3
  summary: '唯一新材料块涵盖完整链路：K3 四机注册、nightly digest 统一、`npu-smi` 判型误解与更正、fresh hardware probe 覆盖旧 inventory，以及 machine-management skill 的 A2/A3/A5 检测改进和 PR #69 合入报告。需重点检索的术语包括 `Ascend910`、`Ascend910_9362`、`board-chip`、`SOC_VERSION`、`VAWS_NPU_SOC`、`npu-smi info -t board -c`、`nightly-main-a3`、`local-latest` 与 `sha256:6ec65f…c303`；来源还保留了“旧版 npu-smi”说法已被纠正、最终验证曾因本地安全策略暂缓，以及成功状态仅为报告性结论。'
  title: 当前任务导航：K3 机器注册与 SoC 探测修复
navigation: '唯一新材料块涵盖完整链路：K3 四机注册、nightly digest 统一、`npu-smi` 判型误解与更正、fresh hardware probe 覆盖旧 inventory，以及 machine-management skill 的 A2/A3/A5 检测改进和 PR #69 合入报告。需重点检索的术语包括 `Ascend910`、`Ascend910_9362`、`board-chip`、`SOC_VERSION`、`VAWS_NPU_SOC`、`npu-smi info -t board -c`、`nightly-main-a3`、`local-latest` 与 `sha256:6ec65f…c303`；来源还保留了“旧版 npu-smi”说法已被纠正、最终验证曾因本地安全策略暂缓，以及成功状态仅为报告性结论。'
schema: mindie-material-task/1
status: complete
task_id: e38986459e3725620b051b4acad0c770e330e77fd9bddfa4a431ba8138106291
---

# 当前任务导航：K3 机器注册与 SoC 探测修复

唯一新材料块涵盖完整链路：K3 四机注册、nightly digest 统一、`npu-smi` 判型误解与更正、fresh hardware probe 覆盖旧 inventory，以及 machine-management skill 的 A2/A3/A5 检测改进和 PR #69 合入报告。需重点检索的术语包括 `Ascend910`、`Ascend910_9362`、`board-chip`、`SOC_VERSION`、`VAWS_NPU_SOC`、`npu-smi info -t board -c`、`nightly-main-a3`、`local-latest` 与 `sha256:6ec65f…c303`；来源还保留了“旧版 npu-smi”说法已被纠正、最终验证曾因本地安全策略暂缓，以及成功状态仅为报告性结论。

## Materials

- [K3 四机注册与 Ascend SoC 探测修复](blocks/20c6eb7a3911dae92f080eebfd5348c077b4f937deb099b6fa19c8176fc9de37.md): 该块记录了 K3 四台 A3 机器注册到 nightly 镜像的过程，以及 `npu-smi` 仅显示通用 `Ascend910` 导致的初始判型不确定性；后续更正为四机硬件信息一致，精确 board-chip 查询得到 `Ascend910_9362 / A3`，并发现旧 `SOC_VERSION=ascend910_9391` 元数据可能污染判断。随后更新了 machine-management skill：优先实时查询、支持 A2/A3/A5 分层探测、保留旧元数据冲突告警，并据称通过 7 项回归和 36 项主线测试；最终报告 PR #69 已合入，但这些成功结论应视为来源中的报告而非独立验证。

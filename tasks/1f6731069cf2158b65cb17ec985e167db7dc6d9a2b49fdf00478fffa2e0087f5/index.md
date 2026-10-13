---
blocks:
- block_id: 0fd811bac1841c4e0d9c102825eea8c9419c315dff944d9451886d6a34474bbd
  indexed: true
  sha256: 08a2206dc07e51eb02b873cedd2a65126ad8e146da62528440d2bcbf3d8b2d8a
  source_range:
    end: 262388
    part: 0
    start: 0
  summary: 在指定容器和 `/mindie` 目录中，初始 `ASCEND_RT_VISIBLE_DEVICES=8` 探测为 `available=False, device_count=0`，改用 `ASCEND_RT_VISIBLE_DEVICES=0` 后报告为可用。作业 `job-20260920T082154Z-0b09e992` 报告退出码 0，FP16/BF16 的 `finite`、`allclose` 和 `passed` 均为 true；这些是脚本和工具报告，不能单独视为已核实成功。期间出现 3 次 `npu-smi info` 不可用提示及一个内部格式 warning，结果文件据称保存于 `/mindie/general-result.json`，SHA256 为 `c0fd57c6a30f2e85666323b47a014c6c211fce506a0e2f083e03e8a24dab897a`；范围仅限单个映射 Ascend910_9362 NPU 上的 5×1536 RMSNorm 小张量测试。
  title: MindIE 远端 NPU RMSNorm 验证记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1f6731069cf2158b65cb17ec985e167db7dc6d9a2b49fdf00478fffa2e0087f5
  kind: experience
  material_digest: 794bfb0fc3786e99acf203429ee2026bfa96f7b1ffc947743b331a09b8db25c3
  revision: c56616552621d896ca24d98bde65e6e31a34f7b34e36281b588831db3a4a8f02
  schema: mindie-entry/3
  summary: 本批次包含 1 个材料块：指定远端容器中的真实 NPU RMSNorm 验证记录。可检索 `ASCEND_RT_VISIBLE_DEVICES`、`device_count=0/1`、`remote_bash`、作业 ID、`npu-smi info`、FP16/BF16、`allclose`、结果文件和 SHA256；报告的通过状态、warning 及适用范围均应保留为未独立核实的记录。
  title: 当前任务导航
navigation: 本批次包含 1 个材料块：指定远端容器中的真实 NPU RMSNorm 验证记录。可检索 `ASCEND_RT_VISIBLE_DEVICES`、`device_count=0/1`、`remote_bash`、作业 ID、`npu-smi info`、FP16/BF16、`allclose`、结果文件和 SHA256；报告的通过状态、warning 及适用范围均应保留为未独立核实的记录。
schema: mindie-material-task/1
status: complete
task_id: 1f6731069cf2158b65cb17ec985e167db7dc6d9a2b49fdf00478fffa2e0087f5
---

# 当前任务导航

本批次包含 1 个材料块：指定远端容器中的真实 NPU RMSNorm 验证记录。可检索 `ASCEND_RT_VISIBLE_DEVICES`、`device_count=0/1`、`remote_bash`、作业 ID、`npu-smi info`、FP16/BF16、`allclose`、结果文件和 SHA256；报告的通过状态、warning 及适用范围均应保留为未独立核实的记录。

## Materials

- [MindIE 远端 NPU RMSNorm 验证记录](blocks/0fd811bac1841c4e0d9c102825eea8c9419c315dff944d9451886d6a34474bbd.md): 在指定容器和 `/mindie` 目录中，初始 `ASCEND_RT_VISIBLE_DEVICES=8` 探测为 `available=False, device_count=0`，改用 `ASCEND_RT_VISIBLE_DEVICES=0` 后报告为可用。作业 `job-20260920T082154Z-0b09e992` 报告退出码 0，FP16/BF16 的 `finite`、`allclose` 和 `passed` 均为 true；这些是脚本和工具报告，不能单独视为已核实成功。期间出现 3 次 `npu-smi info` 不可用提示及一个内部格式 warning，结果文件据称保存于 `/mindie/general-result.json`，SHA256 为 `c0fd57c6a30f2e85666323b47a014c6c211fce506a0e2f083e03e8a24dab897a`；范围仅限单个映射 Ascend910_9362 NPU 上的 5×1536 RMSNorm 小张量测试。

---
blocks:
- block_id: 02a92aec3bd1796409e39643aa737111a232444357e5a8b86ca7552d5a50659c
  indexed: true
  sha256: d50093591595c7e34e71a71f9f416ff99b99ad12694e231b2f8c84f3ca8deabd
  source_range:
    end: 358753
    part: 0
    start: 0
  summary: 用户质疑 config 中 use_rope=True 与 Kimi K3 modeling 未使用 cos/sin 的差异，担心继续构造并传入 cos/sin 造成精度问题，并认为 _restore_per_channel_scale_group_axis 可能应仅为 situ 单独分支处理；相关融合算子尚未开发完成，结论仍属待验证判断。用户要求将仓库改为 vllm-ascend/vllm-ascend-kimi-k3、使用 release/0.23.0 分支，提交包含 AscendC 算子的 PR 并说明修改点，随后先提交以便在其他 A5 机器验证原生权重。此前 assistant 声称发布审计通过、两个提交共涉及 98 个文件且工作树干净，但这些是未独立核验的报告，不能视为验证成功。
  title: Kimi K3 RoPE 与 AscendC PR 适配记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: f59fd6ba463a6bb1907506a1d17222ca3d154eb25608a4ce4bf81363e593f7d1
  kind: experience
  material_digest: 06eea06ef189cb88e9cb098d5fef6ff3d256f74a02d852f4057e0777b2c8e040
  revision: e895f349d47735bcc4c8731f343725bad023b090f9b5e95473fc1454fbfd1773
  schema: mindie-entry/3
  summary: 本批仅包含一个关于 Kimi K3 RoPE/cos-sin 精度风险、_restore_per_channel_scale_group_axis 分支处理、AscendC 算子 PR 及跨 A5 原生权重验证的材料块。需保留待验证假设、融合算子未完成状态，以及此前关于提交审计、文件数量、工作树和安全检查的未核验报告；原始材料仍可独立检索。
  title: 当前任务导航
navigation: 本批仅包含一个关于 Kimi K3 RoPE/cos-sin 精度风险、_restore_per_channel_scale_group_axis 分支处理、AscendC 算子 PR 及跨 A5 原生权重验证的材料块。需保留待验证假设、融合算子未完成状态，以及此前关于提交审计、文件数量、工作树和安全检查的未核验报告；原始材料仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: f59fd6ba463a6bb1907506a1d17222ca3d154eb25608a4ce4bf81363e593f7d1
---

# 当前任务导航

本批仅包含一个关于 Kimi K3 RoPE/cos-sin 精度风险、_restore_per_channel_scale_group_axis 分支处理、AscendC 算子 PR 及跨 A5 原生权重验证的材料块。需保留待验证假设、融合算子未完成状态，以及此前关于提交审计、文件数量、工作树和安全检查的未核验报告；原始材料仍可独立检索。

## Materials

- [Kimi K3 RoPE 与 AscendC PR 适配记录](blocks/02a92aec3bd1796409e39643aa737111a232444357e5a8b86ca7552d5a50659c.md): 用户质疑 config 中 use_rope=True 与 Kimi K3 modeling 未使用 cos/sin 的差异，担心继续构造并传入 cos/sin 造成精度问题，并认为 _restore_per_channel_scale_group_axis 可能应仅为 situ 单独分支处理；相关融合算子尚未开发完成，结论仍属待验证判断。用户要求将仓库改为 vllm-ascend/vllm-ascend-kimi-k3、使用 release/0.23.0 分支，提交包含 AscendC 算子的 PR 并说明修改点，随后先提交以便在其他 A5 机器验证原生权重。此前 assistant 声称发布审计通过、两个提交共涉及 98 个文件且工作树干净，但这些是未独立核验的报告，不能视为验证成功。

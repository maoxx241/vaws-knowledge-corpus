---
blocks:
- block_id: 0b7f72eb61111d5673bdba4e6cb13cd64b89bbe379d2785b88404a56d8cd38e1
  indexed: true
  sha256: b72386b4ee1c18194b03ddbaefffb7c3d97c55598e19554150b18bd05b2df787
  source_range:
    end: 1980635
    part: 0
    start: 0
  summary: 静态审计认为 Python 视觉组网与官方 K3 参考基本一致，涉及视觉 TP/量化边界、projector、占位符注册、MoE 下钻及 metadata 驱动的 rot_proj；多项问题据称已修复，但 rot_proj 权重是否实际加载仍是未确认风险。记录的结构包括 1024 hidden、1536 QKV、12×128 heads、4096 MLP、27 层及 projector 顺序；若干 P2 清理和配置收窄建议仍未完成。定向 UT 未能执行，因为测试环境缺少 torch，结论不能视为验证成功；输入 3D 分支、量化 missing weights 检查等事项保留为不确定或待整改项。
  title: Kimi K3 多模态视觉组网审计索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: e7f70c61bd7f8db8a89b787f479c1220e3a407c48f230b5106e4f3058b3fa663
  kind: experience
  material_digest: 3d8690405b80751e90a088b607f00c36c69ff9fac50cd113293326ddad00740f
  revision: 759be9ad2af2850b4c0e0bdaf12af0908e136ad88f3506317bf920ebf4de7c7f
  schema: mindie-entry/3
  summary: 本批仅包含一个多模态 Python 组网审计块。重点检索：TP8/16 与 data-mode、metadata/rot_proj、量化权重加载、projector 与视觉结构契约、输入 reshape，以及缺少 torch 导致的 UT 未执行；此前所谓“已修复”均为来源中的审计报告，不能替代独立验证，未完成和不确定事项仍需继续追踪。
  title: 当前任务导航：K3 视觉组网审计索引
navigation: 本批仅包含一个多模态 Python 组网审计块。重点检索：TP8/16 与 data-mode、metadata/rot_proj、量化权重加载、projector 与视觉结构契约、输入 reshape，以及缺少 torch 导致的 UT 未执行；此前所谓“已修复”均为来源中的审计报告，不能替代独立验证，未完成和不确定事项仍需继续追踪。
schema: mindie-material-task/1
status: complete
task_id: e7f70c61bd7f8db8a89b787f479c1220e3a407c48f230b5106e4f3058b3fa663
---

# 当前任务导航：K3 视觉组网审计索引

本批仅包含一个多模态 Python 组网审计块。重点检索：TP8/16 与 data-mode、metadata/rot_proj、量化权重加载、projector 与视觉结构契约、输入 reshape，以及缺少 torch 导致的 UT 未执行；此前所谓“已修复”均为来源中的审计报告，不能替代独立验证，未完成和不确定事项仍需继续追踪。

## Materials

- [Kimi K3 多模态视觉组网审计索引](blocks/0b7f72eb61111d5673bdba4e6cb13cd64b89bbe379d2785b88404a56d8cd38e1.md): 静态审计认为 Python 视觉组网与官方 K3 参考基本一致，涉及视觉 TP/量化边界、projector、占位符注册、MoE 下钻及 metadata 驱动的 rot_proj；多项问题据称已修复，但 rot_proj 权重是否实际加载仍是未确认风险。记录的结构包括 1024 hidden、1536 QKV、12×128 heads、4096 MLP、27 层及 projector 顺序；若干 P2 清理和配置收窄建议仍未完成。定向 UT 未能执行，因为测试环境缺少 torch，结论不能视为验证成功；输入 3D 分支、量化 missing weights 检查等事项保留为不确定或待整改项。

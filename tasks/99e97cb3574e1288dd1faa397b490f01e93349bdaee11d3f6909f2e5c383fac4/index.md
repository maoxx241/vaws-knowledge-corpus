---
blocks:
- block_id: 93940cc1b54532b3c32b72a185d664c3a50b470273c4d3e70072e9adde6c20a8
  indexed: true
  sha256: 4375d4c5928c0eb484a5e31468b7ea8e094f46dd29dd3b9f437450b56f7deebd
  source_range:
    end: 5072351
    part: 0
    start: 0
  summary: 围绕 Aurora 的 SparseFlashMla、QLI V2 和融合 Compressor，已按 A2/A3 与 A5 隔离模板，并裁掉模型调用不到的布局、dtype、模式及分支；SparseFlashMla 最终为 A2/A3 6 个 key、A5 12 个 key，QLI V2 两侧各保留 1 个 key。Compressor 原本已按架构各保留 4 个 key，本轮主要收紧 dtype 注册、host 校验，并将空输入计算模板放入 `else` 以消除多余实例化；融合 Compressor 的调用归于 DeepSeek V4，V4.1 使用小算子实现。5 项矩阵回归和 8 项静态回归通过，但整仓仍有已复现的基线格式、拼写及 Meta shape 问题；本机无 CANN/NPU，实际编译耗时和数值回归尚未验证，用户最后仍在确认 Compressor 是否属于原本实现。
  title: 算子与 Compressor 模板裁剪记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 99e97cb3574e1288dd1faa397b490f01e93349bdaee11d3f6909f2e5c383fac4
  kind: experience
  material_digest: 28a6a17836ebb3e4cdf5de11986a88beec5d03647bef919f2a2e66727c83de58
  revision: 30edebd80e020dbc32daa042e409e40aa92fdf205c3a01d394df078e2330b6c6
  schema: mindie-entry/3
  summary: 当前任务聚焦于按硬件架构和模型实际调用收缩编译模板：SparseFlashMla、QLI V2 已完成两层筛选，Compressor 已确认原有架构隔离并修正 dtype 注册与空输入实例化。已有回归验证通过，但 CANN/NPU 实测未完成；整仓基线检查问题仍存在，且 Compressor“是否原本实现”的用户确认尚未完成。
  title: 算子模板裁剪与 Compressor 状态
navigation: 当前任务聚焦于按硬件架构和模型实际调用收缩编译模板：SparseFlashMla、QLI V2 已完成两层筛选，Compressor 已确认原有架构隔离并修正 dtype 注册与空输入实例化。已有回归验证通过，但 CANN/NPU 实测未完成；整仓基线检查问题仍存在，且 Compressor“是否原本实现”的用户确认尚未完成。
schema: mindie-material-task/1
status: complete
task_id: 99e97cb3574e1288dd1faa397b490f01e93349bdaee11d3f6909f2e5c383fac4
---

# 算子模板裁剪与 Compressor 状态

当前任务聚焦于按硬件架构和模型实际调用收缩编译模板：SparseFlashMla、QLI V2 已完成两层筛选，Compressor 已确认原有架构隔离并修正 dtype 注册与空输入实例化。已有回归验证通过，但 CANN/NPU 实测未完成；整仓基线检查问题仍存在，且 Compressor“是否原本实现”的用户确认尚未完成。

## Materials

- [算子与 Compressor 模板裁剪记录](blocks/93940cc1b54532b3c32b72a185d664c3a50b470273c4d3e70072e9adde6c20a8.md): 围绕 Aurora 的 SparseFlashMla、QLI V2 和融合 Compressor，已按 A2/A3 与 A5 隔离模板，并裁掉模型调用不到的布局、dtype、模式及分支；SparseFlashMla 最终为 A2/A3 6 个 key、A5 12 个 key，QLI V2 两侧各保留 1 个 key。Compressor 原本已按架构各保留 4 个 key，本轮主要收紧 dtype 注册、host 校验，并将空输入计算模板放入 `else` 以消除多余实例化；融合 Compressor 的调用归于 DeepSeek V4，V4.1 使用小算子实现。5 项矩阵回归和 8 项静态回归通过，但整仓仍有已复现的基线格式、拼写及 Meta shape 问题；本机无 CANN/NPU，实际编译耗时和数值回归尚未验证，用户最后仍在确认 Compressor 是否属于原本实现。

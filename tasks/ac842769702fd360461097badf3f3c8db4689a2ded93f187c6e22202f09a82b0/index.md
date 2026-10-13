---
blocks:
- block_id: ed3a565c58a7ffa990fef53b77cdc9591c0ad4120600ac56806293df48d5b9c4
  indexed: true
  sha256: a0d826a694c24fd8306b4ff4defc2ae5298ddca09683b33876e55cfa1d83a865
  source_range:
    end: 1001687
    part: 0
    start: 0
  summary: '记录称最终方案为“三类逻辑 page × 四条连续物理 lane”，TP16 下 L0/L1/L2/L3 分别为 46,080/393,216/49,152/49,152 B，统一 page size 为 537,600 B；使用 384-token 调度块、128-token kernel block、3 倍展开、20 个 KV group 和 5 个 raw tensor。地址采用 SoA 与按 lane 分区的 offset，Mamba/MLA NoPE、MLA RoPE/GQA-K 共享对应 lane，GQA-V 使用追加的 L3；记录明确排除了 parent/subslot、b//4 映射、1536 block_size 和 GQA 专用 BlockPool。该材料是 layout 基线与实现/单测引用的记录，不等同于 PR #13277 的 Prefix Cache、多并发或完整四节点验收已完成，且具体实现结果仍应视为未独立验证。'
  title: 四平面最终敲定版：布局、尺寸与分组基线
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ac842769702fd360461097badf3f3c8db4689a2ded93f187c6e22202f09a82b0
  kind: experience
  material_digest: d4bfe1ce649e8765fb9e33da01a970a4bb67a318d67ad4e61fc882761bd0f650
  revision: 4bbfecbafbd75bd9e7b27d0bfbbc790ac1adb22976984edd35900bf0102a98ef
  schema: mindie-entry/3
  summary: 当前可检索材料集中于四平面最终 layout 基线：TP16/TP8 的 lane size 与 shape、537,600 B page size 计算、SoA 地址公式、384→128 block 映射、20 组/5 raw tensor 的分配及 BlockPool 生命周期。记录保留了早期候选与最终方案的区分，并指出 Prefix Cache、多并发和完整四节点验收尚未被该材料证明完成；原始块仍可独立检索，未提供的更早上下文不应推定为已记忆。
  title: 当前任务导航：四平面方案检索索引
navigation: 当前可检索材料集中于四平面最终 layout 基线：TP16/TP8 的 lane size 与 shape、537,600 B page size 计算、SoA 地址公式、384→128 block 映射、20 组/5 raw tensor 的分配及 BlockPool 生命周期。记录保留了早期候选与最终方案的区分，并指出 Prefix Cache、多并发和完整四节点验收尚未被该材料证明完成；原始块仍可独立检索，未提供的更早上下文不应推定为已记忆。
schema: mindie-material-task/1
status: complete
task_id: ac842769702fd360461097badf3f3c8db4689a2ded93f187c6e22202f09a82b0
---

# 当前任务导航：四平面方案检索索引

当前可检索材料集中于四平面最终 layout 基线：TP16/TP8 的 lane size 与 shape、537,600 B page size 计算、SoA 地址公式、384→128 block 映射、20 组/5 raw tensor 的分配及 BlockPool 生命周期。记录保留了早期候选与最终方案的区分，并指出 Prefix Cache、多并发和完整四节点验收尚未被该材料证明完成；原始块仍可独立检索，未提供的更早上下文不应推定为已记忆。

## Materials

- [四平面最终敲定版：布局、尺寸与分组基线](blocks/ed3a565c58a7ffa990fef53b77cdc9591c0ad4120600ac56806293df48d5b9c4.md): 记录称最终方案为“三类逻辑 page × 四条连续物理 lane”，TP16 下 L0/L1/L2/L3 分别为 46,080/393,216/49,152/49,152 B，统一 page size 为 537,600 B；使用 384-token 调度块、128-token kernel block、3 倍展开、20 个 KV group 和 5 个 raw tensor。地址采用 SoA 与按 lane 分区的 offset，Mamba/MLA NoPE、MLA RoPE/GQA-K 共享对应 lane，GQA-V 使用追加的 L3；记录明确排除了 parent/subslot、b//4 映射、1536 block_size 和 GQA 专用 BlockPool。该材料是 layout 基线与实现/单测引用的记录，不等同于 PR #13277 的 Prefix Cache、多并发或完整四节点验收已完成，且具体实现结果仍应视为未独立验证。

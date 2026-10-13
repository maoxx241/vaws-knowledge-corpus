---
blocks:
- block_id: 50f481fe4c2ea0624fa706f8f19f8ef2758b0e51c57eebfe75f7e0bd8ccd0797
  indexed: true
  sha256: ec5d69ff3814738a802c983ba54a900ff40e641e2be89d514f87271601b1ede6
  source_range:
    end: 5071589
    part: 0
    start: 0
  summary: 只读审计确认：四平面中 P1/P2/P3 的 4 KiB 对齐随 N mod 4 变化，但当前 Triton Mamba copy 仅做 uint8 字节搬运，尚无证据证明存在 4 KiB 硬约束；miss896 第二段确实执行与 hit 相同的 prev0→curr2 copy，因此这些观察尚未闭合 hit-only 精度问题的根因。建议以 N=5164 做最小 A/B，并回读真实四平面指针对齐、P0 block-ID residue、P1 0→2 copy 和 guard 区；该验证仍未完成，5164/5166 等结果不得视为成功。r13 已从 153 的已验证树复制到 154–156，四机 selected11 哈希为 6b273da5b4abe7a59ebefdbf3f41d710b7e66a80e456356412234644388d2d67，但回读时发现服务已由其他流程运行，未由本任务启动、停止或修改。
  title: 四平面对齐审计与 r13 部署状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ac027b638df85138224559f1b15fd2774cb8abeb9a1ff88b692c152c39402274
  kind: experience
  material_digest: cc8aa92d92e9f991d12abede84c075ae61fdf31536a77bee8bafeb49335f7aec
  revision: d9427a12a21c24df57436e325aa8918a4fd3287c5beb8f5d0f6aee20b6a92232
  schema: mindie-entry/3
  summary: 当前可用结论包括：旧 3-plane 历史碰撞可由地址等式复现，四-plane 将其变为合法别名；首次 hit384 的运行态目标块会复制命中 block，不能仅凭新分配 block 归因于未清零；miss896 的 0→2 copy 路径与 hit 相同。r13 四机代码树已按 153 单一基线完成部署并冻结哈希，但各节点旧 r12 存在测试/缓存漂移，且运行服务状态来自其他流程。后续重点是 N=5164 最小 A/B 与 direct batch-copy probe，区分 alignment/容量-ID、cached source 内容、ownership/COW、block table 及 P0 block-ID residue；这些仍是候选假设，尚无已验证修复。
  title: DSpark/Mamba hit-only 异常：r13 基线与对齐假设待验证
navigation: 当前可用结论包括：旧 3-plane 历史碰撞可由地址等式复现，四-plane 将其变为合法别名；首次 hit384 的运行态目标块会复制命中 block，不能仅凭新分配 block 归因于未清零；miss896 的 0→2 copy 路径与 hit 相同。r13 四机代码树已按 153 单一基线完成部署并冻结哈希，但各节点旧 r12 存在测试/缓存漂移，且运行服务状态来自其他流程。后续重点是 N=5164 最小 A/B 与 direct batch-copy probe，区分 alignment/容量-ID、cached source 内容、ownership/COW、block table 及 P0 block-ID residue；这些仍是候选假设，尚无已验证修复。
schema: mindie-material-task/1
status: complete
task_id: ac027b638df85138224559f1b15fd2774cb8abeb9a1ff88b692c152c39402274
---

# DSpark/Mamba hit-only 异常：r13 基线与对齐假设待验证

当前可用结论包括：旧 3-plane 历史碰撞可由地址等式复现，四-plane 将其变为合法别名；首次 hit384 的运行态目标块会复制命中 block，不能仅凭新分配 block 归因于未清零；miss896 的 0→2 copy 路径与 hit 相同。r13 四机代码树已按 153 单一基线完成部署并冻结哈希，但各节点旧 r12 存在测试/缓存漂移，且运行服务状态来自其他流程。后续重点是 N=5164 最小 A/B 与 direct batch-copy probe，区分 alignment/容量-ID、cached source 内容、ownership/COW、block table 及 P0 block-ID residue；这些仍是候选假设，尚无已验证修复。

## Materials

- [四平面对齐审计与 r13 部署状态](blocks/50f481fe4c2ea0624fa706f8f19f8ef2758b0e51c57eebfe75f7e0bd8ccd0797.md): 只读审计确认：四平面中 P1/P2/P3 的 4 KiB 对齐随 N mod 4 变化，但当前 Triton Mamba copy 仅做 uint8 字节搬运，尚无证据证明存在 4 KiB 硬约束；miss896 第二段确实执行与 hit 相同的 prev0→curr2 copy，因此这些观察尚未闭合 hit-only 精度问题的根因。建议以 N=5164 做最小 A/B，并回读真实四平面指针对齐、P0 block-ID residue、P1 0→2 copy 和 guard 区；该验证仍未完成，5164/5166 等结果不得视为成功。r13 已从 153 的已验证树复制到 154–156，四机 selected11 哈希为 6b273da5b4abe7a59ebefdbf3f41d710b7e66a80e456356412234644388d2d67，但回读时发现服务已由其他流程运行，未由本任务启动、停止或修改。

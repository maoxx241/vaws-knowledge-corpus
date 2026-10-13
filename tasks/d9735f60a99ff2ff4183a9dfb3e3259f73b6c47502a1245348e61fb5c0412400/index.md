---
blocks:
- block_id: 02b63da21bec6d118ea31372e38f9d545b2ecbd90867fe95531559c88ea256e9
  indexed: true
  sha256: d489bf1a45ba285b3683d9a5cd3b38c23fcd17a160a54589db6293e1dd4ddaf0
  source_range:
    end: 15961386
    part: 0
    start: 0
  summary: 记录了 Kimi 0.38.0、Cursor Agent 2026.08.25-3e8eec8 及生产 facade 的实测：Kimi 未注入原生 mcp__* 工具而使用 Bash fallback，Cursor 受 workspace trust 与 MCP approval 阻断；这些结果不能算严格 client-side MCP 通过。153 上 runtime、专用 SSHD 46168、环境注入和 `acl/torch_npu/vllm/vllm_ascend` 导入均有验收记录，C 任务曾以 `running`、`lease_state=active` 完成 NPU HTTP checksum 检查，随后进入 finishing/stopping；文档还明确保留 `unknown`、subreaper 和未验证的硬件/模型范围。
  title: 多客户端生命周期验收与 C 任务复测
- block_id: 96328c39f22a7a56e06d678d23d72d2812458398d31f210776902c165832b5a9
  indexed: true
  sha256: 5c12bdc75a1e817189420656de6fbfd5c77b3e66673648b6514d94ac5fc3d01f
  source_range:
    end: 15961386
    part: 1
    start: 0
  summary: C 最终收尾记录为 `lease_state=released`、`runtime_returned=true`、`remote quiet=true`、`processes=[]`、`unknown=[]`、`descendants_drained=true`，退出码为受控取消的 `-15`；证据保存在 `.remote-dev/state/pr66-lifecycle/tasks/c/`，哈希已发送给 root。随后 `c finish` 落为 `finished`，仅撤销已核实的 46168 专用 listener、`authorized_keys` 和 host 私钥；默认 SSHD PID 733、容器/runtime、环境、源码、产物和日志保留，18868 与 probe 已清空，并写入 cleanup receipt。
  title: C 任务释放与专用监听清理
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: d9735f60a99ff2ff4183a9dfb3e3259f73b6c47502a1245348e61fb5c0412400
  kind: experience
  material_digest: 732029f0b3fd20a6febfa92cebca1aa065e51a497b6a0df3b5b872b6783ee94a
  revision: 3026b29852095de9d198e4538e407fdd85a774888716d58868411887939d97e5
  schema: mindie-entry/3
  summary: 先查 block 02b63da21bec6d118ea31372e38f9d545b2ecbd90867fe95531559c88ea256e9 获取多客户端 MCP/信任阻断、153 runtime 部署、C 任务运行状态及“显式 adapter 验收不等于端到端通过”的背景；再查 block 96328c39f22a7a56e06d678d23d72d2812458398d31f210776902c165832b5a9 获取 C 的最终释放、进程排空、专用 SSHD 撤销和 cleanup receipt。保留此前的 fallback、approval pending、unknown 保留规则与未验证的模型正确性边界；后续如需核验，应独立搜索原始证据文件。
  title: 当前任务导航：C 生命周期验收、边界与清理
navigation: 先查 block 02b63da21bec6d118ea31372e38f9d545b2ecbd90867fe95531559c88ea256e9 获取多客户端 MCP/信任阻断、153 runtime 部署、C 任务运行状态及“显式 adapter 验收不等于端到端通过”的背景；再查 block 96328c39f22a7a56e06d678d23d72d2812458398d31f210776902c165832b5a9 获取 C 的最终释放、进程排空、专用 SSHD 撤销和 cleanup receipt。保留此前的 fallback、approval pending、unknown 保留规则与未验证的模型正确性边界；后续如需核验，应独立搜索原始证据文件。
schema: mindie-material-task/1
status: complete
task_id: d9735f60a99ff2ff4183a9dfb3e3259f73b6c47502a1245348e61fb5c0412400
---

# 当前任务导航：C 生命周期验收、边界与清理

先查 block 02b63da21bec6d118ea31372e38f9d545b2ecbd90867fe95531559c88ea256e9 获取多客户端 MCP/信任阻断、153 runtime 部署、C 任务运行状态及“显式 adapter 验收不等于端到端通过”的背景；再查 block 96328c39f22a7a56e06d678d23d72d2812458398d31f210776902c165832b5a9 获取 C 的最终释放、进程排空、专用 SSHD 撤销和 cleanup receipt。保留此前的 fallback、approval pending、unknown 保留规则与未验证的模型正确性边界；后续如需核验，应独立搜索原始证据文件。

## Materials

- [多客户端生命周期验收与 C 任务复测](blocks/02b63da21bec6d118ea31372e38f9d545b2ecbd90867fe95531559c88ea256e9.md): 记录了 Kimi 0.38.0、Cursor Agent 2026.08.25-3e8eec8 及生产 facade 的实测：Kimi 未注入原生 mcp__* 工具而使用 Bash fallback，Cursor 受 workspace trust 与 MCP approval 阻断；这些结果不能算严格 client-side MCP 通过。153 上 runtime、专用 SSHD 46168、环境注入和 `acl/torch_npu/vllm/vllm_ascend` 导入均有验收记录，C 任务曾以 `running`、`lease_state=active` 完成 NPU HTTP checksum 检查，随后进入 finishing/stopping；文档还明确保留 `unknown`、subreaper 和未验证的硬件/模型范围。
- [C 任务释放与专用监听清理](blocks/96328c39f22a7a56e06d678d23d72d2812458398d31f210776902c165832b5a9.md): C 最终收尾记录为 `lease_state=released`、`runtime_returned=true`、`remote quiet=true`、`processes=[]`、`unknown=[]`、`descendants_drained=true`，退出码为受控取消的 `-15`；证据保存在 `.remote-dev/state/pr66-lifecycle/tasks/c/`，哈希已发送给 root。随后 `c finish` 落为 `finished`，仅撤销已核实的 46168 专用 listener、`authorized_keys` 和 host 私钥；默认 SSHD PID 733、容器/runtime、环境、源码、产物和日志保留，18868 与 probe 已清空，并写入 cleanup receipt。

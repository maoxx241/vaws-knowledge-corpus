---
blocks:
- block_id: 333b312cd7e92ff0f5b4ffc80df41fec23da99d12c0d4c60d290e4d2962171bf
  indexed: true
  sha256: 3909abdb6f7b31fd60b54b093f44742022b70728dcf2ff6006ac7ab746e5c600
  source_range:
    end: 489476
    part: 0
    start: 0
  summary: 复核结论认为旧 buffer 补丁不能证明解决 `None`/padding 问题，建议在 `q_conv1d` 注册固定形状、dtype 和地址的 `conv_weights_t_packed` Parameter，并通过 `replace_parameter(..., prefer_copy=True)` 在 q/k/v 权重全部离开 meta 后完成 repack。完整 checkpoint reload 可在 finalize 阶段重打包，kernel-format direct reload 必须直接加载派生参数名；partial q/k/v 更新不作保证，ACLGraph 仍需 capture→reload→replay 与 eager 输出对照验证。已核实 v0.25.1 使用 `get_parameter(name).copy_()`，但本轮未改文件或操作服务，当前异常仍应通过 SGLang 同输入、K=7、16-token 图形状的逐层差分调查。
  title: packed conv 参数生命周期与 reload 契约
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 4ca972fbd911115e30df1b27ea1e12f9459574e33f242d9f283a766a6fbe8b50
  kind: experience
  material_digest: db58123d75fdcf2d1794dad8f206fc4bb8546c077e163635946f35af6936b04a
  revision: f2c7e15b9c4e15f6d29ab49107bf2efe858028e466494b480776aaba1a206c5a
  schema: mindie-entry/3
  summary: 重点关注 KDA q/k/v packed Parameter 的固定地址、完整 checkpoint reload、kernel-format direct reload 及 ACLGraph 验证边界。保留不确定性：partial reload 未覆盖，`replace_parameter` 在 meta 状态下可能创建临时 Parameter，且 prepack 方案尚未证明修复 `None`/连续输出异常；后续应进行 SGLang 同条件逐层差分并运行所列四组单元测试。
  title: 当前任务导航：packed weight reload 与异常差分
navigation: 重点关注 KDA q/k/v packed Parameter 的固定地址、完整 checkpoint reload、kernel-format direct reload 及 ACLGraph 验证边界。保留不确定性：partial reload 未覆盖，`replace_parameter` 在 meta 状态下可能创建临时 Parameter，且 prepack 方案尚未证明修复 `None`/连续输出异常；后续应进行 SGLang 同条件逐层差分并运行所列四组单元测试。
schema: mindie-material-task/1
status: complete
task_id: 4ca972fbd911115e30df1b27ea1e12f9459574e33f242d9f283a766a6fbe8b50
---

# 当前任务导航：packed weight reload 与异常差分

重点关注 KDA q/k/v packed Parameter 的固定地址、完整 checkpoint reload、kernel-format direct reload 及 ACLGraph 验证边界。保留不确定性：partial reload 未覆盖，`replace_parameter` 在 meta 状态下可能创建临时 Parameter，且 prepack 方案尚未证明修复 `None`/连续输出异常；后续应进行 SGLang 同条件逐层差分并运行所列四组单元测试。

## Materials

- [packed conv 参数生命周期与 reload 契约](blocks/333b312cd7e92ff0f5b4ffc80df41fec23da99d12c0d4c60d290e4d2962171bf.md): 复核结论认为旧 buffer 补丁不能证明解决 `None`/padding 问题，建议在 `q_conv1d` 注册固定形状、dtype 和地址的 `conv_weights_t_packed` Parameter，并通过 `replace_parameter(..., prefer_copy=True)` 在 q/k/v 权重全部离开 meta 后完成 repack。完整 checkpoint reload 可在 finalize 阶段重打包，kernel-format direct reload 必须直接加载派生参数名；partial q/k/v 更新不作保证，ACLGraph 仍需 capture→reload→replay 与 eager 输出对照验证。已核实 v0.25.1 使用 `get_parameter(name).copy_()`，但本轮未改文件或操作服务，当前异常仍应通过 SGLang 同输入、K=7、16-token 图形状的逐层差分调查。

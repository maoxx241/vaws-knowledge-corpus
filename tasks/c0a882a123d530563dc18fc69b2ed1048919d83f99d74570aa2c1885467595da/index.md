---
blocks:
- block_id: 5c67d69bfcd963f47323133fcd506f8dfa0775e3733328d7875e1320f01789a5
  indexed: true
  sha256: bb381ab7640614ac276d1846fe057238689f8dace76428a4f18ad009427fd610
  source_range:
    end: 793203
    part: 0
    start: 0
  summary: 当前判断认为 K3 的修改未改变 fused MoE 的路由、专家计算、EP 后端或输出归并；若启用 FlashComm 后出现 token 不一致，应优先排查 decode 阶段的 KDA/MLA 状态、ACLGraph replay、row-parallel 归约顺序及 recurrent state，但这仍是定位方向而非已验证结论。enforce eager 场景应先单独确认 fc1 精度，再检查 ACLGraph 首层处理；pad/unpad 原则上复用框架 token 轴原语。K3 仍需保留 KDA/MLA 的一次性 gather、MLA gate 的 hidden-state gather，以及首层 3D learned residual 的 squeeze/chunk/unsqueeze 特例；attention 长度的 ceil-div 和 Dynamo shape anchor 属于待验证的最小调整。
  title: K3 FlashComm 精度与首层布局审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c0a882a123d530563dc18fc69b2ed1048919d83f99d74570aa2c1885467595da
  kind: experience
  material_digest: 8d98ae2a28a9a2280460eb8bca2a22c9ade834cd5f7f448df874291bfdfc3ce6
  revision: 52ab6c68f89a3e894b7ff5bdff7ad31825f421d9fb1bc5b0a584403f323be90d
  schema: mindie-entry/3
  summary: 先在 enforce eager 下确认 fc1 精度，再独立排查 ACLGraph 首层路径；不要将未验证的精度判断或排查方向视为成功结论。继续复用框架 token 轴的 pad/unpad，并保留 K3 必要的 KDA/MLA gather 与首层 learned residual 布局处理；后续需记录失败尝试、不确定性及任何修正。
  title: 当前任务：分离验证 K3 FlashComm 的 eager 精度与 ACLGraph 首层问题
navigation: 先在 enforce eager 下确认 fc1 精度，再独立排查 ACLGraph 首层路径；不要将未验证的精度判断或排查方向视为成功结论。继续复用框架 token 轴的 pad/unpad，并保留 K3 必要的 KDA/MLA gather 与首层 learned residual 布局处理；后续需记录失败尝试、不确定性及任何修正。
schema: mindie-material-task/1
status: complete
task_id: c0a882a123d530563dc18fc69b2ed1048919d83f99d74570aa2c1885467595da
---

# 当前任务：分离验证 K3 FlashComm 的 eager 精度与 ACLGraph 首层问题

先在 enforce eager 下确认 fc1 精度，再独立排查 ACLGraph 首层路径；不要将未验证的精度判断或排查方向视为成功结论。继续复用框架 token 轴的 pad/unpad，并保留 K3 必要的 KDA/MLA gather 与首层 learned residual 布局处理；后续需记录失败尝试、不确定性及任何修正。

## Materials

- [K3 FlashComm 精度与首层布局审计](blocks/5c67d69bfcd963f47323133fcd506f8dfa0775e3733328d7875e1320f01789a5.md): 当前判断认为 K3 的修改未改变 fused MoE 的路由、专家计算、EP 后端或输出归并；若启用 FlashComm 后出现 token 不一致，应优先排查 decode 阶段的 KDA/MLA 状态、ACLGraph replay、row-parallel 归约顺序及 recurrent state，但这仍是定位方向而非已验证结论。enforce eager 场景应先单独确认 fc1 精度，再检查 ACLGraph 首层处理；pad/unpad 原则上复用框架 token 轴原语。K3 仍需保留 KDA/MLA 的一次性 gather、MLA gate 的 hidden-state gather，以及首层 3D learned residual 的 squeeze/chunk/unsqueeze 特例；attention 长度的 ceil-div 和 Dynamo shape anchor 属于待验证的最小调整。

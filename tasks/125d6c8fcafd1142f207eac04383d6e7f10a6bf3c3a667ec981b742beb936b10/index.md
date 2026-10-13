---
blocks:
- block_id: 3ffbaefc4d6d2f3f8d468fa27e829b544f508d999fc98866df789cb5f7fb531d
  indexed: true
  sha256: dfd07943f989e67a6c61a865354ddaf85f9827ca8f955cc7ea6d0c235eb71b01
  source_range:
    end: 870426
    part: 0
    start: 0
  summary: 当前判断是 K3 FlashComm 修改未涉及 fused MoE 路由、专家计算、EP 后端或输出归并；主要差异集中在 KDA/MLA token 布局、MLA gate 和 learned residual，仍可能存在极小 GEMM 浮点舍入差异。只读分析认为首层 global→local 转换、现有 token 轴 pad/unpad 逻辑和 DP1/TP16 布局基本闭合，但这不是已验证的成功结论；`enforce_eager`、`torch.compile + cudagraph NONE` 与 FULL_DECODE_ONLY 仍需分层测试。当前重点风险包括 decode recurrent/conv state、ACLGraph persistent metadata、fake/meta 动态 shape，以及 capture size 16、active request 1 时的 inactive rows 屏蔽；未修改文件或操作远端。
  title: K3 FlashComm 首层布局与精度排查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 125d6c8fcafd1142f207eac04383d6e7f10a6bf3c3a667ec981b742beb936b10
  kind: experience
  material_digest: f70d540e6170c5bc1658638b868bf7f8549feb13c9c143b76c69bfd135ef50d2
  revision: df7b380c54833d0282b0e4defe18f5e5e23a5fe241a2b568d899fb03861c44a7
  schema: mindie-entry/3
  summary: 先按纯文本/图片及 FlashComm 开关对比 `enforce_eager` 的 fc1 和关键 canonical tensor，再区分 compile/fake-meta 与 ACLGraph replay；若 eager 已一致，继续检查 decode recurrent state、`query_start_loc`、`state_index`、inactive rows 清零和 graph padding metadata。现有结论仍属可证伪的诊断参考：首层布局与复用框架 pad/unpad 的判断尚未证明问题不存在，后续结果应保留失败尝试、未完成项和修正。
  title: K3 FlashComm 精度与 Graph 排查导航
navigation: 先按纯文本/图片及 FlashComm 开关对比 `enforce_eager` 的 fc1 和关键 canonical tensor，再区分 compile/fake-meta 与 ACLGraph replay；若 eager 已一致，继续检查 decode recurrent state、`query_start_loc`、`state_index`、inactive rows 清零和 graph padding metadata。现有结论仍属可证伪的诊断参考：首层布局与复用框架 pad/unpad 的判断尚未证明问题不存在，后续结果应保留失败尝试、未完成项和修正。
schema: mindie-material-task/1
status: complete
task_id: 125d6c8fcafd1142f207eac04383d6e7f10a6bf3c3a667ec981b742beb936b10
---

# K3 FlashComm 精度与 Graph 排查导航

先按纯文本/图片及 FlashComm 开关对比 `enforce_eager` 的 fc1 和关键 canonical tensor，再区分 compile/fake-meta 与 ACLGraph replay；若 eager 已一致，继续检查 decode recurrent state、`query_start_loc`、`state_index`、inactive rows 清零和 graph padding metadata。现有结论仍属可证伪的诊断参考：首层布局与复用框架 pad/unpad 的判断尚未证明问题不存在，后续结果应保留失败尝试、未完成项和修正。

## Materials

- [K3 FlashComm 首层布局与精度排查](blocks/3ffbaefc4d6d2f3f8d468fa27e829b544f508d999fc98866df789cb5f7fb531d.md): 当前判断是 K3 FlashComm 修改未涉及 fused MoE 路由、专家计算、EP 后端或输出归并；主要差异集中在 KDA/MLA token 布局、MLA gate 和 learned residual，仍可能存在极小 GEMM 浮点舍入差异。只读分析认为首层 global→local 转换、现有 token 轴 pad/unpad 逻辑和 DP1/TP16 布局基本闭合，但这不是已验证的成功结论；`enforce_eager`、`torch.compile + cudagraph NONE` 与 FULL_DECODE_ONLY 仍需分层测试。当前重点风险包括 decode recurrent/conv state、ACLGraph persistent metadata、fake/meta 动态 shape，以及 capture size 16、active request 1 时的 inactive rows 屏蔽；未修改文件或操作远端。

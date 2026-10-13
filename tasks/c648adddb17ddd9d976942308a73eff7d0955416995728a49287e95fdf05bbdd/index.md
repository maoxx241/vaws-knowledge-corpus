---
blocks:
- block_id: d0c6b5463030cb93922a0de4312b634473e7832044e931a5818f7e96f26b2558
  indexed: true
  sha256: 3c8a973ce9f750cfca4c4707c344646c94101bb49721d5633012230729fa96ce
  source_range:
    end: 19269392
    part: 0
    start: 0
  summary: 用户要求在显存分析工作簿中新增TP通信量页，并通过153/154上的16专家TP8/TP16服务确认通信节点；153的TP16启动进入初始化后失败，未形成可用请求证据。154的TP8服务最终健康检查通过并完成16-token prefill、2-token decode请求，实测记录Embedding AllReduce，以及启用sequence parallel后的KDA/MLA和Shared expert的Attention前AllGather、o_proj后ReduceScatter；decode token还按TP补齐，TP8实测为8行。期间发现154旧版`chunk_kda_fwd` ABI（15参数而当前调用20参数）和远端脚本误指向旧custom OPP，均通过隔离复制/路径修正处理；工作簿已更新为EP+SP路径、`T_pad=CEILING(T/TP)*TP`及实测核对区，并修复合并标题渲染`#VALUE!`与K输入格复用问题，任务服务清理状态需以最后的端口/NPU复查为准。
  title: TP通信量工作簿与TP8服务实测
- block_id: 493f082e9ce0579fd2f40228c4f052d41fcf376834458643a182dc0ffb8e5b5e
  indexed: true
  sha256: 756e6715064ab99cd5187ab6f873e68952c53b498a3a84b2230e6bda8a2bcb12
  source_range:
    end: 32644759
    part: 0
    start: 19269392
  summary: 曾报告在154上对TP8/TP16专家服务完成真实请求并据此修订工作簿：涉及Embedding、KDA/MLA、Dense MLP、Shared expert、LM head的AllReduce、AllGather和ReduceScatter路径，以及decode的T_pad补齐；但Fine embedding、Fine OTP和Fine LM head未动态触发，仅有代码路径证据，EP通信也被排除。随后按用户要求移除154实测、说明文字、对比表和批注，只保留左侧各通信算子的Collective、规模、通信组大小、调用次数及公式结果，右侧保留必要参数和路径开关；报告公式检查无错误，但这些内容仍属于该批次中的报告性结果。
  title: TP通信公式页的实测修订与最终精简
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c648adddb17ddd9d976942308a73eff7d0955416995728a49287e95fdf05bbdd
  kind: experience
  material_digest: 3160b4ea4e014784b8457b3bf704ab8879865af62d751ef24e595311f66aeefd
  revision: 2e1ab1e39668d5c8951a745ed649e485e4ee7e0507a20e7f9bc5536b79b7aa10
  schema: mindie-entry/3
  summary: 当前任务聚焦于显存分析工作簿的TP通信量公式页。此前材料报告了TP8/TP16服务实测、TP>1+EP下的Sequence Parallel路径及decode补齐规则，但部分Fine路径仅有代码证据，且此前存在旧ABI/旧OPP、153 TP16未完成请求等未完成或环境相关问题；本批次随后将154验证信息和展示性说明从最终页面删除，仅保留通信算子公式及其可修改依赖参数。原始实测、失败尝试、环境偏差和未完成结果仍应独立检索，不应将公式无错误或报告的实测自动视为全面验证成功。
  title: TP通信公式页精简与历史实测索引
navigation: 当前任务聚焦于显存分析工作簿的TP通信量公式页。此前材料报告了TP8/TP16服务实测、TP>1+EP下的Sequence Parallel路径及decode补齐规则，但部分Fine路径仅有代码证据，且此前存在旧ABI/旧OPP、153 TP16未完成请求等未完成或环境相关问题；本批次随后将154验证信息和展示性说明从最终页面删除，仅保留通信算子公式及其可修改依赖参数。原始实测、失败尝试、环境偏差和未完成结果仍应独立检索，不应将公式无错误或报告的实测自动视为全面验证成功。
schema: mindie-material-task/1
status: complete
task_id: c648adddb17ddd9d976942308a73eff7d0955416995728a49287e95fdf05bbdd
---

# TP通信公式页精简与历史实测索引

当前任务聚焦于显存分析工作簿的TP通信量公式页。此前材料报告了TP8/TP16服务实测、TP>1+EP下的Sequence Parallel路径及decode补齐规则，但部分Fine路径仅有代码证据，且此前存在旧ABI/旧OPP、153 TP16未完成请求等未完成或环境相关问题；本批次随后将154验证信息和展示性说明从最终页面删除，仅保留通信算子公式及其可修改依赖参数。原始实测、失败尝试、环境偏差和未完成结果仍应独立检索，不应将公式无错误或报告的实测自动视为全面验证成功。

## Materials

- [TP通信量工作簿与TP8服务实测](blocks/d0c6b5463030cb93922a0de4312b634473e7832044e931a5818f7e96f26b2558.md): 用户要求在显存分析工作簿中新增TP通信量页，并通过153/154上的16专家TP8/TP16服务确认通信节点；153的TP16启动进入初始化后失败，未形成可用请求证据。154的TP8服务最终健康检查通过并完成16-token prefill、2-token decode请求，实测记录Embedding AllReduce，以及启用sequence parallel后的KDA/MLA和Shared expert的Attention前AllGather、o_proj后ReduceScatter；decode token还按TP补齐，TP8实测为8行。期间发现154旧版`chunk_kda_fwd` ABI（15参数而当前调用20参数）和远端脚本误指向旧custom OPP，均通过隔离复制/路径修正处理；工作簿已更新为EP+SP路径、`T_pad=CEILING(T/TP)*TP`及实测核对区，并修复合并标题渲染`#VALUE!`与K输入格复用问题，任务服务清理状态需以最后的端口/NPU复查为准。
- [TP通信公式页的实测修订与最终精简](blocks/493f082e9ce0579fd2f40228c4f052d41fcf376834458643a182dc0ffb8e5b5e.md): 曾报告在154上对TP8/TP16专家服务完成真实请求并据此修订工作簿：涉及Embedding、KDA/MLA、Dense MLP、Shared expert、LM head的AllReduce、AllGather和ReduceScatter路径，以及decode的T_pad补齐；但Fine embedding、Fine OTP和Fine LM head未动态触发，仅有代码路径证据，EP通信也被排除。随后按用户要求移除154实测、说明文字、对比表和批注，只保留左侧各通信算子的Collective、规模、通信组大小、调用次数及公式结果，右侧保留必要参数和路径开关；报告公式检查无错误，但这些内容仍属于该批次中的报告性结果。

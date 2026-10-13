---
blocks:
- block_id: 501becb29d49a8f7b215f19fec3c7f2a2e0df8737e4733f69fff1a603b0191e4
  indexed: true
  sha256: ac58e2a4641d7677ad10f4aede3c2818bed8c6dfd7b7c4f83eb7e6d37541e98c
  source_range:
    end: 7446170
    part: 0
    start: 0
  summary: 历史检索将 K3 独立启动失败保守统计为至少 82 起，并指出 VAWS 当前 serving 偏单机、健康检查较浅、残留清理和多机资源协调不足；模型、算子、图、KV 与并行配置缺陷不能靠部署工具消除。VAWS 的并行机制已有 session、worktree、容器和租约，但跨工作区协调、队头阻塞、租约释放及 host 网络/共享挂载隔离仍是风险；submodule 还可能因递归更新切回锁定提交或在本地修改时失败。后续修正了“工具仓复制一份 VA、业务目录再维护一份 VA”的方案：真正的外部仓库模式应让 VAWS 只提供工具和适配器，直接绑定指定 VA/vLLM 工作树，以快照记录版本和未提交修改，不要求持续 rebase；Git 历史可解耦，但工具与 VA/vLLM 版本兼容性仍需由版本化配置和适配器处理。上述统计与判断来自历史记录和源码核对，未进行远端实测，也未修改或重启服务。
  title: K3 部署失败、VAWS 并行与仓库解耦边界
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 136da5a48761e12c4cf7bf9bf73a8e67b492e37d6d6192e2bf4140776d591122
  kind: experience
  material_digest: 1d542bf5a41b48df01932895f97ed72491bbc5a4eb742215322c61c2ebc444fd
  revision: 340d634643d638180d851f145b92d526a1ce9439aa46df231f76892c771c621c
  schema: mindie-entry/3
  summary: 已覆盖 K3 远程部署及拉起失败分类、VAWS 能力边界、多任务并行和 submodule 风险。关键未决方向是补齐四机联合生命周期、统一资源调度、残留清理、分阶段/真实请求验收，并实现不复制业务源码的外部仓库模式；原始记录仍需独立检索，当前结论不等同于验证成功。
  title: 当前任务导航：K3/VAWS 部署与仓库解耦
navigation: 已覆盖 K3 远程部署及拉起失败分类、VAWS 能力边界、多任务并行和 submodule 风险。关键未决方向是补齐四机联合生命周期、统一资源调度、残留清理、分阶段/真实请求验收，并实现不复制业务源码的外部仓库模式；原始记录仍需独立检索，当前结论不等同于验证成功。
schema: mindie-material-task/1
status: complete
task_id: 136da5a48761e12c4cf7bf9bf73a8e67b492e37d6d6192e2bf4140776d591122
---

# 当前任务导航：K3/VAWS 部署与仓库解耦

已覆盖 K3 远程部署及拉起失败分类、VAWS 能力边界、多任务并行和 submodule 风险。关键未决方向是补齐四机联合生命周期、统一资源调度、残留清理、分阶段/真实请求验收，并实现不复制业务源码的外部仓库模式；原始记录仍需独立检索，当前结论不等同于验证成功。

## Materials

- [K3 部署失败、VAWS 并行与仓库解耦边界](blocks/501becb29d49a8f7b215f19fec3c7f2a2e0df8737e4733f69fff1a603b0191e4.md): 历史检索将 K3 独立启动失败保守统计为至少 82 起，并指出 VAWS 当前 serving 偏单机、健康检查较浅、残留清理和多机资源协调不足；模型、算子、图、KV 与并行配置缺陷不能靠部署工具消除。VAWS 的并行机制已有 session、worktree、容器和租约，但跨工作区协调、队头阻塞、租约释放及 host 网络/共享挂载隔离仍是风险；submodule 还可能因递归更新切回锁定提交或在本地修改时失败。后续修正了“工具仓复制一份 VA、业务目录再维护一份 VA”的方案：真正的外部仓库模式应让 VAWS 只提供工具和适配器，直接绑定指定 VA/vLLM 工作树，以快照记录版本和未提交修改，不要求持续 rebase；Git 历史可解耦，但工具与 VA/vLLM 版本兼容性仍需由版本化配置和适配器处理。上述统计与判断来自历史记录和源码核对，未进行远端实测，也未修改或重启服务。

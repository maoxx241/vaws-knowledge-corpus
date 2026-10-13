---
blocks:
- block_id: a71c6c1f17d7cb729bd6f9609a7692d07f062c63c28cf789c8f2eba1489ff2da
  indexed: true
  sha256: 75299951c568c269441f614aa37f8d05665409d38ee1826fe3b1fb7797f8d953
  source_range:
    end: 386399
    part: 0
    start: 0
  summary: 曾将 `upstream` 和 fork 目标误解为 `vllm-project/vllm-ascend`，随后根据用户纠正，确认目标为私有仓库 `vllm-ascend/vllm-ascend-kimi-k3`；此前的 fork 重命名、远端配置和分支推送结果不能视为已完成当前目标。只读检查报告称该仓库默认分支为 `releases/v0.23.0`、fork 被禁用、无团队授权和分支保护，少数管理员可直接 push；但开启仓库级 fork 的请求因组织级禁止私有仓库 fork而以 HTTP 422 失败，未产生设置变更。后续说明认为仓库级 `Allow forking` 需组织级许可作为前置条件，用户虽有仓库 `admin` 权限却不是组织成员或 owner，无法修改组织级策略；分支保护在当前私有仓库套餐下也被报告为不可用，需组织 owner 开启私有仓库 fork，并可能升级套餐或公开仓库后再处理。
  title: Kimi K3 仓库 fork 与权限策略核查
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: c5e1a17389f828a49806b25a20e5ecbf490cfb49ab9533e6b09b42ee49fa254a
  kind: experience
  material_digest: c3ce4eb9439f359649a2706b0ca4031cd79a834deaa9d3b4c81427335f54ed9a
  revision: 983e76c6a15ff152c4423679609e6a35102fa6627c44f6ed99e77d57dde74ea7
  schema: mindie-entry/3
  summary: 当前材料集中于将本地项目改接到私有仓库 `vllm-ascend/vllm-ascend-kimi-k3`、创建个人 fork，以及配置“允许 fork、禁止直接 push”的权限策略。已记录此前目标仓库误判、fork 创建失败（HTTP 403/422）、组织级私有仓库 fork 限制、用户仅具仓库管理员权限，以及分支保护受套餐限制等未完成事项；原始消息块仍需独立检索，不能将报告的检查结果或计划升级为已验证成功。
  title: Kimi K3 仓库协作设置与 fork 状态
navigation: 当前材料集中于将本地项目改接到私有仓库 `vllm-ascend/vllm-ascend-kimi-k3`、创建个人 fork，以及配置“允许 fork、禁止直接 push”的权限策略。已记录此前目标仓库误判、fork 创建失败（HTTP 403/422）、组织级私有仓库 fork 限制、用户仅具仓库管理员权限，以及分支保护受套餐限制等未完成事项；原始消息块仍需独立检索，不能将报告的检查结果或计划升级为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: c5e1a17389f828a49806b25a20e5ecbf490cfb49ab9533e6b09b42ee49fa254a
---

# Kimi K3 仓库协作设置与 fork 状态

当前材料集中于将本地项目改接到私有仓库 `vllm-ascend/vllm-ascend-kimi-k3`、创建个人 fork，以及配置“允许 fork、禁止直接 push”的权限策略。已记录此前目标仓库误判、fork 创建失败（HTTP 403/422）、组织级私有仓库 fork 限制、用户仅具仓库管理员权限，以及分支保护受套餐限制等未完成事项；原始消息块仍需独立检索，不能将报告的检查结果或计划升级为已验证成功。

## Materials

- [Kimi K3 仓库 fork 与权限策略核查](blocks/a71c6c1f17d7cb729bd6f9609a7692d07f062c63c28cf789c8f2eba1489ff2da.md): 曾将 `upstream` 和 fork 目标误解为 `vllm-project/vllm-ascend`，随后根据用户纠正，确认目标为私有仓库 `vllm-ascend/vllm-ascend-kimi-k3`；此前的 fork 重命名、远端配置和分支推送结果不能视为已完成当前目标。只读检查报告称该仓库默认分支为 `releases/v0.23.0`、fork 被禁用、无团队授权和分支保护，少数管理员可直接 push；但开启仓库级 fork 的请求因组织级禁止私有仓库 fork而以 HTTP 422 失败，未产生设置变更。后续说明认为仓库级 `Allow forking` 需组织级许可作为前置条件，用户虽有仓库 `admin` 权限却不是组织成员或 owner，无法修改组织级策略；分支保护在当前私有仓库套餐下也被报告为不可用，需组织 owner 开启私有仓库 fork，并可能升级套餐或公开仓库后再处理。

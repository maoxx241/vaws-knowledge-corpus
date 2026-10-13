---
blocks:
- block_id: 0e01c27052aa72370edef5b3fb8300d054b969b98adae9c5249d0cb481701f3f
  indexed: true
  sha256: 4038d9210b89b527874df9c168b0f89010e6d4d5a9926208cd167d45fd1b8340
  source_range:
    end: 1891895
    part: 0
    start: 0
  summary: '记录了 `token_dispatcher.py` 中 PR #13 与新 CI 基线的条件判断冲突：保留新基线的嵌套 `if with_quant` 结构在控制流上被认为等价，但未重新执行 NPU 运行时验证。随后创建了 `93_4_ci_new`，备份并覆盖 `releases/v0.23.0_ci_new`，再将 PR #37/#38 以 cherry-pick 方式合入；期间出现过 zsh refspec 解析失败、备份分支被外部清理等问题，最终通过新的备份命名空间和原子推送完成。后续只读比较显示 A3/A5 分支主要因 rebase 基线和少量运行时文件不同，尚未证明平台精度差异由代码直接造成；PR #32 也已 rebase 到 A5 分支，但没有重新进行设备精度测试。'
  title: ci_new 冲突处理、回滚分支与发布覆盖记录
- block_id: c77468ec55f6bec03b09e1ac8703a0c123d1291ed14166004c21e704a9ac5e32
  indexed: true
  sha256: 2affc4d1a660811beb5edc1faf99a5d1b2188f04988977ea4b7a66540caf755c
  source_range:
    end: 1891895
    part: 1
    start: 0
  summary: '只读核对确认 `releases/v0.23.0_ci_new` 曾达到合入 PR #37/#38 后的 `8f192c9f`，随后从备份基线创建 `releases/v0.23.0_ci_new_a5` 并重新移植两个 PR，最终 HEAD 为 `520e5bad`；这些验证主要是 patch-id、语法和静态检查，不等同于运行时精度成功。之后 PR #32 被 rebase 到 A5，生成 `3dda4207`；再将已合入 `ci_new` 的 PR #34 补丁 cherry-pick 到 A5，创建 `releases/v0.23.0_ci_new_a3` 作为覆盖前的 `ad26bd3a` 备份，并以精确 lease 将 `ci_new` 覆盖到 `d954213e`。当前记录称 `ci_new` 与 `ci_new_a5` 的 SHA/tree 一致、A3 备份保留旧状态，PR #32 为可合并；但未启动服务或重新执行 NPU 精度验证。'
  title: A3/A5 分支复制、PR 合入与最终同步状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 171e96a45489f491c81edf2da0284d0b4e097370ec2058ba8b7a7bf3ce0395cc
  kind: experience
  material_digest: f9de648f2513c5d4b36c718950de0159a8408b377707d115f7b0caa6a1e893b6
  revision: 2109a9ba9058820fc19aada81e03cea15fb3765891ea4cb24e5faf08fca8cf69
  schema: mindie-entry/3
  summary: '核心脉络是：先处理 rebase 冲突并形成多个可恢复分支，再比较 A3/A5 分支，随后将 PR #32/#34 及此前 PR #37/#38 按不同基线移植。当前记录的远端目标状态为 `ci_new` 与 `ci_new_a5` 指向 `d954213e`，`ci_new_a3` 保留覆盖前的 `ad26bd3a`；这些是分支/静态校验结果，不应视为已完成 NPU 精度验证。后续若继续排查，应重点区分 rebase 造成的历史差异、最终树差异、设备/构建/权重/启动参数差异，并保留任何交叉测试失败或尚未完成的结果。'
  title: 当前任务导航：A3/A5 release 分支同步与精度差异追踪
navigation: '核心脉络是：先处理 rebase 冲突并形成多个可恢复分支，再比较 A3/A5 分支，随后将 PR #32/#34 及此前 PR #37/#38 按不同基线移植。当前记录的远端目标状态为 `ci_new` 与 `ci_new_a5` 指向 `d954213e`，`ci_new_a3` 保留覆盖前的 `ad26bd3a`；这些是分支/静态校验结果，不应视为已完成 NPU 精度验证。后续若继续排查，应重点区分 rebase 造成的历史差异、最终树差异、设备/构建/权重/启动参数差异，并保留任何交叉测试失败或尚未完成的结果。'
schema: mindie-material-task/1
status: complete
task_id: 171e96a45489f491c81edf2da0284d0b4e097370ec2058ba8b7a7bf3ce0395cc
---

# 当前任务导航：A3/A5 release 分支同步与精度差异追踪

核心脉络是：先处理 rebase 冲突并形成多个可恢复分支，再比较 A3/A5 分支，随后将 PR #32/#34 及此前 PR #37/#38 按不同基线移植。当前记录的远端目标状态为 `ci_new` 与 `ci_new_a5` 指向 `d954213e`，`ci_new_a3` 保留覆盖前的 `ad26bd3a`；这些是分支/静态校验结果，不应视为已完成 NPU 精度验证。后续若继续排查，应重点区分 rebase 造成的历史差异、最终树差异、设备/构建/权重/启动参数差异，并保留任何交叉测试失败或尚未完成的结果。

## Materials

- [ci_new 冲突处理、回滚分支与发布覆盖记录](blocks/0e01c27052aa72370edef5b3fb8300d054b969b98adae9c5249d0cb481701f3f.md): 记录了 `token_dispatcher.py` 中 PR #13 与新 CI 基线的条件判断冲突：保留新基线的嵌套 `if with_quant` 结构在控制流上被认为等价，但未重新执行 NPU 运行时验证。随后创建了 `93_4_ci_new`，备份并覆盖 `releases/v0.23.0_ci_new`，再将 PR #37/#38 以 cherry-pick 方式合入；期间出现过 zsh refspec 解析失败、备份分支被外部清理等问题，最终通过新的备份命名空间和原子推送完成。后续只读比较显示 A3/A5 分支主要因 rebase 基线和少量运行时文件不同，尚未证明平台精度差异由代码直接造成；PR #32 也已 rebase 到 A5 分支，但没有重新进行设备精度测试。
- [A3/A5 分支复制、PR 合入与最终同步状态](blocks/c77468ec55f6bec03b09e1ac8703a0c123d1291ed14166004c21e704a9ac5e32.md): 只读核对确认 `releases/v0.23.0_ci_new` 曾达到合入 PR #37/#38 后的 `8f192c9f`，随后从备份基线创建 `releases/v0.23.0_ci_new_a5` 并重新移植两个 PR，最终 HEAD 为 `520e5bad`；这些验证主要是 patch-id、语法和静态检查，不等同于运行时精度成功。之后 PR #32 被 rebase 到 A5，生成 `3dda4207`；再将已合入 `ci_new` 的 PR #34 补丁 cherry-pick 到 A5，创建 `releases/v0.23.0_ci_new_a3` 作为覆盖前的 `ad26bd3a` 备份，并以精确 lease 将 `ci_new` 覆盖到 `d954213e`。当前记录称 `ci_new` 与 `ci_new_a5` 的 SHA/tree 一致、A3 备份保留旧状态，PR #32 为可合并；但未启动服务或重新执行 NPU 精度验证。

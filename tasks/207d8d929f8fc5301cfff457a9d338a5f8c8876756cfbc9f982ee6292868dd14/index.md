---
blocks:
- block_id: 512aee6f09cbc351e3607ecedbdc62c825050a34d4b1a9f9876818f7a0daf05c
  indexed: true
  sha256: f1c372a68a4cd9d49d1eab1118b40c8cdc489d0d20fdd57bc6af1b541613a3d1
  source_range:
    end: 845633
    part: 0
    start: 0
  summary: 材料称第一版已推送到个人 Fork，包含身份/Fork 初始化、约每 5 分钟更新、留言收取、编译缓存和权重复用；但真实服务器协作、NPU 缓存验收及完整更新传播仍未验证。后续改为默认跟随主仓默认分支，曾成功查询到 `main @ b63c78269549e0f16ae6c4554f0b47b5e774a3da`，并报告更新器与原生入口测试 48 项通过；同时说明 9 个文件已修改但未提交或推送，旧 watcher 可能仍在运行。最新材料报告已修复准备、完成和复用阶段的状态残留及旧 `no_stable_release` 错误指针，测试 35 项通过且 `diff --check` 通过，但仍需重启 watcher `51953` 才会加载修复。
  title: 主仓跟随更新器与状态残留修复
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 207d8d929f8fc5301cfff457a9d338a5f8c8876756cfbc9f982ee6292868dd14
  kind: experience
  material_digest: f0fb1a149167a2bbe159b15106bba981c76b6934103486b685dfb765e9e7c707
  revision: 5e38ec3463119a5f42c9ec62d8cdf656018114d7f38d633789ae52231b3c6fdd
  schema: mindie-entry/3
  summary: 当前重点是验证主仓周期性跟随逻辑及 watcher 重启后的状态清理。已有报告中的测试通过、主仓查询成功和个人 Fork 初始化均属于材料中的报告；真实服务器协作、NPU 缓存验收、完整传播及修复加载结果仍未完成或未验证。
  title: 任务导航：主仓跟随更新器
navigation: 当前重点是验证主仓周期性跟随逻辑及 watcher 重启后的状态清理。已有报告中的测试通过、主仓查询成功和个人 Fork 初始化均属于材料中的报告；真实服务器协作、NPU 缓存验收、完整传播及修复加载结果仍未完成或未验证。
schema: mindie-material-task/1
status: complete
task_id: 207d8d929f8fc5301cfff457a9d338a5f8c8876756cfbc9f982ee6292868dd14
---

# 任务导航：主仓跟随更新器

当前重点是验证主仓周期性跟随逻辑及 watcher 重启后的状态清理。已有报告中的测试通过、主仓查询成功和个人 Fork 初始化均属于材料中的报告；真实服务器协作、NPU 缓存验收、完整传播及修复加载结果仍未完成或未验证。

## Materials

- [主仓跟随更新器与状态残留修复](blocks/512aee6f09cbc351e3607ecedbdc62c825050a34d4b1a9f9876818f7a0daf05c.md): 材料称第一版已推送到个人 Fork，包含身份/Fork 初始化、约每 5 分钟更新、留言收取、编译缓存和权重复用；但真实服务器协作、NPU 缓存验收及完整更新传播仍未验证。后续改为默认跟随主仓默认分支，曾成功查询到 `main @ b63c78269549e0f16ae6c4554f0b47b5e774a3da`，并报告更新器与原生入口测试 48 项通过；同时说明 9 个文件已修改但未提交或推送，旧 watcher 可能仍在运行。最新材料报告已修复准备、完成和复用阶段的状态残留及旧 `no_stable_release` 错误指针，测试 35 项通过且 `diff --check` 通过，但仍需重启 watcher `51953` 才会加载修复。

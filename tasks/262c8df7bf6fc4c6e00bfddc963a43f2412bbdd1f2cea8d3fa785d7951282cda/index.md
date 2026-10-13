---
blocks:
- block_id: 2cd25848cda03bbb347b259ed3a2f077e9d265c0abb608e55c7027c31992d063
  indexed: true
  sha256: 430d43ddbeafc2263e9822fb4ea56ecbb3f10167bc3d9eadcf018a0c50c7703c
  source_range:
    end: 1294784
    part: 0
    start: 0
  summary: 材料记录了 cmux 新终端启动时多次出现 pyenv rehash 获取 `.pyenv-shim` 锁失败、等待 60 秒的问题；先前归因包括残留锁、登录配置重复触发，以及旧版 pyenv 缺少陈旧锁清理。记录称曾移除锁、调整 `.zprofile`/`.zshrc`，并将 pyenv 从 2.6.26 升级到 2.8.4，但这些验证结果属于该材料中的报告，仍应保留其不确定性。
  title: pyenv 锁反复导致 cmux 终端卡顿
- block_id: d37e0bb931dc8e92fa13c7b6857d8618795e915d982f23d21370db0bebcdcf1d
  indexed: true
  sha256: 63f213bf4302c5244946a94945d9a4a9fc07f595fe4b420b11bb8eced0fcc5d5
  source_range:
    end: 1294784
    part: 1
    start: 0
  summary: 材料报告 pyenv 已从 2.6.26 升级到 2.8.4，`.zprofile` 不重复执行 rehash、`.zshrc` 保留一次交互式 rehash，并声称 Python、OpenSSL、并发登录 shell 和锁状态验证通过。随后说明禁用自动 rehash本身主要会使新安装命令暂时不可见，最后确认当前配置是升级后的 pyenv 加单次自动 rehash；这些均是来源中的报告，不等同于独立核验。
  title: 升级 pyenv 后恢复单次 rehash
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 262c8df7bf6fc4c6e00bfddc963a43f2412bbdd1f2cea8d3fa785d7951282cda
  kind: experience
  material_digest: 3bc957544a2dd35aa0308753b851c9fbc80ccbc658f208f81e44c96a63f641e6
  revision: e8d5016fda363d1b2fcede2ba6375c73554f72382fead4627f99dc02e454eba8
  schema: mindie-entry/3
  summary: 当前索引集中于 cmux 新终端因 pyenv `.pyenv-shim` 锁和自动 rehash 触发而卡顿的故障链。相关材料记录了残留锁、重复或异常中断、pyenv 2.6.26 的陈旧锁处理缺陷等假设与后续修复，并报告已升级到 2.8.4、恢复单次 rehash；此前的错误、未完全可追溯的触发原因和验证结论仍应作为可错参考保留。
  title: cmux 终端启动与 pyenv rehash 故障
navigation: 当前索引集中于 cmux 新终端因 pyenv `.pyenv-shim` 锁和自动 rehash 触发而卡顿的故障链。相关材料记录了残留锁、重复或异常中断、pyenv 2.6.26 的陈旧锁处理缺陷等假设与后续修复，并报告已升级到 2.8.4、恢复单次 rehash；此前的错误、未完全可追溯的触发原因和验证结论仍应作为可错参考保留。
schema: mindie-material-task/1
status: complete
task_id: 262c8df7bf6fc4c6e00bfddc963a43f2412bbdd1f2cea8d3fa785d7951282cda
---

# cmux 终端启动与 pyenv rehash 故障

当前索引集中于 cmux 新终端因 pyenv `.pyenv-shim` 锁和自动 rehash 触发而卡顿的故障链。相关材料记录了残留锁、重复或异常中断、pyenv 2.6.26 的陈旧锁处理缺陷等假设与后续修复，并报告已升级到 2.8.4、恢复单次 rehash；此前的错误、未完全可追溯的触发原因和验证结论仍应作为可错参考保留。

## Materials

- [pyenv 锁反复导致 cmux 终端卡顿](blocks/2cd25848cda03bbb347b259ed3a2f077e9d265c0abb608e55c7027c31992d063.md): 材料记录了 cmux 新终端启动时多次出现 pyenv rehash 获取 `.pyenv-shim` 锁失败、等待 60 秒的问题；先前归因包括残留锁、登录配置重复触发，以及旧版 pyenv 缺少陈旧锁清理。记录称曾移除锁、调整 `.zprofile`/`.zshrc`，并将 pyenv 从 2.6.26 升级到 2.8.4，但这些验证结果属于该材料中的报告，仍应保留其不确定性。
- [升级 pyenv 后恢复单次 rehash](blocks/d37e0bb931dc8e92fa13c7b6857d8618795e915d982f23d21370db0bebcdcf1d.md): 材料报告 pyenv 已从 2.6.26 升级到 2.8.4，`.zprofile` 不重复执行 rehash、`.zshrc` 保留一次交互式 rehash，并声称 Python、OpenSSL、并发登录 shell 和锁状态验证通过。随后说明禁用自动 rehash本身主要会使新安装命令暂时不可见，最后确认当前配置是升级后的 pyenv 加单次自动 rehash；这些均是来源中的报告，不等同于独立核验。

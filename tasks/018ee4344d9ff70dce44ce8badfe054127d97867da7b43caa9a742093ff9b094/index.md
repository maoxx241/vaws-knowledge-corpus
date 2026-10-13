---
blocks:
- block_id: 52785e8cee16fe1eb74c658f2304238b10cc30ddf8009a205b41653d32f067b2
  indexed: true
  sha256: 460a198cfff5c3bdffeb2de500eb855a2f14ad499bb0a7d431351b1bd5c1af61
  source_range:
    end: 3902593
    part: 0
    start: 0
  summary: 记录了一台 Mac 的磁盘盘点、删除与外置 SSD 配置：内置 251/245 GB 存储曾约 99.3% 已用；来源称删除 `cann91-a3.tar` 和旧用户目录后可用空间升至约 27.5 GB，但旧目录曾受 `Mobile Documents` 权限保护，后续“彻底删除”仍属于来源中的完成声明。绿联雷电扩展坞识别到致态 TiPlus7100s 2 TB 后，来源称已格式化为 GPT+APFS、卷名 `data`，挂载于 `/Volumes/data`，并报告写入读回、符号链接及文件系统检查通过；此前设备未识别、硬盘无可识别分区的状态也应保留。随后来源称将约 61.1 GB profiling/分析资料、16 项旧安装包与备份、以及约 13.43 GB Claude/Grok/Cursor 旧资料迁移或归档到外置盘，声称 SHA-256 校验通过并保留原路径软链接；OpenClaw 卸载和约 1.81 GB 数据清理也只是来源报告，Time Machine 快照可能延迟释放空间，uv 等约 11 GB 缓存尚未清理。
  title: Mac 磁盘扩容、数据迁移与归档清理记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 018ee4344d9ff70dce44ce8badfe054127d97867da7b43caa9a742093ff9b094
  kind: experience
  material_digest: 859c51551cb9506e82c380b71d6709f6690fb542703d88244397a7a646031aed
  revision: 6e2e29ea501c3ddf992e07780762391103f7af66a5d64928092e162ff78e1627
  schema: mindie-entry/3
  summary: 已记录磁盘占用诊断、目标删除、外置 `data` 盘配置、profiling/历史分析资料迁移、OpenClaw 卸载及旧资料归档。后续重点是核对迁移链接和实际空间回收，区分仍在使用的 Cursor/Claude 数据、持续写入日志或缓存，并保留来源中“已完成”声明与 Time Machine 快照、权限、设备识别等未完全确定因素；原始块仍需独立检索，不能将来源报告自动视为已验证成功。
  title: 当前任务导航：Mac 存储治理与外置盘归档
navigation: 已记录磁盘占用诊断、目标删除、外置 `data` 盘配置、profiling/历史分析资料迁移、OpenClaw 卸载及旧资料归档。后续重点是核对迁移链接和实际空间回收，区分仍在使用的 Cursor/Claude 数据、持续写入日志或缓存，并保留来源中“已完成”声明与 Time Machine 快照、权限、设备识别等未完全确定因素；原始块仍需独立检索，不能将来源报告自动视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 018ee4344d9ff70dce44ce8badfe054127d97867da7b43caa9a742093ff9b094
---

# 当前任务导航：Mac 存储治理与外置盘归档

已记录磁盘占用诊断、目标删除、外置 `data` 盘配置、profiling/历史分析资料迁移、OpenClaw 卸载及旧资料归档。后续重点是核对迁移链接和实际空间回收，区分仍在使用的 Cursor/Claude 数据、持续写入日志或缓存，并保留来源中“已完成”声明与 Time Machine 快照、权限、设备识别等未完全确定因素；原始块仍需独立检索，不能将来源报告自动视为已验证成功。

## Materials

- [Mac 磁盘扩容、数据迁移与归档清理记录](blocks/52785e8cee16fe1eb74c658f2304238b10cc30ddf8009a205b41653d32f067b2.md): 记录了一台 Mac 的磁盘盘点、删除与外置 SSD 配置：内置 251/245 GB 存储曾约 99.3% 已用；来源称删除 `cann91-a3.tar` 和旧用户目录后可用空间升至约 27.5 GB，但旧目录曾受 `Mobile Documents` 权限保护，后续“彻底删除”仍属于来源中的完成声明。绿联雷电扩展坞识别到致态 TiPlus7100s 2 TB 后，来源称已格式化为 GPT+APFS、卷名 `data`，挂载于 `/Volumes/data`，并报告写入读回、符号链接及文件系统检查通过；此前设备未识别、硬盘无可识别分区的状态也应保留。随后来源称将约 61.1 GB profiling/分析资料、16 项旧安装包与备份、以及约 13.43 GB Claude/Grok/Cursor 旧资料迁移或归档到外置盘，声称 SHA-256 校验通过并保留原路径软链接；OpenClaw 卸载和约 1.81 GB 数据清理也只是来源报告，Time Machine 快照可能延迟释放空间，uv 等约 11 GB 缓存尚未清理。

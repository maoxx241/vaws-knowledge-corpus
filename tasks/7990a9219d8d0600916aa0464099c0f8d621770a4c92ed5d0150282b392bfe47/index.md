---
blocks:
- block_id: dd9ab98d660fad2e8480e2a5c79fe6ddaa58edff5bda57ef00b21beddd21222a
  indexed: true
  sha256: 5b10c463d14ee81067028c31cd0a85179c72468c8ef91183b17daaca6ebab153
  source_range:
    end: 3200871
    part: 0
    start: 0
  summary: 记录称 Cursor 3.18.9 主程序无法正常启动，存在孤立插件宿主进程、锁/IPC 疑点，以及大量签名保护资源缺失；后续判断旧进程和锁更像残留现象。下载过程中曾发生对端中断与 HTTP/2 重置，改用断点续传和 HTTP/1.1；记录称最终获得并安装 Cursor 3.18.25，DMG、代码签名和 Apple 公证检查通过，并保留损坏版本备份。工作区、窗口、MCP 与扩展宿主被报告为恢复正常，但这些检查和成功结论仍属于该记录中的未独立验证报告。
  title: Cursor 应用包损坏与重装恢复记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7990a9219d8d0600916aa0464099c0f8d621770a4c92ed5d0150282b392bfe47
  kind: experience
  material_digest: cf6a295488399785397623b9aeca989816a3cf159d435c299169ccd5468cb480
  revision: 6b982c56e4a0d6fd42866c58a3ba0ff3020a831c0b1093edb7e203a8b2b7522a
  schema: mindie-entry/3
  summary: 本批次包含一个关于 Cursor 启动失败、应用签名资源缺失、自动更新留下不完整目录、下载重试及据称重装恢复的记录。可检索关键词：Cursor 3.18.9/3.18.25、孤立插件宿主、单实例锁、IPC、代码签名、Apple 公证、DMG 校验、HTTP/2 重置、HTTP/1.1 断点续传、vllm-ascend；旧版备份与最终恢复状态均应视为来源报告，而非已独立核实事实。
  title: 当前任务导航：Cursor 启动故障索引
navigation: 本批次包含一个关于 Cursor 启动失败、应用签名资源缺失、自动更新留下不完整目录、下载重试及据称重装恢复的记录。可检索关键词：Cursor 3.18.9/3.18.25、孤立插件宿主、单实例锁、IPC、代码签名、Apple 公证、DMG 校验、HTTP/2 重置、HTTP/1.1 断点续传、vllm-ascend；旧版备份与最终恢复状态均应视为来源报告，而非已独立核实事实。
schema: mindie-material-task/1
status: complete
task_id: 7990a9219d8d0600916aa0464099c0f8d621770a4c92ed5d0150282b392bfe47
---

# 当前任务导航：Cursor 启动故障索引

本批次包含一个关于 Cursor 启动失败、应用签名资源缺失、自动更新留下不完整目录、下载重试及据称重装恢复的记录。可检索关键词：Cursor 3.18.9/3.18.25、孤立插件宿主、单实例锁、IPC、代码签名、Apple 公证、DMG 校验、HTTP/2 重置、HTTP/1.1 断点续传、vllm-ascend；旧版备份与最终恢复状态均应视为来源报告，而非已独立核实事实。

## Materials

- [Cursor 应用包损坏与重装恢复记录](blocks/dd9ab98d660fad2e8480e2a5c79fe6ddaa58edff5bda57ef00b21beddd21222a.md): 记录称 Cursor 3.18.9 主程序无法正常启动，存在孤立插件宿主进程、锁/IPC 疑点，以及大量签名保护资源缺失；后续判断旧进程和锁更像残留现象。下载过程中曾发生对端中断与 HTTP/2 重置，改用断点续传和 HTTP/1.1；记录称最终获得并安装 Cursor 3.18.25，DMG、代码签名和 Apple 公证检查通过，并保留损坏版本备份。工作区、窗口、MCP 与扩展宿主被报告为恢复正常，但这些检查和成功结论仍属于该记录中的未独立验证报告。

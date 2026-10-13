---
blocks:
- block_id: 74ecec75033b94adef66196a82c5b48fce2d7cad305db88a434e2c8bb4d3edef
  indexed: true
  sha256: 62d0e4ef14e9b753e7ad8ed1664be9521a39d5dbb6186653bfa17abefee138dd
  source_range:
    end: 2297678
    part: 0
    start: 0
  summary: 采样尾部泄漏路径已被排除，但新证据推翻了“仅 dummy padding”假设：首坏 replay 包含两个真实请求、exact-shape 16 token，且主模型 hidden 已出现 NaN。后续需检查 FULL graph 下 KDA/GDN recurrent state 的读写别名、accepted-count 及 graph 输入更新生命周期，尚未形成验证成功。另已调整验证顺序：先用完整权重验证 DSpark 功能、prefix cache、接收率和精度，再用 5 层权重验证 P/D 分离及 Mooncake KV 传输；5 层目录缺少 tokenizer 暂不处理。
  title: FULL graph 首坏 replay 与验证顺序调整
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: ac106216041ae203fd6d8358c9e35ddbdbf6b4c0ba1c77f913e24ccf4d53a7c7
  kind: experience
  material_digest: 16659c2774bcd7d5fee92ea193d276d38e28884ce3f28d2827982c10a0aed6d3
  revision: 4f5dfcc87ac32a446246425935e405c112820c77465e7320309737ec4f34dd26
  schema: mindie-entry/3
  summary: 重点追查真实请求 exact-shape replay 中 hidden NaN 的 FULL graph 状态读写、accepted-count 和输入更新生命周期；padding-only guard 假设已被推翻。验证仍按完整权重 DSpark 基线、再到 5 层 P/D 与 Mooncake KV 传输的顺序推进，相关结果尚未被确认。
  title: 当前任务导航：FULL graph NaN 定位与分阶段验证
navigation: 重点追查真实请求 exact-shape replay 中 hidden NaN 的 FULL graph 状态读写、accepted-count 和输入更新生命周期；padding-only guard 假设已被推翻。验证仍按完整权重 DSpark 基线、再到 5 层 P/D 与 Mooncake KV 传输的顺序推进，相关结果尚未被确认。
schema: mindie-material-task/1
status: complete
task_id: ac106216041ae203fd6d8358c9e35ddbdbf6b4c0ba1c77f913e24ccf4d53a7c7
---

# 当前任务导航：FULL graph NaN 定位与分阶段验证

重点追查真实请求 exact-shape replay 中 hidden NaN 的 FULL graph 状态读写、accepted-count 和输入更新生命周期；padding-only guard 假设已被推翻。验证仍按完整权重 DSpark 基线、再到 5 层 P/D 与 Mooncake KV 传输的顺序推进，相关结果尚未被确认。

## Materials

- [FULL graph 首坏 replay 与验证顺序调整](blocks/74ecec75033b94adef66196a82c5b48fce2d7cad305db88a434e2c8bb4d3edef.md): 采样尾部泄漏路径已被排除，但新证据推翻了“仅 dummy padding”假设：首坏 replay 包含两个真实请求、exact-shape 16 token，且主模型 hidden 已出现 NaN。后续需检查 FULL graph 下 KDA/GDN recurrent state 的读写别名、accepted-count 及 graph 输入更新生命周期，尚未形成验证成功。另已调整验证顺序：先用完整权重验证 DSpark 功能、prefix cache、接收率和精度，再用 5 层权重验证 P/D 分离及 Mooncake KV 传输；5 层目录缺少 tokenizer 暂不处理。

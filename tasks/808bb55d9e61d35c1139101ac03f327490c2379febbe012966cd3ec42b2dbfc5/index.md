---
blocks:
- block_id: 19dae05395038146db789cd01e12a401a00b4d0c59626c9758ae90f6af6806df
  indexed: true
  sha256: adba8b62cd969d8a1b35d4d47d6bdb77fd8b1607fb112788121e7cb27345b7e6
  source_range:
    end: 17291666
    part: 0
    start: 0
  summary: 围绕 RMSNorm、mHC/Engram、QLI 后处理和 query 量化开展了多轮优化；部分结果声称在 153/A3 上通过测试并提交 PR，但应视为来源中的报告而非独立验证。QLI 后处理 Triton 融合涉及过滤、排序、无效索引恢复，曾遇到 INT32 原生排序链接缺失并改用可逆 FP32 编码；hash TopK 在 CANN 9.1 上未复现编译错误，后续运行时验证尚在下一块材料中完成。
  title: RMSNorm、量化与 QLI/Hash TopK 优化记录
- block_id: 3cb9da007b7d6749be691a7dde6729f4be8598d81644e5c83a3cced58e640d83
  indexed: true
  sha256: e9f0c893d49e5192cfeb9946b80518bf101c484185f0f7a6b9bcbccede0f935d
  source_range:
    end: 17291666
    part: 1
    start: 0
  summary: hash TopK 在 153 A3/CANN 9.1 上的内核、绑定和运行时均未复现报错，FP32/BF16、hash 开关及 eager/NPUGraph 共 8 项用例通过，随后未修改代码。pool 后两处 where 被移到 Attention metadata builder，相关材料报告 34 项回归、44 组 NPU 对照通过；RMSNormCast 已接入 V4.1 decoder并去掉 enable_custom_op 校验，但尚未验证整模精度和吞吐。moe_gating_topk 前的 input_ids == -1 处理据报告已前移到模型入口、每次 forward 只做一次，114 项回归和 32 组 NPU检查通过；当前新问题是 compressor 中重复的 hidden_states.float()，尚处于定位阶段。
  title: metadata 前移、RMSNormCast 与路由前置处理
- block_id: 6410fcf7a73e99b55fbc232291eb37f0483c5ed995cf1dfbeeb6a4b13a23f72b
  indexed: true
  sha256: e8a10da16fd88ffae0460b5285efd548b5b7dec91c563710555c0ce7d119b76b
  source_range:
    end: 20218940
    part: 0
    start: 17291666
  summary: 'V4.1 attention 的 C2 compressor 将 wkv 与 wgate 的 hidden_states.float() 从两次合并为一次；报告称相关 Ruff、146 项测试及 18 组 NPU/eager/图回放检查通过，但一项既有 PD 配置测试仍失败，且全库格式问题被记录。后续检查认为 pool kernel 前的 out.zero_() 可移除：kernel 只写完成压缩对，未完成行和 padding 行通过 [-1, -1] 坐标跳过；预填 NaN/旧值的有效输出及后续部分结果与清零版本一致。RoPE 和最终 scatter 仅做了源码核对，代码尚未修改，且需要调整“无效输出行必须为零”的测试约定；PR #30 的提交信息属于来源中的报告，不等同于此处独立验证。'
  title: C2 compressor：复用 FP32 转换并评估移除 pool 前清零
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 808bb55d9e61d35c1139101ac03f327490c2379febbe012966cd3ec42b2dbfc5
  kind: experience
  material_digest: 471bca3560c15f47ecf36d91849a015320bbacdf9a2e50c7f077b9b8032c092f
  revision: 92cb73abf0cdde714a4ca968bf5434cd879d0d9c93f24f818b14b943864ebac3
  schema: mindie-entry/3
  summary: 重点追踪 C2 compressor 的两项变化：wkv/wgate 复用一次 hidden_states.float()，以及在不改 pool kernel 的前提下评估移除 out.zero_()。已有报告称低精度 Cast、NPU 检查和图回放结果一致，但保留既有 PD 测试失败、全库格式问题及其证据边界；清零移除尚未落地，RoPE/最终 scatter 仅源码核对，需关注无效输出行测试约定和缓冲区复用场景。此前 metadata builder、RMSNormCast、QLI 后处理、路由前置处理及 hash TopK 的未验证或未复现背景仍应独立检索，不能由本批材料覆盖。
  title: 当前任务导航：C2 compressor 优化与未完验证边界
navigation: 重点追踪 C2 compressor 的两项变化：wkv/wgate 复用一次 hidden_states.float()，以及在不改 pool kernel 的前提下评估移除 out.zero_()。已有报告称低精度 Cast、NPU 检查和图回放结果一致，但保留既有 PD 测试失败、全库格式问题及其证据边界；清零移除尚未落地，RoPE/最终 scatter 仅源码核对，需关注无效输出行测试约定和缓冲区复用场景。此前 metadata builder、RMSNormCast、QLI 后处理、路由前置处理及 hash TopK 的未验证或未复现背景仍应独立检索，不能由本批材料覆盖。
schema: mindie-material-task/1
status: complete
task_id: 808bb55d9e61d35c1139101ac03f327490c2379febbe012966cd3ec42b2dbfc5
---

# 当前任务导航：C2 compressor 优化与未完验证边界

重点追踪 C2 compressor 的两项变化：wkv/wgate 复用一次 hidden_states.float()，以及在不改 pool kernel 的前提下评估移除 out.zero_()。已有报告称低精度 Cast、NPU 检查和图回放结果一致，但保留既有 PD 测试失败、全库格式问题及其证据边界；清零移除尚未落地，RoPE/最终 scatter 仅源码核对，需关注无效输出行测试约定和缓冲区复用场景。此前 metadata builder、RMSNormCast、QLI 后处理、路由前置处理及 hash TopK 的未验证或未复现背景仍应独立检索，不能由本批材料覆盖。

## Materials

- [RMSNorm、量化与 QLI/Hash TopK 优化记录](blocks/19dae05395038146db789cd01e12a401a00b4d0c59626c9758ae90f6af6806df.md): 围绕 RMSNorm、mHC/Engram、QLI 后处理和 query 量化开展了多轮优化；部分结果声称在 153/A3 上通过测试并提交 PR，但应视为来源中的报告而非独立验证。QLI 后处理 Triton 融合涉及过滤、排序、无效索引恢复，曾遇到 INT32 原生排序链接缺失并改用可逆 FP32 编码；hash TopK 在 CANN 9.1 上未复现编译错误，后续运行时验证尚在下一块材料中完成。
- [metadata 前移、RMSNormCast 与路由前置处理](blocks/3cb9da007b7d6749be691a7dde6729f4be8598d81644e5c83a3cced58e640d83.md): hash TopK 在 153 A3/CANN 9.1 上的内核、绑定和运行时均未复现报错，FP32/BF16、hash 开关及 eager/NPUGraph 共 8 项用例通过，随后未修改代码。pool 后两处 where 被移到 Attention metadata builder，相关材料报告 34 项回归、44 组 NPU 对照通过；RMSNormCast 已接入 V4.1 decoder并去掉 enable_custom_op 校验，但尚未验证整模精度和吞吐。moe_gating_topk 前的 input_ids == -1 处理据报告已前移到模型入口、每次 forward 只做一次，114 项回归和 32 组 NPU检查通过；当前新问题是 compressor 中重复的 hidden_states.float()，尚处于定位阶段。
- [C2 compressor：复用 FP32 转换并评估移除 pool 前清零](blocks/6410fcf7a73e99b55fbc232291eb37f0483c5ed995cf1dfbeeb6a4b13a23f72b.md): V4.1 attention 的 C2 compressor 将 wkv 与 wgate 的 hidden_states.float() 从两次合并为一次；报告称相关 Ruff、146 项测试及 18 组 NPU/eager/图回放检查通过，但一项既有 PD 配置测试仍失败，且全库格式问题被记录。后续检查认为 pool kernel 前的 out.zero_() 可移除：kernel 只写完成压缩对，未完成行和 padding 行通过 [-1, -1] 坐标跳过；预填 NaN/旧值的有效输出及后续部分结果与清零版本一致。RoPE 和最终 scatter 仅做了源码核对，代码尚未修改，且需要调整“无效输出行必须为零”的测试约定；PR #30 的提交信息属于来源中的报告，不等同于此处独立验证。

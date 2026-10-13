---
blocks:
- block_id: 9ad192eb1fba8d2173af98c8d659943e11a8a639b6970ded58e93b0c22d780cf
  indexed: true
  sha256: 5c2ef2e600e9b12121910c800b92a80f05a090694e2d760106450917dda76524
  source_range:
    end: 11227460
    part: 0
    start: 0
  summary: '记录了知识/经验分离存储与检索、29 篇经验整理、跨 lane 复核、PR #150 的依赖重放与多平台 CI；其中报告称测试通过、Windows 失败重跑通过且 PR 为 MERGEABLE，但这些均属于来源中的报告，不能视为本索引已验证的成功。后续恢复稿复核保留了未完成设备闭环、第三方来源排除及 OP15 范围收窄；13 篇案例深审还修正了 named tool 验收断言、采样 OOM 临时排序空间、prefix cache 冷/命中/重置区分、算子安装验证坐标和 MRV2 热 prefix 失败等证据边界。若干后续补证涉及日期、SP 分片与 residual bank 顺序，以及 dispatcher/FIA metadata 对 48/96 tokens 与 8/16 padded requests 的换算；正文据称未变，但原始记录、哈希和审阅结论仍应作为可检索的来源材料看待。'
  title: 本地经验整理、PR 消费与案例复核记录
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3ab1bf4ca0cea771119edf6f63cdee3c02aa614b048b777c6de8735d7faefe03
  kind: experience
  material_digest: c17f7736a716f1f2e664f832ef777782e4f7761469d70772a0bdcd404d014e42
  revision: 09911f597031e3450f3409faf777477b50e4eea116d79872dfa1dfdc202d32e5
  schema: mindie-entry/3
  summary: '当前材料覆盖本地经验入库、知识包消费、PR #150 验证、恢复稿权限审查，以及 13 篇案例的深度证据复核。重点检索词包括 knowledge_query、experience_query、MERGEABLE、Windows 进程探测/时间窗口测试、named tool、采样 OOM、prefix cache、MRV2、KDA packed parameter、P/D 重试、SP shard、residual bank、dispatcher、FIA metadata；后续更正和未完成设备验收限制均需保留，不能把报告性通过结论升级为已验证事实。'
  title: 经验汇总与证据边界复核
navigation: '当前材料覆盖本地经验入库、知识包消费、PR #150 验证、恢复稿权限审查，以及 13 篇案例的深度证据复核。重点检索词包括 knowledge_query、experience_query、MERGEABLE、Windows 进程探测/时间窗口测试、named tool、采样 OOM、prefix cache、MRV2、KDA packed parameter、P/D 重试、SP shard、residual bank、dispatcher、FIA metadata；后续更正和未完成设备验收限制均需保留，不能把报告性通过结论升级为已验证事实。'
schema: mindie-material-task/1
status: complete
task_id: 3ab1bf4ca0cea771119edf6f63cdee3c02aa614b048b777c6de8735d7faefe03
---

# 经验汇总与证据边界复核

当前材料覆盖本地经验入库、知识包消费、PR #150 验证、恢复稿权限审查，以及 13 篇案例的深度证据复核。重点检索词包括 knowledge_query、experience_query、MERGEABLE、Windows 进程探测/时间窗口测试、named tool、采样 OOM、prefix cache、MRV2、KDA packed parameter、P/D 重试、SP shard、residual bank、dispatcher、FIA metadata；后续更正和未完成设备验收限制均需保留，不能把报告性通过结论升级为已验证事实。

## Materials

- [本地经验整理、PR 消费与案例复核记录](blocks/9ad192eb1fba8d2173af98c8d659943e11a8a639b6970ded58e93b0c22d780cf.md): 记录了知识/经验分离存储与检索、29 篇经验整理、跨 lane 复核、PR #150 的依赖重放与多平台 CI；其中报告称测试通过、Windows 失败重跑通过且 PR 为 MERGEABLE，但这些均属于来源中的报告，不能视为本索引已验证的成功。后续恢复稿复核保留了未完成设备闭环、第三方来源排除及 OP15 范围收窄；13 篇案例深审还修正了 named tool 验收断言、采样 OOM 临时排序空间、prefix cache 冷/命中/重置区分、算子安装验证坐标和 MRV2 热 prefix 失败等证据边界。若干后续补证涉及日期、SP 分片与 residual bank 顺序，以及 dispatcher/FIA metadata 对 48/96 tokens 与 8/16 padded requests 的换算；正文据称未变，但原始记录、哈希和审阅结论仍应作为可检索的来源材料看待。

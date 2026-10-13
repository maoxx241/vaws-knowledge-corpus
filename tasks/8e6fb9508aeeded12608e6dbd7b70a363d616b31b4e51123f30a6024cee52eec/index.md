---
blocks:
- block_id: 5332cf5a217646f463e9193b52984f86b8629351ce3ce1465f533ffb4aceb771
  indexed: true
  sha256: b1d2fcf75b64fd05160116c612b5d437318386b082381c4a0ef95339e9f52f32
  source_range:
    end: 3740717
    part: 0
    start: 0
  summary: 确认 commit `71c21f76f5d7d0674185340cdff86772c78b8508` 将 Ring MLA 替换为 FIA；目标是仅恢复 A2/A3 非 CP prefill，保留 A5、decode FIA、PCP 和 FlashComm1。A3 实测 H=96、qk=128+64、v=128 的 `npu_ring_mla` 无需 padding，针对性测试 8/8 通过。四机 prefix cache 配置成功启动并通过权重加载、编译和 ACLGraph capture，但初次长前缀探针返回 HTTP 500，功能验收当时尚未完成；期间还暴露并修复隔离部署缺失 custom-op bundle 和 ABI 不匹配扩展的问题。
  title: A2/A3 Ring MLA 恢复与 prefix cache 初次失败
- block_id: 4d87facf90e26d4bc71013c6ea9d2d7c2ed82e4e9566b2e715a1af18ecef7dfa
  indexed: true
  sha256: e0903eabf01fb663bf7f28c9b3ff9d4881037447e08a99994b16d1d90c21eb8c
  source_range:
    end: 3740717
    part: 1
    start: 0
  summary: '初次 prefix-cache 500 后确认原因是测试 `max_tokens=8` 截断 Kimi XTML 结束标记，并非服务故障；改用足够上限后，16/16 请求 HTTP 200、输出一致，四个 DP engine 各增加 5376 个命中 token。完整 GPQA runner 曾因容器内数据集路径漂移未启动，随后复用 SHA-256 为 `8258034b...fda` 的 198 题 CSV；前 9 题 9/9 正确后创建 Draft PR #41，CI 后续仅暴露两个未修改基线文件的全仓 `ruff-format` 漂移。评测期间多次检查显示服务健康、无新增 benchmark 500，但中途精度快照和剩余题目估计均只是阶段性结果。'
  title: 'Prefix cache 命中确认与 Draft PR #41'
- block_id: d42388db4574b23e362043f8e629e5b2cb5b3b645f3c72e78a778a3a1471eaf6
  indexed: true
  sha256: 4f303253a90690045955abd607421c626767f73042ca9070a8a74f0bb8b7a2c2
  source_range:
    end: 3740717
    part: 2
    start: 0
  summary: '最后长尾题持续真实 decode，曾通过 token 增量和 running 数确认没有死锁；ID 88、185、79、145 等结果陆续修正了此前基于历史命中率的预估。最终逐行对账与 `summary.json` 一致：198/198、181 正确、17 错，得分 91.4141%，0 error、0 unparsed、全部 `stop`；质量扫描未发现重复循环或 XTML 泄漏。PR #41 已回填完整 GPQA、prefix-cache 证据和 CI 基线格式漂移说明，但 CI 仍因未修改文件失败，不能将其表述为全套检查通过。'
  title: GPQA 完成：181/198
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8e6fb9508aeeded12608e6dbd7b70a363d616b31b4e51123f30a6024cee52eec
  kind: experience
  material_digest: 542aa4ded94f7e615c97ba9690cc79008059c8bc437aec084aa793f32e7cbd5f
  revision: e71d7baf4b4eb2ddf748674411a069efabb5f777a577ec4f2da98f825d3d78c2
  schema: mindie-entry/3
  summary: '任务已完成并记录在 Draft PR #41（HEAD `89840e922`）：A2/A3 非 CP prefill 恢复 Ring MLA，原生 96 heads 无需 padding，A5/decode FIA、PCP 保持不变。四机 FlashComm1 与 prefix cache 均已实测，prefix cache 热命中验证通过；GPQA-Diamond 最终为 181/198（91.4141%）。需保留的未完成/风险项：PR 的全仓 CI 因两个未修改基线文件的 `ruff-format` 漂移失败，且不应将阶段性估计或先前 500 探针误认为最终失败；四机服务按记录仍在运行。'
  title: A2/A3 Ring MLA 恢复任务导航
navigation: '任务已完成并记录在 Draft PR #41（HEAD `89840e922`）：A2/A3 非 CP prefill 恢复 Ring MLA，原生 96 heads 无需 padding，A5/decode FIA、PCP 保持不变。四机 FlashComm1 与 prefix cache 均已实测，prefix cache 热命中验证通过；GPQA-Diamond 最终为 181/198（91.4141%）。需保留的未完成/风险项：PR 的全仓 CI 因两个未修改基线文件的 `ruff-format` 漂移失败，且不应将阶段性估计或先前 500 探针误认为最终失败；四机服务按记录仍在运行。'
schema: mindie-material-task/1
status: complete
task_id: 8e6fb9508aeeded12608e6dbd7b70a363d616b31b4e51123f30a6024cee52eec
---

# A2/A3 Ring MLA 恢复任务导航

任务已完成并记录在 Draft PR #41（HEAD `89840e922`）：A2/A3 非 CP prefill 恢复 Ring MLA，原生 96 heads 无需 padding，A5/decode FIA、PCP 保持不变。四机 FlashComm1 与 prefix cache 均已实测，prefix cache 热命中验证通过；GPQA-Diamond 最终为 181/198（91.4141%）。需保留的未完成/风险项：PR 的全仓 CI 因两个未修改基线文件的 `ruff-format` 漂移失败，且不应将阶段性估计或先前 500 探针误认为最终失败；四机服务按记录仍在运行。

## Materials

- [A2/A3 Ring MLA 恢复与 prefix cache 初次失败](blocks/5332cf5a217646f463e9193b52984f86b8629351ce3ce1465f533ffb4aceb771.md): 确认 commit `71c21f76f5d7d0674185340cdff86772c78b8508` 将 Ring MLA 替换为 FIA；目标是仅恢复 A2/A3 非 CP prefill，保留 A5、decode FIA、PCP 和 FlashComm1。A3 实测 H=96、qk=128+64、v=128 的 `npu_ring_mla` 无需 padding，针对性测试 8/8 通过。四机 prefix cache 配置成功启动并通过权重加载、编译和 ACLGraph capture，但初次长前缀探针返回 HTTP 500，功能验收当时尚未完成；期间还暴露并修复隔离部署缺失 custom-op bundle 和 ABI 不匹配扩展的问题。
- [Prefix cache 命中确认与 Draft PR #41](blocks/4d87facf90e26d4bc71013c6ea9d2d7c2ed82e4e9566b2e715a1af18ecef7dfa.md): 初次 prefix-cache 500 后确认原因是测试 `max_tokens=8` 截断 Kimi XTML 结束标记，并非服务故障；改用足够上限后，16/16 请求 HTTP 200、输出一致，四个 DP engine 各增加 5376 个命中 token。完整 GPQA runner 曾因容器内数据集路径漂移未启动，随后复用 SHA-256 为 `8258034b...fda` 的 198 题 CSV；前 9 题 9/9 正确后创建 Draft PR #41，CI 后续仅暴露两个未修改基线文件的全仓 `ruff-format` 漂移。评测期间多次检查显示服务健康、无新增 benchmark 500，但中途精度快照和剩余题目估计均只是阶段性结果。
- [GPQA 完成：181/198](blocks/d42388db4574b23e362043f8e629e5b2cb5b3b645f3c72e78a778a3a1471eaf6.md): 最后长尾题持续真实 decode，曾通过 token 增量和 running 数确认没有死锁；ID 88、185、79、145 等结果陆续修正了此前基于历史命中率的预估。最终逐行对账与 `summary.json` 一致：198/198、181 正确、17 错，得分 91.4141%，0 error、0 unparsed、全部 `stop`；质量扫描未发现重复循环或 XTML 泄漏。PR #41 已回填完整 GPQA、prefix-cache 证据和 CI 基线格式漂移说明，但 CI 仍因未修改文件失败，不能将其表述为全套检查通过。

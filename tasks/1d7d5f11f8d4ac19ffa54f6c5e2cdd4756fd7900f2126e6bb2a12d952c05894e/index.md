---
blocks:
- block_id: 4929da13605f5de022976b476aa2792d294e416d865c3ba58e6300ad8055cb14
  indexed: true
  sha256: 07152c03cd4eddfb43aff236823ecc4ced9beec2868952351007c80ef080319a
  source_range:
    end: 16778084
    part: 0
    start: 0
  summary: 材料记录了 knowledge gate、comparability、组织地图、规范化算法及输入门禁等工作的复现、候选提交和验收状态：曾发现错误 PASS、崩溃被包装为 reject、声明冲突、漏观测、错误来源 metadata 等问题，部分已通过独立测试修复，但不能将计划或局部通过推广为整体成功。Phase B 在提交 `35157e52` 上通过哈希和共享向量检查，却仍存在合法仓库名篡改可被接受、新测试依赖本机私有路径等边界；Grok advisory adapter 的零外发、建议输出限制、超时/响应大小及凭据泄露检查仍在处理中。#5、中央收集、CI 权限、来源 provenance、缓存失败刷新和后续客户端验收均需保留其各自的提交、失败、取消或未完成状态。
  title: 多项验收、修复与 Phase B 余留问题索引
- block_id: a7c47cd06a5cbf424c24d2cf36bb725e9177144c5a492bf1b2e361c61a230060
  indexed: true
  sha256: 914483e673eddd865fbfcb872cd35f8eca4935f112b48a60b2c1580b377329ea
  source_range:
    end: 27448651
    part: 0
    start: 16778084
  summary: 材料记录了多轮独立验收、CI 与受控 live run：曾发现冲突元数据、读取绑定丢失、部分失败被报为成功、零 eligible 时 artifact 缺失，以及 bare 仓库默认分支导致 clone 未检出文件；显式选择 main 的修正已在 main/master/自定义分支 fixture 中通过，但未重跑整套测试。随后公开 main、558 项 CI、preview/propose/snapshot 和完整恢复流程被分别报告为通过；不过真实 provider 评审、正例提案、消费者刷新及后续公开发布仍未执行或曾暂停。材料还保留了权限与迁移审计、foreign-head PR 覆盖不足、发布包暂停，以及恢复后来源表绑定证据等后续修正，不能将局部或受控结果视为全部生产验证。
  title: 恢复验收与 live 流程：从 clone 修正到暂停发布
- block_id: 8d74b24952640dce75bf5126bf7e6782591e6867897f2f91e039107f280421ad
  indexed: true
  sha256: d368fdb5e54c3d27453cc7d0bc962f4355db44606e0f009a84e3b50b64e8fd01
  source_range:
    end: 27448651
    part: 1
    start: 16778084
  summary: '来源表在完整恢复 PASS 后继续用于发布文档；tracking issue #2 已关联两份 PR，scaffold PR #102 和 `.github` PR 均已实际创建并合并。最后一次状态修订将验收标为完成，并把 `.github` PR 的实际 ID 填为 4469341770；同时保留历史证据、原 issue 正文、K2/C3 延期和知识库零有效输入等限制。该块记录的是发布状态与证据字段的回读/修正，不应外推为更广泛的业务或生产成功。'
  title: '公开发布收尾：issue #2 与两条 PR 的状态修订'
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 1d7d5f11f8d4ac19ffa54f6c5e2cdd4756fd7900f2126e6bb2a12d952c05894e
  kind: experience
  material_digest: 252d3cdc360191e94419f4d92fd2ddb621e78b74bbd6827a71f43e4e2f1189c5
  revision: 9d15fa6ccb46bf8afc0f74bac9e251cb6685bb2c8e1195f49a25e3296ca3b9f6
  schema: mindie-entry/3
  summary: '已索引材料覆盖原始漏洞复现、Phase B 集成、共享缓存、中央收集、advisory adapter，以及从 clone/CI 修正到受控 live 验收、恢复审计和公开发布收尾。检索重点包括错误 PASS/崩溃传播、schema 与输入门禁、commit/PR 绑定、provenance、缓存 metadata 篡改、bare 仓库默认分支、artifact 缺失、权限与迁移边界、issue/PR 状态及实际 merge ID；需保留失败、取消、暂停、零 eligible 和未执行 provider/消费者刷新等不确定性。后续材料显示 tracking issue #2、scaffold PR #102 与 `.github` PR 已进入合并收尾，但局部控制、受控 live 或文档状态完成不等于全面生产验证；原始块仍独立可检索，不能补写被省略的细节。'
  title: 当前任务导航：验收、发布收尾与未覆盖边界
navigation: '已索引材料覆盖原始漏洞复现、Phase B 集成、共享缓存、中央收集、advisory adapter，以及从 clone/CI 修正到受控 live 验收、恢复审计和公开发布收尾。检索重点包括错误 PASS/崩溃传播、schema 与输入门禁、commit/PR 绑定、provenance、缓存 metadata 篡改、bare 仓库默认分支、artifact 缺失、权限与迁移边界、issue/PR 状态及实际 merge ID；需保留失败、取消、暂停、零 eligible 和未执行 provider/消费者刷新等不确定性。后续材料显示 tracking issue #2、scaffold PR #102 与 `.github` PR 已进入合并收尾，但局部控制、受控 live 或文档状态完成不等于全面生产验证；原始块仍独立可检索，不能补写被省略的细节。'
schema: mindie-material-task/1
status: complete
task_id: 1d7d5f11f8d4ac19ffa54f6c5e2cdd4756fd7900f2126e6bb2a12d952c05894e
---

# 当前任务导航：验收、发布收尾与未覆盖边界

已索引材料覆盖原始漏洞复现、Phase B 集成、共享缓存、中央收集、advisory adapter，以及从 clone/CI 修正到受控 live 验收、恢复审计和公开发布收尾。检索重点包括错误 PASS/崩溃传播、schema 与输入门禁、commit/PR 绑定、provenance、缓存 metadata 篡改、bare 仓库默认分支、artifact 缺失、权限与迁移边界、issue/PR 状态及实际 merge ID；需保留失败、取消、暂停、零 eligible 和未执行 provider/消费者刷新等不确定性。后续材料显示 tracking issue #2、scaffold PR #102 与 `.github` PR 已进入合并收尾，但局部控制、受控 live 或文档状态完成不等于全面生产验证；原始块仍独立可检索，不能补写被省略的细节。

## Materials

- [多项验收、修复与 Phase B 余留问题索引](blocks/4929da13605f5de022976b476aa2792d294e416d865c3ba58e6300ad8055cb14.md): 材料记录了 knowledge gate、comparability、组织地图、规范化算法及输入门禁等工作的复现、候选提交和验收状态：曾发现错误 PASS、崩溃被包装为 reject、声明冲突、漏观测、错误来源 metadata 等问题，部分已通过独立测试修复，但不能将计划或局部通过推广为整体成功。Phase B 在提交 `35157e52` 上通过哈希和共享向量检查，却仍存在合法仓库名篡改可被接受、新测试依赖本机私有路径等边界；Grok advisory adapter 的零外发、建议输出限制、超时/响应大小及凭据泄露检查仍在处理中。#5、中央收集、CI 权限、来源 provenance、缓存失败刷新和后续客户端验收均需保留其各自的提交、失败、取消或未完成状态。
- [恢复验收与 live 流程：从 clone 修正到暂停发布](blocks/a7c47cd06a5cbf424c24d2cf36bb725e9177144c5a492bf1b2e361c61a230060.md): 材料记录了多轮独立验收、CI 与受控 live run：曾发现冲突元数据、读取绑定丢失、部分失败被报为成功、零 eligible 时 artifact 缺失，以及 bare 仓库默认分支导致 clone 未检出文件；显式选择 main 的修正已在 main/master/自定义分支 fixture 中通过，但未重跑整套测试。随后公开 main、558 项 CI、preview/propose/snapshot 和完整恢复流程被分别报告为通过；不过真实 provider 评审、正例提案、消费者刷新及后续公开发布仍未执行或曾暂停。材料还保留了权限与迁移审计、foreign-head PR 覆盖不足、发布包暂停，以及恢复后来源表绑定证据等后续修正，不能将局部或受控结果视为全部生产验证。
- [公开发布收尾：issue #2 与两条 PR 的状态修订](blocks/8d74b24952640dce75bf5126bf7e6782591e6867897f2f91e039107f280421ad.md): 来源表在完整恢复 PASS 后继续用于发布文档；tracking issue #2 已关联两份 PR，scaffold PR #102 和 `.github` PR 均已实际创建并合并。最后一次状态修订将验收标为完成，并把 `.github` PR 的实际 ID 填为 4469341770；同时保留历史证据、原 issue 正文、K2/C3 延期和知识库零有效输入等限制。该块记录的是发布状态与证据字段的回读/修正，不应外推为更广泛的业务或生产成功。

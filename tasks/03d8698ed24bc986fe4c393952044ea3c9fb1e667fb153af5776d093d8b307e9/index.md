---
blocks:
- block_id: a6f04300fae90e4ff68da34bbecd7b529970912ff78fcf39df28d2e32445361c
  indexed: true
  sha256: 6abac783e6c5e02b8e1ce0577c661ba6f19b36164bbb9c193de24f18c54f322c
  source_range:
    end: 1770790
    part: 0
    start: 0
  summary: 提出将经验与知识分开存储并提供区别化查询接口；相关报告称已实现独立目录、canonical URI、捕获/删除流程、`experience.layers`、`VAWS_EXPERIENCE_*` 与 `ServiceConfig.for_kind()`，并保留旧 Markdown，声称责任范围测试为 65 passed 且 `git diff --check` 通过。后续报告称审阅未发现阻塞问题，包括查询在向量 top-k 前限定 kind、不同布局的类型范围限制，以及查询和 explain 使用一致范围；但这些内容均是来源中的报告，不能据此视为独立验证的成功。来源还提到引擎拒绝反斜杠和 `..` 路径段，以及未发现 SDK/引擎读取链导致跨类读取的编码路径。
  title: 经验与知识分离存储及类型限定审阅
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 03d8698ed24bc986fe4c393952044ea3c9fb1e667fb153af5776d093d8b307e9
  kind: experience
  material_digest: 4c5c186fef9bd60a0f54b5ebb69cc59dcf4a82ff937091fe077e21bfc7afe322
  revision: 6891fd0521a2c0a1adeacc3729cc094bdd702288f6ef288a4fe6d24c76f4ba95
  schema: mindie-entry/3
  summary: 当前任务聚焦于经验与知识的独立存储、按 kind 限定查询范围及防止跨类读取。来源报告了目录布局、配置接口、迁移策略、路径安全规则和测试/审阅结果，但这些结果仍属于待独立核验的参考信息；旧布局兼容、旧 URI 迁移及完整验证状态应继续单独检索。
  title: 经验/知识分离存储与类型安全查询
navigation: 当前任务聚焦于经验与知识的独立存储、按 kind 限定查询范围及防止跨类读取。来源报告了目录布局、配置接口、迁移策略、路径安全规则和测试/审阅结果，但这些结果仍属于待独立核验的参考信息；旧布局兼容、旧 URI 迁移及完整验证状态应继续单独检索。
schema: mindie-material-task/1
status: complete
task_id: 03d8698ed24bc986fe4c393952044ea3c9fb1e667fb153af5776d093d8b307e9
---

# 经验/知识分离存储与类型安全查询

当前任务聚焦于经验与知识的独立存储、按 kind 限定查询范围及防止跨类读取。来源报告了目录布局、配置接口、迁移策略、路径安全规则和测试/审阅结果，但这些结果仍属于待独立核验的参考信息；旧布局兼容、旧 URI 迁移及完整验证状态应继续单独检索。

## Materials

- [经验与知识分离存储及类型限定审阅](blocks/a6f04300fae90e4ff68da34bbecd7b529970912ff78fcf39df28d2e32445361c.md): 提出将经验与知识分开存储并提供区别化查询接口；相关报告称已实现独立目录、canonical URI、捕获/删除流程、`experience.layers`、`VAWS_EXPERIENCE_*` 与 `ServiceConfig.for_kind()`，并保留旧 Markdown，声称责任范围测试为 65 passed 且 `git diff --check` 通过。后续报告称审阅未发现阻塞问题，包括查询在向量 top-k 前限定 kind、不同布局的类型范围限制，以及查询和 explain 使用一致范围；但这些内容均是来源中的报告，不能据此视为独立验证的成功。来源还提到引擎拒绝反斜杠和 `..` 路径段，以及未发现 SDK/引擎读取链导致跨类读取的编码路径。

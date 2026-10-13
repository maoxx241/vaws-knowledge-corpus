---
blocks:
- block_id: 5d3540bc98f93c4d1f284c16c847db5190571d6844d3502fefe6ae3c9bef7acb
  indexed: true
  sha256: df19256f3691a77fc2bb9eb965301b6d6c01614067c109e4274262fd070d507d
  source_range:
    end: 1512708
    part: 0
    start: 0
  summary: 用户提出将经验与知识分开存储，并区分查询接口。后续消息声称已将贡献与分发分离，使用 `corpus/knowledge/` 和 `corpus/experience/`、双类 OVPack 及兼容旧 pending/manifest 的导入逻辑，但这些内容属于报告，不能据此视为已验证成功。消息还报告原生链测试待主 agent 执行，同时声称部分测试和 `git diff --check` 通过；测试范围、环境和完整结果仍需独立核验。
  title: 经验与知识分离存储及检索
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3f74d4eebdaccc329265efbb4b55cd2354ffa519a3605357c38e1bbf26562bd7
  kind: experience
  material_digest: fa51bbbdd3a716590b6225e67f10134732e972af52ec535770f8d970f5bb7ec9
  revision: 3111710af573ac354d602178539eac10f132dda470195780949d4a73328b4468
  schema: mindie-entry/3
  summary: 当前材料聚焦于把经验与知识分开存储，并提供区别化查询接口；报告中的实现涉及 `corpus/knowledge/`、`corpus/experience/`、OVPack、manifest、pending、分支、去重、导入及完整性修复。已有消息同时保留了未执行的原生链测试与若干声称通过的测试结果，不能将其升级为已验证结论；原始材料仍应按 block_id 独立检索。
  title: 任务导航：经验/知识分离与检索索引
navigation: 当前材料聚焦于把经验与知识分开存储，并提供区别化查询接口；报告中的实现涉及 `corpus/knowledge/`、`corpus/experience/`、OVPack、manifest、pending、分支、去重、导入及完整性修复。已有消息同时保留了未执行的原生链测试与若干声称通过的测试结果，不能将其升级为已验证结论；原始材料仍应按 block_id 独立检索。
schema: mindie-material-task/1
status: complete
task_id: 3f74d4eebdaccc329265efbb4b55cd2354ffa519a3605357c38e1bbf26562bd7
---

# 任务导航：经验/知识分离与检索索引

当前材料聚焦于把经验与知识分开存储，并提供区别化查询接口；报告中的实现涉及 `corpus/knowledge/`、`corpus/experience/`、OVPack、manifest、pending、分支、去重、导入及完整性修复。已有消息同时保留了未执行的原生链测试与若干声称通过的测试结果，不能将其升级为已验证结论；原始材料仍应按 block_id 独立检索。

## Materials

- [经验与知识分离存储及检索](blocks/5d3540bc98f93c4d1f284c16c847db5190571d6844d3502fefe6ae3c9bef7acb.md): 用户提出将经验与知识分开存储，并区分查询接口。后续消息声称已将贡献与分发分离，使用 `corpus/knowledge/` 和 `corpus/experience/`、双类 OVPack 及兼容旧 pending/manifest 的导入逻辑，但这些内容属于报告，不能据此视为已验证成功。消息还报告原生链测试待主 agent 执行，同时声称部分测试和 `git diff --check` 通过；测试范围、环境和完整结果仍需独立核验。

---
blocks:
- block_id: afc542d2e966dab16252d2759157a2162bfe8f084bfac6416c20f866b020b04d
  indexed: true
  sha256: cba3d628a928fb470d8f122577ce9068d68138d3b496bb2057d5b1342b764379
  source_range:
    end: 1250458
    part: 0
    start: 0
  summary: 来源表示将经验与知识分开存储，并区分查询接口；据其报告，新增 experience_query/explain/capture、CLI 命令、经验路径与自动摘要逻辑，同时保留幂等、非阻塞行为及部分兼容性限制。报告还称已处理 Windows 路径转换和配置迁移，并在后续修复了配置覆盖问题：无 override 时保留完整经验配置、仅覆盖 project 时保留 candidate/shared 自定义路径，且不写回源文件。所谓测试通过、联测结果及“完成”均为来源中的报告，仍注明未提交发布、未修改依赖 pin/lock 或 live config，旧资料也未必通过当前 main 验证。
  title: 经验存储与配置覆盖修复索引
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 64c9a0412b2fb836ea5c619e50aea5f5adb4e7e89e0e82936dd6ea69fe157680
  kind: experience
  material_digest: 54c72717013861fb17e6b8f0336611e5be65323efeba564dcd0410a883771909
  revision: e5a50d2fe2483ccdaf68058078ef1f1c0ac6fa878718984495a47fbe45e02fa4
  schema: mindie-entry/3
  summary: 本批唯一材料涉及经验与知识分离存储、独立查询接口、consumer lane、经验路径、自动摘要、Windows 路径转换及配置迁移。后续更正聚焦配置覆盖合并：保留文件中的完整经验配置和 candidate/shared 自定义路径，且不写回源文件；来源报告的测试和联测结果、未提交发布状态及未验证限制仍需保留。原始材料块可独立检索，未提供的早期细节不应推断。
  title: 当前任务导航：经验/知识索引与配置修复
navigation: 本批唯一材料涉及经验与知识分离存储、独立查询接口、consumer lane、经验路径、自动摘要、Windows 路径转换及配置迁移。后续更正聚焦配置覆盖合并：保留文件中的完整经验配置和 candidate/shared 自定义路径，且不写回源文件；来源报告的测试和联测结果、未提交发布状态及未验证限制仍需保留。原始材料块可独立检索，未提供的早期细节不应推断。
schema: mindie-material-task/1
status: complete
task_id: 64c9a0412b2fb836ea5c619e50aea5f5adb4e7e89e0e82936dd6ea69fe157680
---

# 当前任务导航：经验/知识索引与配置修复

本批唯一材料涉及经验与知识分离存储、独立查询接口、consumer lane、经验路径、自动摘要、Windows 路径转换及配置迁移。后续更正聚焦配置覆盖合并：保留文件中的完整经验配置和 candidate/shared 自定义路径，且不写回源文件；来源报告的测试和联测结果、未提交发布状态及未验证限制仍需保留。原始材料块可独立检索，未提供的早期细节不应推断。

## Materials

- [经验存储与配置覆盖修复索引](blocks/afc542d2e966dab16252d2759157a2162bfe8f084bfac6416c20f866b020b04d.md): 来源表示将经验与知识分开存储，并区分查询接口；据其报告，新增 experience_query/explain/capture、CLI 命令、经验路径与自动摘要逻辑，同时保留幂等、非阻塞行为及部分兼容性限制。报告还称已处理 Windows 路径转换和配置迁移，并在后续修复了配置覆盖问题：无 override 时保留完整经验配置、仅覆盖 project 时保留 candidate/shared 自定义路径，且不写回源文件。所谓测试通过、联测结果及“完成”均为来源中的报告，仍注明未提交发布、未修改依赖 pin/lock 或 live config，旧资料也未必通过当前 main 验证。

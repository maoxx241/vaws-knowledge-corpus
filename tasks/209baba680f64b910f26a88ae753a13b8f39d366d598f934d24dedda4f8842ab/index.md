---
blocks:
- block_id: 16f61325f313bdfffec35b6db4e90db22d97a38e854d7d6a426e850c5b90a6b3
  indexed: true
  sha256: e57bc39857e873bf80abfc466abbe15c7e71f4a4022a8c444617290826a24855
  source_range:
    end: 824008
    part: 0
    start: 0
  summary: 只读审计认为，a9e34e17 的唯一运行时行为变化是对既有 num_accepted_tokens_event 执行 host-side event synchronize；该 fence 位于 Mamba 后处理及 accepted-token D2H enqueue 之后、返回 scheduler 之前，不是 device-wide synchronize。提交直接基于 0a3560aa，998347889 虽非其祖先但相关 zeroer 语义被认为已折叠；若要求源码排除 cached-source DFX，应以 43ae432 为基线移植 completion patch。CPU UT 报告为 2 passed、15 warnings，但结论仍保留 ancestry、DFX 源码残留及静态移植判断中的不确定性。
  title: a9e34e17 完成事件 fence 只读审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 209baba680f64b910f26a88ae753a13b8f39d366d598f934d24dedda4f8842ab
  kind: experience
  material_digest: 3a5f59f84fb83799005989df91d0881a9b7eac02bd61f25ecde2e3f46115d399
  revision: 2e68b6547daed8de1b2012defc9d8cf56d27bcea83ab318bdae02c9cbf615ebb
  schema: mindie-entry/3
  summary: 当前材料聚焦 a9e34e17 的对象闭包、单变量运行时 diff、global-stream 事件顺序、基线兼容性与 CPU UT。检索关键词包括 num_accepted_tokens_event、Mamba postprocess、host wait、device-wide synchronize、patch-id、43ae432、0a3560aa、998347889、cached-source DFX、2 passed/15 warnings；需区分已报告的测试结果、静态兼容性判断与尚未验证的部署/源码排除要求。
  title: 任务导航：completion event fence 审计
navigation: 当前材料聚焦 a9e34e17 的对象闭包、单变量运行时 diff、global-stream 事件顺序、基线兼容性与 CPU UT。检索关键词包括 num_accepted_tokens_event、Mamba postprocess、host wait、device-wide synchronize、patch-id、43ae432、0a3560aa、998347889、cached-source DFX、2 passed/15 warnings；需区分已报告的测试结果、静态兼容性判断与尚未验证的部署/源码排除要求。
schema: mindie-material-task/1
status: complete
task_id: 209baba680f64b910f26a88ae753a13b8f39d366d598f934d24dedda4f8842ab
---

# 任务导航：completion event fence 审计

当前材料聚焦 a9e34e17 的对象闭包、单变量运行时 diff、global-stream 事件顺序、基线兼容性与 CPU UT。检索关键词包括 num_accepted_tokens_event、Mamba postprocess、host wait、device-wide synchronize、patch-id、43ae432、0a3560aa、998347889、cached-source DFX、2 passed/15 warnings；需区分已报告的测试结果、静态兼容性判断与尚未验证的部署/源码排除要求。

## Materials

- [a9e34e17 完成事件 fence 只读审计](blocks/16f61325f313bdfffec35b6db4e90db22d97a38e854d7d6a426e850c5b90a6b3.md): 只读审计认为，a9e34e17 的唯一运行时行为变化是对既有 num_accepted_tokens_event 执行 host-side event synchronize；该 fence 位于 Mamba 后处理及 accepted-token D2H enqueue 之后、返回 scheduler 之前，不是 device-wide synchronize。提交直接基于 0a3560aa，998347889 虽非其祖先但相关 zeroer 语义被认为已折叠；若要求源码排除 cached-source DFX，应以 43ae432 为基线移植 completion patch。CPU UT 报告为 2 passed、15 warnings，但结论仍保留 ancestry、DFX 源码残留及静态移植判断中的不确定性。

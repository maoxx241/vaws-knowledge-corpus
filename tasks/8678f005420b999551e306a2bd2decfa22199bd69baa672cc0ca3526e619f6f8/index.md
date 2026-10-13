---
blocks:
- block_id: 75a71207fd7d4c25f2b43d8422ac86ad06e4025d5889502e79ec3d4e9575065c
  indexed: true
  sha256: 3cb50a7df20c9bfcef86c11178ff7620a813c399a464c31fd39f6f2b18ac2178
  source_range:
    end: 5885523
    part: 0
    start: 0
  summary: 围绕 zeroer 在不同 page-size bucket 首轮提前返回、stream/event 覆盖范围、hit-preprocess 写集合，以及 recurrent/MLA K 异常而 V 正常等线索，进行了静态审计和可证伪的 C0–C5、八至十一 case 实验矩阵设计；这些仍主要是可疑点和待区分因素，并非已验证根因。记录称已完成第一轮单变量拆分、q2 Mamba copy 范围核对和只读闭包；最终报告为 PASS，称 commit `449378d6890a…` clean、`py_compile` 通过、客户端 UT 6/6 通过，且 contract、`num_accepted_tokens=1`、19 checkpoints、5 input stages、57 records/stage 和 classifier fail-close 一致，但消息中未提供独立实验输出或错误复现细节。
  title: NPU 缓存页与事件覆盖实验审计
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 8678f005420b999551e306a2bd2decfa22199bd69baa672cc0ca3526e619f6f8
  kind: experience
  material_digest: 053d4d7856e46b9fc28965296028fcf94ffe3fdd2a14513795e88bbe92482f27
  revision: 720aa5a75ad3727093d0f302c93527e10d70fd059c683cc3107842184a2323b8
  schema: mindie-entry/3
  summary: 当前唯一材料围绕 zeroer 提前返回、stream/event 覆盖、hit-preprocess/postprocess 写集合及 K/V 异常的可证伪定位；曾保留多个未证实假设和不可区分项，随后记录称实现拆分、审计闭包及测试检查完成并给出 PASS。PASS 结论来自来源中的报告，不能据此把疑点、计划或检查结果提升为已验证根因；原始消息仍可独立检索。
  title: 当前任务导航：缓存页清零与 NPU 写者定位
navigation: 当前唯一材料围绕 zeroer 提前返回、stream/event 覆盖、hit-preprocess/postprocess 写集合及 K/V 异常的可证伪定位；曾保留多个未证实假设和不可区分项，随后记录称实现拆分、审计闭包及测试检查完成并给出 PASS。PASS 结论来自来源中的报告，不能据此把疑点、计划或检查结果提升为已验证根因；原始消息仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: 8678f005420b999551e306a2bd2decfa22199bd69baa672cc0ca3526e619f6f8
---

# 当前任务导航：缓存页清零与 NPU 写者定位

当前唯一材料围绕 zeroer 提前返回、stream/event 覆盖、hit-preprocess/postprocess 写集合及 K/V 异常的可证伪定位；曾保留多个未证实假设和不可区分项，随后记录称实现拆分、审计闭包及测试检查完成并给出 PASS。PASS 结论来自来源中的报告，不能据此把疑点、计划或检查结果提升为已验证根因；原始消息仍可独立检索。

## Materials

- [NPU 缓存页与事件覆盖实验审计](blocks/75a71207fd7d4c25f2b43d8422ac86ad06e4025d5889502e79ec3d4e9575065c.md): 围绕 zeroer 在不同 page-size bucket 首轮提前返回、stream/event 覆盖范围、hit-preprocess 写集合，以及 recurrent/MLA K 异常而 V 正常等线索，进行了静态审计和可证伪的 C0–C5、八至十一 case 实验矩阵设计；这些仍主要是可疑点和待区分因素，并非已验证根因。记录称已完成第一轮单变量拆分、q2 Mamba copy 范围核对和只读闭包；最终报告为 PASS，称 commit `449378d6890a…` clean、`py_compile` 通过、客户端 UT 6/6 通过，且 contract、`num_accepted_tokens=1`、19 checkpoints、5 input stages、57 records/stage 和 classifier fail-close 一致，但消息中未提供独立实验输出或错误复现细节。

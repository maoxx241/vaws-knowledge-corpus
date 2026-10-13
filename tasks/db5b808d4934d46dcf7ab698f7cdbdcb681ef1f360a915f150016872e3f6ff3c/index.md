---
blocks:
- block_id: b0a7f62d6648a554d937484ff36cace9fcee2db8d7916dd00bbba1678f608c48
  indexed: true
  sha256: 1763ab712467ceaaf8dfa25e1f2c18dad3504a56b416351cbe9cbddaa1215b7e
  source_range:
    end: 1428463
    part: 0
    start: 0
  summary: 只读审查围绕 W4A16 packed 死分支、W4A8 加载期别名契约，以及 target/draft 通信选择展开；随后报告称已修正 W4A8 packed 标记误传给 Linear scheme 的构造缺陷，使 packed 参数仅用于 MoE，并新增 Linear 构造和 packed MoE canonical 属性回归测试，同时删除 3 个 W4A16 永久 skip 测试。报告称 Ruff、格式检查和 `git diff --check` 通过，但 pytest 未运行，原因是环境缺少 `pytest` 模块；因此这些改动和测试结果应视为报告内容，未验证为成功。
  title: 量化收口与测试状态
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: db5b808d4934d46dcf7ab698f7cdbdcb681ef1f360a915f150016872e3f6ff3c
  kind: experience
  material_digest: 25d663e2e586fbbc05ce74ca34f039567cde4c98af4e75be409b7bf9f65ca7a0
  revision: c4b7e2ce71c6146eecba0aa21a172959674a18967d3ab47fc73862787fe91ae6
  schema: mindie-entry/3
  summary: 重点检索 W4A8 packed 参数与 Linear/MoE scheme 构造、加载期别名契约、W4A16 skip 测试、target/draft 通信选择及静态检查结果。已报告修正和检查通过，但 pytest 因缺少 `pytest` 模块未运行；相关结论仍保留为未独立验证的收口状态。
  title: 当前任务导航：量化收口审查
navigation: 重点检索 W4A8 packed 参数与 Linear/MoE scheme 构造、加载期别名契约、W4A16 skip 测试、target/draft 通信选择及静态检查结果。已报告修正和检查通过，但 pytest 因缺少 `pytest` 模块未运行；相关结论仍保留为未独立验证的收口状态。
schema: mindie-material-task/1
status: complete
task_id: db5b808d4934d46dcf7ab698f7cdbdcb681ef1f360a915f150016872e3f6ff3c
---

# 当前任务导航：量化收口审查

重点检索 W4A8 packed 参数与 Linear/MoE scheme 构造、加载期别名契约、W4A16 skip 测试、target/draft 通信选择及静态检查结果。已报告修正和检查通过，但 pytest 因缺少 `pytest` 模块未运行；相关结论仍保留为未独立验证的收口状态。

## Materials

- [量化收口与测试状态](blocks/b0a7f62d6648a554d937484ff36cace9fcee2db8d7916dd00bbba1678f608c48.md): 只读审查围绕 W4A16 packed 死分支、W4A8 加载期别名契约，以及 target/draft 通信选择展开；随后报告称已修正 W4A8 packed 标记误传给 Linear scheme 的构造缺陷，使 packed 参数仅用于 MoE，并新增 Linear 构造和 packed MoE canonical 属性回归测试，同时删除 3 个 W4A16 永久 skip 测试。报告称 Ruff、格式检查和 `git diff --check` 通过，但 pytest 未运行，原因是环境缺少 `pytest` 模块；因此这些改动和测试结果应视为报告内容，未验证为成功。

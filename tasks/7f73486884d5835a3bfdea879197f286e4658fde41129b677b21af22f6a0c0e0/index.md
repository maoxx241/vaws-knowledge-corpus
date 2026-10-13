---
blocks:
- block_id: 4af1a2a697a9856fa26b6e7131dcfe934c517401c9695fc8aafd70a1a902b21e
  indexed: true
  sha256: f1134bfa87916e579f42dd07549c013527e0d56941242a6ffdc8aaf51e23e2ca
  source_range:
    end: 1197580
    part: 0
    start: 0
  summary: 审计认为现有 builder 级 UT 已覆盖清零内容，但缺少 `model_runner._dummy_run` 中清零时机与 builder 范围的真实接线回归测试；随后新增了直接调用 `_dummy_run` 的 CPU 测试，覆盖两个 Ascend GDN builder、非 GDN/非 spec decode 哨兵，以及 graph capturing 条件。测试用事件序列验证 metadata build → 所有 GDN builder reset → model forward 的顺序，并断言最终 `num_reqs_padded=3`；未修改 production 文件。`py_compile`、`ruff check`、`ruff format --check` 和 `git diff --check` 通过，但 pytest 因本地 venv 缺少 `partial_json_parser` 等依赖在 collection 阶段阻塞，未执行测试断言。
  title: _dummy_run GDN 清零接线测试
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 7f73486884d5835a3bfdea879197f286e4658fde41129b677b21af22f6a0c0e0
  kind: experience
  material_digest: ea00d6b2f54ca902ea25763c5c37f2beae067226dd36350bc8664d5cf004659e
  revision: b1108571fa9aeee4e01cb054d50c66fa2029e888ef26d96c646b0b52fa8c1b2f
  schema: mindie-entry/3
  summary: 本批材料聚焦 Ascend GDN builder 在 `_dummy_run` 中的 reset 接线、触发条件和顺序验证。已完成测试文件调整并通过静态检查；pytest 仍因环境依赖缺失未完成，不能将测试运行或功能行为视为已验证成功。
  title: 当前任务导航：GDN _dummy_run 清零测试
navigation: 本批材料聚焦 Ascend GDN builder 在 `_dummy_run` 中的 reset 接线、触发条件和顺序验证。已完成测试文件调整并通过静态检查；pytest 仍因环境依赖缺失未完成，不能将测试运行或功能行为视为已验证成功。
schema: mindie-material-task/1
status: complete
task_id: 7f73486884d5835a3bfdea879197f286e4658fde41129b677b21af22f6a0c0e0
---

# 当前任务导航：GDN _dummy_run 清零测试

本批材料聚焦 Ascend GDN builder 在 `_dummy_run` 中的 reset 接线、触发条件和顺序验证。已完成测试文件调整并通过静态检查；pytest 仍因环境依赖缺失未完成，不能将测试运行或功能行为视为已验证成功。

## Materials

- [_dummy_run GDN 清零接线测试](blocks/4af1a2a697a9856fa26b6e7131dcfe934c517401c9695fc8aafd70a1a902b21e.md): 审计认为现有 builder 级 UT 已覆盖清零内容，但缺少 `model_runner._dummy_run` 中清零时机与 builder 范围的真实接线回归测试；随后新增了直接调用 `_dummy_run` 的 CPU 测试，覆盖两个 Ascend GDN builder、非 GDN/非 spec decode 哨兵，以及 graph capturing 条件。测试用事件序列验证 metadata build → 所有 GDN builder reset → model forward 的顺序，并断言最终 `num_reqs_padded=3`；未修改 production 文件。`py_compile`、`ruff check`、`ruff format --check` 和 `git diff --check` 通过，但 pytest 因本地 venv 缺少 `partial_json_parser` 等依赖在 collection 阶段阻塞，未执行测试断言。

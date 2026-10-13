---
blocks:
- block_id: e23bef04fa8697bf121fbfc64b6510b578dd83441c367c0a3f476abe1affc933
  indexed: true
  sha256: b6aa3048a7d8c24769cf211ab10bcd50853a6539169d584be567d65b0c47d052
  source_range:
    end: 669053
    part: 0
    start: 0
  summary: 据报告，新增并调整了 Kimi KDA 与 K3 decoder layer 的单元测试，覆盖 VL 首层、后续层、纯文本场景，以及 FlashComm 开关和 token 分片对齐；未修改生产代码，也未提交。报告称 compileall、ruff/format、codespell、typos 等门禁通过，但目标 pytest 因当前 Python 环境缺少 torch，在加载 tests/ut/conftest.py 时失败，因此测试逻辑未被实际验证。
  title: FlashComm 纯文本与视觉场景测试补充
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 0cf0bd6d46377acfa7928002e5411b8c2b122844bf2eaa297e3fe55dd3ace6ed
  kind: experience
  material_digest: a66537bbd9d4228938598c0af9f85c4072651e3a9541e6e55b0758a50bcbb859
  revision: 921639cddc4d000f0f66a14fb45d5a89e1225c52ab2f4aaed629d55b967ccd93
  schema: mindie-entry/3
  summary: 当前批次记录了纯文本和有图片输入下 FlashComm 开关测试的补充范围、相关 KDA/K3 测试方法及静态检查结果。pytest 受缺少 torch 阻塞，不能将报告的门禁通过或测试设计视为完整测试成功；生产代码未改动，后续仍需在具备依赖的环境中执行目标测试。
  title: FlashComm 精度开关测试索引
navigation: 当前批次记录了纯文本和有图片输入下 FlashComm 开关测试的补充范围、相关 KDA/K3 测试方法及静态检查结果。pytest 受缺少 torch 阻塞，不能将报告的门禁通过或测试设计视为完整测试成功；生产代码未改动，后续仍需在具备依赖的环境中执行目标测试。
schema: mindie-material-task/1
status: complete
task_id: 0cf0bd6d46377acfa7928002e5411b8c2b122844bf2eaa297e3fe55dd3ace6ed
---

# FlashComm 精度开关测试索引

当前批次记录了纯文本和有图片输入下 FlashComm 开关测试的补充范围、相关 KDA/K3 测试方法及静态检查结果。pytest 受缺少 torch 阻塞，不能将报告的门禁通过或测试设计视为完整测试成功；生产代码未改动，后续仍需在具备依赖的环境中执行目标测试。

## Materials

- [FlashComm 纯文本与视觉场景测试补充](blocks/e23bef04fa8697bf121fbfc64b6510b578dd83441c367c0a3f476abe1affc933.md): 据报告，新增并调整了 Kimi KDA 与 K3 decoder layer 的单元测试，覆盖 VL 首层、后续层、纯文本场景，以及 FlashComm 开关和 token 分片对齐；未修改生产代码，也未提交。报告称 compileall、ruff/format、codespell、typos 等门禁通过，但目标 pytest 因当前 Python 环境缺少 torch，在加载 tests/ut/conftest.py 时失败，因此测试逻辑未被实际验证。

---
blocks:
- block_id: 7538894d14ce54c2e4d7192ca73b31c70281aade2bf1b6ccfdf59d9c97bdd0f3
  indexed: true
  sha256: 3dca3819715daeef2a52073c6e4839d2a61e8b7c52b5bc19116ec40d0cfa0608
  source_range:
    end: 782995
    part: 0
    start: 0
  summary: 据报告，已删除真实权重环境测试，精简 SiTU nightly 与 recurrent KDA 覆盖，并增加配置注册幂等/冲突及多项 Parser 契约测试；未修改生产代码或 C++，且未新增 renderer/request-adapter 测试。Ruff format/check、py_compile 和 git diff --check 报告为通过，但本地缺少 pytest、torch、torch_npu、vllm，因此未执行 pytest/NPU 用例；这些结果不应视为完整测试成功。
  title: 测试侧清理与契约测试补强
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: a9c6b3ad550a242572d45c993e2db429cf6930b4418774fb9bd5784b6b8fdf4f
  kind: experience
  material_digest: ba704f91850962de784829eb05ec49f0b4c921d6cdd64f7166f19dde2ec35878
  revision: 4ce22b536972d8f6bfab36976500de2a0badef3f82dc7262556440fd58b62cb1
  schema: mindie-entry/3
  summary: 当前索引涵盖测试文件清理、SiTU nightly 和 recurrent KDA 精简、配置注册及 Parser 契约测试补强。已报告的静态检查通过，但 pytest/NPU 用例因环境缺少 pytest、torch、torch_npu、vllm 未执行；后续检索应同时关注这些未验证项及任何后续修正。
  title: 测试清理、契约补强与验证限制
navigation: 当前索引涵盖测试文件清理、SiTU nightly 和 recurrent KDA 精简、配置注册及 Parser 契约测试补强。已报告的静态检查通过，但 pytest/NPU 用例因环境缺少 pytest、torch、torch_npu、vllm 未执行；后续检索应同时关注这些未验证项及任何后续修正。
schema: mindie-material-task/1
status: complete
task_id: a9c6b3ad550a242572d45c993e2db429cf6930b4418774fb9bd5784b6b8fdf4f
---

# 测试清理、契约补强与验证限制

当前索引涵盖测试文件清理、SiTU nightly 和 recurrent KDA 精简、配置注册及 Parser 契约测试补强。已报告的静态检查通过，但 pytest/NPU 用例因环境缺少 pytest、torch、torch_npu、vllm 未执行；后续检索应同时关注这些未验证项及任何后续修正。

## Materials

- [测试侧清理与契约测试补强](blocks/7538894d14ce54c2e4d7192ca73b31c70281aade2bf1b6ccfdf59d9c97bdd0f3.md): 据报告，已删除真实权重环境测试，精简 SiTU nightly 与 recurrent KDA 覆盖，并增加配置注册幂等/冲突及多项 Parser 契约测试；未修改生产代码或 C++，且未新增 renderer/request-adapter 测试。Ruff format/check、py_compile 和 git diff --check 报告为通过，但本地缺少 pytest、torch、torch_npu、vllm，因此未执行 pytest/NPU 用例；这些结果不应视为完整测试成功。

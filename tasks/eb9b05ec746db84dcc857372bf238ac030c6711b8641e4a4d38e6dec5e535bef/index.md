---
blocks:
- block_id: 4922cf321d02eb65ddce3dc3ccc9860c8fcdfc256ca0e124cf1fe0a4729d3b95
  indexed: true
  sha256: 5e84d981c31c4b3458514fa07b2ff95b2da48fadfaeac558a5f1a4fd77f3e18f
  source_range:
    end: 756190
    part: 0
    start: 0
  summary: 只读审计认为公共 runtime 路径未新增进入 FC1 或共享专家主路径的 Kimi/model-name 分支；activation、quant、shape/config contract 分流方向基本正确。待处理项包括收窄 ModelSlim 缺失权重的 unquantized 回退、将 renderer 的 model_type 判断改为 renderer/protocol capability、在 AscendKimiGatedDeltaNetAttention 中无条件安装 shape-aware A_log loader，以及移除重复注册。必须保留 ModelRegistry architecture key、model_type/checkpoint ABI、parser 分流和 reasoning/tool/renderer registry 标识；当前结论是审计建议，不是已验证成功。
  title: Kimi 模型耦合只读审计与待处理项
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: eb9b05ec746db84dcc857372bf238ac030c6711b8641e4a4d38e6dec5e535bef
  kind: experience
  material_digest: 880e9cc08a3394715f8f436a70adfafa22a1a41ab6fc5d3882aae004abaf6ccc
  revision: e242f5ff72d3a263bc6e047ed39a6f4001fca057a6932e816800dc9adc48cdcd
  schema: mindie-entry/3
  summary: 本批次聚焦公共 runtime 中模型名耦合的只读审计与修正边界。已识别若干方向正确的并发改动，同时保留三项待处理问题、重复注册问题及未验证的 shape-aware loader 建议；原始块仍可独立检索。
  title: 当前任务导航：Kimi 模型耦合审计
navigation: 本批次聚焦公共 runtime 中模型名耦合的只读审计与修正边界。已识别若干方向正确的并发改动，同时保留三项待处理问题、重复注册问题及未验证的 shape-aware loader 建议；原始块仍可独立检索。
schema: mindie-material-task/1
status: complete
task_id: eb9b05ec746db84dcc857372bf238ac030c6711b8641e4a4d38e6dec5e535bef
---

# 当前任务导航：Kimi 模型耦合审计

本批次聚焦公共 runtime 中模型名耦合的只读审计与修正边界。已识别若干方向正确的并发改动，同时保留三项待处理问题、重复注册问题及未验证的 shape-aware loader 建议；原始块仍可独立检索。

## Materials

- [Kimi 模型耦合只读审计与待处理项](blocks/4922cf321d02eb65ddce3dc3ccc9860c8fcdfc256ca0e124cf1fe0a4729d3b95.md): 只读审计认为公共 runtime 路径未新增进入 FC1 或共享专家主路径的 Kimi/model-name 分支；activation、quant、shape/config contract 分流方向基本正确。待处理项包括收窄 ModelSlim 缺失权重的 unquantized 回退、将 renderer 的 model_type 判断改为 renderer/protocol capability、在 AscendKimiGatedDeltaNetAttention 中无条件安装 shape-aware A_log loader，以及移除重复注册。必须保留 ModelRegistry architecture key、model_type/checkpoint ABI、parser 分流和 reasoning/tool/renderer registry 标识；当前结论是审计建议，不是已验证成功。

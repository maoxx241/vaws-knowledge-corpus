---
blocks:
- block_id: e83b674e35e7ace2dde9b1e840a851d135674ff2900fdcbde5e613e07c694817
  indexed: true
  sha256: 18f6dd67a9007ec904fc0a6692d60e03a576b629c20e3bb0f210e11317159c7f
  source_range:
    end: 584717
    part: 0
    start: 0
  summary: 据该段记录，0724 版 `encoding_k3.py` 的变化被判断为历史 assistant 消息 `<think>` 渲染语义变化，而非权重张量、量化或 MoE 处理逻辑变化；`ci-new` 线上 renderer reportedly 会委托 tokenizer 的 `apply_chat_template`，但真实 tokenizer 测试仍写死旧 SHA256（`c386…`），新文件为 `b9cb…`。随后在共享存储原 multicard 同路径下创建了隔离目录：多数条目软链原目录，仅替换新的 `encoding_k3.py`，记录称 367 个软链无断链且原目录未改动；这些是该记录中的检查与报告，未在此索引中独立验证。记录还认为 SHA 校验用于固定 `trust_remote_code=True` 加载的 tokenizer 快照，但可能混淆版本锁定与行为测试，并建议改用显式行为测试或独立 manifest。
  title: 0724 encoding_k3 更新与 multicard 替换
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 09d86907e0a42e3dc2ac3c1c55e1e5829ada0a3372bdf859d316d6353dab1927
  kind: experience
  material_digest: 6f01d60f8adc064aaf36a26f9d6565d15d8b22010f6d14068073873f88b47d32
  revision: 413975984926968133e91ebded6957c4957a79caf7699f613648d0c1d4cf73da
  schema: mindie-entry/3
  summary: 本批仅包含一个材料块，聚焦共享 multicard 目录中的 `encoding_k3.py` 更新、历史 `<think>` 渲染语义、旧 SHA256 固定校验及其对真实 tokenizer 测试的影响。记录中的目录创建、跨节点可见性、软链完整性和 hash 检查均属于报告的操作结果，原文未提供独立复核；后续应区分已报告的环境变更、尚未修改的测试代码，以及建议中的行为测试/manifest 调整。原始材料块仍可独立检索，未据此补全被省略的早期上下文。
  title: 当前任务导航：0724 tokenizer 变更与测试契约
navigation: 本批仅包含一个材料块，聚焦共享 multicard 目录中的 `encoding_k3.py` 更新、历史 `<think>` 渲染语义、旧 SHA256 固定校验及其对真实 tokenizer 测试的影响。记录中的目录创建、跨节点可见性、软链完整性和 hash 检查均属于报告的操作结果，原文未提供独立复核；后续应区分已报告的环境变更、尚未修改的测试代码，以及建议中的行为测试/manifest 调整。原始材料块仍可独立检索，未据此补全被省略的早期上下文。
schema: mindie-material-task/1
status: complete
task_id: 09d86907e0a42e3dc2ac3c1c55e1e5829ada0a3372bdf859d316d6353dab1927
---

# 当前任务导航：0724 tokenizer 变更与测试契约

本批仅包含一个材料块，聚焦共享 multicard 目录中的 `encoding_k3.py` 更新、历史 `<think>` 渲染语义、旧 SHA256 固定校验及其对真实 tokenizer 测试的影响。记录中的目录创建、跨节点可见性、软链完整性和 hash 检查均属于报告的操作结果，原文未提供独立复核；后续应区分已报告的环境变更、尚未修改的测试代码，以及建议中的行为测试/manifest 调整。原始材料块仍可独立检索，未据此补全被省略的早期上下文。

## Materials

- [0724 encoding_k3 更新与 multicard 替换](blocks/e83b674e35e7ace2dde9b1e840a851d135674ff2900fdcbde5e613e07c694817.md): 据该段记录，0724 版 `encoding_k3.py` 的变化被判断为历史 assistant 消息 `<think>` 渲染语义变化，而非权重张量、量化或 MoE 处理逻辑变化；`ci-new` 线上 renderer reportedly 会委托 tokenizer 的 `apply_chat_template`，但真实 tokenizer 测试仍写死旧 SHA256（`c386…`），新文件为 `b9cb…`。随后在共享存储原 multicard 同路径下创建了隔离目录：多数条目软链原目录，仅替换新的 `encoding_k3.py`，记录称 367 个软链无断链且原目录未改动；这些是该记录中的检查与报告，未在此索引中独立验证。记录还认为 SHA 校验用于固定 `trust_remote_code=True` 加载的 tokenizer 快照，但可能混淆版本锁定与行为测试，并建议改用显式行为测试或独立 manifest。

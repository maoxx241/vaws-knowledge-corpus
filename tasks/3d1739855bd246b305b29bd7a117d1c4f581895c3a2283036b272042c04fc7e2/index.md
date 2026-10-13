---
blocks:
- block_id: 5d453863574053bc3ff59dbb71855dbe4cad2196004be5ad866cada30a4507d1
  indexed: true
  sha256: 363cfbf2bf1246f2428df5e601a0d58f447a49b32e40afc42a52c0e32755e994
  source_range:
    end: 619442
    part: 0
    start: 0
  summary: 只读审计报告称四机服务健康，并记录了 rank0–rank3 的 serve PID/PGID；建议切换时仅向对应负 PGID 发送 TERM。当前运行的是旧服务构建与 text-only 配置，含 `--language-model-only` 和 `{"image":0}`，因此不能作为 ViT 证明。后续建议使用全编译产物 `a7491446` 与 pinned vLLM `v0.26.0-568afb3a`，移除旧 core、删除该语言模型专用参数，并采用指定多模态、缓存、显存和并行配置；这些仍是启动建议而非已验证成功。
  title: 四机服务审计与 ViT 启动建议
entry:
  conditions: {}
  domain: vllm-ascend
  entry_id: 3d1739855bd246b305b29bd7a117d1c4f581895c3a2283036b272042c04fc7e2
  kind: experience
  material_digest: c7aec510be3c877621617b9b65c07e88837bc1bc0fbe6967d412cf095716a468
  revision: 52467e284f8f397c1ac369296dcc73ca1f044ddbc90206300fed67814e7b3396
  schema: mindie-entry/3
  summary: 已索引一份关于四机服务只读审计、现有 text-only 配置限制及 ViT 启动建议的材料。相关 PID/PGID、构建版本、vLLM 版本和多模态参数可据此检索；建议配置与 K3 ViT 在 TP16 下的回退判断尚未被该材料证明为已成功运行。
  title: 当前任务导航
navigation: 已索引一份关于四机服务只读审计、现有 text-only 配置限制及 ViT 启动建议的材料。相关 PID/PGID、构建版本、vLLM 版本和多模态参数可据此检索；建议配置与 K3 ViT 在 TP16 下的回退判断尚未被该材料证明为已成功运行。
schema: mindie-material-task/1
status: complete
task_id: 3d1739855bd246b305b29bd7a117d1c4f581895c3a2283036b272042c04fc7e2
---

# 当前任务导航

已索引一份关于四机服务只读审计、现有 text-only 配置限制及 ViT 启动建议的材料。相关 PID/PGID、构建版本、vLLM 版本和多模态参数可据此检索；建议配置与 K3 ViT 在 TP16 下的回退判断尚未被该材料证明为已成功运行。

## Materials

- [四机服务审计与 ViT 启动建议](blocks/5d453863574053bc3ff59dbb71855dbe4cad2196004be5ad866cada30a4507d1.md): 只读审计报告称四机服务健康，并记录了 rank0–rank3 的 serve PID/PGID；建议切换时仅向对应负 PGID 发送 TERM。当前运行的是旧服务构建与 text-only 配置，含 `--language-model-only` 和 `{"image":0}`，因此不能作为 ViT 证明。后续建议使用全编译产物 `a7491446` 与 pinned vLLM `v0.26.0-568afb3a`，移除旧 core、删除该语言模型专用参数，并采用指定多模态、缓存、显存和并行配置；这些仍是启动建议而非已验证成功。

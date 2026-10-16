<p align="center">
  <img src="assets/brand/banner.svg" alt="MindIE Knowledge — vLLM-Ascend Corpus" width="900">
</p>

# MindIE Knowledge — vLLM-Ascend 公共工程知识库

中文 | [English](docs/README.en.md)

**面向 `vLLM-Ascend` 与昇腾推理工程的公共 Markdown 任务知识包与有用性反馈语料库**

本仓库是 [MindIE Agent](https://github.com/mindie-agent/mindie-agent) 在 `vLLM` / `vLLM-Ascend` 领域的官方公共知识分发源（Canonical Feed），由共享知识运行时 [`mindie-agent/knowledge`](https://github.com/mindie-agent/knowledge) 负责同步、本地索引、外发打包与独立反馈记录。

---

## 核心特性

- **真实完整的工程材料（Complete Redacted Material）**：每个任务包保留经确定性规则与 Gitleaks 严格脱敏后的有序公开对话分块（`blocks/<block_id>.md`），包含排障过程、失败尝试、修正结论与适用条件，绝不用大模型摘要替代原始正文。
- **结构化检索导航（`index.md`）**：每个任务包提供 `mindie-material-task/1` 清单（内嵌 `mindie-entry/3` 元数据头）、分块标题/摘要与短导航，方便 Agent 先检索全貌、再按需读取具体分块。
- **不可变哈希绑定与闭锁式脱敏**：任务与分块均采用 64 位小写十六进制标识，清单严格绑定每个分块文件的 SHA-256 摘要；任何未通过 `publication-contract.json` 校验或脱敏门禁的内容均被拒绝。
- **独立有用性反馈（`feedback/*.json`）**：消费方 Agent 在实际参考某条知识或经验后，可针对具体观测修订提交可选的 `up` / `down` 反馈（`mindie-feedback/1`），辅助语料演进与维护。

---

## 目录结构

```text
knowledge-vllm-ascend/
├── publication-contract.json      # 内容 Schema、校验器版本与审核机器人契约声明
├── tasks/
│   └── <64-hex-task-id>/          # 单个完整任务知识包
│       ├── index.md               # 任务清单 (mindie-material-task/1 + mindie-entry/3)
│       ├── blocks/
│       │   └── <64-hex-block-id>.md  # 不可变脱敏正文分块 (mindie-material-block/1)
│       └── feedback/              # 可选的独立有用性反馈记录 (mindie-feedback/1)
│           └── <feedback-id>.json
└── docs/
    ├── README.en.md               # 英文仓库主页 (English Homepage)
    ├── CONTRIBUTING.en.md         # 英文贡献指南 (English Contributing Guide)
    ├── package-format.md          # 任务知识包格式规范（中文）
    └── package-format.en.md       # 任务知识包格式规范（英文）
```

> **布局契约说明**：为兼容已发布的客户端同步校验器（`mindie-knowledge`），本仓库根目录仅放置标准元数据文件（`README.md`、`CONTRIBUTING.md`、`AGENTS.md`、`LICENSE` 等），英文版主页与英文版贡献指南统一位于 [`docs/README.en.md`](docs/README.en.md) 与 [`docs/CONTRIBUTING.en.md`](docs/CONTRIBUTING.en.md)。

---

## 如何使用

### 1. 在 MindIE Agent 插件中自动使用（推荐）

安装 [`mindie-agent-codex`](https://github.com/mindie-agent/mindie-agent-codex) 或 [`mindie-agent-cc`](https://github.com/mindie-agent/mindie-agent-cc) 后，插件会自动从本仓库同步最新知识包并在本地构建 BM25 与引用检索索引，无需手动克隆本仓库。

### 2. 使用 `mindie-knowledge` CLI 独立同步与检索

```bash
# 同步最新 vLLM-Ascend 公共知识包到本地缓存
mindie-knowledge sync --domain vllm-ascend

# 检索相关工程经验与知识包
mindie-knowledge query --domain vllm-ascend --query "AscendHccl 通信超时或算子精度排查"
```

---

## 文档与贡献指南

- **任务知识包格式规范**：[中文（`docs/package-format.md`）](docs/package-format.md) · [English (`docs/package-format.en.md`)](docs/package-format.en.md)
- **贡献指南**：[中文（`CONTRIBUTING.md`）](CONTRIBUTING.md) · [English (`docs/CONTRIBUTING.en.md`)](docs/CONTRIBUTING.en.md)
- **审核机器人契约**：[`mindie-content-review/2`](https://github.com/mindie-agent/knowledge/blob/main/docs/repository-bot-contract.md)
- **核心设计原则**：[MindIE Agent 九条设计原则](https://github.com/mindie-agent/mindie-agent/blob/main/docs/design-principles.md)

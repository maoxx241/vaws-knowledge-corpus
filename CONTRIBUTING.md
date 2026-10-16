# 参与贡献（Contributing to `knowledge-vllm-ascend`）

**中文** | [English](docs/CONTRIBUTING.en.md) · [返回仓库首页](README.md)

欢迎为 `knowledge-vllm-ascend` 贡献昇腾推理工程与 `vLLM-Ascend` 排障经验！为了让每一份公开材料都**真实可靠、无隐私泄露、且能被所有客户端稳定检索**，本仓库实行自动化格式校验、双层隐私脱敏与分层审核机制。

---

## 1. 两种贡献方式

### 方式一：通过 MindIE Agent 自动沉淀与提交（最省心 ✨）

在日常使用 [`mindie-agent-cc`](https://github.com/mindie-agent/mindie-agent-cc)、[`mindie-agent-codex`](https://github.com/mindie-agent/mindie-agent-codex) 或 [`mindie-agent-kimi`](https://github.com/mindie-agent/mindie-agent-kimi) 时，如果你开启了**社区共享（Opt-in）**，你无需手动编写任何 YAML 或 Markdown 文件。

当你完成一个工程任务后，本地 [`mindie-knowledge`](https://github.com/mindie-agent/knowledge) 引擎会自动在后台完成：
1. **提取公开可见对话**：只保留用户输入与助手可见回复，自动剔除工具调用噪音、隐藏推理链与系统提示词；
2. **双重确定性脱敏**：使用固定版本 Gitleaks 与 `mindie_knowledge.redact`（规则集 `r3`）清除敏感信息，绝不用大模型改写你的技术正文；
3. **生成导航与提交 PR**：调用轻量级模型生成分块标题与 `index.md` 导航大纲，打包为 `mindie-contribution/1` 批次并自动向本仓库发起 Pull Request。

### 方式二：手动修订、勘误或申请撤下（Pull Request）

如果你发现已有知识包需要补充后续观察、修正导航摘要，或需要撤下某个不再适用的任务包，可以直接提交 Pull Request：
- **补充或修正分块**：`blocks/<block_id>.md` 的正文一旦发布即不可原地修改。如需修改分块正文（例如补充脱敏或修正命令），请生成新的 `<64位十六进制block_id>.md` 文件、删除被替换的旧分块文件，并同步更新 `index.md` 中的 `sha256`、分块列表与任务 `revision`。
- **仅修改元数据/导航**：如果只修正 `index.md` 中的标题、摘要或适用条件，而不改动 `blocks/*.md` 的字节内容，可保持原有 `block_id` 不变。
- **撤下整个任务包（Withdrawal）**：直接删除整个 `tasks/<task_id>/` 目录并在 PR 描述中说明撤下原因。合入后，客户端执行 `mindie-knowledge sync` 时会自动将该条目标记为已撤下。

---

## 2. 格式与隐私红线（CI 强制检查）

每个 PR 都会由 GitHub Actions（`publication-head` 检查）在零信任模式下把候选提交当作纯数据 Git Blob 进行流式校验：

1. **目录拓扑与完整性**：
   - 每个任务知识包必须位于 `tasks/<64位小写十六进制task_id>/` 下，包含 `index.md`（`mindie-material-task/1`）和对应的 `blocks/<64位小写十六进制block_id>.md`（`mindie-material-block/1`）。
   - `index.md` 声明的分块集合必须与 `blocks/` 目录下的实际文件完全一致，不允许存在未引用的孤立文件，且每个分块的 `sha256` 必须精确匹配。详见 [《任务知识包格式规范》](docs/package-format.md)。
2. **零敏感数据（Fail-Closed Redaction）**：
   - **PR 标题、PR 描述正文以及所有提交文件**都会经过 `mindie_knowledge.redact`（`r3`）扫描。
   - 严禁包含：API 密钥、Token、私钥片段、内网 IPv4/IPv6 地址、编号机器名（如带槽位号的内部服务器名）、`<user>@<host>` 登录串、个人主目录路径（请用 `/home/<user>/...` 占位符代替）或内部工单号。
   - 提交前可在本地运行脱敏自检：
     ```bash
     python3 -m mindie_knowledge.redact --check README.md CONTRIBUTING.md docs
     ```
3. **根目录白名单约束**：
   - 仓库根目录仅允许 `publication-contract.json`、`README.md`、`CONTRIBUTING.md`、`AGENTS.md`、`LICENSE` 等标准元数据文件；英文文档与规范文档统一放在 `docs/` 下。

---

## 3. 内容 PR 与工程开发 PR 的审核分工

本仓库的自动化校验器会自动识别 PR 修改的文件范围，并分流到两条不同的审核路径：

| PR 类型 | 涉及路径 | `publication-head` 结论 | 合入方式 |
| --- | --- | --- | --- |
| **普通内容贡献 (`content`)** | 仅修改 `tasks/**` 或 `feedback/**` | `success` | 通过格式、哈希与脱敏检查后，由仓库审核 Bot（遵循 [`mindie-content-review/2`](https://github.com/mindie-agent/knowledge/blob/main/docs/repository-bot-contract.zh.md)）复核并自动合入 |
| **工程与文档变更 (`development`)** | 修改 `README.md`、`CONTRIBUTING.md`、`docs/**`、`.github/**` 或 `publication-contract.json` | `neutral` | **禁止 Bot 自动合入**；必须在 PR 描述中附带 [工程设计原则审查记录](https://github.com/mindie-agent/mindie-agent/blob/main/docs/design-principles.md)，由维护者人工审查合入 |

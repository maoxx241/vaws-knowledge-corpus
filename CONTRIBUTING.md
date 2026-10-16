# 参与贡献（Contributing to `knowledge-vllm-ascend`）

中文 | [English](docs/CONTRIBUTING.en.md) · [返回首页](README.md)

感谢为 `vLLM-Ascend` 公共工程知识库贡献经验与修正！本仓库遵循确定性格式校验、隐私脱敏门禁与 `mindie-content-review/2` 审核协议。

---

## 1. 贡献方式

1. **通过 MindIE Agent 自动外发（推荐）**：在开启社区共享的 [`mindie-agent-codex`](https://github.com/mindie-agent/mindie-agent-codex) 或 [`mindie-agent-cc`](https://github.com/mindie-agent/mindie-agent-cc) 会话中完成工程任务后，本地 `mindie-knowledge` 运行时会自动完成公开对话筛选、Gitleaks + `r3` 规则脱敏、分块导航索引，并向本仓库提交符合 `mindie-contribution/1` 规范的 Pull Request。
2. **手动勘误、修订或撤下 PR**：维护者或贡献者可通过 Pull Request 修正元数据、补充新的观测分块，或通过删除整个 `tasks/<task_id>/` 目录撤下不恰当的任务包（PR 描述需说明撤下原因）。

---

## 2. 必须遵守的格式与安全规则

- **拓扑完整性**：每个任务包必须位于 `tasks/<64位小写十六进制ID>/`，包含 `index.md`（`mindie-material-task/1`）与 `blocks/<64位小写十六进制ID>.md`（`mindie-material-block/1`）。任务目录下不得包含未被 `index.md` 引用的孤立文件。详见 [任务知识包格式规范](docs/package-format.md)。
- **分块不可变性**：`blocks/<block_id>.md` 的正文字节在其标识下不可变；若需修改分块正文，必须生成新的分块 ID 并重新计算 `index.md` 清单哈希。
- **严禁包含敏感或私有信息**：PR 标题、PR 正文以及所有提交文件必须通过 `mindie_knowledge.redact`（规则集 `r3`）与 Gitleaks 检查，严禁出现凭据、私钥、内网地址、SSH 登录端点、本地个人主目录路径或未脱敏的原始会话记录。
- **内容发布与工程变更分离**：
  - 仅修改 `tasks/` 下知识包或反馈的 PR 属于普通内容发布（`content` 模式），通过 `publication-head` 与 Grok Bot 审核后合入。
  - 修改仓库文档（`README.md`、`CONTRIBUTING.md`、`docs/**`）、工作流或配置的 PR 属于工程开发变更（`development` 模式），必须遵循 [MindIE Agent 九条设计原则审查](https://github.com/mindie-agent/mindie-agent/blob/main/docs/design-principles.md) 并由维护者人工合入。

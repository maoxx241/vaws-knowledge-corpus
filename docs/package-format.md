# 任务知识包格式规范（Package Format Specification）

中文 | [English](package-format.en.md) · [返回首页](../README.md)

本文定义 `knowledge-vllm-ascend` 仓库中公开任务知识包（`tasks/<task_id>/`）的目录拓扑、YAML Frontmatter Schema 与校验规则，对应 [`publication-contract.json`](../publication-contract.json) 中的当前契约声明。

---

## 1. 目录与文件拓扑

每个公开任务包位于 `tasks/<task_id>/` 下，其中 `<task_id>` 为 64 位小写十六进制字符串：

```text
tasks/<64-hex-task-id>/
├── index.md                        # 任务级清单 (format: mindie-material-task/1)
├── blocks/
│   ├── <64-hex-block-id-1>.md      # 第 1 个有序正文分块 (format: mindie-material-block/1)
│   └── <64-hex-block-id-2>.md      # 第 2 个有序正文分块 (format: mindie-material-block/1)
└── feedback/                       # 可选目录：独立有用性反馈
    └── <feedback-id>.json          # 单条反馈记录 (format: mindie-feedback/1)
```

- **白名单顶层路径**：仓库根目录仅允许 `publication-contract.json`、标准文档元数据文件（`README.md`、`CONTRIBUTING.md`、`AGENTS.md`、`LICENSE` 等）、`docs/**` 与 `tasks/<64-hex-task-id>/**`。
- **无孤立文件**：`tasks/<task_id>/blocks/` 下的所有文件必须与 `index.md` 的 `blocks` 数组严格一一对应。

---

## 2. 任务清单（`index.md`）

`index.md` 使用 YAML Frontmatter 声明 `mindie-material-task/1` 清单及内嵌的 `mindie-entry/3` 头部信息，正文区域为当前任务的整体检索导航摘要：

- `format`: 固定为 `"mindie-material-task/1"`
- `task_id`: 与父目录名一致的 64 位小写十六进制任务标识
- `status`: 索引状态（如 `"indexed"`），仅表示材料索引进度，不代表业务结论的真理性认证
- `entry`: 内嵌 `mindie-entry/3` 元数据（包含 `id`、`domain`、`kind`、`title`、`summary`、`keywords`、可选 `conditions` 等）
- `blocks`: 有序分块描述符列表，每项记录对应分块的 `block_id`、`path`、`sha256`、`seq`、`source_range`、`title`、`summary` 与 `index_state`

---

## 3. 正文分块（`blocks/<block_id>.md`）

每个分块文件保存一段经确定性投影与隐私脱敏后的有序公开对话：

- `format`: 固定为 `"mindie-material-block/1"`
- `task_id`: 所属任务的 64 位十六进制 ID
- `block_id`: 与文件名一致的 64 位十六进制分块 ID
- `seq`: 从 1 开始递增的连续整数序号
- `source_range`: 原始公开消息序号范围
- **不可变性**：分块正文一经发布不可原地修改；若勘误需要修改正文字节，必须生成新的 `block_id` 文件并更新 `index.md` 中的 `sha256` 与引用。

---

## 4. 独立有用性反馈（`feedback/*.json`）

消费方任务在实际参考某个任务包后，可在 `tasks/<task_id>/feedback/` 下提交 JSON 格式的反馈记录：

- `format`: 固定为 `"mindie-feedback/1"`
- 绑定精确的观测修订（`observed_revision`）、投票方向（`up` 或 `down`）与可选的单行脱敏原因说明。

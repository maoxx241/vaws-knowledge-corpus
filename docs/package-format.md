# 任务知识包格式规范（Package Format Specification）

**中文** | [English](package-format.en.md) · [返回仓库首页](../README.md)

本文用清晰直观的方式说明 `knowledge-vllm-ascend` 仓库中公开任务知识包（`tasks/<task_id>/`）与有用性反馈（`feedback/*.json`）的目录结构、YAML Frontmatter 字段定义及校验规则。所有格式版本均由仓库根目录的 [`publication-contract.json`](../publication-contract.json) 统一声明。

---

## 1. 为什么采用“目录清单 + 不可变分块”设计？

在 AI Agent 排障场景中，一次完整的昇腾推理调试对话往往包含几万甚至几十万字节的输出。
- 如果把整段对话塞进单个大文件，Agent 每次检索都要读取整篇长文，瞬间撑爆上下文窗口；
- 如果只保留一段大模型总结的摘要，又会丢失最关键的算子报错栈、环境变量设置和失败对比实验。

因此，我们将每个任务拆分为**两层结构**：
1. **`index.md`（轻量级任务导航清单）**：包含任务级摘要与每个分块的简短目录，供 Agent 在检索时快速浏览全貌；
2. **`blocks/<block_id>.md`（无损脱敏原文分块）**：按最多 16 KiB 切分的有序公开对话原文，供 Agent 按需精确读取某个具体步骤。

---

## 2. 目录与文件拓扑

每个公开任务知识包位于 `tasks/<task_id>/` 下，其中 `<task_id>` 是 64 位小写十六进制字符串：

```text
knowledge-vllm-ascend/
├── publication-contract.json                # 仓库级发布与校验契约 (mindie-publication-contract/1)
├── tasks/
│   └── <64-hex-task-id>/                    # 单个完整任务知识包目录
│       ├── index.md                         # 任务导航清单 (schema: mindie-material-task/1)
│       └── blocks/
│           ├── <64-hex-block-id-1>.md       # 第 1 个不可变原文分块 (schema: mindie-material-block/1)
│           └── <64-hex-block-id-2>.md       # 第 2 个不可变原文分块 (schema: mindie-material-block/1)
└── feedback/
    └── votes-<id>.json                      # 独立有用性反馈批次文件 (schema: mindie-feedback/1)
```

### 校验器强约束（`mindie_knowledge.publication_check`）

- **顶层路径白名单**：仓库仅允许 `publication-contract.json`、标准根元数据文件（`README.md`、`CONTRIBUTING.md`、`AGENTS.md`、`LICENSE`）、`docs/**`、`tasks/<64-hex-task-id>/**` 与 `feedback/*.json`。任何已退役的历史目录名（如 `cases/`、`topics/`、`corpus/`）均会被直接拒绝。
- **集合严格一致（无孤立文件）**：`tasks/<task_id>/blocks/` 下的实际文件集合必须与 `index.md` 中 `blocks` 列表声明的 `block_id` 集合完全一致，既不能缺失文件，也不能多出未被清单引用的文件。
- **纯数据文件模式**：所有公开文件必须是普通非可执行 Git Blob（模式 `100644`），编码为标准 UTF-8 且使用 LF（`\n`）换行符。

---

## 3. 任务清单格式（`tasks/<task_id>/index.md`）

`index.md` 由顶部的 YAML Frontmatter 和底部的 Markdown 导航正文组成。

### 字段说明

| 字段路径 | 类型 | 说明 |
| --- | --- | --- |
| `schema` | `string` | 固定为 `"mindie-material-task/1"` |
| `task_id` | `string` | 64 位小写十六进制任务 ID，必须与父目录名 `<task_id>` 完全一致 |
| `status` | `string` | 固定为 `"complete"`（仅表示本知识包的所有分块均已完成检索索引，**不代表对业务结论的权威认证**） |
| `navigation` | `string` | 当前任务的整体检索导航摘要，帮助 Agent 判断是否需要展开具体分块 |
| `entry` | `object` | 内嵌的 `mindie-entry/3` 检索头部元数据（见下表） |
| `entry.schema` | `string` | 固定为 `"mindie-entry/3"` |
| `entry.domain` | `string` | 领域标识，本仓库固定为 `"vllm-ascend"` |
| `entry.entry_id` | `string` | 与 `task_id` 一致的 64 位小写十六进制 ID |
| `entry.kind` | `string` | 条目类型（如 `"experience"` 或 `"knowledge"`） |
| `entry.title` | `string` | 任务标题 |
| `entry.summary` | `string` | 任务级检索摘要 |
| `entry.conditions` | `object` | 观测到的软件版本、硬件或环境条件键值映射（如 `torch_npu_version`），仅描述观测背景，不代表普适条件 |
| `entry.material_digest` | `string` | 绑定当前全部分块内容的 64 位十六进制 SHA-256 摘要 |
| `entry.revision` | `string` | 当前任务包修订号（64 位十六进制 SHA-256），供有用性反馈精确绑定 |
| `blocks` | `list` | 有序分块描述符列表，每项包含 `block_id`、`indexed`（公开库中必须为 `true`）、`sha256`、`source_range`、`title` 与 `summary` |

---

## 4. 原文分块格式（`tasks/<task_id>/blocks/<block_id>.md`）

每个分块文件保存一段经确定性公开消息提取与双层隐私脱敏后的原始对话或实验记录（单块上限 16 KiB）：

```markdown
---
block_id: 89bafb72f7ba1dc374e81c1f4c302ab4f20d7fbb1edc0afe568356190bc89f99
schema: mindie-material-block/1
source_range:
  body_end: 2503
  body_start: 0
---

这里是经确定性脱敏后的公开对话或实机验证正文……
```

- **顶层 Frontmatter 仅含三个字段**：`schema`（固定为 `"mindie-material-block/1"`）、`block_id`（必须与文件名 `<block_id>.md` 一致）以及 `source_range`（记录原始字节区间或来源偏移）。
- **分块不可变性（Immutability）**：在同一个 `block_id` 下，分块文件的字节内容绝对不可原地修改（CI 会拦截对已有 `tasks/<id>/blocks/<block_id>.md` 的 `M` 修改状态）。如果需要勘误或补充脱敏，必须生成新的 `block_id` 文件、移除旧文件，并更新 `index.md`。

---

## 5. 独立有用性反馈格式（`feedback/votes-<id>.json`）

当消费端 Agent 在实机任务中参考了某个知识包后，可以通过 `knowledge_feedback` 记录 `up`（有用）或 `down`（无用/过时）评价。开启社区共享后，这些评价会打包为 `feedback/votes-<id>.json` 发布：

```json
{
  "schema": "mindie-feedback/1",
  "votes": [
    {
      "vote_id": "1cc64dc5135c586faa95be0761c20ced6e8318fa8d3b9b174741d041c6f1f4aa",
      "root_id": "5459766cc74b745fc5069b1042dc25ab7c34db62afc3b339a319c22f7227a1e5",
      "entry_id": "c3f998fdce9eecfbf1171624d6f791d4fc8e4e1b2bf7636f59dfa05e37327815",
      "revision": "937d2219f062ff5b1992faf75d885e68736497a0228032136f29cabc1cfc060b",
      "rating": "up",
      "reason": "单行脱敏原因说明（可选，最多 1000 字符）"
    }
  ]
}
```

- **版本绑定**：`revision` 必须是 64 位 SHA-256 摘要，确保针对旧版本内容的评价不会误挂到后续修订版本上。
- **匿名防重**：`root_id` 是不透明的随机匿名根标识（绝不暴露本地真实会话 ID），同一 `(root_id, entry_id, revision)` 三元组只保留最新一票，不会重复刷票。

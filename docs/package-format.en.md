# Task Knowledge Package Format Specification

[中文](package-format.md) | English · [Back to Home](README.en.md)

This document specifies the directory topology, YAML frontmatter schemas, and validation rules for public task knowledge packages (`tasks/<task_id>/`) in `knowledge-vllm-ascend`, governed by [`publication-contract.json`](../publication-contract.json).

---

## 1. Directory & File Topology

Every public task package resides under `tasks/<task_id>/`, where `<task_id>` is a 64-character lowercase hexadecimal string:

```text
tasks/<64-hex-task-id>/
├── index.md                        # Task manifest (format: mindie-material-task/1)
├── blocks/
│   ├── <64-hex-block-id-1>.md      # Ordered material block 1 (format: mindie-material-block/1)
│   └── <64-hex-block-id-2>.md      # Ordered material block 2 (format: mindie-material-block/1)
└── feedback/                       # Optional directory: independent usefulness feedback
    └── <feedback-id>.json          # Feedback record (format: mindie-feedback/1)
```

- **Allowed Repository Paths**: At the repository root, only `publication-contract.json`, standard metadata files (`README.md`, `CONTRIBUTING.md`, `AGENTS.md`, `LICENSE`), `docs/**`, and `tasks/<64-hex-task-id>/**` are permitted by the canonical feed validator.
- **No Unreferenced Files**: Every file under `tasks/<task_id>/blocks/` must be explicitly listed in the `blocks` array of `index.md` with a matching SHA-256 digest.

---

## 2. Task Manifest (`index.md`)

`index.md` declares a `mindie-material-task/1` YAML frontmatter header embedding a `mindie-entry/3` descriptor, followed by the task-level retrieval navigation body:

- `format`: Fixed to `"mindie-material-task/1"`
- `task_id`: 64-character lowercase hex identifier matching the directory name
- `status`: Indexing progress state (e.g., `"indexed"`), which describes indexing completion rather than certifying the truth of the task's conclusions
- `entry`: Embedded `mindie-entry/3` metadata (`id`, `domain`, `kind`, `title`, `summary`, `keywords`, optional `conditions`)
- `blocks`: Ordered list of block descriptors binding each block's `block_id`, `path`, `sha256`, `seq`, `source_range`, `title`, `summary`, and `index_state`

---

## 3. Material Blocks (`blocks/<block_id>.md`)

Each block file stores a contiguous slice of deterministically projected, privacy-redacted public conversation:

- `format`: Fixed to `"mindie-material-block/1"`
- `task_id`: Parent task's 64-character hex ID
- `block_id`: 64-character hex ID matching the filename
- `seq`: 1-based contiguous sequence number
- `source_range`: Inclusive range of projected public message indices
- **Immutability**: Block body bytes are immutable under a given `block_id`. Any correction that alters body bytes requires a new `block_id` file and an updated `index.md` manifest.

---

## 4. Independent Usefulness Feedback (`feedback/*.json`)

After consulting a task package in real work, a consumer may submit a JSON feedback record under `tasks/<task_id>/feedback/`:

- `format`: Fixed to `"mindie-feedback/1"`
- Binds the exact `observed_revision`, vote direction (`up` or `down`), and an optional single-line redacted reason.

# Task Knowledge Package Format Specification

[中文](package-format.md) | **English** · [Back to Homepage](README.en.md)

This document explains the directory layout, YAML frontmatter schemas, and validation rules for public task knowledge packages (`tasks/<task_id>/`) and usefulness feedback (`feedback/*.json`) in `knowledge-vllm-ascend`. All format identifiers are pinned in [`publication-contract.json`](../publication-contract.json) at the repository root.

---

## 1. Why a "Manifest + Immutable Blocks" Design?

Real-world Ascend NPU debugging sessions can easily span tens or hundreds of kilobytes of logs and code experiments:
- Storing an entire session as a single monolithic document forces Agents to load huge files into context on every search;
- Storing only an LLM-generated summary loses the exact stack traces, environment variables, and failed hypotheses that engineers actually need.

To get the best of both worlds, every task package is split into **two layers**:
1. **`index.md` (Lightweight Navigation Manifest)**: Holds the task-level overview and a table of contents with per-block summaries so Agents can scan search hits quickly;
2. **`blocks/<block_id>.md` (Lossless Redacted Material Blocks)**: Stores ordered slices of the deterministically redacted public conversation (up to 16 KiB per block) so Agents can read exact steps on demand.

---

## 2. Directory & File Topology

Every public task package lives under `tasks/<task_id>/`, where `<task_id>` is a 64-character lowercase hexadecimal string:

```text
knowledge-vllm-ascend/
├── publication-contract.json                # Repository-wide publication contract (mindie-publication-contract/1)
├── tasks/
│   └── <64-hex-task-id>/                    # Complete task knowledge package directory
│       ├── index.md                         # Task navigation manifest (schema: mindie-material-task/1)
│       └── blocks/
│           ├── <64-hex-block-id-1>.md       # Immutable material block 1 (schema: mindie-material-block/1)
│           └── <64-hex-block-id-2>.md       # Immutable material block 2 (schema: mindie-material-block/1)
└── feedback/
    └── votes-<id>.json                      # Independent usefulness feedback file (schema: mindie-feedback/1)
```

### Validator Invariants (`mindie_knowledge.publication_check`)

- **Allowed Repository Paths**: Only `publication-contract.json`, standard root metadata files (`README.md`, `CONTRIBUTING.md`, `AGENTS.md`, `LICENSE`), `docs/**`, `tasks/<64-hex-task-id>/**`, and `feedback/*.json` are allowed. Retired legacy directories (`cases/`, `topics/`, `corpus/`) are rejected.
- **Exact Block Set Match (No Orphan Files)**: The files present under `tasks/<task_id>/blocks/` must match the `blocks` list in `index.md` one-to-one, with every file's `sha256` verified.
- **Plain UTF-8 Data Blobs**: Every published file must be a regular non-executable Git blob (`100644`) encoded in valid UTF-8 with canonical LF (`\n`) line endings.

---

## 3. Task Manifest Schema (`tasks/<task_id>/index.md`)

`index.md` contains a YAML frontmatter header followed by a human- and Agent-readable Markdown navigation summary.

### Frontmatter Fields

| Field Path | Type | Description |
| --- | --- | --- |
| `schema` | `string` | Fixed to `"mindie-material-task/1"` |
| `task_id` | `string` | 64-character lowercase hex identifier matching the parent directory name |
| `status` | `string` | Fixed to `"complete"` in public packages (indicates that all blocks are indexed for search—**not** a certification that the task's technical claims are universally true) |
| `navigation` | `string` | Task-level navigation summary helping Agents decide which blocks to inspect |
| `entry` | `object` | Embedded `mindie-entry/3` header metadata (see below) |
| `entry.schema` | `string` | Fixed to `"mindie-entry/3"` |
| `entry.domain` | `string` | Domain identifier; fixed to `"vllm-ascend"` in this repository |
| `entry.entry_id` | `string` | 64-character lowercase hex identifier matching `task_id` |
| `entry.kind` | `string` | Entry category (e.g., `"experience"` or `"knowledge"`) |
| `entry.title` | `string` | Concise task title |
| `entry.summary` | `string` | Task-level retrieval summary |
| `entry.conditions` | `object` | Observed software/hardware version map (e.g., `torch_npu_version`); describes the observed environment rather than claiming universal applicability |
| `entry.material_digest` | `string` | 64-character hex SHA-256 digest over the complete ordered block set |
| `entry.revision` | `string` | Current package revision digest (64-character hex SHA-256) used by feedback references |
| `blocks` | `list` | Ordered list of block descriptors, each binding `block_id`, `indexed` (`true`), `sha256`, `source_range`, `title`, and `summary` |

---

## 4. Material Block Schema (`tasks/<task_id>/blocks/<block_id>.md`)

Each block file stores a contiguous slice (up to 16 KiB) of deterministically projected and privacy-redacted public conversation or verification notes:

```markdown
---
block_id: 89bafb72f7ba1dc374e81c1f4c302ab4f20d7fbb1edc0afe568356190bc89f99
schema: mindie-material-block/1
source_range:
  body_end: 2503
  body_start: 0
---

Deterministically redacted public troubleshooting conversation or verification steps...
```

- **Minimal Frontmatter**: Contains exactly `schema` (`"mindie-material-block/1"`), `block_id` (matching `<block_id>.md`), and `source_range` (recording source byte offsets).
- **Block Immutability**: A block's bytes are immutable under its `block_id` (CI rejects in-place `M` modifications to existing `tasks/<id>/blocks/<block_id>.md` paths). Any edit to block body text requires writing a new `<block_id>.md` file, removing the old file, and updating `index.md`.

---

## 5. Independent Usefulness Feedback (`feedback/votes-<id>.json`)

After consulting a package in real engineering work, a consuming Agent can record an `up` or `down` vote via `knowledge_feedback`. When community sharing is enabled, publishable votes are exported under `feedback/votes-<id>.json`:

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
      "reason": "Optional single-line redacted reason (up to 1000 characters)"
    }
  ]
}
```

- **Revision Pinning**: `revision` must be a 64-character SHA-256 digest so delayed votes never attach to a newer revision of the task.
- **Anonymous Deduplication**: `root_id` is an opaque random identifier (raw local session IDs never leave the machine). At most one current vote is kept per `(root_id, entry_id, revision)` tuple.

# `knowledge-vllm-ascend` — Repository Guidelines for AI Agents & Maintainers

This repository is the canonical public engineering knowledge feed for `vLLM` / `vLLM-Ascend` in [MindIE Agent](https://github.com/mindie-agent/mindie-agent). Runtime synchronization, local indexing, redaction, and contribution packaging live in [`mindie-agent/knowledge`](https://github.com/mindie-agent/knowledge).

---

## 1. Repository Layout & Allowed Paths

To stay compatible with deployed `mindie-knowledge` feed validators (`mindie_knowledge/loop/feed.py` and `mindie_knowledge/publication_check.py`):

- **Root metadata allowlist**: Keep only `publication-contract.json`, `README.md`, `CONTRIBUTING.md`, `AGENTS.md`, and `LICENSE` at the repository root. Place English documentation and format guides inside `docs/` (`docs/README.en.md`, `docs/CONTRIBUTING.en.md`, `docs/package-format.md`, `docs/package-format.en.md`).
- **Task packages (`tasks/<64-hex-task-id>/`)**:
  - `index.md` uses `schema: mindie-material-task/1` and embeds an `entry` header with `schema: mindie-entry/3`.
  - `blocks/<64-hex-block-id>.md` files use `schema: mindie-material-block/1`.
  - Every file inside `tasks/<task_id>/blocks/` must be referenced by `index.md` with an exact matching `sha256` digest; unreferenced files are rejected.
- **Usefulness feedback (`feedback/*.json`)**:
  - Uses `schema: mindie-feedback/1`, recording explicit `up` / `down` votes bound to `(root_id, entry_id, revision)`.

---

## 2. Content Integrity & Privacy Rules

1. **Preserve Authentic Redacted Material**: Keep the complete deterministically redacted public material—including unsuccessful hypotheses, error traces, corrected conclusions, and applicability limits. Never replace block bodies with LLM-rewritten summaries, and never fabricate hardware, version, accuracy, or performance evidence.
2. **Block Immutability**: A block's body bytes are immutable under its `<block_id>`. Any correction that changes block body bytes requires creating a new `<block_id>.md`, removing the old block file, and recomputing the manifest hashes and task revision in `index.md`. Metadata-only edits in `index.md` keep existing block IDs.
3. **Withdrawal**: Deleting a complete `tasks/<task_id>/` directory via a reviewed PR withdraws the package; client syncs automatically reflect the withdrawal.
4. **Fail-Closed Redaction**: All committed files, PR titles, and PR bodies must pass `mindie_knowledge.redact` (ruleset `r3`) and Gitleaks checks with zero findings:
   ```bash
   python3 -m mindie_knowledge.redact --check README.md CONTRIBUTING.md AGENTS.md docs
   ```

---

## 3. Content PRs vs. Development PRs

- **Zero-trust CI validation**: `.github/workflows/check.yml` reads `publication-contract.json` from the trusted `main` base commit, installs the exact pinned `mindie-knowledge` validator commit, and inspects candidate PRs purely as Git blobs without executing candidate code.
- **Content mode (`content_only: true`)**: PRs touching only `tasks/**` or `feedback/**` produce a `success` `publication-head` check and follow the [`mindie-content-review/2`](https://github.com/mindie-agent/knowledge/blob/main/docs/repository-bot-contract.md) bot review workflow.
- **Development mode (`review_mode: development`)**: PRs touching `README.md`, `CONTRIBUTING.md`, `AGENTS.md`, `docs/**`, `.github/**`, or `publication-contract.json` produce a `neutral` `publication-head` check, cannot be auto-merged by the content bot, and must complete a [MindIE Agent Design Principles Review](https://github.com/mindie-agent/mindie-agent/blob/main/docs/design-principles.md) in the PR body before maintainer merge.

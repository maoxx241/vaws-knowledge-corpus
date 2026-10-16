# Contributing to `knowledge-vllm-ascend`

[中文](../CONTRIBUTING.md) | **English** · [Back to Homepage](README.en.md)

Thank you for contributing troubleshooting experience and improvements to `knowledge-vllm-ascend`! To keep every public package **authentic, leak-free, and reliably searchable across all clients**, this repository enforces automated schema checks, fail-closed privacy redaction, and a two-track review workflow.

---

## 1. Two Ways to Contribute

### Option A: Automated Contribution via MindIE Agent (Recommended ✨)

When using [`mindie-agent-cc`](https://github.com/mindie-agent/mindie-agent-cc), [`mindie-agent-codex`](https://github.com/mindie-agent/mindie-agent-codex), or [`mindie-agent-kimi`](https://github.com/mindie-agent/mindie-agent-kimi) with **Community Sharing (Opt-in)** enabled, you don't need to hand-craft YAML headers or Markdown manifests.

After you finish an engineering task, the local [`mindie-knowledge`](https://github.com/mindie-agent/knowledge) runtime handles the entire pipeline in the background:
1. **Projects Public Messages Only**: Keeps user prompts and visible assistant replies while stripping tool call noise, hidden reasoning traces, and system prompts;
2. **Runs Double Deterministic Redaction**: Scans text with pinned Gitleaks and `mindie_knowledge.redact` (ruleset `r3`) without letting an LLM rewrite your technical findings;
3. **Indexes & Opens a PR**: Generates block titles and `index.md` navigation summaries via a small native worker, packs a `mindie-contribution/1` batch, and opens a Pull Request against this repository.

### Option B: Manual Corrections, Appends, or Withdrawals (Pull Request)

Contributors and maintainers can also open Pull Requests directly to refine navigation metadata, append follow-up observations, or withdraw outdated packages:
- **Replacing or Updating Block Text**: The bytes inside `blocks/<block_id>.md` are **immutable** under that `block_id`. If you need to edit a block's body (for example, to redact an overlooked token or fix a command), create a new `<64-hex-block-id>.md` file, remove the superseded block file, and recompute the `sha256` digest, block list, and task `revision` in `index.md`.
- **Metadata-Only Edits**: If you are only improving titles, summaries, or applicability conditions in `index.md` without changing any `blocks/*.md` body bytes, keep the existing `block_id` values intact.
- **Withdrawing a Package**: Delete the entire `tasks/<task_id>/` directory in a PR and explain the reason in the PR description. Once merged, `mindie-knowledge sync` automatically marks the task as withdrawn on client machines.

---

## 2. Format & Privacy Rules (Enforced by CI)

Every PR is validated by GitHub Actions (`publication-head`) in zero-trust mode—reading candidate files strictly as immutable Git blobs without ever executing code from the PR:

1. **Package Topology & Hash Integrity**:
   - Every task package lives under `tasks/<64-lowercase-hex-task-id>/` and contains `index.md` (`mindie-material-task/1`) plus its referenced `blocks/<64-lowercase-hex-block-id>.md` files (`mindie-material-block/1`).
   - Every file under `blocks/` must be listed in `index.md` with an exact matching `sha256` digest; unreferenced files are rejected. See the [Package Format Specification](package-format.en.md).
2. **Zero Private or Sensitive Data (Fail-Closed Redaction)**:
   - **PR titles, PR descriptions, and all committed files** are scanned by `mindie_knowledge.redact` (`r3`).
   - Never include API keys, tokens, private key fragments, internal IPv4/IPv6 addresses, numbered machine identifiers, `<user>@<host>` login targets, personal home paths (use placeholders like `/home/<user>/...`), or internal ticket IDs.
   - You can run the privacy check locally before pushing:
     ```bash
     python3 -m mindie_knowledge.redact --check README.md CONTRIBUTING.md docs
     ```
3. **Root Directory Allowlist**:
   - Only `publication-contract.json` and standard root metadata files (`README.md`, `CONTRIBUTING.md`, `AGENTS.md`, `LICENSE`) may reside at the repository root. Keep English docs and format specifications under `docs/`.

---

## 3. Content PRs vs. Development PRs

The repository validator automatically classifies each PR by its touched file paths:

| PR Mode | Touched Paths | `publication-head` Check | Review & Merge Path |
| --- | --- | --- | --- |
| **Content Contribution (`content`)** | Only `tasks/**` or `feedback/**` | `success` | Reviewed and auto-merged by the repository review bot under the [`mindie-content-review/2`](https://github.com/mindie-agent/knowledge/blob/main/docs/repository-bot-contract.md) contract once all checks pass |
| **Engineering / Docs Change (`development`)** | Touches `README.md`, `CONTRIBUTING.md`, `docs/**`, `.github/**`, or `publication-contract.json` | `neutral` | **Never auto-merged by the bot**; requires a [Design Principles Review](https://github.com/mindie-agent/mindie-agent/blob/main/docs/design-principles.en.md) in the PR description and maintainer review |

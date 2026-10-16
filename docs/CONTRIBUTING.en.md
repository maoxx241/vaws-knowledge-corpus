# Contributing to `knowledge-vllm-ascend`

[中文](../CONTRIBUTING.md) | English · [Back to Home](README.en.md)

Thank you for contributing to the `vLLM-Ascend` public engineering knowledge corpus! This repository enforces deterministic schema validation, fail-closed privacy redaction, and the `mindie-content-review/2` automated review contract.

---

## 1. How to Contribute

1. **Automated Contribution via MindIE Agent (Recommended)**: When community sharing is enabled in [`mindie-agent-codex`](https://github.com/mindie-agent/mindie-agent-codex) or [`mindie-agent-cc`](https://github.com/mindie-agent/mindie-agent-cc), the local `mindie-knowledge` runtime automatically projects public conversation turns, applies Gitleaks and `r3` deterministic redaction, indexes navigation blocks, and opens a `mindie-contribution/1` Pull Request against this repository.
2. **Manual Corrections, Appends, or Withdrawals**: Contributors and maintainers may open Pull Requests to update metadata, append new observation blocks, or withdraw a package by deleting the complete `tasks/<task_id>/` directory (stating the reason in the PR description).

---

## 2. Format & Safety Requirements

- **Package Topology**: Every task package must reside under `tasks/<64-hex-task-id>/` with an `index.md` manifest (`mindie-material-task/1`) and referenced `blocks/<64-hex-block-id>.md` files (`mindie-material-block/1`). Unreferenced files inside a task directory are rejected. See the [Package Format Specification](package-format.en.md).
- **Block Immutability**: A block's body bytes are immutable under its `<block_id>`. Any change to block body text requires a new block ID and an updated `index.md` manifest.
- **Zero Private or Sensitive Data**: PR titles, PR bodies, and all committed blobs must pass `mindie_knowledge.redact` (`r3`) and Gitleaks checks—no credentials, private keys, internal IPs/hostnames, SSH endpoints, personal home paths, or raw transcripts may be published.
- **Content vs. Development PR Modes**:
  - PRs touching only `tasks/**` run in `content` mode and are eligible for automated review and merge once `publication-head` and Grok Bot checks succeed.
  - PRs touching documentation (`README.md`, `CONTRIBUTING.md`, `docs/**`), workflows, or contracts run in `development` mode, require a [Nine Design Principles Review](https://github.com/mindie-agent/mindie-agent/blob/main/docs/design-principles.en.md) in the PR body, and require maintainer review.

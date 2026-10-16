# Knowledge content repository

The 20 entries already public at commit `9aab5e1e2f1298f992b1b41e403c2b1e0232d12c`
were explicitly repackaged once on 2026-10-04 with exact body bytes preserved
(historical migration records are archived in `mindie-agent/engineering-archive`).
See `docs/package-format.md` (`docs/package-format.en.md`) for the current package
specification. Admit new contributions individually; do not automatically import
private material or older Git history.

A contribution is a complete task package under `tasks/<task_id>/`: `index.md`
uses `mindie-material-task/1`, and `blocks/<block_id>.md` files use
`mindie-material-block/1`. The manifest embeds a `mindie-entry/3` header, ordered
block descriptors, current navigation and task status. Optional explicit votes
use `feedback/*.json` with `mindie-feedback/1`. Task and block identifiers are
64-character lowercase hexadecimal values; changing a title keeps the identity.

Preserve the complete mechanically redacted material, including unsuccessful
attempts, corrected conclusions and uncertainty. A block body is immutable;
append new blocks for later observations. The manifest binds each block file's
SHA256, source range, title, summary and indexing state. All referenced blocks
must be indexed before publication. Task `status` describes indexing progress,
not the success or truth of the underlying business task. A summary is a fallible
retrieval aid; it must not replace material or certify a claim. Never invent
hardware, version, accuracy, performance or acceptance evidence.

Public bytes must pass the reviewed validator's canonical schema, complete
package topology, hash and redaction checks. No raw private transcript, endpoint,
user path or credential may be published. Within a task package, files not
referenced by its manifest are rejected. Withdrawal deletes the task package
through a reviewed PR; Git/PR history retains the reason. An empty corpus is valid.

Runtime code belongs in `mindie-agent/knowledge`. CI treats proposed content as
Git blobs and never imports executable code from the candidate checkout. The
validator is selected by `publication-contract.json` at the trusted base commit,
never by the candidate PR. Its `publication-head` check attaches to the exact
validated head: success permits the ordinary content path; neutral indicates valid
development changes that still require maintainer review. A contract, workflow or
Bot instruction change can never authorize its own automatic merge. Local
validation does not establish event delivery, remote CI or merge acceptance.

The [repository Bot operating contract](https://github.com/mindie-agent/knowledge/blob/main/docs/repository-bot-contract.md)
uses `mindie-content-review/2`. A correction that changes body bytes needs a new
block identity and a recomputed manifest; metadata-only corrections preserve the
body identity. Withdrawal removes the complete package. Later contributions use
the confirmed remote version and unsent blocks, preserving maintainer corrections.

A consumer pins this declaration's SHA256 as part of its product combination.
Content may advance under the same declaration; a changed declaration requires a
matching plugin combination. Missing or mismatched declarations are explicit
failures, not empty corpora. Release order is runtime, content and Bot contract,
then adapter. Local checks do not establish external Bot adoption or native use.

# VAWS knowledge corpus

Public Markdown knowledge for vLLM-Ascend development. Git is the content
authority; OpenViking indexes and OVPack releases can be rebuilt.

Contribute a Markdown document under `corpus/` with a title and a non-empty
body. Preserve the conditions and limits of your observation. Remove private
paths, endpoints and credentials before opening a pull request. The
`vaws-knowledge` package can prepare a separate redacted public copy and
submit it through your fork.

Pull requests currently receive format and redaction checks, followed by
human review and merge.
Publishing a report does not establish that it has been reproduced elsewhere.

After a merge to `main`, the release workflow builds a dense OVPack on CPU
from the exact Git commit and publishes its manifest, pack and hash-bound
reference metadata together. The Markdown, context spans and any available
aliases or topics refer to the same source revision. Checks and publishing use
the same fixed reviewed engine; content changes do not choose executable code.
Configured clients download and verify the release, import its stored vectors,
then switch the shared version. Failed updates retain the previous version;
project and candidate knowledge stay local.

Runtime code and client setup belong to
[vaws-knowledge](https://github.com/vllm-ascend-workspace/vaws-knowledge).
This repository contains knowledge, publishing policy and thin CI entrypoints.

## References migrated from the development workspace

The shared corpus maintains the former workspace notes in
[`corpus/models/`](corpus/models/) (nine model observations),
[`corpus/debugging/`](corpus/debugging/) (twelve debugging observations), and
[`corpus/infra/`](corpus/infra/) (one SSH transport observation).
These directories are browsing aids, not required authoring categories.

Every migrated note links to its exact source commit and retains historical
conditions and missing evidence. Model titles explicitly mark their unverified
status; the conflicting GLM-5 layer counts remain unresolved. The old workspace
filenames remain stable here. Publication does not turn a historical observation
into a current configuration recommendation.

Maintain reusable vLLM, Ascend NPU, AI and inference infrastructure references
here. Tool commands, executable analyzer rules, API contracts and VAWS engineering
validation belong with their implementation. Workspace clients consume these
notes through the existing shared release; task agents need no migration step,
extra lookup requirement or local authoring mirror.

License: MIT.

# VAWS knowledge corpus

Public Markdown knowledge and experience for vLLM-Ascend development. Git is the content
authority; OpenViking indexes and OVPack releases can be rebuilt.

Experience covers vLLM Ascend model development, operator integration, debugging,
performance and serving. Development of VAWS packages, control-plane services,
client wiring or the knowledge engine belongs with those projects.

The two stores are separate:

- `corpus/knowledge/` holds maintained conclusions. Use `knowledge_query` and
  `knowledge_explain`; check applicability against current code and evidence.
- `corpus/experience/` holds historical cases: the problem, investigation,
  action, observed outcome and remaining uncertainty. Use `experience_query`
  and `experience_explain`. Historical commands and implementations are clues
  for an investigation, not current operating instructions.

Both accept a Markdown title and non-empty body. Preserve conditions and limits,
including unsuccessful attempts and corrected explanations. Similar cases can
later inform maintained knowledge, skills or tools; storing a case does not
perform that conversion automatically.

Contribute only the redacted public copy prepared by the `vaws-knowledge`
package. Private source transcripts, paths, endpoints and credentials remain
local. The package preserves the selected kind when submitting through a fork.

Pull requests currently receive format and redaction checks, followed by
human review and merge.
Publishing a report does not establish that it has been reproduced elsewhere.

After a merge to `main`, the release workflow builds a dense OVPack on CPU
from the exact Git commit and publishes its manifest and pack together.
Configured clients download and verify the release, import its stored vectors,
then switch the shared version. Releases use schema `vaws-knowledge-release/2`
with `content.layout=kinds/v1`; older packs must be rebuilt. Failed updates
retain the previous version; project and candidate material stay local.

Runtime code and client setup belong to
[vaws-knowledge](https://github.com/vllm-ascend-workspace/vaws-knowledge).
This repository contains knowledge, publishing policy and thin CI entrypoints.

License: MIT.

# VAWS knowledge corpus

Public Markdown knowledge for vLLM-Ascend development. Git is the content
authority; OpenViking indexes and OVPack releases can be rebuilt.

Contribute a Markdown document under `corpus/` with a title and a non-empty
body. Preserve the conditions and limits of your observation. Remove private
paths, endpoints and credentials before opening a pull request. The
`vaws-knowledge` package can prepare a separate redacted public copy and
submit it through your fork.

Pull requests currently receive format and redaction checks, followed by
human review and merge. Grok review and automatic merge are not enabled.
Publishing a report does not establish that it has been reproduced elsewhere.

After a merge to `main`, the release workflow builds a dense OVPack on CPU
from the exact Git commit and publishes its manifest and pack together.
Configured clients download and verify the release, import its stored vectors,
then switch the shared version. Failed updates retain the previous version;
project and candidate knowledge stay local.

Runtime code and client setup belong to
[vaws-knowledge](https://github.com/vllm-ascend-workspace/vaws-knowledge).
This repository contains knowledge, publishing policy and thin CI entrypoints.

License: MIT.

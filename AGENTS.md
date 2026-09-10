# Knowledge content repository

Keep knowledge in `corpus/*.md` or its subdirectories. Each document needs a
title and non-empty body. Keep source conditions when known; do not invent
hardware, version, accuracy or performance evidence.

Public contributions must pass the installed `vaws-knowledge` redaction and
Markdown checks. Do not commit private endpoints, user paths or credentials.

Runtime implementation belongs in `vllm-ascend-workspace/vaws-knowledge`.
CI uses a fixed reviewed package revision. It never imports Python modules
from a proposed corpus checkout. Human reviewers decide whether to merge;
Grok and automatic PR merging are deferred.

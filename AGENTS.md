# Knowledge and experience content repository

Keep maintained knowledge in `corpus/knowledge/` and historical cases in
`corpus/experience/`. Each document needs a title and non-empty body. Keep
source conditions when known; do not invent hardware, version, accuracy or
performance evidence. Historical observations and implementations remain dated
experience, not current guidance. Correct mistaken explanations while preserving
the observed outcome and uncertainty.

Experience here concerns vLLM Ascend models, operators and business execution.
Do not contribute VAWS package, control-plane, client wiring or knowledge-engine
development cases; keep those with their owning projects.

Public contributions must pass the installed `vaws-knowledge` redaction and
Markdown checks. Do not commit private endpoints, user paths or credentials.

Runtime implementation belongs in `vllm-ascend-workspace/vaws-knowledge`.
CI uses a fixed reviewed package revision. It never imports Python modules
from a proposed corpus checkout. Human reviewers decide whether to merge.

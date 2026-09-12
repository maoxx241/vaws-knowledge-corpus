# Knowledge and experience content repository

Keep maintained knowledge in `corpus/knowledge/` and historical cases in
`corpus/experience/`. Each document needs a title and non-empty body. Keep
source conditions when known; do not invent hardware, version, accuracy or
performance evidence. Historical observations and implementations remain dated
experience, not current guidance. Correct mistaken explanations while preserving
the observed outcome and uncertainty.

Preserve enough technical detail for an unfamiliar reader to understand why the
investigation or change mattered. For complex cases this can include the actual
call path, input layout, state or event lifetime, reference calculation and
discriminating results. Read the underlying source or output before adding a
claim; summaries alone are not new evidence. Keep simple cases concise. There is
no required article length or section template, and unresolved findings may
remain explicitly unresolved.

Experience here concerns vLLM Ascend models, operators and business execution.
Do not contribute VAWS package, control-plane, client wiring or knowledge-engine
development cases; keep those with their owning projects.

Public contributions must pass the installed `vaws-knowledge` redaction and
Markdown checks. Do not commit private endpoints, user paths or credentials.

Runtime implementation belongs in `vllm-ascend-workspace/vaws-knowledge`.
CI uses a fixed reviewed package revision. It never imports Python modules
from a proposed corpus checkout. Human reviewers decide whether to merge.

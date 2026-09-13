# Pre-clear NaN or Inf alone does not establish a defect root cause

Status: historical, unverified. Confidence: low.

Migration clarification: dismissing an unwritten value requires evidence that the relevant rows are initialized before consumption and that the actual consumer does not read outside them. The historical observation below was not revalidated during migration.

Imported from the project note dated 2026-09-07. The source did not provide a complete reproducible evidence chain. Claims of verification in the historical description are not current support guarantees.

## Avoidance

Do not treat the first non-finite stage in a single dump as the answer. Name the capture point relative to initialization, for example after_clear rather than initial_state, so the manifest cannot be misread later.

## Search terms

- nan in initial state before clear
- nan destination before copy expected
- first nonfinite stage also present in passing run

## Resolution

Compare the candidate dump against a known-good request before attributing any non-finite finding, and place capture points after initialization rather than before it. Report the first non-finite stage that a passing run does not also exhibit.

## Root cause

Those stages observe memory that has not been initialized yet for this step. Non-finite content there carries no information about correctness.

## Symptom

A dump scan flags a stage such as an initial state before clear or a destination before copy as the first non-finite stage, and the investigation concludes there is state corruption. A known-good request shows the same non-finite values.

## Recorded context

- component: dump-interpretation, non-finite-value-attribution.

Other environment and version details were not recorded.

## Source

Source: vllm-ascend-workspace/vllm-ascend-workspace; legacy identifier: nonfinite-values-in-pre-clear-scratch-buffers-are-not-root-cause; first observed: 2026-09-07.

Migrated from the [versioned workspace note](https://github.com/vllm-ascend-workspace/vllm-ascend-workspace/blob/f85b57165600d7da98c760304dc854cdf611f25f/.agents/knowledge/nonfinite-values-in-pre-clear-scratch-buffers-are-not-root-cause.md). Historical implementation paths and verification wording above describe that source, not the current VAWS or vLLM-Ascend release.

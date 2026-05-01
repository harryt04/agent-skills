# Cross-Check Protocol

Use this protocol whenever a stage says to run a cross-check or audit loop.

## Goal

Compare the source-of-truth artifacts for the current stage against the generated or implemented outputs, then either patch the gaps or surface them clearly.

## Standard Loop

1. Identify the source artifacts.
2. Identify the target artifacts or branch state.
3. Spawn one or more focused read-only audit sub-agents unless the work is too small to justify that overhead.
4. Give each audit agent a narrow lens, such as:
   - source-to-target traceability
   - truth check against current code or generated files
   - gap detection for omitted requirements, constraints, or edge cases
5. Consolidate findings.
6. If the stage allows patching, patch straightforward gaps before surfacing the result.
7. If ambiguity remains, present it explicitly to the developer instead of guessing.

## Pass Limit

- Run at most 2 passes.
- If a second pass does not produce meaningful new findings, stop and escalate the remaining ambiguity to the developer.

## Stage-Specific Application

- **Stage 2**: compare source context or interview findings against `prd.md` or `plan.md`.
- **Stage 3**: compare the finalized Stage 2 artifact against `trd.md` and `trd-summary.md`.
- **Stage 4**: compare `trd.md` against `orchestration.md` and `memory.md`.
- **Stage 6**: compare the finalized TRD and orchestration artifacts against the branch implementation and current code state.
- **Stage 8**: compare the finalized implementation and test-planning artifacts against the final branch state.

## Reporting Shape

For implementation audits, use these sections:

- `Fully Satisfied`
- `Gaps / Missing Work`
- `Recommended Fixes`

For document-generation stages, either patch directly or report concise, actionable gaps before asking for review.

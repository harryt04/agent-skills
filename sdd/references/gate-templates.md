# Gate Templates

Use fixed gate templates so pass and fail decisions are explicit and comparable across runs.

## Common Rules

- Every gate file must end with a formal verdict.
- If `Dropped or Changed Items` is non-empty and not explicitly accepted, the gate fails.
- If open questions remain, they must be recorded as `assumption` or `wont-fix` before the gate can pass.
- For planning stages, record whether the mandatory post-generation grill was performed, what follow-up questions were asked, and what changes were applied afterward.

## Stage 1 Gate Template: `context-gate.md`

```markdown
# Context Gate

## Source Context Reviewed
- [list the source messages, files, repo findings, or notes reviewed]

## Preserved From Source
- [item]

## Open Questions
- [question]

## Intentional Assumptions
- [assumption]

## Wont-Fix Items
- [wont-fix]

## Dropped or Changed Items
- [item]

## Post-Generation Grill Performed
- YES or NO

## Post-Generation Questions Asked
- [question asked after reviewing the generated artifact]

## Post-Generation Changes Applied
- [change made after the grill]

## Recommendation
- Proceed or do not proceed, with one short reason.

## Formal Verdict
- PASS or FAIL
```

## Stage 2 Gate Template: `prd-gate.md`

```markdown
# PRD Gate

## Source Artifact
- `path/to/source`

## Preserved From Source
- [requirement or constraint]

## Open Questions
- [question]

## Intentional Assumptions
- [assumption]

## Wont-Fix Items
- [wont-fix]

## Dropped or Changed Items
- [item]

## Testing Expectations Captured
- [high-level expectation]

## Post-Generation Grill Performed
- YES or NO

## Post-Generation Questions Asked
- [question asked after reviewing the generated artifact]

## Post-Generation Changes Applied
- [change made after the grill]

## Recommendation
- Proceed or do not proceed, with one short reason.

## Formal Verdict
- PASS or FAIL
```

## Stage 3 Gate Template: `trd-gate.md`

```markdown
# TRD Gate

## Source Artifact
- `path/to/prd.md`

## Preserved From Source
- [requirement or constraint]

## Open Questions
- [question]

## Intentional Assumptions
- [assumption]

## Wont-Fix Items
- [wont-fix]

## Dropped or Changed Items
- [item]

## Test Requirements Mapped
- [feature or work unit] -> [concrete test requirement]

## Post-Generation Grill Performed
- YES or NO

## Post-Generation Questions Asked
- [question asked after reviewing the generated artifact]

## Post-Generation Changes Applied
- [change made after the grill]

## Recommendation
- Proceed or do not proceed, with one short reason.

## Formal Verdict
- PASS or FAIL
```

## Stage 4 Gate Template: `orchestration-gate.md`

```markdown
# Orchestration Gate

## Source Artifact
- `path/to/trd.md`

## Preserved From Source
- [requirement, constraint, or work unit expectation]

## Open Questions
- [question]

## Intentional Assumptions
- [assumption]

## Wont-Fix Items
- [wont-fix]

## Dropped or Changed Items
- [item]

## Work Unit Contract Coverage
- [work unit or layer] -> [coverage note]

## Recommendation
- Proceed or do not proceed, with one short reason.

## Formal Verdict
- PASS or FAIL
```

## Stage 5 Cross-Check Template: `execution-cross-check.md`

Use one section per lens plus a final consolidated verdict.

```markdown
# Execution Cross-Check

## Implementation Scope Reviewed
- [orchestration path]
- [memory path]
- [branch or workspace summary]

## Lens: Requirements Traceability
### Fully Satisfied
- [item]
### Gaps / Missing Work
- [item]
### Recommended Fixes
- [item]
### Formal Verdict
- PASS or FAIL

## Lens: Wiring / Completeness
### Fully Satisfied
- [item]
### Gaps / Missing Work
- [item]
### Recommended Fixes
- [item]
### Formal Verdict
- PASS or FAIL

## Lens: Test Compliance
### Fully Satisfied
- [item]
### Gaps / Missing Work
- [item]
### Recommended Fixes
- [item]
### Formal Verdict
- PASS or FAIL

## Additional Lens: [Optional]
### Fully Satisfied
- [item]
### Gaps / Missing Work
- [item]
### Recommended Fixes
- [item]
### Formal Verdict
- PASS or FAIL

## Build / Lint / Test Verification
- Build: PASS or FAIL
- Lint: PASS or FAIL
- Tests: PASS or FAIL
- Notes: [short note]

## Consolidated Recommendation
- Proceed or do not proceed, with one short reason.

## Formal Verdict
- PASS or FAIL
```

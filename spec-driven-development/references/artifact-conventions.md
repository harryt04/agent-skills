# Artifact Conventions

Use this file as the single source of truth for session folders, canonical filenames, alias handling, and resume discovery.

## Session Folder Selection

Before reading or writing planning artifacts, resolve one active session folder:

1. If the developer explicitly mentions a work item ID, use `.agents/specs/<work-item-id>/`.
2. If the conversation already references a path under `.agents/specs/<id>/...` or `.agents/spec/...`, continue in that folder.
3. Otherwise use `.agents/spec/`.

Keep all artifacts for the current request in that folder unless the developer explicitly changes context.

## Canonical Filenames

Use lowercase kebab-case names for newly created artifacts:

- `app-outline.md`
- `prd.md`
- `plan.md`
- `trd.md`
- `trd-summary.md`
- `orchestration.md`
- `memory.md`
- `updates/`
- `implementation-cross-check.md`
- `test-analysis.md`
- `test-trd.md`
- `test-orchestration.md`
- `test-memory.md`
- `test-updates/`
- `post-test-cross-check.md`
- `pr-description.md`

## Compatibility and Alias Rules

- If an artifact already exists with a different casing or older name, keep using the existing file instead of renaming it.
- Do not rename existing files unless the developer explicitly asks.
- When checking for resumable context, inspect canonical names first, then common aliases.

Common aliases:

- `PRD.md`
- `TRD.md`
- `TRD-summary.md`
- `ORCHESTRATION.md`
- `IMPLEMENTATION-CROSS-CHECK.md`
- `TRD-tests.md`
- `ORCHESTRATION-tests.md`
- `memory-tests.md`
- `updates-tests/`
- `IMPLEMENTATION-CROSS-CHECK-AFTER-TESTS.md`

## Resume Discovery

Use resume discovery only when the developer explicitly asks to resume, or when they invoke the exact shortcut `/spec-driven-development` or `/spec-driven-development plan`.

Search these candidate locations:

1. `.agents/spec/`
2. each direct child folder under `.agents/specs/`

For each candidate, summarize:

- folder path
- visible artifacts
- highest likely stage reached
- whether it looks actively resumable

If more than one candidate looks viable, present the strongest one first and then list the other options.

## Ownership Rules

- Shared planning artifacts are orchestrator-owned.
- Sub-agents may read `memory.md`, `test-memory.md`, and the active orchestration artifact.
- Sub-agents should report through `updates/` or `test-updates/`, not by editing shared planning files directly.
- The orchestrator consolidates sub-agent outcomes into the shared artifacts.

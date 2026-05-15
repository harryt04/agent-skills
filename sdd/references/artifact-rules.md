# Artifact Rules

Use this file for session-folder handling, canonical filenames, and path rules.

## Session Folders

- Keep using `.agents/spec/` and `.agents/specs/*`.
- Do not auto-discover prior sessions.
- Do not scan those folders trying to infer where the developer left off.
- If the developer is resuming, they should provide the relevant file paths.

## Default Working Folder

If the developer is starting Stage 1 from context only and has not provided planning-file paths:

1. Use `.agents/spec/`.
2. Before writing, check for colliding canonical files.
3. If the folder already contains conflicting session files, stop and ask the developer to reorganize or provide a different directory.

## Canonical Filenames

- `context.md`
- `context-gate.md`
- `prd.md`
- `prd-gate.md`
- `trd.md`
- `trd-gate.md`
- `orchestration.md`
- `orchestration-gate.md`
- `memory.md`
- `updates/`
- `execution-cross-check.md`
- `pr-description.md`

## Path Rules

- If the developer provides an explicit file path, trust that path.
- The skill may modify that file or create sibling canonical files next to it.
- If the developer provides `path/to/orchestration.md`, treat Stage 4 as complete enough to attempt Stage 5.
- If a needed sibling file such as `memory.md` is missing, stop and ask for it.
- Do not invent alternate filenames unless the developer explicitly asks.

## Ownership Rules

- Main planning artifacts and gate files are orchestrator-owned.
- Sub-agents may read `memory.md`, the active orchestration artifact, and explicitly referenced planning files.
- Sub-agents should report through `updates/` rather than editing orchestrator-owned artifacts directly.
- The orchestrator consolidates sub-agent outcomes into shared artifacts.

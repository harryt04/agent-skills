# Execution Rules

Use this file as the source of truth for Stage 5 behavior.

## Top-Level Orchestrator Contract

- The orchestrator must never do primary implementation itself.
- The orchestrator may run formatting, build, lint, and test commands.
- The orchestrator may update `memory.md`, `execution-cross-check.md`, and orchestrator-owned status files.
- If code changes are needed, spawn sub-agents.
- Prefer spawning fresh sub-agents over broad inline reasoning or large direct edits.

## Execution Topology

1. Validate prerequisites.
2. Execute one layer at a time.
3. Spawn one sub-agent per work unit in that layer.
4. Wait for the whole layer to complete.
5. Consolidate sub-agent summaries into `memory.md`.
6. Move to the next layer only when the current layer is complete or the user has approved a deviation.

## Mandatory Cross-Check Lenses

Always run these after implementation:

1. Requirements traceability
2. Wiring and completeness
3. Test compliance

Add context-specific lenses when relevant:

1. Security
2. Backward compatibility / breaking changes
3. Performance / scale
4. Data migration / operational risk

## Remediation Policy

Auto-remediation is allowed only for small, low-judgment issues such as:

- typos
- broken references
- small build errors
- similarly mechanical fixes

When those are found:

1. Spawn fix sub-agents.
2. Re-run the affected cross-check lens.
3. Re-run build, lint, or tests as needed.
4. Repeat until the accepted requirements are satisfied and verification is green, or a blocker requires the developer.

## Escalation Rule

If the problem suggests a bad plan, conflicting artifacts, missing product decisions, or likely re-planning:

1. Stop.
2. Summarize what went wrong.
3. Do not improvise a new plan or restart execution without explicit developer direction.

## Verification Rule

Stage 5 is not complete until all repo-appropriate local verification succeeds where feasible:

- build
- lint
- locally runnable CI-style tests

If some behavior cannot be tested automatically, call that out explicitly in `execution-cross-check.md`.

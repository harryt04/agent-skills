# Agent Routing for Multi-Agent SDD Orchestration

This guide explains how the Orchestrator decides which sub-agent to spawn at each stage, when to parallelize, and how to handle large tasks without blowing out context windows.

## Agent Roster

| Agent ID | Role | Model | Stages | Key Capability |
|----------|------|-------|--------|----------------|
| `sdd-spec-writer` | Planning & Design | claude-opus-4.5 | 1, 2, 3, 4 | Rigorous requirements and technical design with interviewing |
| `sdd-researcher` | Codebase Exploration | claude-haiku-4.5 | 1, 2, 3 | Read-only file exploration, returns structured summaries |
| `sdd-qa-reviewer` | QA Gate | gpt-5.4-mini | 2b | Evaluates PRD for testability and test strategy (cross-family) |
| `sdd-test-coverage` | Test Gap Analysis | gpt-5.4-mini | 3b | Identifies missing test cases from TRD (cross-family) |
| `sdd-panel-expert` | Risk Review | gpt-5.4 | 3.5 | Blind TRD review for architectural/design risks (cross-family) |
| `sdd-executor` | Implementation | gpt-5.3-codex | 5 | Code and test implementation with Beads work-unit tracking |
| `sdd-cross-checker` | Execution Verification | gpt-5.4-mini | 5 | Post-layer requirements traceability and test compliance |
| `sdd-fix-agent` | Mechanical Remediation | gpt-5.4-mini | 5 | Small mechanical fixes: build errors, lint, broken imports |
| `sdd-documentation` | PR Writing | gpt-5.4 | 6 | PR descriptions from git diff |

---

## Stage-by-Stage Routing

### Stage 1: Context

```
1. SPAWN sdd-researcher (if repo context is needed)
   INPUT: "Summarize the existing architecture relevant to [ticket topic]"
   OUTPUT: Structured research summary

2. SPAWN sdd-spec-writer
   INPUT: User's ticket description + researcher summary (if run)
   OUTPUT: context.md + context-gate.md

3. Wait for user approval
```

Spawn the researcher only if the ticket touches existing code. For greenfield work, go straight to spec-writer.

### Stage 2: PRD

```
1. SPAWN sdd-researcher (if PRD needs codebase constraints)
   INPUT: "Find existing [relevant patterns] that will constrain the PRD"
   OUTPUT: Structured constraint summary

2. SPAWN sdd-spec-writer
   INPUT: context.md summary + researcher findings (if run)
   OUTPUT: prd.md + prd-gate.md

3. Wait for user approval of PRD

4. SPAWN sdd-qa-reviewer
   INPUT: approved prd.md
   OUTPUT: QA review report (testability, test strategy, coverage gaps)

5. Wait for user approval of QA review
```

### Stage 3: TRD

```
1. SPAWN sdd-researcher (if TRD needs existing code patterns)
   INPUT: "Find existing [component/pattern] implementations relevant to [TRD scope]"
   OUTPUT: Structured findings

2. SPAWN sdd-spec-writer
   INPUT: prd.md summary + researcher findings (if run)
   OUTPUT: trd.md + trd-gate.md

   [If trd.md is large (>600 lines projected), see Parallelization section below]

3. Wait for user approval of TRD

4. SPAWN sdd-test-coverage
   INPUT: trd.md
   OUTPUT: Test coverage gap analysis

5. Wait for user approval of test coverage

6. SPAWN sdd-panel-expert
   INPUT: trd.md ONLY (blind review — do not include prd.md)
   OUTPUT: Technical risk assessment

7. Review panel findings:
   - BLOCKERS: Stop, report to user, do not proceed to Stage 4
   - MAJOR CONCERNS: Report to user, let them decide
   - APPROVED: Proceed to Stage 4

8. Wait for explicit user approval
```

### Stage 4: Orchestration

```
1. SPAWN sdd-spec-writer
   INPUT: trd.md summary + test coverage gaps + panel concerns
   OUTPUT: orchestration.md + memory.md + orchestration-gate.md

2. Verify every work unit in orchestration.md follows work-unit-template.md

3. Wait for user approval
```

### Stage 5: Execution

```
For each layer in orchestration.md:

  1. Create Beads issues for each work unit in this layer
  2. SPAWN sdd-executor per work unit (parallelize within a layer)
     INPUT: work-unit-id, orchestration.md path, memory.md path
     OUTPUT: Implemented code + tests + closed Beads issue

  3. Consolidate: Read closed Beads issues, update memory.md

  4. SPAWN sdd-cross-checker
     INPUT: completed work unit IDs, orchestration.md, trd.md, memory.md, changed files
     OUTPUT: Cross-check report (traceability, wiring, test compliance)

  5. If cross-checker finds BLOCKERS or MAJORS:
     - SPAWN sdd-fix-agent for mechanical issues (see Fix Agent Scope below)
     - If fix-agent resolves: re-run cross-checker to confirm
     - If not resolvable mechanically: STOP, report to user

  6. Run build/lint/test verification
     - If failures: SPAWN sdd-fix-agent for build/lint errors
     - If tests fail due to logic errors: STOP, report to user

  7. Report layer status to user, wait for approval before next layer

After all layers:
  - Write execution-cross-check.md
  - Verify all Beads work units are closed
  - Wait for user approval before Stage 6
```

### Stage 6: PR Description

```
1. SPAWN sdd-documentation
   INPUT: git diff from current branch
   OUTPUT: pr-description.md

2. Wait for user approval
```

---

## Parallelization: When and How

The orchestrator decomposes large tasks. Sub-agents do not self-decompose or self-loop.

### When to Parallelize

Parallelize when a task is too large for one sub-agent invocation without risking context exhaustion or quality degradation. Signs this is needed:

- Spec-writer reports the TRD will cover 5+ distinct components or subsystems
- TRD draft is already >600 lines and still growing
- Orchestration plan has 10+ work units that could be grouped into independent streams
- Stage 5 layer has multiple work units with no inter-dependencies

### How the Orchestrator Parallelizes

**Planning work (e.g., large TRD)**:

1. Spec-writer returns a preliminary outline: "This TRD covers [N] independent components: A, B, C"
2. Orchestrator reviews the outline and confirms the split makes sense
3. Orchestrator spawns N spec-writer instances in parallel, each assigned one component
4. Each spec-writer writes its section to a temp file: `trd-part-A.md`, `trd-part-B.md`, etc.
5. Orchestrator reads all parts, resolves any overlaps or contradictions, and assembles the final `trd.md`
6. Presents the unified TRD to the user as a single artifact

**Execution work (Stage 5)**:

Work units within the same layer that have no dependencies on each other can be spawned in parallel. The Beads issue structure makes this safe — each executor claims its own issue atomically.

### Sub-Agent Token Exhaustion Protocol

If a sub-agent reports it is approaching its context limit with work remaining:

1. Sub-agent stops and reports: "I've completed [X] and have [Y] remaining. I'm near my context limit."
2. Orchestrator asks user: "The spec-writer has completed [X]. Should I continue with a fresh agent, or is this sufficient to proceed?"
3. If continuing: orchestrator spawns a fresh instance with a concise handoff summary (not the full prior output)
4. If sufficient: proceed to next stage with what exists

**Sub-agents must never silently self-loop when running low on tokens.** Stopping and reporting is always the correct behavior.

---

## Fix Agent Scope

Spawn `sdd-fix-agent` for:
- Build errors (missing imports, type errors, syntax errors)
- Lint failures
- Broken references (renamed function not updated everywhere)
- Typos in identifiers or string literals
- Test setup issues (wrong import paths, missing fixtures)
- Assertion mismatches where the implementation is clearly correct

Escalate to user (do not spawn fix-agent) for:
- Logic errors where correct behavior is ambiguous
- Test failures where the implementation itself appears wrong
- Issues requiring new code beyond ~15 lines
- Anything touching a public API or database schema

---

## Sub-Agent Communication Pattern

Pass file paths + concise summaries. Never pass full artifact content in the spawn prompt.

```
Task: sdd-spec-writer

Context:
- Stage: 3 (TRD generation)
- Session directory: .agents/specs/123456/
- Input: .agents/specs/123456/prd.md

Summary of prd.md (read the full file before starting):
- [3-5 key points from the PRD]
- Key constraints from QA review: [1-2 points]

Your task:
- Read prd.md from the session directory
- Generate trd.md + trd-gate.md
- If the design covers more than 5 independent components, return a component outline first
  before drafting the full TRD so the orchestrator can decide whether to parallelize
```

---

## Parallel Execution Rules

### Safe to Parallelize
- Multiple executors (Stage 5, different work units within a layer)
- Multiple panel reviewers (Stage 3.5, for blind diversity)
- Multiple spec-writers on non-overlapping TRD components
- Research sub-agents (parallel codebase inspection in Stages 1-3)

### Must Serialize
- Stages: 1 → 2 → 2b → 3 → 3b → 3.5 → 4 → 5 → 6 (never skip or parallelize stages)
- Gates: After each stage output, always wait for explicit user approval
- Beads layers: A layer must complete before the next layer starts

---

## Fallback Routing

If a sub-agent is unavailable or fails:

1. Stop immediately
2. Report to user: "Sub-agent [name] failed with: [error]. How would you like to proceed?"
3. Do not retry automatically
4. Do not continue the workflow without explicit user direction

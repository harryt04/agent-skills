---
name: spec-driven-development
description: >
  Guides heavyweight software work through a document-driven workflow. Use this skill for cross-repo or high-risk feature planning,
  formal PRD/TRD generation, orchestration plans for parallel sub-agents, branch-wide implementation audits, substantial test-planning,
  and final PR descriptions once a branch is complete. Trigger when the work is multi-stage, spans repositories or systems,
  needs durable planning artifacts, must be checked for completeness against a spec, or when the user needs help figuring out
  which stage of the workflow they are in and which model fits that stage. Do not trigger for small tickets, isolated bug fixes,
  one-off test help, casual architecture advice, or simple PR/commit writing.
---

# Spec-Driven Development

Use this skill for heavyweight work where durable artifacts, staged execution, and explicit audits reduce risk. Do not force it onto small asks.

## Router

Classify the request before doing stage work:

1. **No-skill path**: small, local, or already concrete enough to execute directly. Examples: one bug fix, one test file, lightweight refactor, casual architecture advice, simple PR description.
2. **Partial-flow path**: medium-sized work, or the developer already has artifacts. Start at the first missing heavyweight stage.
3. **Full-flow path**: cross-repo, high-risk, ambiguous, or formal spec work. Run the full pipeline.

Default heuristics:

- Single repo and under roughly 3 meaningful work units: usually no-skill path
- Single repo but risky, ambiguous, or requiring durable planning: partial-flow path
- Cross-repo, tall-thin-slice, branch-wide audit, or formal spec work: full-flow path

If uncertain, ask one short routing question instead of starting the full pipeline.

## Entry Order

Use this precedence order every time:

1. **Exact shortcut**: if the developer enters exactly `/spec-driven-development` or `/spec-driven-development plan`, output the orientation block, run resume discovery, then ask whether to resume or start fresh. Do not generate files yet.
2. **Explicit stage request**: if the developer clearly asks to start at a stage and the request is viable, honor it.
3. **Existing session**: resolve the active session folder and inspect artifacts to infer the latest viable stage.
4. **Unclear intent**: if the stage is still unclear, output the orientation block and ask one short routing question.

Prefer entering at the latest viable stage. Do not regenerate PRD, TRD, or orchestration artifacts just because this skill triggered.

## Orientation Block

When the developer has not provided enough context to determine their stage, or when they invoked the exact shortcut, output the following block exactly as written. Substitute only the placeholder in angle brackets.

=== BEGIN ORIENTATION OUTPUT ===

This is the **Spec-Driven Development** skill — a 9-stage document-driven workflow for heavyweight software work. You can enter at any stage.

| Stage | Name                                   | Recommended Model                             |
| ----- | -------------------------------------- | --------------------------------------------- |
| 1     | App Outline / Context Gathering        | sonnet 4.6 (haiku latest if lightweight)      |
| 2     | PRD — Product Requirements Doc         | opus 4.6 (or gpt 5.4)                         |
| 3     | TRD — Technical Requirements Doc       | opus 4.6 (or gpt 5.4)                         |
| 4     | Orchestration Plan                     | gpt 5.4 (or opus 4.6)                         |
| 5     | Execute the Orchestration Plan         | codex 5.3 (or sonnet 4.6)                     |
| 6     | Implementation Cross-Check             | gpt 5.4 (or opus 4.6)                         |
| 7     | Test Coverage                          | gpt 5.4 (or opus 4.6)                         |
| 8     | Implementation Cross-Check After Tests | sonnet 4.6 (or gpt 5.4)                       |
| 9     | PR Description                         | gpt-5.4 (or sonnet latest, then haiku latest) |

**Which stage are you at?**

| If you have...                                    | Start at...  |
| ------------------------------------------------- | ------------ |
| An idea but no durable spec yet                   | Stage 1 or 2 |
| An approved PRD or plan                           | Stage 3      |
| An approved TRD                                   | Stage 4      |
| Implementation underway or ready to dispatch      | Stage 5      |
| Implementation done, needs audit                  | Stage 6      |
| Tests still need planning or writing              | Stage 7      |
| Implementation and tests done, final audit needed | Stage 8      |
| A final branch, only PR write-up remains          | Stage 9      |

**You are currently on:** `<state your active model name here>`

The recommended model for each stage is listed above. If your current model does not match the stage you want to start, switch before we begin — the skill will remind you at each stage transition.

To switch models in OpenCode: **Ctrl+P → change model**, or start a new session with the target model active.

What would you like to work on, or which stage would you like to start at?

=== END ORIENTATION OUTPUT ===

Do not add commentary before or after the block.

## Global Rules

- Follow `references/artifact-conventions.md` for session-folder selection, canonical filenames, alias handling, and resume discovery.
- Shared planning files are orchestrator-owned. Sub-agents may read them, but they should report through per-work-unit update files instead of editing shared status artifacts directly.
- Use sub-agents for heavyweight generation, parallel repo research, plan execution, and read-only audits. Stay inline for routing, approvals, and small updates.
- Record intentional scope deviations once and carry them forward instead of re-flagging them repeatedly.
- Never execute an orchestration plan without explicit developer approval.
- If the request is too small for this workflow, do the work directly instead of trying to force a stage.

## Reference Files

Read only the files needed for the current stage.

| File                                        | Purpose                                                     |
| ------------------------------------------- | ----------------------------------------------------------- |
| `references/artifact-conventions.md`        | Session folders, canonical names, aliases, resume discovery |
| `references/model-routing.md`               | Recommended models and lightweight mismatch handling        |
| `references/cross-check-protocol.md`        | Shared audit and cross-check loop                           |
| `references/app-outline-template.md`        | Stage 1 outline structure                                   |
| `references/prd-template.md`                | Stage 2 written PRD path                                    |
| `references/plan-template.md`               | Stage 2 plan-driven PRD path                                |
| `references/plan-interview-guide.md`        | Stage 2 interview flow                                      |
| `references/trd-template.md`                | Stage 3 TRD generation                                      |
| `references/orchestration-instructions.md`  | Stage 4 orchestration construction                          |
| `references/orchestration-template.md`      | Stage 4 orchestration structure                             |
| `references/memory-template.md`             | Stage 4 companion memory structure                          |
| `references/test-coverage-instructions.md`  | Stage 7 test planning and execution                         |
| `references/pull-request-instructions.md`   | Stage 9 PR description                                      |
| `references/ado-wiki-markdown-reference.md` | Optional markdown syntax reference for PR text boxes        |

## Stage Map

| Stage | Use When                                                | Primary Outputs                                                                            | Default Next Stage |
| ----- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ------------------ |
| 1     | Idea is still fuzzy and needs structured context        | `app-outline.md`                                                                           | 2                  |
| 2     | Product requirements need to be written or formalized   | `prd.md` or `plan.md`                                                                      | 3                  |
| 3     | A finalized PRD/plan needs technical decomposition      | `trd.md`, `trd-summary.md`                                                                 | 4                  |
| 4     | A finalized TRD needs executable work-unit prompts      | `orchestration.md`, `memory.md`                                                            | 5                  |
| 5     | Approved orchestration is ready to run                  | updated orchestration/memory + `updates/`                                                  | 6                  |
| 6     | Branch work needs a TRD completeness audit before tests | `implementation-cross-check.md`                                                            | 7                  |
| 7     | Tests need planning or execution                        | `test-analysis.md` and optionally `test-trd.md`, `test-orchestration.md`, `test-memory.md` | 8                  |
| 8     | Final branch state needs a post-test audit              | `post-test-cross-check.md`                                                                 | 9                  |
| 9     | Branch is final and only write-up remains               | `pr-description.md`                                                                        | done               |

## Stage 1: App Outline / Context Gathering

Use when the developer has an idea but no durable artifact yet.

1. Read `references/app-outline-template.md`.
2. Interview only for missing or unclear areas; do not walk the developer through the whole template if they already gave enough detail.
3. If repo or doc research is needed, use sub-agents to gather it.
4. Write `app-outline.md` in the active session folder using the template as a structural guide.
5. Stop for review. Do not move to Stage 2 until the developer is satisfied.

## Stage 2: PRD

Use when requirements need to be formalized.

Choose the lighter valid path:

- **Written PRD path**: use `references/prd-template.md` and draft `prd.md` from an outline or other durable context.
- **Plan-driven path**: use `references/plan-interview-guide.md` and `references/plan-template.md` when the developer knows the feature but has no written PRD, especially for cross-repo work.

Rules:

1. Use sub-agents for repo exploration and large document drafting.
2. Never include effort estimates in `plan.md`.
3. Before surfacing the artifact, run the shared protocol in `references/cross-check-protocol.md` against the source context and patch or report gaps.
4. Stop for review. Do not move to Stage 3 until the developer explicitly confirms the PRD or plan is final.

## Stage 3: TRD

Use when a finalized PRD or plan needs technical decomposition.

1. Read `references/trd-template.md`.
2. Use the finalized Stage 2 artifact as the source document.
3. Use a sub-agent to generate `trd.md`.
4. Always generate a concise `trd-summary.md` for humans who need the design without work-unit detail.
5. Run the shared cross-check protocol against the Stage 2 source and both TRD outputs.
6. Stop for review. Do not move to Stage 4 until the developer explicitly confirms the TRD is final.

## Stage 4: Orchestration Plan

Use when a finalized TRD needs executable, self-contained work-unit prompts.

1. Read `references/orchestration-instructions.md`, `references/orchestration-template.md`, and `references/memory-template.md`.
2. Use a sub-agent to generate `orchestration.md` and `memory.md`.
3. Make every work-unit prompt self-contained enough to pass directly to `task(description, prompt)`.
4. Make `memory.md` the orchestrator-owned coordination file and point sub-agents to per-work-unit update files.
5. Run the shared cross-check protocol against the TRD, orchestration plan, and memory file.
6. Stop for review. Do not begin Stage 5 until the developer explicitly approves execution.

## Stage 5: Execute the Orchestration Plan

Use when the orchestration plan has been reviewed and approved.

1. Read `orchestration.md` and `memory.md` from the active session folder.
2. Check prerequisites first. If any are missing, stop and ask the developer to address them.
3. Execute layer by layer.
4. For each layer, spawn parallel sub-agents using the full prompt in each work unit.
5. Sub-agents read `memory.md` and write results to the appropriate update file. The orchestrator consolidates status into shared artifacts.
6. If any work unit fails, stop before the next layer and ask whether to retry, modify, or skip.
7. When execution completes, summarize outcomes and offer Stage 6 as the default next step.

## Stage 6: Implementation Cross-Check

Use when implementation is done or mostly done and the developer wants to verify completeness against the TRD before tests.

1. Read the active TRD, TRD summary, orchestration, and memory artifacts.
2. Inspect the current branch diff and resulting code state.
3. Spawn overlapping read-only audit sub-agents unless the branch is too small to justify it.
4. Give audit agents distinct lenses such as TRD traceability, execution truth check, and gap detection.
5. Consolidate findings into `implementation-cross-check.md` with:
   - `Fully Satisfied`
   - `Gaps / Missing Work`
   - `Recommended Fixes`
6. If gaps exist, stop and ask whether to patch now, update the TRD to reflect intentional deviations, or accept specific deviations explicitly.
7. Use the shared cross-check protocol to cap the loop at 2 passes.

## Stage 7: Test Coverage

Use when tests need analysis, planning, or execution.

1. Read `references/test-coverage-instructions.md`.
2. Ask for any existing test guidelines, context files, or style examples.
3. Choose the lightest safe path:
   - **Direct test path**: for small or localized diffs, write `test-analysis.md`, stop for review, then write tests directly.
   - **Planned test path**: for large, risky, or cross-cutting diffs, generate `test-trd.md`, `test-orchestration.md`, and `test-memory.md`, stop for review, then execute the approved test orchestration.
4. Preserve traceability from planned coverage back to changed behavior so Stage 8 can audit it cleanly.
5. After test work completes, point the developer to Stage 8 instead of jumping straight to PR description.

## Stage 8: Implementation Cross-Check After Tests

Use when implementation and test work are both in place and the branch needs a final audit before write-up.

1. Read the implementation artifacts plus any test-analysis, test TRD, test orchestration, and test memory artifacts that exist.
2. Inspect the final branch state.
3. Spawn overlapping read-only audit sub-agents unless the branch is too small to justify it.
4. Audit three things: final TRD completeness, test completeness, and planning-artifact truthfulness.
5. Consolidate findings into `post-test-cross-check.md` with the same three sections as Stage 6.
6. If gaps exist, stop and ask whether to patch now, update the planning artifacts, or accept explicit deviations.
7. Use the shared cross-check protocol to cap the loop at 2 passes.

## Stage 9: PR Description

Use only when the branch is final and no more code or test changes are expected.

1. Read `references/pull-request-instructions.md`.
2. Confirm the branch is really final. If not, route back to the correct earlier stage.
3. Use a sub-agent to compare the current branch against the default branch and summarize the diff.
4. Optionally read `references/ado-wiki-markdown-reference.md` for markdown quirks.
5. Write `pr-description.md` in the active session folder.
6. Keep it under 4000 characters and target under 1000 when possible.
7. Show the result and ask whether the developer wants edits.

## Model Guidance

Use `references/model-routing.md` for the current model recommendation and mismatch handling.

Default principle: model guidance is advisory, not a reason to block useful work. Suggest a switch when the stage is heavy enough that quality is likely to suffer, but do not repeatedly interrupt the workflow with mandatory confirmation loops.

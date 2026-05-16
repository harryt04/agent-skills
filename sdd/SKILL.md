---
name: sdd
description: >
  Deterministic spec-driven development workflow for heavyweight software work. Use whenever the user invokes /sdd,
  asks for deterministic staged planning, wants stricter PRD/TRD/orchestration gates, needs grill-me style clarification
  before moving between planning stages, or wants orchestration execution that delegates implementation to sub-agents
  instead of improvising inline. Prefer this over looser planning flows when the user wants hard model gates,
  explicit approval between stages, separate gate files, and repeatable execution with cross-checks.
---

# Spec-Driven Development (SDD) Workflow

Use this skill for heavyweight work that needs a deterministic, gated workflow. Do not force it onto small tickets or one-off coding tasks.

## Core Contract

- This workflow has 6 stages and never auto-advances.
- A passed gate is not permission to continue. Stop after every stage and wait for explicit user approval.
- Enforce model-family gates as hard stops. If the current model is not approved for the requested stage, stop and tell the user to switch. Proceed on the wrong model only if the user explicitly overrides after being warned.
- Read only the minimum required artifacts for the current stage.
- Delegate research, repo exploration, audits, and implementation to sub-agents wherever that makes sense.
- Prefer acting on sub-agent summaries over broad inline exploration.
- If session size or drift risk gets high, stop at a clean checkpoint instead of pushing through.

## Bare Shortcut Output

If the developer enters exactly `/sdd` with no extra prompt, output the following block exactly and do nothing else.

=== BEGIN SDD ORIENTATION OUTPUT ===

This is the **SDD** skill — a 6-stage deterministic spec-driven workflow for heavyweight software work. You can enter at any stage.

| Stage | Name                                                   | Recommended Model Family     |
| ----- | ------------------------------------------------------ | ---------------------------- |
| 1     | Context / Grilled Planning Input                       | gpt, sonnet, haiku if simple |
| 2     | PRD                                                    | gpt or opus                  |
| 3     | TRD                                                    | gpt or opus                  |
| 4     | Orchestration                                          | gpt                          |
| 5     | Execution + Cross-Check + Build/Lint/Test Verification | codex or sonnet              |
| 6     | PR Description                                         | gpt, sonnet, haiku if simple |

=== END SDD ORIENTATION OUTPUT ===

Do not add commentary before or after the block.

## Entry Rules

Use this precedence order every time:

1. If the developer entered bare `/sdd`, output the orientation block exactly and stop.
2. If the developer provided an explicit artifact path or stage request, trust that first.
3. If the developer provided a prompt that clearly implies a stage, route to that stage.
4. If the stage still cannot be determined, state that the stage could not be determined automatically, then output the same orientation block exactly and stop.

Important routing rules:

- Do not auto-discover, auto-resume, or scan `.agents/specs/*` or `.agents/spec/*` trying to infer the active session.
- If the developer is resuming, expect them to provide the relevant file paths.
- If Stage 1 starts from context only and no planning-file paths are provided, use `.agents/spec/` as the working folder.
- If `.agents/spec/` already contains colliding canonical files for the new session, stop and ask the user to reorganize or provide a different directory.
- If the developer says `execute path/to/orchestration.md`, treat Stage 4 as complete enough to attempt Stage 5. Do not reopen upstream stages unless a concrete prerequisite is missing.

## Reference Files

Read only the files needed for the active stage.

| File                               | Purpose                                                               |
| ---------------------------------- | --------------------------------------------------------------------- |
| `references/model-routing.md`      | Hard model-family gate rules by stage                                 |
| `references/artifact-rules.md`     | Session folder rules, canonical filenames, and path handling          |
| `references/gate-templates.md`     | Fixed gate report templates for each planning and execution gate      |
| `references/work-unit-template.md` | Required work-unit contract for orchestration                         |
| `references/execution-rules.md`    | Stage 5 execution topology, cross-check lenses, and remediation rules |

## Stage Map

| Stage | Use When                                         | Primary Outputs                                             |
| ----- | ------------------------------------------------ | ----------------------------------------------------------- |
| 1     | Idea is fuzzy and needs clarified planning input | `context.md`, `context-gate.md`                             |
| 2     | Requirements need a durable product artifact     | `prd.md`, `prd-gate.md`                                     |
| 3     | PRD needs technical decomposition                | `trd.md`, `trd-gate.md`                                     |
| 4     | TRD needs executable work-unit instructions      | `orchestration.md`, `orchestration-gate.md`, `memory.md`    |
| 5     | Approved orchestration is ready to run           | updated `memory.md`, `updates/`, `execution-cross-check.md` |
| 6     | Branch is final and needs write-up               | `pr-description.md`                                         |

## Global Stage Rules

- Use separate gate files rather than embedding review chatter in the main artifact.
- Treat unresolved questions as blockers unless they are explicitly recorded in the artifact as `assumption` or `wont-fix`.
- Business behavior, scope boundaries, and risk tradeoffs belong to the user. If the repo or source files can answer a question, research that first. Otherwise ask the user.
- Use the grill-me interviewing style for planning gates: ask one question at a time, keep the questioning targeted, and optimize for omitted requirements, hidden assumptions, edge cases, and overcomplication.
- For `context.md`, `prd.md`, and `trd.md`, always run a post-generation grill on the newly generated artifact before declaring the stage ready for the next one.
- If the post-generation grill produces no follow-up questions, say that explicitly instead of silently skipping the review.
- For planning work, use sub-agents by default for research, repo inspection, drafting, and cross-checking so the orchestrator keeps a small context footprint.

## Stage 1: Context / Grilled Planning Input

Use when the developer has an idea, rough requirements, or scattered context and wants to create planning input.

1. Enforce the Stage 1 model-family gate using `references/model-routing.md`.
2. Resolve the working folder using `references/artifact-rules.md`.
3. Read `AGENTS.md` and any files explicitly provided by the developer. Do not fan out into unrelated planning files.
4. If repo context matters, spawn one research sub-agent to inspect the relevant code or docs and summarize constraints.
5. Run a grill-me style clarification pass inline, one question at a time, focused on omitted requirements, hidden assumptions, edge cases, testing expectations, and overcomplication.
6. Draft `context.md`.
7. Run a post-generation grill on `context.md` before suggesting readiness for Stage 2. Ask targeted follow-up questions about missing requirements, hidden assumptions, edge cases, testing expectations, and overcomplication.
8. Apply any resulting updates to `context.md`.
9. Spawn one cross-check sub-agent to verify that the final `context.md` preserves the relevant source context and clearly marks any remaining assumptions or wont-fix items.
10. Write `context-gate.md` using the Stage 1 gate template from `references/gate-templates.md`.
11. Stop and wait for explicit approval before Stage 2.

## Stage 2: PRD

Use when the developer wants a durable product requirements artifact.

1. Enforce the Stage 2 model-family gate.
2. Read `context.md` or the explicitly provided source artifact.
3. If repo context matters, spawn one research sub-agent first.
4. Run a grill-me style clarification pass against the current source artifact before conversion.
5. Draft `prd.md`. Include high-level testing expectations for the new behavior.
6. Run a post-generation grill on `prd.md` before suggesting readiness for Stage 3.
7. Apply any resulting updates to `prd.md`.
8. Spawn one cross-check sub-agent to compare the final `prd.md` against the source artifact and identify dropped, changed, or newly assumed requirements.
9. Write `prd-gate.md` using the Stage 2 gate template.
10. Stop and wait for explicit approval before Stage 3.

## Stage 3: TRD

Use when the developer wants a technical design derived from the approved PRD.

1. Enforce the Stage 3 model-family gate.
2. Read `prd.md` or the explicitly provided source artifact.
3. If repo context matters, spawn one research sub-agent first.
4. Run a grill-me style clarification pass against the PRD before conversion.
5. Draft `trd.md`. Turn high-level testing expectations into concrete test requirements per feature or work unit.
6. Run a post-generation grill on `trd.md` before suggesting readiness for Stage 4.
7. Apply any resulting updates to `trd.md`.
8. Spawn one cross-check sub-agent to compare the final `trd.md` against the source PRD and identify anything dropped, changed, or newly assumed.
9. Write `trd-gate.md` using the Stage 3 gate template.
10. Stop and wait for explicit approval before Stage 4.

## Stage 4: Orchestration

Use when the approved TRD needs self-contained work units for delegated execution.

1. Enforce the Stage 4 model-family gate.
2. Read `trd.md` or the explicitly provided source artifact.
3. Run a grill-me style clarification pass against the TRD before conversion.
4. Draft `orchestration.md` and `memory.md`.
5. Every work unit must follow `references/work-unit-template.md`.
6. `orchestration.md` should be the TRD translated into delegated execution instructions, not a brand new design.
7. `memory.md` is orchestrator-owned state. Sub-agents may read it but should report through per-work-unit update files.
8. Spawn one cross-check sub-agent to compare the source TRD against `orchestration.md` and `memory.md` and identify anything dropped, changed, or newly assumed.
9. Write `orchestration-gate.md` using the Stage 4 gate template.
10. Do not stop for artifact review by default just because `orchestration.md` exists. The user already said the real review gates are earlier. Still stop here because stages never auto-advance.
11. Wait for explicit approval before Stage 5.

## Stage 5: Execution + Cross-Check + Build/Lint/Test Verification

Use when the developer explicitly asks to execute an orchestration artifact.

1. Enforce the Stage 5 model-family gate.
2. Confirm these blocking prerequisites exist:
   - the target `orchestration.md`
   - the companion `memory.md` or other required memory files
   - explicit user approval to execute
3. Read only the minimum required artifacts: the orchestration, memory, AGENTS instructions, and any files referenced as immediate prerequisites.
4. The top-level orchestrator must never write implementation code itself during Stage 5.
5. The top-level orchestrator may run formatting, build, lint, and test commands, and may update `memory.md`, `execution-cross-check.md`, and orchestrator-owned status artifacts.
6. Execute one layer at a time.
7. Spawn one sub-agent per work unit in the active layer. Give each sub-agent the full work-unit prompt plus the path to `memory.md` and its update file.
8. Consolidate sub-agent summaries back into `memory.md` before moving to the next layer.
9. After execution, run overlapping cross-checkers using `references/execution-rules.md`.
10. Always include these lenses:

- requirements traceability
- wiring and completeness
- test compliance

11. Add extra lenses when relevant, such as security, backward compatibility, performance, or data migration risk.
12. If cross-checkers find small, mechanical fixes such as typos, broken references, or small build issues, spawn fix sub-agents and repeat the loop `cross-check -> fix -> verify`.
13. If cross-checkers find a deeper problem that suggests re-planning, conflicting artifacts, or a serious intervention, stop, summarize what is wrong, and wait for the user.
14. Verification is not optional. Before Stage 5 can be considered complete, run repo-appropriate build, lint, and locally runnable CI-style tests if they exist.
15. Write `execution-cross-check.md` using the Stage 5 templates from `references/gate-templates.md`.
16. Stop and wait for explicit approval before Stage 6.

## Stage 6: PR Description

Use only when the branch is final and no more code or test changes are expected.

1. Enforce the Stage 6 model-family gate.
2. Treat the changed files in the branch as the primary source of truth. Read additional context only if the developer explicitly includes context files in the Stage 6 prompt.
3. Do not read `prd.md`, `trd.md`, `orchestration.md`, `memory.md`, or other planning artifacts unless the developer explicitly includes them for Stage 6.
4. Use sub-agents to understand the git diff before drafting the description. If more than 15 files changed, strongly prefer splitting the diff across multiple sub-agents by subsystem, area, or other natural grouping, then synthesize their summaries.
5. Optimize for fellow software developers reviewing the branch. Focus on what changed, what matters, what is risky, and what may affect their own work.
6. Emphasize behavior changes, integration points, risky refactors, schema or data-shape changes, externally visible contract changes, and anything that may affect other open branches or require teammates to pull latest `master`.
7. Use concrete identifiers when they materially improve reviewer orientation, such as API names, GraphQL fields or types, major services, tables or collections, config keys, feature flags, background jobs, and externally visible endpoints.
8. Use Azure DevOps-safe markdown only. Prefer headings, numbered lists, short paragraphs, and simple pipe tables. Avoid relying on advanced HTML, collapsible sections, alerts, wiki-only markdown features, or other formatting that may render inconsistently in Azure DevOps PR descriptions.
9. Use one canonical information contract, but omit sections that do not add value for the current branch. The usual information categories are summary, what changed, reviewer focus or risks, breaking changes, and follow-up. You may rename headings when that improves clarity for the specific PR.
10. Choose the format that makes each section easiest to scan at a glance. Prefer numbered lists when later reference will help reviewers. Use simple tables when they make grouped facts, impacted areas, or before-and-after data easier to consume. Do not force a section into prose if a table or numbered list is clearer.
11. Do not include a testing section. Do not include screenshots or examples by default. Mention verification only if it materially changes reviewer understanding, such as an important validation gap or a manual concern reviewers should know about.
12. Keep the description concise, neutral, professional, and low ceremony. Avoid hype, filler, and generic process narration.
13. Never exceed 4000 characters. For shorter or easier-to-understand branches, target roughly 1500 characters. For larger branches, spend more of the budget only when the added detail materially improves reviewer understanding.
14. If the draft is too long, decide what is least important and condense or drop it. Preserve reviewer-critical signal over completeness. Collapse repetitive mechanical edits and low-signal churn unless they help reviewers understand the branch.
15. `Breaking Changes` should include anything that may force another developer to react, including contract changes, renamed or removed behavior, changed defaults or assumptions, migration-sensitive changes, and changes that may disrupt other active branches or PR builds.
16. `Follow-Up` should include only concrete post-merge work, known deferred tasks, or explicit non-goals that reviewers should know were intentionally left out. Do not use it as a dumping ground for vague future ideas.
17. Draft `pr-description.md` as Azure DevOps-ready markdown only. The file should contain just the final PR description text for copy-paste, with no diagnostics, no character-count note, and no internal commentary.
18. Show the result and wait for edits.

## Stop Conditions

Stop and wait for the user when any of the following happens:

- the active model family is not approved for the stage and the user has not overridden it
- a required source artifact is missing
- a gate fails
- the artifact still contains unresolved questions that are not explicitly recorded as assumptions or wont-fix items
- cross-checking reveals a deeper problem that needs serious intervention
- `.agents/spec/` would collide with an existing session for a new Stage 1 effort
- the session is getting too large and the clean next move is to checkpoint and hand back control

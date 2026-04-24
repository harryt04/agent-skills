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

This skill guides heavyweight software work through a structured, document-driven workflow. Use it when the work is large enough that durable artifacts, staged execution, and explicit audits reduce risk. Do not force this process onto small or local asks.

## Activation Router

Before starting Stage 1, classify the request into one of three paths:

1. **No-skill path:** The ask is small, local, or already concrete enough to execute directly. Examples: a small ticket, one test file, a lightweight refactor, a basic architecture question, a simple PR description. Do not use this skill.
2. **Partial-flow path:** The ask is medium-sized or the developer already has artifacts. Start at the first missing heavyweight stage, usually TRD, orchestration, audit, or PR description.
3. **Full-flow path:** The ask is cross-repo, high-risk, ambiguous, or needs formal planning and staged execution. Run the full pipeline.

Default routing:

- Single-repo and under roughly 3 meaningful work units: usually no-skill path
- Single-repo but high-risk, ambiguous, or requiring durable planning: partial-flow path
- Cross-repo, tall-thin-slice, branch-wide audit, or formal spec work: full-flow path

If uncertain, ask one short routing question instead of starting the full pipeline by default.

## The Pipeline

```
Stage 1: App Outline / Context Gathering   (sonnet 4.6 default; haiku latest for lightweight)
Stage 2: PRD — Product Requirements Doc   (opus 4.6 default; gpt 5.4 strong alternative)
Stage 3: TRD — Technical Requirements Doc (opus 4.6 default; gpt 5.4 strong alternative)
Stage 4: Orchestration Plan               (gpt 5.4 default; opus 4.6 alternative)
Stage 5: Execute the Orchestration Plan   (codex 5.3 default per WU; sonnet 4.6 alternative)
Stage 6: Implementation Cross-Check       (gpt 5.4 default; opus 4.6 alternative)
Stage 7: Test Coverage                    (sonnet 4.6 default; codex 5.3 for test writing)
Stage 8: Implementation Cross-Check After Tests (gpt 5.4 default; opus 4.6 alternative)
Stage 9: PR Description                   (haiku latest default; sonnet 4.6 alternative)
```

You can enter at any stage. If you already have a TRD, start at Stage 4. If execution is already done, jump straight to Stage 6 for implementation cross-check. If tests are already implemented and you need a final audit before write-up, jump to Stage 8. If you just want a PR description and the branch is truly final with no more code changes needed, jump straight to Stage 9.

## Default Orientation Behavior

When the user has not provided enough context to determine their stage or intent, output the following block EXACTLY as written. Do not paraphrase, reorder, summarize, or add to it. This is a script — copy it character-for-character, substituting only the placeholder marked with angle brackets.

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
| 7     | Test Coverage                          | sonnet 4.6 (codex 5.3 for heavy test writing) |
| 8     | Implementation Cross-Check After Tests | gpt 5.4 (or opus 4.6)                         |
| 9     | PR Description                         | haiku latest (or sonnet 4.6)                  |

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

After outputting this block, proceed to resume discovery or routing as described in the sections below. Do not add commentary before or after the block.

## Session Folder Selection

Before reading or writing planning artifacts, resolve a single session folder for the current request:

1. If the developer explicitly mentions a work item ID, use `.agents/specs/<work-item-id>/`.
2. If they previously referenced a path like `.agents/specs/160409/...` in the conversation, treat that as the active session folder and continue there.
3. Otherwise, use `.agents/spec/` as the default non-ticket session folder.

Keep all artifacts for that session in the selected folder unless the developer explicitly switches context.

## Artifact Naming Convention

For any newly created planning artifact, use lowercase kebab-case names:

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

Compatibility rules:

- If an artifact already exists with a different casing or older name, keep using that existing file when reading or editing it.
- Do not rename existing files unless the developer explicitly asks.
- When discovering resumable context, check both canonical names and common existing aliases such as `PRD.md`, `TRD.md`, `TRD-summary.md`, `ORCHESTRATION.md`, `IMPLEMENTATION-CROSS-CHECK.md`, `TRD-tests.md`, `ORCHESTRATION-tests.md`, `memory-tests.md`, `updates-tests/`, and `IMPLEMENTATION-CROSS-CHECK-AFTER-TESTS.md`.

## Resume Discovery Mode

When the developer asks for `/spec-driven-development` with no additional prompt, do not assume a single session folder yet. First scan for resumable session candidates across:

1. `.agents/spec/`
2. every direct child folder under `.agents/specs/`

For each candidate folder, inspect for recognizable planning artifacts using the canonical lowercase names first and the common aliases second. Summarize:

- the folder path
- which artifacts exist
- the highest likely stage reached
- whether it appears actively resumable

If more than one folder looks resumable, present the strongest candidate first and then list the other viable options. Only after this discovery step should you ask whether to resume an existing session or start a new one.

---

## Special Shortcut: `/spec-driven-development`

If the developer enters exactly `/spec-driven-development` with no additional prompt, or exactly `/spec-driven-development plan` as a compatibility alias:

1. Do not begin Stage 1 or generate any files.
2. Output the orientation block EXACTLY as written in the `Default Orientation Behavior` section above — character-for-character, no paraphrasing or summarizing.
3. Then run resume discovery mode across `.agents/spec/` and `.agents/specs/*/`.
4. Report:
   - which folders you checked
   - which artifacts are visible in each resumable candidate
   - the highest likely stage reached for each viable candidate
   - whether a resumable session was found
5. If a resumable session exists, ask whether they want to continue from that context or start a new session.
6. If no resumable session exists, say that clearly and ask what they want to start with.

The shortcut is a status-and-routing command, not a stage-execution command.

For natural-language invocations that trigger this skill, also start with the default orientation behavior before routing or artifact discovery unless the developer clearly asks to resume a specific stage immediately.

---

## How to Begin

First, figure out where the developer is:

1. If this is a fresh invocation and the developer has not provided enough context to determine their stage or intent, output the orientation block EXACTLY as written in the `Default Orientation Behavior` section above — character-for-character, no paraphrasing or summarizing. Then proceed below.
2. If the developer entered exactly `/spec-driven-development` with no additional prompt, or the compatibility alias `/spec-driven-development plan`, then run resume discovery mode and stop after asking whether to resume or start fresh.
3. Otherwise resolve the session folder.
4. Check that folder for existing documents using the canonical lowercase names first, then the common aliases listed above.
5. Route the request using the Activation Router above.
6. Infer the most likely stage from the developer's request, existing artifacts, and branch state. Say which stage you believe they are at and why before beginning stage work.
7. If no artifacts exist and the work is heavyweight, ask what they want to do — or jump straight to Stage 1 if they've already described the problem.
8. If artifacts exist, identify the latest complete stage and continue from there instead of regenerating earlier documents.

If the intent is unclear, ask: "Where would you like to start? I can help you outline an idea, write a PRD, generate a TRD, build an orchestration plan, run an implementation cross-check, add test coverage, or write a PR description once the branch is final."

When the developer does not know which stage to start with, ask one short routing question in plain language about their current ticket state, such as whether they are still defining scope, already implementing, auditing a branch, or only need PR write-up.

## Global Rules

- Shared planning files are **orchestrator-owned**. Sub-agents may read the session memory file and the active orchestration artifact, but they should not update shared status files directly.
- If sub-agents need to report progress or results, have them write a per-work-unit report under `<session-folder>/updates/` (or `<session-folder>/test-updates/` for test work). The orchestrator consolidates those reports into shared artifacts.
- Cross-check loops are not open-ended. Run at most 2 passes before surfacing unresolved ambiguity to the developer.
- If a stage reveals intentional scope deviation, record it explicitly in the stage output and carry it forward rather than repeatedly re-flagging it.
- Prefer entering at the latest viable stage. Do not generate PRD/TRD/orchestration artifacts just because the skill triggered.
- In user-facing responses, refer to the actual file that exists when editing a prior artifact. When creating a new artifact, use the lowercase canonical filename.

---

## Reference Files

All templates and prompt instructions are in the `references/` directory alongside this file. Read them when you reach the relevant stage — there is no need to load them all upfront.

| File                                        | Used in Stage                                    |
| ------------------------------------------- | ------------------------------------------------ |
| `references/app-outline-template.md`        | Stage 1                                          |
| `references/prd-template.md`                | Stage 2 (written PRD path)                       |
| `references/plan-template.md`               | Stage 2 (plan-driven PRD path)                   |
| `references/plan-interview-guide.md`        | Stage 2 (plan-driven PRD path)                   |
| `references/trd-template.md`                | Stage 3                                          |
| `references/orchestration-template.md`      | Stage 4                                          |
| `references/memory-template.md`             | Stage 4 (companion memory file)                  |
| `references/orchestration-instructions.md`  | Stage 4 (how to build the plan)                  |
| `references/test-coverage-instructions.md`  | Stage 7                                          |
| `references/pull-request-instructions.md`   | Stage 9                                          |
| `references/ado-wiki-markdown-reference.md` | Stage 9 (optional markdown formatting reference) |

---

## Stage 1: App Outline / Context Gathering

**Recommended model:** Haiku or Sonnet — this is a brainstorming and interview stage, not heavy reasoning.

**Goal:** Capture the developer's vision in a structured outline that will seed the PRD.

**Steps:**

1. Read `references/app-outline-template.md`.
2. Interview the developer. You don't need to ask every question in the template — ask only about areas that are unclear or missing from what they've already told you. Template sections: Vision, Mission, Governance, Feature List (MVP + Wishlist), Architecture, Pricing, Communication & Project Management, Resources.
3. If the project context is complex and you need to research the codebase or existing docs, use a sub-agent — this keeps the orchestrating session lean.
4. Write a filled-in outline to `<session-folder>/app-outline.md` using the template as a structural guide.

**Gate:** Show the developer the outline. Ask them to review it and let you know when they're satisfied before proceeding to Stage 2.

---

## Stage 2: PRD — Product Requirements Document

**Recommended model:** Opus — this requires nuanced reasoning about architectural trade-offs, scope, and product decisions.

**Suggest a model switch before starting this stage if the developer is on a lighter model.**

**Goal:** Produce a document that captures what's being built in enough detail to generate a TRD. There are two paths to get there depending on where the developer is starting from.

### Choosing a path

- **Written PRD path:** The developer already has a written PRD, or they're building a single-repo product and want to generate a formal PRD from an outline. Follow the steps below.
- **Plan-driven PRD path:** The developer knows what they want to build (often a feature spanning multiple repositories) but doesn't have a written PRD. They have repo paths, maybe a work item, maybe some API docs — but no structured requirements document yet. Read `references/plan-interview-guide.md` and `references/plan-template.md`, then follow the interview-driven flow described there.

The plan-driven path is especially useful for tall-thin slice features that cut across server, web, and mobile repos. Instead of asking the developer to write a PRD upfront, the agent interviews them, explores the relevant codebases, and drafts a plan that serves as the PRD.

### Written PRD path

1. Read `references/prd-template.md`.
2. Read `<session-folder>/app-outline.md` if it exists (or use whatever context the developer has provided if no outline file exists).
3. Use a sub-agent to generate the PRD — this keeps the orchestrating session's context window clean. The sub-agent prompt: read the outline, read the PRD template, write a complete PRD to `<session-folder>/prd.md` unless an existing PRD artifact in that folder should be updated in place.
4. The PRD should cover all template sections: Problem Statement, Product Overview, Target Users, MVP Feature Requirements, System Architecture, Data Model, Non-Functional Requirements, Pricing, Third-Party Integrations, Out of Scope, Testing & Operations, Success Criteria, and Appendix.

5. **Cross-check loop (written PRD path):** Before surfacing the PRD to the developer, spawn a cross-check sub-agent whose sole job is to compare the source outline against the generated PRD and identify anything that was dropped, misrepresented, or underdeveloped. The sub-agent should: read `<session-folder>/app-outline.md` (or the equivalent input), read the PRD artifact in the selected session folder, list every gap or omission, and either patch the PRD directly or produce a clear list of required additions. Run no more than 2 passes before surfacing unresolved ambiguity to the developer. Only then surface the document to the developer. Do not tell the developer the PRD is ready until this cross-check has passed.

**Gate:** Tell the developer where the PRD was written in the selected session folder. Please review and refine it until it meets all product requirements. Let me know when it's final and I'll generate the TRD.

### Plan-driven PRD path

1. Read `references/plan-interview-guide.md` and `references/plan-template.md`.
2. Follow the interview guide's phased approach: understand the feature, research the repositories (using sub-agents to explore each repo in parallel), ask targeted questions, then draft the plan.
3. Save to `<session-folder>/plan.md`.
4. Never include effort estimates (day counts, story points, t-shirt sizes) in the plan.
5. **Cross-check loop (plan-driven path):** Before surfacing the plan to the developer, spawn a cross-check sub-agent that compares every detail gathered during the interview against the generated plan — look for features, constraints, integration points, or decisions that were discussed but not reflected in the output. Patch any gaps. Run no more than 2 passes before surfacing unresolved ambiguity to the developer. Do not tell the developer the plan is ready until this passes.

**Gate:** Tell the developer: "Your draft plan is at [path]. Please review — once you're satisfied, this will serve as the basis for the TRD. Let me know when it's final."

Do not proceed to Stage 3 until the developer explicitly confirms the PRD or plan is final.

---

## Stage 3: TRD — Technical Requirements Document

**Recommended model:** Opus — work unit decomposition and dependency graph design are where mistakes become expensive.

**Suggest a model switch if needed.**

**Goal:** Translate the finalized PRD into a parallelizable, actionable Technical Requirements Document. Always produce two files: a verbose agent-ready TRD and a concise human-readable summary.

**Steps:**

1. Read `references/trd-template.md`.
2. Read the source document — either the PRD or the plan in the selected session folder (whichever was produced in Stage 2). Both contain sufficient detail to generate a TRD.
3. Use a sub-agent to generate the TRD. The sub-agent should: read the Stage 2 source document, read the TRD template, analyze the codebase structure if relevant (to understand existing patterns, file layout, and tech stack), then write a complete TRD to `<session-folder>/trd.md` unless an existing TRD artifact in that folder should be updated in place.
4. The TRD must include: Technology Decisions, Project Structure, Database Schema, Work Units (organized by dependency layer), Dependency Graph, Environment Variables, Testing Strategy, and Migration Notes.
5. Work units are the heart of the TRD. Each one must be self-contained: files to create/modify, acceptance criteria, test requirements, and an explicit list of which other work units it depends on. Units in the same layer should be fully independent of each other so they can run in parallel.
6. **Always generate a companion human-readable summary at `<session-folder>/trd-summary.md`.** This is a separate, concise document intended for teammates who understand the codebase and just need to understand the proposed changes. Keep it as short as possible. It should cover: what is being built (1–2 sentences), new components (table: component, location, purpose), modified components (table: component, change), data model changes, request/data flow (numbered steps), reused scaffolding, config changes, and error handling. Omit work unit decomposition, layer structure, acceptance criteria, and test scaffolding — those details belong only in the full TRD.
7. **Cross-check loop:** Before surfacing the TRD to the developer, spawn a cross-check sub-agent whose sole job is to compare the source PRD (or plan) against the TRD artifact and its summary in the selected session folder and identify anything dropped, misrepresented, or underspecified — including requirements, constraints, data model details, integration points, and non-functional requirements. The sub-agent should patch the TRD directly for any gaps found. Run no more than 2 passes before surfacing unresolved ambiguity to the developer. Do not tell the developer the TRD is ready until this cross-check has passed.

**Gate:** Tell the developer where the TRD and summary were written in the selected session folder. Please review and refine until you're satisfied. Let me know when they're final and I'll generate the orchestration plan.

Do not proceed to Stage 4 until the developer explicitly confirms the TRD is final.

---

## Stage 4: Orchestration Plan

**Recommended model:** Opus — translating a TRD into complete, agent-ready prompts requires precision; gaps here mean failed or confused sub-agents.

**Suggest a model switch if needed.**

**Goal:** Produce a ready-to-execute orchestration plan and companion memory file.

**Steps:**

1. Read `references/orchestration-instructions.md` and `references/orchestration-template.md`.
2. Read the TRD artifact in the selected session folder.
3. Use a sub-agent to generate both files. The sub-agent should:
   - Write the orchestration plan to `<session-folder>/orchestration.md`, following the template format, unless an existing orchestration artifact in that folder should be updated in place.
   - Write the memory file to `<session-folder>/memory.md`, using `references/memory-template.md` as the structural guide and adapting it to the actual project.
4. The orchestration plan must include: header metadata, prerequisites checklist, execution phases table, and fully self-contained work unit prompts. Each work unit's `### Prompt` section must be complete enough to pass directly to `task(description, prompt)` without needing additional context from the conversation.
5. The memory file must document: project root path, implementation status by phase (tracking each file's status), codebase patterns, key files to be modified, open questions with assumed defaults, coordination notes for race condition prevention, and the per-work-unit update location that sub-agents should write to.
6. **Cross-check loop:** Before surfacing either file to the developer, spawn a cross-check sub-agent that reads the TRD, orchestration, and memory artifacts together and verifies: every work unit in the TRD has a corresponding, fully-specified prompt in the orchestration plan; the dependency layers are faithfully preserved; no TRD acceptance criteria were dropped; the memory file accurately reflects all files to be modified and all open questions surfaced in the TRD. Patch any gaps found. Run no more than 2 passes before escalating unresolved ambiguity to the developer. Do not tell the developer the orchestration plan is ready until this cross-check has passed.

**Gate:** Tell the developer where the orchestration plan and memory file were written in the selected session folder. Please review both — once execution starts, agents will make real changes to your codebase. Let me know when you're ready to proceed.

Do not begin execution until the developer explicitly approves.

---

## Stage 5: Execute the Orchestration Plan

**Recommended model:** Haiku or Sonnet per work unit (use Sonnet for complex or high-risk units).

**Goal:** Execute the plan by dispatching parallel sub-agents layer by layer.

**Steps:**

1. Read the orchestration and memory artifacts in the selected session folder.
2. Check the prerequisites checklist at the top of the orchestration plan. If anything is missing, halt and ask the developer to address it first.
3. Execute layer by layer:
   - For each layer, spawn parallel `task()` calls — one per work unit in that layer. Pass each work unit's full `### Prompt` section as the task prompt.
   - Each sub-agent should read the session memory file for context, then write its outcome to `<session-folder>/updates/<work-unit-id>.md` instead of editing shared planning files directly.
   - Wait for all work units in the layer to complete before spawning the next layer.
   - The orchestrator is the only writer for the active orchestration and memory artifacts. After each layer, consolidate sub-agent updates into those files and update status checkboxes.
4. If a work unit fails, save the failure details in its per-work-unit update file, surface the error to the developer immediately, and do not continue to the next layer until they decide to retry, modify, or skip.

**After execution:** Summarize what was done, note any failures or skipped units, then ask: "Execution is complete. Do you want to run the Implementation Cross-Check now to verify TRD completeness before tests?" If they say no, explicitly tell them they can start a fresh session later and jump directly to Stage 6.

---

## Stage 6: Implementation Cross-Check

**Recommended model:** Sonnet or Opus — this stage requires careful requirement-by-requirement verification and gap detection.

**Goal:** Verify that implementation is complete and aligned with the finalized TRD at 100%, with nothing missed before test planning.

**Steps:**

1. Read the TRD, TRD summary (if present), orchestration, and memory artifacts in the selected session folder.
2. Inspect the current branch changes and resulting code state.
3. By default, spawn parallel read-only audit sub-agents to independently validate completeness. Use more than one audit agent unless the branch is so small that parallelization adds no value. Keep the audits intentionally overlapping enough to catch omissions from different angles.
4. Give the audit agents distinct primary lenses such as:
   - TRD traceability: map every requirement, work unit, and acceptance criterion to concrete implementation evidence (files, functions, tests, config, migrations, or docs).
   - Execution truth check: identify anything marked done in orchestration or memory that does not actually appear in code or configuration.
   - Gap detection: flag anything partially implemented, missing, inconsistent with the TRD, or implemented in a way that appears to violate stated constraints.
5. Audit agents are read-only. They inspect code, diffs, planning artifacts, and tests, but they do not patch files or make branch changes themselves.
6. Consolidate findings into `<session-folder>/implementation-cross-check.md` unless an existing cross-check artifact in that folder should be updated in place, with three sections:
   - `Fully Satisfied` (with evidence)
   - `Gaps / Missing Work`
   - `Recommended Fixes`
7. If gaps exist, do not proceed automatically. Present the gaps to the developer and ask whether to:
   - patch now,
   - update the TRD to reflect intentional scope changes,
   - or accept specific deviations explicitly.
8. Repeat this cross-check until either:
   - all required TRD items are covered, or
   - the developer explicitly signs off on known deviations.
     Stop after 2 cross-check passes without meaningful new findings and escalate the remaining ambiguity to the developer.

**Gate:** Tell the developer where the implementation cross-check report was written. Ask whether to proceed to test coverage (Stage 7) or address gaps first.

**If skipped now:** Record that Stage 6 was deferred and remind the developer they can open a fresh session and request: "Start at Stage 6 implementation cross-check" to run this phase immediately.

---

## Stage 7: Test Coverage

**Recommended model:** Haiku or Sonnet for analysis and implementation; Opus for the test TRD step.

**Goal:** Analyze branch changes and add the right amount of test planning. For small or localized diffs, go straight to direct test work. For large, risky, or cross-cutting diffs, generate a test TRD and orchestration plan so the branch can undergo a final post-test implementation audit.

**Steps:**

1. Read `references/test-coverage-instructions.md`.
2. Ask the developer: "Do you have any existing test guidelines, context files, or test style examples I should reference? If so, share the file paths."
3. Choose the test path:
   - **Direct test path:** small or localized branch changes with obvious test destinations. Summarize the needed coverage in `<session-folder>/test-analysis.md`, get approval, then write tests without generating the heavier test-planning artifacts.
   - **Planned test path:** large, risky, or cross-cutting changes. Generate `test-trd.md`, `test-orchestration.md`, and `test-memory.md` unless existing alias files in the selected session folder should be updated in place.
4. Use sub-agents to:
   - Analyze the changes on the current branch relative to master/main.
   - Produce a markdown summary of needed test cases per file/function, saved to `<session-folder>/test-analysis.md`.
   - Search for related existing tests to understand where new tests should go and what conventions to follow.
5. For the planned test path, use a sub-agent (Opus recommended) to generate a test-focused TRD at `<session-folder>/test-trd.md`, based on the test analysis and any context files provided, unless an existing test TRD artifact in that folder should be updated in place.
6. For the planned test path, generate a test orchestration plan at `<session-folder>/test-orchestration.md` and a companion memory file at `<session-folder>/test-memory.md`.
7. Make sure the test artifacts preserve enough traceability for a later read-only audit. The test TRD, orchestration plan, and memory file should make it easy to map each planned test back to the implementation behaviors and gaps it is meant to cover.

**Gate:**

- Direct test path: Tell the developer where `test-analysis.md` was written. Ask them to review before test writing begins.
- Planned test path: Tell the developer where the test TRD, test orchestration plan, and test memory file were written. Ask them to review before execution begins.

Do not execute the test orchestration until the developer approves.

8. For the planned test path, execute the test orchestration plan using the same layer-by-layer parallel approach as Stage 5, with the orchestrator as the only writer of shared planning files.
9. After test execution, do not jump straight to PR description. Tell the developer the default next step is Stage 8, a final implementation cross-check by read-only audit sub-agents to verify the branch is truly done before PR write-up.

---

## Stage 8: Implementation Cross-Check After Tests

**Recommended model:** Sonnet or Opus — this stage is a final audit of the full branch state, including the tests that were just added.

**Goal:** Verify that the final branch state is complete after test implementation, with implementation, tests, and planning artifacts all aligned before PR write-up.

**Steps:**

1. Read the implementation artifacts in the selected session folder, plus `test-analysis.md`, `test-trd.md`, `test-orchestration.md`, and `test-memory.md` when present. If the folder contains older alias names instead, use those existing files.
2. Inspect the current branch changes and resulting code-and-test state.
3. By default, spawn parallel read-only audit sub-agents. Use multiple audit agents unless the branch is small enough that a single pass is clearly sufficient.
4. Give the audit agents distinct primary lenses such as:
   - Final TRD completeness: verify the implementation still satisfies the finalized TRD and any explicitly accepted deviations are the only remaining gaps.
   - Test completeness: verify implemented tests cover the behaviors and risks identified in `test-analysis.md` and the active test TRD artifact, and that claimed coverage actually exists in the repository.
   - Planning artifact truth check: verify orchestration and memory files for both implementation and test work reflect the real final branch state.
5. Audit agents are read-only. They inspect and report, but they do not patch files or modify planning artifacts.
6. Consolidate findings into `<session-folder>/post-test-cross-check.md` unless an existing post-test cross-check artifact in that folder should be updated in place, with three sections:
   - `Fully Satisfied` (with evidence)
   - `Gaps / Missing Work`
   - `Recommended Fixes`
7. If gaps exist, do not start PR description work. Present the gaps to the developer and ask whether to:
   - patch now,
   - update the TRD or test-planning artifacts to reflect intentional scope changes,
   - or accept specific deviations explicitly.
8. Repeat this cross-check until either:
   - the branch is complete with no further changes needed, or
   - the developer explicitly signs off on known deviations and confirms the branch is otherwise final.
     Stop after 2 passes without meaningful new findings and escalate the remaining ambiguity to the developer.

**Gate:** Tell the developer where the post-test implementation cross-check report was written. If there are no more branch changes to make, the next step is PR description (Stage 9).

---

## Stage 9: PR Description

**Recommended model:** Haiku or Sonnet — this is a summarization task.

**Goal:** Generate a concise, well-structured PR description from the final branch diff once there are no more branch changes to make.

**Steps:**

1. Read `references/pull-request-instructions.md`.
2. Confirm with the developer that the branch is final and no further code or test changes are expected. If more changes are still likely, stop here and return to the appropriate earlier stage instead of drafting the PR description prematurely.
3. Use a sub-agent to compare the current branch against master/main and summarize the diff.
4. Optionally read `references/ado-wiki-markdown-reference.md` for markdown syntax guidance — it documents what renders well in browser-based PR text boxes (tables, numbered lists, code blocks all work; some advanced features like Mermaid do not).
5. Write the PR description to `<session-folder>/pr-description.md`.
6. Hard limit: 4000 characters. Target: under 1000 characters. Use numbered lists and tables where they aid readability; prefer them over prose and bullet points.

**Paste reminder:** Tell the developer — when pasting the PR description into their platform's web UI, use **Paste and Match Style** (Cmd+Shift+V on Mac, Ctrl+Shift+V on Windows/Linux) to avoid rich-text formatting artifacts.

**Gate:** Show the description and ask if they want any changes.

---

## Model Switching Guidance

At each stage transition, briefly mention the recommended model and why. These are defaults — use your judgment if the project's complexity calls for something different.

| Stage                       | Recommended                                   | Why                                         |
| --------------------------- | --------------------------------------------- | ------------------------------------------- |
| 1 — Outline                 | sonnet 4.6 (haiku latest if lightweight)      | Balanced interview quality/speed            |
| 2 — PRD                     | opus 4.6 (or gpt 5.4)                         | High-stakes scope and product trade-offs    |
| 3 — TRD                     | opus 4.6 (or gpt 5.4)                         | Decomposition and dependency precision      |
| 4 — Orchestration           | gpt 5.4 (or opus 4.6)                         | Prompt completeness/instruction fidelity    |
| 5 — Execution               | codex 5.3 (or sonnet 4.6)                     | Efficient code-level work per unit          |
| 6 — Cross-Check             | gpt 5.4 (or opus 4.6)                         | Maximum gap detection and traceability      |
| 7 — Tests                   | sonnet 4.6 (codex 5.3 for heavy test writing) | Good balance for analysis + implementation  |
| 8 — Cross-Check After Tests | gpt 5.4 (or opus 4.6)                         | Final branch audit before write-up          |
| 9 — PR                      | haiku latest (or sonnet 4.6)                  | Fast summarization once the branch is final |

To switch models in OpenCode: use the model picker in the interface, or start a new session with the target model active.

---

## Mandatory Model Gate (Do Not Skip)

At the start of **every stage**, the orchestrator must run this check before doing stage work:

1. Identify the currently active model.
2. State which stage you believe the developer is currently at and why.
3. Compare the current model with the recommendation for that stage.
4. If model is recommended, say so plainly and proceed normally.
5. If model is not recommended:
   - Warn clearly.
   - Explain expected trade-off in one sentence (quality/risk/speed/cost).
   - Ask for explicit confirmation before proceeding.
   - Do not continue stage execution until confirmation is received.

Before giving the mismatch prompt, include one short sentence that maps the developer's current ticket state to the selected stage. Example: `This looks like Stage 4 because your TRD already exists and you need agent-ready execution prompts next.`

Use this exact prompt format on mismatch:

`Model mismatch for Stage X: recommended <model(s)>, current <model>. To switch in OpenCode: Ctrl+P → change model, or start a new session with the target model active. Reply with: (1) Switch model now, or (2) Proceed anyway on current model.`

If the developer selects **(1) Switch model now**:

- Pause stage work.
- Instruct them to switch via model picker or new session.
- Resume from the same stage once they confirm the switch.

If the developer selects **(2) Proceed anyway**:

- Continue the stage on current model.
- Record this as an intentional deviation in stage output notes (and `memory.md` if present).

---

## Sub-Agent Philosophy

Use sub-agents liberally to keep the orchestrating session's context window clean. The orchestrating session's job is to coordinate the pipeline and communicate with the developer — not to do all the heavy lifting itself.

Sub-agents are especially important for:

- **Document generation** (PRD, TRD, orchestration plan) — they read many reference files and produce large outputs
- **Execution** (Stage 5) — each work unit is independent and benefits from an isolated context
- **Cross-check and branch analysis** (Stages 6, 7, 8, and 9) — verification and diff analysis can be large and will otherwise pollute the conversation

A bloated orchestrating session loses track of the overall workflow. Use sub-agents when they reduce context load or enable safe parallelism, not as a default reflex for small work.

---

## Document Conventions

- All planning artifacts go in the selected session folder. Create it if it doesn't exist.
- Ticket-scoped artifacts belong under `.agents/specs/<work-item-id>/` when work item context exists.
- Default non-ticket artifacts belong under `.agents/spec/`.
- Orchestration files go as siblings to the TRD they were generated from in the selected session folder.
- A memory file is always created alongside its orchestration plan.
- Shared status artifacts are orchestrator-owned; sub-agents report via per-work-unit update files.
- Cross-check findings should be written to the selected session folder using the canonical lowercase names, unless an existing alias file is being updated in place.
- Never execute an orchestration plan without the developer's explicit approval.
- Do not begin PR description work until the post-test implementation cross-check is complete and the developer confirms there are no more branch changes to make.
- Treat a "final" document as immutable — create a new versioned file rather than overwriting if the developer wants to revise after approval.

## IMPORTANT

If the user doesn't know what to do or didn't include enough context for you to know what we are doing in this session, please see if you can resume a previous session in .agents/spec/ **(NOT .agents/specs/)**. If not, please output the verbatim the `Default Orientation Behavior` above.

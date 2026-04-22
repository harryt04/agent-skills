---
name: spec-driven-development
description: >
  Guides feature planning and implementation through a document-driven workflow. Stages: App Outline → PRD → TRD → Orchestration → Execution → Tests → PR Description.
  Use this skill to: write or refine a PRD (product requirements), build a cross-repo feature plan through interactive Q&A,
  write or refine a TRD (technical requirements), create an orchestration plan to execute work in parallel,
  generate test coverage for a branch, write a PR description, plan a new feature or project idea, spec out a product,
  outline an application, generate tests. Trigger on any developer request that mentions PRD, TRD, specification,
  product requirements, technical requirements, orchestration, test coverage, branch changes, PR description, feature planning,
  cross-repo feature, multi-repo changes, building a plan, tall-thin slice, feature that spans repositories,
  or if someone describes an idea and wants help fleshing it out. Also trigger when a user wants to plan a feature across
  multiple repositories but doesn't have a written PRD — the plan-driven PRD path handles this. Works at any stage independently.
---

# Spec-Driven Development

This skill guides you through a structured, document-driven workflow for building software. Each stage produces a planning document that you review and refine before moving on. This keeps context windows clean, models focused, and humans in control.

## The Pipeline

```
Stage 1: App Outline / Context Gathering   (Haiku or Sonnet)
Stage 2: PRD — Product Requirements Doc   (Opus)
Stage 3: TRD — Technical Requirements Doc (Opus)
Stage 4: Orchestration Plan               (Opus)
Stage 5: Execute the Orchestration Plan   (Haiku or Sonnet per work unit)
Stage 6: Test Coverage                    (Haiku or Sonnet)
Stage 7: PR Description                   (Haiku or Sonnet)
```

You can enter at any stage. If you already have a TRD, start at Stage 4. If you just want a PR description, jump straight to Stage 7.

All generated documents go in `.agents/spec/` in the project root.

---

## How to Begin

First, figure out where the developer is:

1. Check `.agents/spec/` for existing documents (app-outline.md, PRD.md, TRD.md, ORCHESTRATION.md, memory.md).
2. If none exist, ask what they want to do — or jump straight to Stage 1 if they've already described an idea.
3. If documents exist, identify the latest complete stage and offer to continue from there.

If the intent is unclear, ask: "Where would you like to start? I can help you outline an idea, write a PRD, generate a TRD, build an orchestration plan, add test coverage, or write a PR description."

---

## Reference Files

All templates and prompt instructions are in the `references/` directory alongside this file. Read them when you reach the relevant stage — there is no need to load them all upfront.

| File | Used in Stage |
|---|---|
| `references/app-outline-template.md` | Stage 1 |
| `references/prd-template.md` | Stage 2 (written PRD path) |
| `references/plan-template.md` | Stage 2 (plan-driven PRD path) |
| `references/plan-interview-guide.md` | Stage 2 (plan-driven PRD path) |
| `references/trd-template.md` | Stage 3 |
| `references/orchestration-template.md` | Stage 4 |
| `references/memory-template.md` | Stage 4 (companion memory file) |
| `references/orchestration-instructions.md` | Stage 4 (how to build the plan) |
| `references/test-coverage-instructions.md` | Stage 6 |
| `references/pull-request-instructions.md` | Stage 7 |
| `references/ado-wiki-markdown-reference.md` | Stage 7 (optional markdown formatting reference) |

---

## Stage 1: App Outline / Context Gathering

**Recommended model:** Haiku or Sonnet — this is a brainstorming and interview stage, not heavy reasoning.

**Goal:** Capture the developer's vision in a structured outline that will seed the PRD.

**Steps:**

1. Read `references/app-outline-template.md`.
2. Interview the developer. You don't need to ask every question in the template — ask only about areas that are unclear or missing from what they've already told you. Template sections: Vision, Mission, Governance, Feature List (MVP + Wishlist), Architecture, Pricing, Communication & Project Management, Resources.
3. If the project context is complex and you need to research the codebase or existing docs, use a sub-agent — this keeps the orchestrating session lean.
4. Write a filled-in outline to `.agents/spec/app-outline.md` using the template as a structural guide.

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
2. Read `.agents/spec/app-outline.md` (or use whatever context the developer has provided if no outline file exists).
3. Use a sub-agent to generate the PRD — this keeps the orchestrating session's context window clean. The sub-agent prompt: read the outline, read the PRD template, write a complete PRD to `.agents/spec/PRD.md`.
4. The PRD should cover all template sections: Problem Statement, Product Overview, Target Users, MVP Feature Requirements, System Architecture, Data Model, Non-Functional Requirements, Pricing, Third-Party Integrations, Out of Scope, Testing & Operations, Success Criteria, and Appendix.

5. **Cross-check loop (written PRD path):** Before surfacing the PRD to the developer, spawn a cross-check sub-agent whose sole job is to compare the source outline against the generated PRD and identify anything that was dropped, misrepresented, or underdeveloped. The sub-agent should: read `.agents/spec/app-outline.md` (or the equivalent input), read `.agents/spec/PRD.md`, list every gap or omission, and either patch the PRD directly or produce a clear list of required additions. Repeat until the cross-check sub-agent reports no remaining gaps. Only then surface the document to the developer. Do not tell the developer the PRD is ready until this cross-check has passed.

**Gate:** Tell the developer: "Your draft PRD is at `.agents/spec/PRD.md`. Please review and refine it until it meets all your product requirements. Let me know when it's final and I'll generate the TRD."

### Plan-driven PRD path

1. Read `references/plan-interview-guide.md` and `references/plan-template.md`.
2. Follow the interview guide's phased approach: understand the feature, research the repositories (using sub-agents to explore each repo in parallel), ask targeted questions, then draft the plan.
3. Save to `.agents/specs/{work-item-id}/plan.md` if a work item ID is known, otherwise `.agents/spec/plan.md`.
4. Never include effort estimates (day counts, story points, t-shirt sizes) in the plan.
5. **Cross-check loop (plan-driven path):** Before surfacing the plan to the developer, spawn a cross-check sub-agent that compares every detail gathered during the interview against the generated plan — look for features, constraints, integration points, or decisions that were discussed but not reflected in the output. Patch any gaps. Repeat until the sub-agent reports nothing missing. Do not tell the developer the plan is ready until this passes.

**Gate:** Tell the developer: "Your draft plan is at [path]. Please review — once you're satisfied, this will serve as the basis for the TRD. Let me know when it's final."

Do not proceed to Stage 3 until the developer explicitly confirms the PRD or plan is final.

---

## Stage 3: TRD — Technical Requirements Document

**Recommended model:** Opus — work unit decomposition and dependency graph design are where mistakes become expensive.

**Suggest a model switch if needed.**

**Goal:** Translate the finalized PRD into a parallelizable, actionable Technical Requirements Document. Always produce two files: a verbose agent-ready TRD and a concise human-readable summary.

**Steps:**

1. Read `references/trd-template.md`.
2. Read the source document — either `.agents/spec/PRD.md` or `.agents/specs/{id}/plan.md` (whichever was produced in Stage 2). Both contain sufficient detail to generate a TRD.
3. Use a sub-agent to generate the TRD. The sub-agent should: read the PRD, read the TRD template, analyze the codebase structure if relevant (to understand existing patterns, file layout, and tech stack), then write a complete TRD to `.agents/spec/TRD.md`.
4. The TRD must include: Technology Decisions, Project Structure, Database Schema, Work Units (organized by dependency layer), Dependency Graph, Environment Variables, Testing Strategy, and Migration Notes.
5. Work units are the heart of the TRD. Each one must be self-contained: files to create/modify, acceptance criteria, test requirements, and an explicit list of which other work units it depends on. Units in the same layer should be fully independent of each other so they can run in parallel.
6. **Always generate a companion human-readable summary at `.agents/spec/TRD-summary.md`.** This is a separate, concise document intended for teammates who understand the codebase and just need to understand the proposed changes. Keep it as short as possible. It should cover: what is being built (1–2 sentences), new components (table: component, location, purpose), modified components (table: component, change), data model changes, request/data flow (numbered steps), reused scaffolding, config changes, and error handling. Omit work unit decomposition, layer structure, acceptance criteria, and test scaffolding — those details belong only in `TRD.md`.
7. **Cross-check loop:** Before surfacing the TRD to the developer, spawn a cross-check sub-agent whose sole job is to compare the source PRD (or plan) against `TRD.md` and `TRD-summary.md` and identify anything dropped, misrepresented, or underspecified — including requirements, constraints, data model details, integration points, and non-functional requirements. The sub-agent should patch the TRD directly for any gaps found. Repeat until the sub-agent reports no remaining gaps. Do not tell the developer the TRD is ready until this cross-check has passed.

**Gate:** Tell the developer: "Your draft TRD is at `.agents/spec/TRD.md` and a human-readable summary is at `.agents/spec/TRD-summary.md`. Please review and refine until you're satisfied. Let me know when it's final and I'll generate the orchestration plan."

Do not proceed to Stage 4 until the developer explicitly confirms the TRD is final.

---

## Stage 4: Orchestration Plan

**Recommended model:** Opus — translating a TRD into complete, agent-ready prompts requires precision; gaps here mean failed or confused sub-agents.

**Suggest a model switch if needed.**

**Goal:** Produce a ready-to-execute orchestration plan and companion memory file.

**Steps:**

1. Read `references/orchestration-instructions.md` and `references/orchestration-template.md`.
2. Read `.agents/spec/TRD.md`.
3. Use a sub-agent to generate both files. The sub-agent should:
   - Write the orchestration plan to `.agents/spec/ORCHESTRATION.md`, following the template format.
   - Write the memory file to `.agents/spec/memory.md`, using `references/memory-template.md` as the structural guide and adapting it to the actual project.
4. The orchestration plan must include: header metadata, prerequisites checklist, execution phases table, and fully self-contained work unit prompts. Each work unit's `### Prompt` section must be complete enough to pass directly to `task(description, prompt)` without needing additional context from the conversation.
5. The memory file must document: project root path, implementation status by phase (tracking each file's status), codebase patterns, key files to be modified, open questions with assumed defaults, and coordination notes for race condition prevention.
6. **Cross-check loop:** Before surfacing either file to the developer, spawn a cross-check sub-agent that reads `TRD.md`, `ORCHESTRATION.md`, and `memory.md` together and verifies: every work unit in the TRD has a corresponding, fully-specified prompt in the orchestration plan; the dependency layers are faithfully preserved; no TRD acceptance criteria were dropped; the memory file accurately reflects all files to be modified and all open questions surfaced in the TRD. Patch any gaps found. Repeat until the sub-agent reports nothing missing. Do not tell the developer the orchestration plan is ready until this cross-check has passed.

**Gate:** Tell the developer: "Your orchestration plan and memory file are at `.agents/spec/ORCHESTRATION.md` and `.agents/spec/memory.md`. Please review both — once execution starts, agents will make real changes to your codebase. Let me know when you're ready to proceed."

Do not begin execution until the developer explicitly approves.

---

## Stage 5: Execute the Orchestration Plan

**Recommended model:** Haiku or Sonnet per work unit (use Sonnet for complex or high-risk units).

**Goal:** Execute the plan by dispatching parallel sub-agents layer by layer.

**Steps:**

1. Read `.agents/spec/ORCHESTRATION.md` and `.agents/spec/memory.md`.
2. Check the prerequisites checklist at the top of the orchestration plan. If anything is missing, halt and ask the developer to address it first.
3. Execute layer by layer:
   - For each layer, spawn parallel `task()` calls — one per work unit in that layer. Pass each work unit's full `### Prompt` section as the task prompt. Each sub-agent should also be told to read and update `.agents/spec/memory.md` at the start and end of their task.
   - Wait for all work units in the layer to complete before spawning the next layer.
   - Update the status checkboxes in `ORCHESTRATION.md` as units complete or fail.
4. If a work unit fails, surface the error to the developer immediately. Do not continue to the next layer until they decide to retry, modify, or skip.

**After execution:** Summarize what was done, note any failures or skipped units, and ask if they want to proceed to test coverage (Stage 6) or make manual adjustments first.

---

## Stage 6: Test Coverage

**Recommended model:** Haiku or Sonnet for analysis and implementation; Opus for the test TRD step.

**Goal:** Analyze branch changes, plan coverage gaps, generate a test TRD and orchestration plan, then implement the tests.

**Steps:**

1. Read `references/test-coverage-instructions.md`.
2. Ask the developer: "Do you have any existing test guidelines, context files, or test style examples I should reference? If so, share the file paths."
3. Use sub-agents to:
   - Analyze the changes on the current branch relative to master/main.
   - Produce a markdown summary of needed test cases per file/function, saved to `.agents/spec/test-analysis.md`.
   - Search for related existing tests to understand where new tests should go and what conventions to follow.
4. Use a sub-agent (Opus recommended) to generate a test-focused TRD at `.agents/spec/TRD-tests.md`, based on the test analysis and any context files provided.
5. Generate a test orchestration plan at `.agents/spec/ORCHESTRATION-tests.md` and a companion memory file at `.agents/spec/memory-tests.md`.

**Gate:** Tell the developer: "Your test plan is ready. TRD at `.agents/spec/TRD-tests.md`, orchestration plan at `.agents/spec/ORCHESTRATION-tests.md`. Please review before I begin — tests will be written to your codebase. Let me know when ready."

Do not execute the test orchestration until the developer approves.

6. Execute the test orchestration plan using the same layer-by-layer parallel approach as Stage 5.

---

## Stage 7: PR Description

**Recommended model:** Haiku or Sonnet — this is a summarization task.

**Goal:** Generate a concise, well-structured PR description from the branch diff.

**Steps:**

1. Read `references/pull-request-instructions.md`.
2. Use a sub-agent to compare the current branch against master/main and summarize the diff.
3. Optionally read `references/ado-wiki-markdown-reference.md` for markdown syntax guidance — it documents what renders well in browser-based PR text boxes (tables, numbered lists, code blocks all work; some advanced features like Mermaid do not).
4. Write the PR description to `.agents/spec/pr-description.md`.
5. Hard limit: 4000 characters. Target: under 1000 characters. Use numbered lists and tables where they aid readability; prefer them over prose and bullet points.

**Paste reminder:** Tell the developer — when pasting the PR description into their platform's web UI, use **Paste and Match Style** (Cmd+Shift+V on Mac, Ctrl+Shift+V on Windows/Linux) to avoid rich-text formatting artifacts.

**Gate:** Show the description and ask if they want any changes.

---

## Model Switching Guidance

At each stage transition, briefly mention the recommended model and why. These are defaults — use your judgment if the project's complexity calls for something different.

| Stage | Recommended | Why |
|---|---|---|
| 1 — Outline | Haiku or Sonnet | Brainstorming and template-filling; depth not required |
| 2 — PRD | Opus | Architectural trade-offs, scope decisions, nuanced product requirements |
| 3 — TRD | Opus | Work unit decomposition and dependency design; mistakes here cascade |
| 4 — Orchestration | Opus | Precision matters; gaps in agent prompts cause failures at execution time |
| 5 — Execution | Haiku or Sonnet | Each work unit is a focused, bounded task |
| 6 — Tests | Haiku/Sonnet + Opus (TRD step) | Analysis and implementation are routine; test TRD benefits from Opus |
| 7 — PR | Haiku or Sonnet | Summarization; model capability not the bottleneck |

To switch models in OpenCode: use the model picker in the interface, or start a new session with the target model active.

---

## Sub-Agent Philosophy

Use sub-agents liberally to keep the orchestrating session's context window clean. The orchestrating session's job is to coordinate the pipeline and communicate with the developer — not to do all the heavy lifting itself.

Sub-agents are especially important for:
- **Document generation** (PRD, TRD, orchestration plan) — they read many reference files and produce large outputs
- **Execution** (Stages 5 and 6) — each work unit is independent and benefits from an isolated context
- **Branch analysis** (Stages 6 and 7) — diffs can be large and will otherwise pollute the conversation

A bloated orchestrating session loses track of the overall workflow. When in doubt, spawn a sub-agent.

---

## Document Conventions

- All planning artifacts go in `.agents/spec/` at the project root. Create the directory if it doesn't exist.
- Orchestration files go as siblings to the TRD they were generated from (so both live in `.agents/spec/`).
- A memory file is always created alongside its orchestration plan.
- Never execute an orchestration plan without the developer's explicit approval.
- Treat a "final" document as immutable — create a new versioned file rather than overwriting if the developer wants to revise after approval.

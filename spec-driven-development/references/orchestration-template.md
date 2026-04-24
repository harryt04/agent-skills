# [PROJECT_NAME] MVP -- Orchestration Plan

**Version:** 1.0  
**Date:** [DATE]  
**Purpose:** Self-contained work unit plan for LLM orchestration agents dispatching via OpenCode Task tool  
**Source:** `<session-folder>/[spec-document].md` (translated into executable agent prompts)  
**Status:** Ready for orchestration

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Execution Phases](#execution-phases)
4. [Work Units (Agent Prompts)](#work-units-agent-prompts)
5. [Dependency Graph](#dependency-graph)

---

## Overview

This document breaks down the [PROJECT_NAME] MVP Technical Requirements Document (TRD) into **self-contained work units**, each of which can be dispatched to an LLM agent via the OpenCode `task()` tool.

**Key characteristics:**

- Each work unit includes all information needed to execute it independently
- Dependencies are explicit (blockers must complete first)
- Work units within a layer can execute in parallel
- Status tracked via checkboxes and layer completion
- All file paths, acceptance criteria, and tests are inline

**Orchestration strategy:**

1. Orchestrator reads this file
2. Executes all work units in a given layer in parallel (via concurrent `task()` calls)
3. Waits for layer to complete before moving to next layer
4. Consolidates per-work-unit update files into shared status artifacts as units complete
5. If a unit fails, orchestrator halts and surfaces error; human decides to retry or skip

---

## Prerequisites

**Before starting orchestration, the following must be in place:**

- [ ] **[RUNTIME_ENV] installed** -- required for [RUNTIME_COMMAND] (e.g., `docker compose up`)
- [ ] **`.env` file created** from `.env.sample` with all values (see `.agents/SETUP-CHECKLIST.md`)
- [ ] **`[PACKAGE_MANAGER] install` completed** -- all production and dev dependencies installed
- [ ] **Current git branch:** `[BRANCH_NAME]` (or main branch where work will land)

**If any prerequisite is missing, orchestrator must halt and ask human to complete it before proceeding.**

---

## Execution Phases

| Phase | Layer | Work Units            | Can Parallelize | Status      |
| ----- | ----- | --------------------- | --------------- | ----------- |
| 1     | 0     | [WU-0.1, WU-0.2, ...] | ✅ All          | [ ] Pending |
| 2     | 1     | [WU-1.1, WU-1.2, ...] | ✅ All          | [ ] Pending |
| 3     | 2     | [WU-2.1, WU-2.2, ...] | ✅ All          | [ ] Pending |
| N     | N     | [WU-N.1, WU-N.2, ...] | ✅ / ❌ Mixed   | [ ] Pending |

---

## Work Units (Agent Prompts)

Each work unit below is formatted as a ready-to-execute agent prompt. The orchestrator can pass the entire **Prompt** section (starting with `## Prompt`) directly to `task(description, prompt)`. Shared planning files are orchestrator-owned; sub-agents should report results via per-work-unit update files.

---

### Phase 1: [PHASE_NAME] (Layer 0) -- All Parallel

---

#### WU-0.1: [BRIEF_TITLE]

**Status:** [ ] Pending | [ ] In Progress | [ ] Complete | [ ] Failed

**Priority:** [P0/P1/P2]  
**Effort:** [Small/Medium/Large]  
**Dependencies:** None  
**Parallel group:** Layer 0

### Prompt

You are a [ROLE/EXPERTISE]. Your task is [HIGH_LEVEL_OBJECTIVE].

**Acceptance Criteria (Validation):**

- [Criterion 1: must be testable and measurable]
- [Criterion 2: ...]
- [All criteria listed in bullet format]

**Files to Create:**

1. `[path/to/file.ext]` -- [Brief description]:
   - [Specific implementation detail 1]
   - [Specific implementation detail 2]

2. `[path/to/another/file.ext]` -- [Brief description]:
   - [Detail...]

**Files to Modify:**

1. `[path/to/existing/file.ext]`:
   - [What to add/change and why]
   - [Keep specific...]

**Dependencies to Install (if applicable):**

```bash
# Production
npm install [package1] [package2]

# Dev
npm install -D [dev-package1] [dev-package2]
```

**Environment Variables Required (if applicable):**

```bash
[VAR_NAME]=[description or example]
[ANOTHER_VAR]=
```

**Tests:**

Create `tests/unit/[feature].test.ts`:

- Test: [Specific test description]
- Test: [Another test]

Create `tests/e2e/[feature].spec.ts`:

- Test: [E2E test description]

**Notes:**

- [Implementation note 1]
- [Implementation note 2]
- [Important constraint or assumption]
- Read `<session-folder>/memory.md` first, then write your outcome to `<session-folder>/updates/WU-0.1.md` instead of editing shared status files

---

#### WU-0.2: [ANOTHER_BRIEF_TITLE]

**Status:** [ ] Pending | [ ] In Progress | [ ] Complete | [ ] Failed

**Priority:** [P0/P1]  
**Effort:** [Medium/Large]  
**Dependencies:** WU-0.1  
**Parallel group:** Layer 0

### Prompt

You are a [ROLE]. Your task is [HIGH_LEVEL_OBJECTIVE].

**Acceptance Criteria (Validation):**

- [Criterion 1]
- [Criterion 2]

**Files to Create:**

1. `[path/file]` -- [Description]

**Files to Modify:**

1. `[path/file]` -- [Change description]

**Tests:**

Create `tests/[test-location].test.ts`:

- Test: [Description]

**Notes:**

- [Implementation note]
- Read `<session-folder>/memory.md` first, then write your outcome to `<session-folder>/updates/WU-0.2.md` instead of editing shared status files

---

### Phase 2: [PHASE_NAME] (Layer 1) -- Both Parallel

---

#### WU-1.1: [TITLE]

**Status:** [ ] Pending | [ ] In Progress | [ ] Complete | [ ] Failed

**Priority:** [P0]  
**Effort:** [Medium]  
**Dependencies:** WU-0.1, WU-0.2  
**Parallel group:** Layer 1

### Prompt

You are a [ROLE]. Your task is [OBJECTIVE].

**Acceptance Criteria (Validation):**

- [Criterion 1]
- [Criterion 2]

**Files to Create:**

1. `[path/file]` -- [Description]

**Files to Modify:**

1. `[path/file]` -- [Changes]

**Tests:**

Create `tests/e2e/[feature].spec.ts`:

- Test: [Description]

**Notes:**

- [Important note]
- Read `<session-folder>/memory.md` first, then write your outcome to `<session-folder>/updates/WU-1.1.md` instead of editing shared status files

---

## Template Usage Guide

### For Project Leads

1. **Copy this template** to `orchestration.md` in the selected session folder. If an existing `ORCHESTRATION.md` is already in active use, update that file instead of renaming it.
2. **Fill in project metadata** at the top (project name, date, spec document reference)
3. **Define execution phases** in the Execution Phases table:
   - List all work units per layer
   - Mark which can run in parallel (✅) vs sequential (❌)
4. **Create work units** by:
   - Duplicating WU-0.X sections
   - Replacing all `[PLACEHOLDER]` values with specific details
   - Ensuring dependencies are explicit and accurate
5. **Set prerequisites** based on your tech stack and environment requirements

### For Orchestrators (AI Agents)

1. **Read Prerequisites section** first; halt if any are missing
2. **Execute layer by layer**:
    - For each layer, spawn parallel `task()` calls for all work units marked ✅
    - Wait for all to complete before moving to next layer
    - Update checkboxes in-place after consolidating each work unit's update file
3. **Handle failures**:
    - If a work unit fails, surface the error to human
    - Offer to retry or skip; human decides
    - Do NOT continue to next layer until human approves
4. **Maintain this document** as the source of truth for project status
5. **Own shared writes**:
   - Sub-agents may read shared planning files but should not edit them directly
   - Sub-agents should write per-work-unit reports under `<session-folder>/updates/`
   - The orchestrator consolidates those reports into the orchestration and memory artifacts in the selected session folder

### Placeholder Key

| Placeholder              | Meaning                                                            |
| ------------------------ | ------------------------------------------------------------------ |
| `[PROJECT_NAME]`         | Name of your SaaS application                                      |
| `[DATE]`                 | Current date (YYYY-MM-DD)                                          |
| `[SPEC_DOCUMENT]`        | Reference to your TRD or PRD in the selected session folder        |
| `[RUNTIME_ENV]`          | Docker, Node.js, Python, etc.                                      |
| `[RUNTIME_COMMAND]`      | `docker compose up`, `npm run dev`, etc.                           |
| `[PACKAGE_MANAGER]`      | `npm`, `yarn`, `pnpm`, etc.                                        |
| `[BRANCH_NAME]`          | Feature branch or development branch name                          |
| `[PHASE_NAME]`           | e.g., "Foundation", "Authentication", "Core Features", "AI & Chat" |
| `[BRIEF_TITLE]`          | Short, action-oriented title (e.g., "Project Scaffolding")         |
| `[HIGH_LEVEL_OBJECTIVE]` | Complete sentence describing what the agent should accomplish      |
| `[ROLE/EXPERTISE]`       | Agent expertise area (e.g., "React UI specialist")                 |
| `[P0/P1/P2]`             | Priority (P0=blocks others, P1=important, P2=nice-to-have)         |
| `[Small/Medium/Large]`   | Estimated effort (affects parallelization and scheduling)          |
| `[path/file]`            | Absolute or relative file path from project root                   |
| `[Description]`          | 1-2 sentence explanation of file's purpose                         |
| `[Criterion 1]`          | Measurable, testable acceptance criterion                          |
| `[Test Description]`     | Specific test case (e.g., "User can sign up with email+password")  |

### Best Practices

1. **Ensure dependencies are explicit**: Each work unit should list all WU IDs it depends on.
2. **Keep acceptance criteria testable**: Avoid vague language; make each criterion measurable and automatable.
3. **Group by layer**: Work units in the same layer should be independent (no cross-layer dependencies within a layer).
4. **Estimate effort conservatively**: "Large" work units are suitable for parallel execution but may take longer.
5. **Include both unit and E2E tests**: Catch regressions early with unit tests; verify user flows with E2E tests.
6. **Document environment assumptions**: Be explicit about required databases, external APIs, services, etc.
7. **Provide concrete code examples**: Include small code snippets or pseudocode to clarify intent.

### Extending the Template

- **For larger projects**: Add more phases and layers (e.g., Phase 5, 6, etc.)
- **For rapid prototyping**: Consolidate multiple WUs into single larger WUs; adjust effort estimates
- **For cross-team work**: Add a "Owner" field to each WU to assign responsibility
- **For tracking costs**: Add a "EstimatedCost" field (hours or dollars) to enable budget tracking

---

## Notes for Future Use

- This template is designed to be **technology-agnostic**: It works for Node.js, Python, Go, Rust, etc.
- Modify the **dependency syntax** as needed for your project (e.g., if you use a different versioning or task runner)
- **Environment variables** section is optional; include only if relevant to your stack
- **Tests** section should align with your testing framework (Vitest, Jest, pytest, etc.)
- Keep this document **up-to-date** as the project evolves; treat it as the source of truth for orchestration

---

## Example Customization

For a hypothetical "BlogEngine" project:

```markdown
# BlogEngine MVP -- Orchestration Plan

**Version:** 1.0
**Date:** 2026-04-15
**Purpose:** Self-contained work unit plan for LLM orchestration agents
**Source:** `<session-folder>/trd.md`
**Status:** Ready for orchestration

## Prerequisites

- [ ] **Docker Desktop installed** -- required for `docker compose up` (Postgres 15 + Redis cache)
- [ ] **`.env` file created** from `.env.sample`
- [ ] **`npm install` completed**
- [ ] **Current git branch:** `mvp`

## Execution Phases

| Phase | Layer | Work Units             | Parallelizable | Status      |
| ----- | ----- | ---------------------- | -------------- | ----------- |
| 1     | 0     | WU-0.1, WU-0.2         | ✅ Both        | [ ] Pending |
| 2     | 1     | WU-1.1, WU-1.2         | ✅ Both        | [ ] Pending |
| 3     | 2     | WU-2.1, WU-2.2, WU-2.3 | ✅ All 3       | [ ] Pending |
```

---

**For more details on how to use this template, see your project's README or contact your lead architect.**

# Test Coverage Instructions

These instructions are self-contained. An agent following them should be able to analyze a branch, plan test coverage, and produce all planning documents without additional context from the dispatching session.

---

## Session Folder

Before reading or writing artifacts, resolve the selected session folder:

- Use `.agents/specs/<work-item-id>/` when the developer has provided or already implied a work item context.
- Otherwise use `.agents/spec/`.

For any new test-planning artifact, use lowercase names such as `test-trd.md`, `test-orchestration.md`, `test-memory.md`, and `test-updates/`. If the selected folder already contains older aliases such as `TRD-tests.md`, `ORCHESTRATION-tests.md`, `memory-tests.md`, or `updates-tests/`, keep using those existing artifacts when editing them.

## Goal

Analyze the changes on the current branch relative to the default branch, identify what tests are needed, and choose the lightest planning path that still gives safe coverage. For small or localized diffs, stop at `test-analysis.md` and write tests directly after approval. For large, risky, or cross-cutting diffs, produce a test-focused TRD and orchestration plan.

---

## Step 1: Analyze the Branch Diff

Use sub-agents if the diff is large enough to warrant it. Compare the current branch against the repo's default branch and produce a markdown summary of needed test cases, organized by changed file and/or function.

Save the analysis to: **`<session-folder>/test-analysis.md`**

For each changed file or function, the analysis should cover:

- What the code does and what behavior needs test coverage
- Edge cases and error paths worth testing
- Whether existing tests already cover any of this (and if so, what gaps remain)

---

## Step 2: Study Existing Test Patterns

Before writing the test TRD, search the repository for existing test code. The goal is to understand the project's testing conventions so that new tests fit in naturally. Look for:

- **Directory structure** — where test files live (e.g., `tests/`, `__tests__/`, colocated with source files)
- **Naming conventions** — how test files and test functions are named (e.g., `*.test.ts`, `*.spec.ts`, `Test*.cs`)
- **Test framework** — what framework is in use (Jest, Vitest, xUnit, pytest, etc.) and what assertion style the project uses
- **Helper patterns** — shared fixtures, factories, mocks, or test utilities the project already has
- **Test granularity** — whether the project favors unit tests, integration tests, E2E tests, or a mix

Capture these findings in the test analysis file or carry them forward into the TRD. New tests should follow the existing conventions, not introduce new ones.

---

## Step 3: Write the Test TRD

Only do this for the planned test path. Skip this step for small or localized diffs where direct test work is sufficient.

Use the TRD template at `references/trd-template.md` (relative to this skill's directory) as the structural base. Adapt it for a test-focused scope — you won't need every section (e.g., "Database Schema" is likely irrelevant), but the work unit structure, dependency layers, and acceptance criteria format should be followed.

If the developer has provided test guidelines, context files, or specific testing requirements, incorporate them into the TRD.

Save the test TRD to: **`<session-folder>/test-trd.md`** unless an existing `TRD-tests.md` in the selected session folder should be updated in place.

Each work unit in the test TRD should specify:

- Which test files to create and where they go
- What functions, behaviors, or scenarios each test covers
- Acceptance criteria (e.g., "all tests pass", "covers the happy path and at least two error paths for function X")
- Any fixtures, mocks, or test data that need to be set up

---

## Step 4: Write the Test Orchestration Plan and Memory File

Only do this for the planned test path.

Use the orchestration template at `references/orchestration-template.md` and the memory template at `references/memory-template.md` (both relative to this skill's directory) as structural guides.

Save the orchestration plan to: **`<session-folder>/test-orchestration.md`** unless an existing `ORCHESTRATION-tests.md` in the selected session folder should be updated in place.
Save the memory file to: **`<session-folder>/test-memory.md`** unless an existing `memory-tests.md` in the selected session folder should be updated in place.

Follow the same principles as the main orchestration plan:

- Organize test work units into dependency layers (tests for foundational code in earlier layers, tests that depend on those in later layers)
- Each work unit prompt must be self-contained — sub-agents have no access to the dispatching session
- Include an instruction in each prompt to read the session test memory file and write results to `<session-folder>/test-updates/<work-unit-id>.md` unless an existing `updates-tests/` directory is already in active use
- The memory file should document: project root, implementation status per test file, existing test patterns discovered in Step 2, and coordination notes
- Preserve traceability from planned tests to covered behaviors so a later read-only audit can verify whether the final branch actually implemented the intended coverage

---

## Step 5: Handoff to the Post-Test Cross-Check

The planning artifacts created here should support a later implementation cross-check after test execution. That later stage is expected to use parallel read-only audit sub-agents, so make the artifacts easy to audit:

- In `test-analysis.md`, clearly identify the behaviors, files, and gaps each planned test is meant to cover
- In `test-trd.md`, keep work units and acceptance criteria concrete enough that an audit agent can later verify whether the tests were actually implemented
- In `test-memory.md`, preserve enough status detail that an audit agent can compare claimed progress against the real branch state

---

## Gate

Do not execute any test-writing work until the developer reviews the relevant planning artifact for the chosen path:

- `<session-folder>/test-analysis.md` — the branch diff analysis
- `<session-folder>/test-trd.md` or the existing `TRD-tests.md` alias — the test TRD
- `<session-folder>/test-orchestration.md` or the existing `ORCHESTRATION-tests.md` alias — the test orchestration plan
- `<session-folder>/test-memory.md` or the existing `memory-tests.md` alias — the orchestrator-owned memory file

For the direct test path, only `test-analysis.md` is required before approval. Tell the developer the relevant files are ready for review and wait for explicit approval before executing.

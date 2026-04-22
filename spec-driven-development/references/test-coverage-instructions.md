# Test Coverage Instructions

These instructions are self-contained. An agent following them should be able to analyze a branch, plan test coverage, and produce all planning documents without additional context from the dispatching session.

---

## Goal

Analyze the changes on the current branch relative to master/main, identify what tests are needed, and produce a test-focused TRD and orchestration plan that sub-agents can execute to implement the tests.

---

## Step 1: Analyze the Branch Diff

Use sub-agents if the diff is large enough to warrant it. Compare the current branch against master/main and produce a markdown summary of needed test cases, organized by changed file and/or function.

Save the analysis to: **`.agents/spec/test-analysis.md`**

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

Use the TRD template at `references/trd-template.md` (relative to this skill's directory) as the structural base. Adapt it for a test-focused scope — you won't need every section (e.g., "Database Schema" is likely irrelevant), but the work unit structure, dependency layers, and acceptance criteria format should be followed.

If the developer has provided test guidelines, context files, or specific testing requirements, incorporate them into the TRD.

Save the test TRD to: **`.agents/spec/TRD-tests.md`**

Each work unit in the test TRD should specify:

- Which test files to create and where they go
- What functions, behaviors, or scenarios each test covers
- Acceptance criteria (e.g., "all tests pass", "covers the happy path and at least two error paths for function X")
- Any fixtures, mocks, or test data that need to be set up

---

## Step 4: Write the Test Orchestration Plan and Memory File

Use the orchestration template at `references/orchestration-template.md` and the memory template at `references/memory-template.md` (both relative to this skill's directory) as structural guides.

Save the orchestration plan to: **`.agents/spec/ORCHESTRATION-tests.md`**
Save the memory file to: **`.agents/spec/memory-tests.md`**

Follow the same principles as the main orchestration plan:

- Organize test work units into dependency layers (tests for foundational code in earlier layers, tests that depend on those in later layers)
- Each work unit prompt must be self-contained — sub-agents have no access to the dispatching session
- Include an instruction in each prompt to read and update `.agents/spec/memory-tests.md`
- The memory file should document: project root, implementation status per test file, existing test patterns discovered in Step 2, and coordination notes

---

## Gate

Do not execute the test orchestration plan. The developer wants to review all planning documents before proceeding:

- `.agents/spec/test-analysis.md` — the branch diff analysis
- `.agents/spec/TRD-tests.md` — the test TRD
- `.agents/spec/ORCHESTRATION-tests.md` — the test orchestration plan
- `.agents/spec/memory-tests.md` — the shared memory file

Tell the developer these files are ready for review and wait for explicit approval before executing.

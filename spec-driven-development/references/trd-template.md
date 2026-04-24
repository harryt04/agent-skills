# [PROJECT_NAME] -- Technical Requirements Document (MVP)

**Version:** 1.0 (Draft 1)
**Date:** [DATE]
**Author:** [AUTHOR_NAME]
**Status:** Draft -- Awaiting Review
**Branch:** `mvp`
**Source PRD:** `<session-folder>/prd.md`

---

## 0. Document Purpose

This TRD translates the PRD-FINAL into actionable, parallelizable work units for an orchestration agent dispatching sub-agents. Each work unit is self-contained with: files to create/modify, dependencies on other work units, acceptance criteria, and test requirements.

**Scope:** [DESCRIBE SCOPE: What code changes will this TRD drive? What is out of scope?]

---

## 1. Technology Decisions

| Layer             | Choice            | Version   | Notes   |
| ----------------- | ----------------- | --------- | ------- |
| **Framework**     | [FRAMEWORK]       | [VERSION] | [NOTES] |
| **Language**      | [LANGUAGE]        | [VERSION] | [NOTES] |
| **Styling**       | [STYLING]         | [VERSION] | [NOTES] |
| **UI Components** | [UI_LIBRARY]      | [VERSION] | [NOTES] |
| **Auth**          | [AUTH_SOLUTION]   | [VERSION] | [NOTES] |
| **ORM**           | [ORM_CHOICE]      | [VERSION] | [NOTES] |
| **Database**      | [DATABASE]        | [VERSION] | [NOTES] |
| **Sync Layer**    | [SYNC_SOLUTION]   | [VERSION] | [NOTES] |
| **LLM SDK**       | [LLM_SDK]         | [VERSION] | [NOTES] |
| **Analytics**     | [ANALYTICS]       | [VERSION] | [NOTES] |
| **Testing**       | [TEST_FRAMEWORKS] | [VERSION] | [NOTES] |
| **Node.js**       | [NODE_VERSION]    | [VERSION] | [NOTES] |

### Key Library Additions (to install)

```
# Production dependencies
npm install [DEPENDENCY_1] [DEPENDENCY_2] [DEPENDENCY_3]

# Dev dependencies
npm install -D [DEV_DEPENDENCY_1] [DEV_DEPENDENCY_2] [DEV_DEPENDENCY_3]
```

> **Note:** [IMPORTANT NOTES ABOUT SPECIFIC VERSIONS OR COMPATIBILITY]

---

## 2. Project Structure (Target)

```
[PROJECT_NAME]/
├── [DESCRIBE_DIRECTORY_STRUCTURE]
│   ├── [SUBDIRECTORY]
│   └── [SUBDIRECTORY]
└── [ROOT_FILES]
```

---

## 3. Database Schema

### 3.1 [AUTH_SYSTEM] Tables (auto-managed)

[AUTH_SYSTEM] creates and manages: [LIST_OF_TABLES]. We reference [FOREIGN_KEY_REFERENCE] in our application tables.

### 3.2 Application Tables

```sql
-- [TABLE_FILE_PATH]
[TABLE_NAME] (
  [COLUMN_NAME]    [COLUMN_TYPE] [CONSTRAINTS],
  [COLUMN_NAME]    [COLUMN_TYPE] [CONSTRAINTS]
)

[ADD_MORE_TABLES_AS_NEEDED]
```

---

## 4. Work Units

Work units are organized by dependency layer. Units within the same layer can be executed in parallel. Units in later layers depend on earlier layers completing first.

### Layer 0: Foundation (No dependencies -- fully parallel)

---

#### WU-0.1: [WORK_UNIT_NAME]

**Priority:** P0
**Estimated effort:** [Small|Medium|Large]
**Parallel group:** Layer 0

**Description:** [DESCRIBE WHAT THIS WORK UNIT ACCOMPLISHES]

**Files to create:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]
- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Files to modify:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]
- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Acceptance criteria:**

- [ ] [CRITERION_1]
- [ ] [CRITERION_2]
- [ ] [CRITERION_3]

**Tests:** [DESCRIBE_TEST_APPROACH]

---

#### WU-0.2: [WORK_UNIT_NAME]

**Priority:** [P0|P1|P2]
**Estimated effort:** [Small|Medium|Large]
**Parallel group:** Layer 0

**Description:** [DESCRIBE_WHAT_THIS_WORK_UNIT_ACCOMPLISHES]

**Files to create:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Files to modify:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Acceptance criteria:**

- [ ] [CRITERION_1]
- [ ] [CRITERION_2]

**Tests:** [DESCRIBE_TEST_APPROACH]

---

### Layer 1: [LAYER_NAME] (Depends on Layer 0)

---

#### WU-1.1: [WORK_UNIT_NAME]

**Priority:** [P0|P1|P2]
**Estimated effort:** [Small|Medium|Large]
**Parallel group:** Layer 1

**Description:** [DESCRIBE_WHAT_THIS_WORK_UNIT_ACCOMPLISHES]

**Files to create:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Files to modify:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Acceptance criteria:**

- [ ] [CRITERION_1]
- [ ] [CRITERION_2]

**Tests:**

- [TEST_FILE_PATH] -- [DESCRIBE_TEST_COVERAGE]

---

### Layer 2: [LAYER_NAME] (Depends on Layer 1)

---

#### WU-2.1: [WORK_UNIT_NAME]

**Priority:** [P0|P1|P2]
**Estimated effort:** [Small|Medium|Large]
**Parallel group:** Layer 2

**Description:** [DESCRIBE_WHAT_THIS_WORK_UNIT_ACCOMPLISHES]

**Files to create:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Files to modify:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Acceptance criteria:**

- [ ] [CRITERION_1]
- [ ] [CRITERION_2]

**Tests:**

- [TEST_FILE_PATH] -- [DESCRIBE_TEST_COVERAGE]

---

### Layer 3: [LAYER_NAME] (Depends on Layer 2)

---

#### WU-3.1: [WORK_UNIT_NAME]

**Priority:** [P0|P1|P2]
**Estimated effort:** [Small|Medium|Large]
**Parallel group:** Layer 3

**Description:** [DESCRIBE_WHAT_THIS_WORK_UNIT_ACCOMPLISHES]

**Files to create:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Files to modify:**

- [FILE_PATH] -- [BRIEF_DESCRIPTION]

**Acceptance criteria:**

- [ ] [CRITERION_1]
- [ ] [CRITERION_2]

**Tests:**

- [TEST_FILE_PATH] -- [DESCRIBE_TEST_COVERAGE]

---

## 5. Dependency Graph

```
Layer 0 (parallel):
  WU-0.1 ([DESCRIPTION])
  WU-0.2 ([DESCRIPTION])
  WU-0.3 ([DESCRIPTION])

Layer 1 (depends on Layer 0):
  WU-1.1 ([DESCRIPTION]) → depends on WU-0.X, WU-0.Y
  WU-1.2 ([DESCRIPTION]) → depends on WU-0.Z, WU-1.1

Layer 2 (depends on Layer 1):
  WU-2.1 ([DESCRIPTION]) → depends on WU-1.1
  WU-2.2 ([DESCRIPTION]) → depends on WU-0.2

[ADD_MORE_LAYERS_AS_NEEDED]
```

### Maximum Parallelism Schedule

| Phase | Work Units             | Can Parallelize            |
| ----- | ---------------------- | -------------------------- |
| 1     | WU-0.1, WU-0.2, WU-0.3 | All 3                      |
| 2     | WU-1.1, WU-1.2         | Both (if interface agreed) |
| 3     | WU-2.1, WU-2.2, WU-2.3 | All 3                      |
| 4     | WU-3.1, WU-3.2         | Both                       |
| 5     | WU-4.1                 | 1 (final)                  |

---

## 6. Environment Variables

Complete list of env vars the application requires:

```bash
# [CATEGORY]
[VAR_NAME]=[DESCRIPTION]
[VAR_NAME]=[DESCRIPTION]

# [CATEGORY]
[VAR_NAME]=[DESCRIPTION]
```

---

## 7. Testing Strategy

### Unit Tests

- Location: `tests/unit/`
- Scope: [DESCRIBE_SCOPE]
- Mocking: [DESCRIBE_WHAT_GETS_MOCKED]
- Run: `npm run test:unit`

### E2E Tests

- Location: `tests/e2e/`
- Scope: [DESCRIBE_SCOPE]
- Environment: [DESCRIBE_REQUIREMENTS]
- Seed data: [DESCRIBE_HOW_SEED_DATA_PROVIDED]
- Run: `npm run test:e2e`

### Test Coverage Priorities

| Area           | Test Type | Priority |
| -------------- | --------- | -------- | ----- | --- | --- |
| [FEATURE_AREA] | [Unit     | E2E      | Both] | [P0 | P1] |
| [FEATURE_AREA] | [Unit     | E2E      | Both] | [P0 | P1] |
| [FEATURE_AREA] | [Unit     | E2E      | Both] | [P1 | P2] |

---

## 8. Migration Notes

### Existing Code to Preserve

- [FILE_PATH] -- [REASON_FOR_PRESERVATION]
- [FILE_PATH] -- [REASON_FOR_PRESERVATION]

### Breaking Changes to Watch For

- [DESCRIPTION_OF_POTENTIAL_ISSUE] -- [MITIGATION_STRATEGY]
- [DESCRIPTION_OF_POTENTIAL_ISSUE] -- [MITIGATION_STRATEGY]

---

**Document prepared:** [DATE]
**Prepared by:** [AUTHOR]
**Status:** [STATUS]

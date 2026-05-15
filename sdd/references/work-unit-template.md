# Work Unit Template

Every orchestration work unit should be self-contained enough to hand directly to a sub-agent. Keep the contract fixed so execution and cross-checking stay deterministic.

## Required Work Unit Fields

- `Work Unit ID`
- `Objective`
- `In Scope`
- `Out of Scope`
- `Dependencies / Prerequisites`
- `Implementation Requirements`
- `Test Requirements`
- `Verification Commands`
- `Definition of Done`
- `Expected Files or Artifacts to Change`
- `Assumptions / Wont-Fix Items`
- `Execution Notes`

## Template

```markdown
### Work Unit: [ID]

**Objective**
- [What this unit must accomplish]

**In Scope**
- [item]

**Out of Scope**
- [item]

**Dependencies / Prerequisites**
- [dependency]

**Implementation Requirements**
- [concrete requirement]

**Test Requirements**
- [required automated test or manual verification note]

**Verification Commands**
- `[command]`

**Definition of Done**
- [observable completion signal]

**Expected Files or Artifacts to Change**
- `path/to/file`

**Assumptions / Wont-Fix Items**
- [assumption or wont-fix]

**Execution Notes**
- Read `memory.md` first.
- Write results to the assigned update file.
- Do not edit orchestrator-owned files directly.
```

## Testing Rule

New code should be tested when feasible. If automated testing is not feasible, call that out explicitly and describe the manual verification that would be required.

# Orchestration Plan Instructions

These instructions are self-contained. An agent following them should be able to produce a complete orchestration plan and companion memory file without additional context from the dispatching session.

---

## Inputs

1. **TRD** at `.agents/spec/TRD.md` (relative to the project root). This is the source document you are translating into an executable plan.
2. **Orchestration template** at `references/orchestration-template.md` (relative to this skill's directory). This defines the structural format your output must follow.
3. **Memory template** at `references/memory-template.md` (relative to this skill's directory). This defines the structural format for the companion memory file.

Read all three before you begin writing.

---

## What You Are Building

Three artifacts:

1. **`.agents/spec/ORCHESTRATION.md`** — a layered execution plan where each work unit is a fully self-contained agent prompt.
2. **`.agents/spec/memory.md`** — an orchestrator-owned state file that sub-agents read for context.
3. **`.agents/spec/updates/`** — a directory where each sub-agent writes a per-work-unit update file for the orchestrator to consolidate.

All of them live in `.agents/spec/`, as siblings to the TRD.

---

## How to Build the Orchestration Plan

Follow the orchestration template for structure. The key principles:

### Layer design

Organize work units into dependency layers. Units within the same layer have no dependencies on each other and can execute in parallel. Units in later layers depend on earlier layers completing first.

Study the TRD's dependency graph and work unit definitions carefully. If the TRD has implicit dependencies that aren't explicitly stated (e.g., a UI component that needs an API endpoint from another work unit), make them explicit in your plan.

### Self-contained prompts

Each work unit's `### Prompt` section is the most important part of the plan. Sub-agents dispatched with these prompts have **no access to the orchestrating session** — they cannot ask follow-up questions or reference conversation history. Everything the sub-agent needs must be in the prompt itself:

- The specific files to create and modify, with paths relative to the project root
- Acceptance criteria that are testable and measurable
- Any relevant codebase patterns, naming conventions, or architectural constraints
- Dependencies to install (if any)
- Tests to write and where they go
- An instruction to read `.agents/spec/memory.md` at the start of the task and write results to `.agents/spec/updates/<work-unit-id>.md`

If a sub-agent would need to "figure out" something that could be answered now, answer it in the prompt. Ambiguity in a prompt leads to inconsistent or incorrect output.

### Metadata and tracking

Include the header metadata block (version, date, source TRD, status), a prerequisites checklist, and an execution phases table — all as shown in the template. Use status checkboxes so the orchestrator can track progress in-place as units complete or fail.

---

## How to Build the Memory File

Follow the memory template for structure. Adapt every section to the actual project — do not leave placeholder content. The memory file should include:

- **Project root path** — the absolute path to the project
- **Implementation status** — every file being created or modified, organized by phase/layer, with status tracking (NOT STARTED / IN PROGRESS / COMPLETE / BLOCKED)
- **Codebase patterns** — namespaces, code patterns, DI registration conventions, and anything else a sub-agent needs to produce code that fits the existing project style. Study the codebase to fill this in accurately.
- **Key files to be modified** — for each file that will be modified (not created new), describe its current shape and the exact changes needed, so agents don't misunderstand the scope
- **Open questions** — any unresolved design decisions from the TRD, with an assumed default for each so agents aren't blocked
- **Coordination notes** — file conflict risks between parallel work units, ordering constraints (which unit must run last), and project-wide style rules
- **Agent completion log** — an empty table that the orchestrator fills in as it consolidates sub-agent updates
- **Update directory** — the exact path sub-agents should write their per-work-unit reports to

---

## Output Checklist

Before finishing, verify:

- [ ] Every work unit prompt is self-contained (could be passed to `task(description, prompt)` as-is)
- [ ] Dependencies between work units are explicit and accurate
- [ ] The execution phases table matches the actual work units and layers
- [ ] The memory file has no placeholder content — every section is filled in for this specific project
- [ ] Shared status files are clearly orchestrator-owned; sub-agents only write per-work-unit update files
- [ ] The orchestration and memory files are written to `.agents/spec/`, and the update directory is documented

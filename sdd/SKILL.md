---
name: sdd
description: >
  Deterministic spec-driven development workflow for heavyweight software work. Use whenever the user invokes /sdd,
  asks for deterministic staged planning, wants stricter PRD/TRD/orchestration gates, needs grill-me style clarification
  before moving between planning stages, or wants orchestration execution that delegates implementation to sub-agents
  instead of improvising inline. Prefer this over looser planning flows when the user wants hard model gates,
  explicit approval between stages, separate gate files, and repeatable execution with cross-checks.
---

# Spec-Driven Development (SDD) Skill

This skill is the **entry and routing layer** for the SDD workflow. Its job is to bootstrap the session, resolve the working directory, and hand off to the `sdd-orchestrator` sub-agent which owns all stage logic.

Do not force this workflow onto small tickets or one-off coding tasks. Use it for work that is complex enough to need a durable plan.

---

## Bare Shortcut Output

If the developer enters exactly `/sdd` with no extra prompt, output the following block exactly and do nothing else.

=== BEGIN SDD ORIENTATION OUTPUT ===

This is the **SDD** skill — a 6-stage deterministic spec-driven workflow for heavyweight software work, with full multi-agent orchestration.

| Stage | Name                              | Sub-Agent(s)                                    |
| ----- | --------------------------------- | ----------------------------------------------- |
| 1     | Context / Grilled Planning Input  | sdd-spec-writer, sdd-researcher                 |
| 2     | PRD                               | sdd-spec-writer, sdd-researcher                 |
| 2b    | QA Review of PRD                  | sdd-qa-reviewer                                 |
| 3     | TRD                               | sdd-spec-writer, sdd-researcher                 |
| 3b    | Test Coverage Analysis            | sdd-test-coverage                               |
| 3.5   | Panel of Experts Review           | sdd-panel-expert                                |
| 4     | Orchestration                     | sdd-spec-writer                                 |
| 5     | Execution + Cross-Check + Verify  | sdd-executor, sdd-cross-checker, sdd-fix-agent  |
| 6     | PR Description                    | sdd-documentation                               |

**To start**: Run `/sdd <ticket-id>` or `/sdd path/to/session/dir` or `/sdd` followed by a ticket description.
**To resume**: Run `/sdd <ticket-id>` — you will be asked which stage to resume from.

=== END SDD ORIENTATION OUTPUT ===

Do not add commentary before or after the block.

---

## Session Bootstrap

This runs every time `/sdd` is invoked with anything beyond the bare shortcut.

### Step 1: Resolve the session directory

**Do not auto-detect or scan `.agents/specs/` for active sessions.** The developer may be working multiple tickets simultaneously and auto-detection is almost always wrong.

Instead, determine the session directory from user input using this precedence:

1. **Ticket ID provided** (e.g., `/sdd 123456`): resolve to `.agents/specs/123456/`
2. **Explicit path provided** (e.g., `/sdd path/to/context.md` or `/sdd .agents/specs/123456/`): use that path directly
3. **Ticket description pasted with no path**: ask the user: "What is the Azure DevOps work item ID or session directory for this ticket?"
4. **Stage explicitly named** (e.g., `/sdd stage 3 .agents/specs/123456/`): use the provided path and route directly to that stage

### Step 2: Validate or create the session directory

- If the directory exists: confirm with the user — "I found `.agents/specs/123456/`. Resuming this session — which stage would you like to start from?"
  - Wait for explicit stage selection. Do not infer the stage from files present.
- If the directory does not exist: confirm — "I'll create `.agents/specs/123456/` for this session. Ready to start at Stage 1?"
  - Wait for confirmation before creating anything.

### Step 3: Spawn the orchestrator

Once the session directory is confirmed and the starting stage is known, spawn the `sdd-orchestrator` sub-agent:

```
Task: sdd-orchestrator

Session directory: <resolved path>
Starting stage: <stage number or "1" if new session>
User input: <the original ticket description, task text, or context provided by the user>
Ticket ID: <ticket ID if provided>
```

Then step back. The orchestrator owns everything from here. Do not re-inject into the workflow unless the orchestrator explicitly hands back to you.

---

## Entry Rules Summary

| User Input | Action |
|------------|--------|
| Bare `/sdd` | Output orientation block, stop |
| `/sdd 123456` | Resolve `.agents/specs/123456/`, ask which stage, spawn orchestrator |
| `/sdd .agents/specs/123456/prd.md` | Use that path, ask which stage, spawn orchestrator |
| `/sdd <ticket description pasted>` | Ask for ticket ID or directory first, then proceed |
| `/sdd stage 3 .agents/specs/123456/` | Spawn orchestrator at Stage 3 directly |

---

## Reference Files

The orchestrator and sub-agents read these as needed. You do not need to read them during session bootstrap.

| File                               | Purpose                                                                |
| ---------------------------------- | ---------------------------------------------------------------------- |
| `references/model-routing.md`      | Model family gate rules and cross-family checking policy               |
| `references/artifact-rules.md`     | Session folder rules, canonical filenames, and path handling           |
| `references/gate-templates.md`     | Fixed gate report templates for each planning and execution gate       |
| `references/work-unit-template.md` | Required work-unit contract for orchestration                          |
| `references/execution-rules.md`    | Stage 5 execution topology, cross-check lenses, and remediation rules  |
| `references/agent-routing.md`      | How the orchestrator routes to sub-agents at each stage                |
| `references/beads-integration.md`  | Beads CLI integration for Stage 5 work-unit tracking                   |

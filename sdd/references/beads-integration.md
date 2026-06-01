# Beads Integration for SDD Stage 5

This guide explains how the Orchestrator uses Beads (`bd` CLI) for work-unit tracking during Stage 5 execution.

## Overview

- **Planning layer** (`orchestration.md`): Describes *what* work needs to be done and *why*
- **State layer** (Beads): Tracks *status* of work during execution (who's working, what's done, blockers)

Markdown is for durable specifications. Beads is for runtime coordination.

## Workflow

### Phase 1: Session Setup

When Stage 5 execution begins:

```bash
# Initialize Beads in the project (if not already done)
cd /path/to/project
bd init

# Create an epic for this SDD session
bd create "[Project] SDD Execution Session" --epic
# Returns: bd-abc1 (epic ID)

# Store epic ID for work-unit creation
EPIC_ID=bd-abc1
```

### Phase 2: Work-Unit Creation (Orchestrator)

For each work unit in `orchestration.md`:

```bash
# Create a Beads issue for the work unit
bd create "Work Unit: [WU-ID] - [Objective]" --epic $EPIC_ID --description "[Summary from orchestration.md]"

# Returns: bd-abc1.1 (work unit ID)
# Orchestrator stores this ID and passes it to the executor
```

### Phase 3: Work-Unit Execution (Executor)

When `sdd-executor` is spawned for a work unit:

1. **Read the work unit**:
   ```bash
   bd show <work-unit-id>
   bd show <work-unit-id> --json  # For parsing
   ```

2. **Claim the work**:
   ```bash
   bd update <work-unit-id> --claim
   # Atomic: sets assignee + status to "in_progress"
   ```

3. **Report progress** (optional, during work):
   ```bash
   bd update <work-unit-id> --note "Completed module X; starting tests"
   bd update <work-unit-id> --note "All tests passing; ready for review"
   ```

4. **Close when done**:
   ```bash
   bd close <work-unit-id> "Implementation complete; tests passing; build verified"
   # Marks status as "closed" and records the completion note
   ```

### Phase 4: Orchestrator Consolidation

After a layer of work units completes:

```bash
# List all issues under the epic to see status
bd show $EPIC_ID --json | jq '.children[]' # See all work units

# List unblocked work (ready to execute next layer)
bd ready --json

# Query specific status
bd show <work-unit-id> --json | jq '.status, .assignee, .closed_at'
```

Update `memory.md` with consolidated findings.

## Key Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `bd init` | Initialize Beads in a project | `bd init` |
| `bd create <title>` | Create a new issue | `bd create "Auth module" --epic <id>` |
| `bd show <id>` | View issue details | `bd show bd-a1b2.1` |
| `bd show <id> --json` | JSON output (for parsing) | `bd show bd-a1b2.1 --json` |
| `bd update <id> --claim` | Claim and mark in_progress | `bd update bd-a1b2.1 --claim` |
| `bd update <id> --note "..."` | Add progress note | `bd update bd-a1b2.1 --note "Done"` |
| `bd close <id> "..."` | Close and mark complete | `bd close bd-a1b2.1 "Finished"` |
| `bd ready --json` | List unblocked issues | `bd ready --json \| jq '.[]'` |
| `bd dep add <child> <parent>` | Create dependency link | `bd dep add bd-a1b2.1 bd-a1b2` |

## Beads Issue Structure

Each work-unit issue has:

- **ID**: `bd-xyz.1` (epic-scoped numeric ID)
- **Title**: From work unit objective
- **Description**: Summary from `orchestration.md`
- **Status**: `open`, `in_progress`, `closed`
- **Assignee**: Set by executor when claimed
- **Labels**: `wip`, `blocked`, `complete` (optional, for filtering)
- **Notes**: Progress updates from executor
- **Closed at**: Timestamp when closed

## Error Handling

If an executor fails or gets blocked:

```bash
# Mark as blocked (don't close)
bd update <id> --note "Blocked on: [reason]. Waiting for [dependency]."

# Orchestrator checks status
bd show <id> --json | jq '.status'

# If truly stuck, revert claim
# (Beads doesn't have explicit "unclaim", so just note the issue in memory.md)
```

## Querying Session State

Get a high-level view of progress:

```bash
# Count issues by status
bd show $EPIC_ID --json | jq '[.children[] | .status] | group_by(.) | map({status: .[0], count: length})'

# Find blockers
bd show $EPIC_ID --json | jq '.children[] | select(.status != "closed")'

# Get all closure notes
bd show $EPIC_ID --json | jq '.children[] | select(.closed_at) | {id, title, closure_note}'
```

## Notes

- **Beads is append-only**: Updates don't overwrite; they add notes. This is intentional for audit trails.
- **No partial close**: Once closed, an issue is done. If work resumes, it's a new issue.
- **Dependency links**: Use `bd dep add` if work unit B is blocked by A (optional but useful for complex DAGs).
- **Memory precedence**: `memory.md` is orchestrator-owned. Beads tracks individual work-unit state. Cross-reference as needed.

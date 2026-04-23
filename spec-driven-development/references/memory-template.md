# [FEATURE NAME] -- Orchestrator Memory

> **Purpose:** Orchestrator-owned state file for orchestration work. Sub-agents read this file for context. The orchestrator is the only writer of this file.
> **Source TRD:** `[path/to/technical-requirements-document.md]`
> **Orchestration:** `[path/to/orchestration-plan.md]`
> **Per-work-unit updates:** `.agents/spec/updates/<work-unit-id>.md`

---

## Project Root

`[/absolute/path/to/project-root]`

**Main source project:** `[SourceProject/]`

---

## Implementation Status

<!-- 
  Organize implementation into phased layers. Lower layers have zero 
  dependencies on higher ones. Higher layers depend on lower ones.
  Each phase groups work units (WU) that can run in parallel.
  Status values: NOT STARTED | IN PROGRESS | COMPLETE | BLOCKED
  Update this table only from orchestrator consolidation, not directly from sub-agents.
-->

### Phase 1: [Layer Name] (Layer 0 -- no internal dependencies)

| File | Status | Agent | Notes |
|------|--------|-------|-------|
| `path/to/NewFile.cs` | NOT STARTED | WU-0.1 | Brief description of purpose |
| `path/to/ExistingFile.cs` | NOT STARTED | WU-0.1 | MODIFY: describe what changes |
| `path/to/AnotherNewFile.cs` | NOT STARTED | WU-0.2 | Brief description of purpose |

### Phase 2: [Layer Name] (Layer 1 -- depends on Phase 1)

| File | Status | Agent | Notes |
|------|--------|-------|-------|
| `path/to/File.cs` | NOT STARTED | WU-1.1 | Brief description |
| `path/to/ExistingFile.cs` | NOT STARTED | WU-1.2 | MODIFY: describe changes |

### Phase 3: [Layer Name] (Layer 2 -- depends on Phases 1-2)

| File | Status | Agent | Notes |
|------|--------|-------|-------|
| `path/to/File.cs` | NOT STARTED | WU-2.1 | Brief description |

### Phase 4: [Layer Name] (Layer 3 -- depends on Phases 1-3)

| File | Status | Agent | Notes |
|------|--------|-------|-------|
| `path/to/File.cs` | NOT STARTED | WU-3.1 | Brief description |
| `path/to/EntryPoint.cs` | NOT STARTED | WU-3.5 | MODIFY: register all new services (run LAST) |

---

## Codebase Patterns Reference

<!-- 
  Document the project's existing conventions so every sub-agent 
  produces consistent code without re-discovering patterns. 
  Include namespaces, code patterns, and DI registration conventions.
-->

### Namespaces

- Domain models: `Project.Namespace.Domain`
- Persistence models: `Project.Namespace.Persistence`
- Repository interfaces: `Project.Namespace.Data.Interfaces`
- Repository implementations: `Project.Namespace.Data`
- Commands: `Project.Namespace.Commands`
- Queries: `Project.Namespace.Queries`
- Handlers: `Project.Namespace.Handlers`
- Controllers/API: `Project.Namespace.Api`
- Configuration: `Project.Namespace.Configuration`
<!-- Add/remove namespace categories as appropriate for your project -->

### [Pattern Name] Pattern

<!-- 
  For each significant code pattern used in the project, provide:
  1. A brief description of the convention
  2. A minimal code example
  3. Any gotchas or deviations specific to this feature
-->

```language
// Example code showing the pattern
public record ExampleCommand : IRequest<ExampleResponse>
{
    public required string SomeProperty { get; set; }
}
```

### [Another Pattern Name] Pattern

```language
// Example code showing the pattern
```

### DI / Service Registration

<!-- How new services get wired up in the project -->

- Config binding: `builder.Services.Configure<T>(builder.Configuration.GetSection(...))`
- Repositories: registered via [scanning/manual registration]
- Background services: `builder.Services.AddHostedService<T>()`
<!-- Add project-specific registration conventions -->

### Key Existing Files That Will Be Modified

<!--
  List every file that will be MODIFIED (not created new).
  Include current size/shape and exactly what changes are needed.
  This prevents agents from misunderstanding the scope of a modification.
-->

1. **`path/to/ExistingFile.cs`** -- [current size/shape]. [Describe exact modification needed].
2. **`path/to/AnotherFile.cs`** -- [current size/shape]. [Describe exact modification needed].

---

## Open Questions

<!--
  Document unresolved design decisions from the TRD. 
  For each, state the assumed default so agents aren't blocked.
-->

- **Q1 ([topic]):** [Assumed default or decision].
- **Q2 ([topic]):** [Assumed default or decision].

---

## Coordination Notes

<!--
  Call out anything that prevents race conditions or merge conflicts 
  between parallel agents. Include:
  - File conflict risks between work units
  - Ordering constraints (which WU must run last)
  - Project-wide style rules all agents must follow
-->

- **File conflicts:** [Describe any overlap between work units and why it's safe or how to resolve]
- **Ordering:** [Which work unit(s) must run last and why]
- **Style rules:** [List project-wide conventions all agents must follow, e.g., file-scoped namespaces, nullable reference types, etc.]
- **All agents** should check this memory file first for context, then write their result to their assigned per-work-unit update file.

---

## Agent Completion Log

| Agent | Completed At | Update File | Notes |
|-------|-------------|-------------|-------|
| (none yet) | | | |

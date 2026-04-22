# [FEATURE_NAME] — Cross-Repository Plan

---

## What We're Building

[2-3 sentences describing the feature from the user's perspective. What does the user see and do? What happens behind the scenes at a high level?]

### Feature Parity

[If the feature spans multiple clients (web, mobile, API consumers), list the shared capabilities here. This makes it immediately clear what each client needs to implement and where they converge.]

- [Capability 1]
- [Capability 2]
- [Capability 3]

---

## How It Works

[End-to-end request/data flow for the happy path. An ASCII diagram is strongly encouraged — it makes the architecture scannable at a glance. Show the entry point (user action or trigger), each service/component in the chain, and where data lands.]

```
User does X → Client calls Y
  → Server handles Z:
      1. Step one
      2. Step two
      3. Step three
  → Result flows back to client
```

---

## Repositories

[Table of every repository involved. Each row identifies the layer it serves and any high-level notes about what changes there.]

| Layer | Repository | Notes |
|---|---|---|
| [Server/Web/Mobile/Infra] | `repo-name` | [Brief description of changes] |

---

## [Repository 1 Name]

[One section per repository. Each section should give a reader everything they need to understand what changes in that repo without reading the other sections.]

### Data Layer

[New collections/tables/schemas and modifications to existing ones. Include field names, types, indexes, and relationships where known.]

**New:**

[Describe new data structures — collections, documents, schemas, enums.]

**Modified:**

[Describe changes to existing data structures — new fields, changed types, new indexes.]

### Processing Pipeline

[The key components that do the work in this repo. Table format works well for listing services, handlers, clients, etc.]

| Component | What It Does |
|---|---|
| `ComponentName` | [Description] |

### API Surface

[New and modified API endpoints, GraphQL mutations/queries, hooks, exported functions — whatever this repo exposes to other layers or to the user.]

```graphql
# Example for GraphQL repos
mutation doSomething($input: Input!): Result!
query getSomething($id: ID!): Result
```

### Configuration

[New config entries, environment variables, feature flags. Note whether new secrets are needed or if existing config is reused.]

### Files

**New:**

| File | Purpose |
|---|---|
| `path/to/file.ext` | [What it does] |

**Modified:**

| File | Change |
|---|---|
| `path/to/existing.ext` | [What changes] |

---

## [Repository 2 Name]

[Repeat the same structure: Data Layer, Processing Pipeline / Components, API Surface, Configuration, Files.]

---

## [Repository N Name]

[Repeat for each additional repository.]

---

## Error Handling & Edge Cases

[Cross-repo matrix. Rows are scenarios, columns are how each layer handles them. This is one of the most valuable sections — it forces you to think through failures before writing code.]

| Scenario | Server | Client |
|---|---|---|
| [Error condition 1] | [Server behavior] | [Client behavior] |
| [Error condition 2] | [Server behavior] | [Client behavior] |
| [Happy-path edge case] | [Server behavior] | [Client behavior] |

---

## Dependencies & Prerequisites

[What does this feature build on? What already exists, what needs to be created, and what is unknown? The status column is critical for identifying blockers early.]

| Item | Status | Notes |
|---|---|---|
| [Existing service/API/config] | **Available** | [How it's reused] |
| [New thing that must be created] | **Must create** | [Details] |
| [Unknown or needs verification] | **Unknown** | [What to verify and with whom] |

---

## Feature Flag Rollout

[If the feature is gated behind a flag, document the flag key, type, default value, and rollout sequence.]

| Flag Key | `flag-key-name` |
|---|---|
| Type | Boolean |
| Default | `false` |

**Rollout sequence:** [e.g., dev → test → accept → prod, gated by QA sign-off at each stage.]

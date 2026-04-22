# Plan-Driven PRD: Interview Guide

This guide is for when a developer wants to build a PRD interactively — they know what they want to build but don't have a written PRD. The goal is to produce a plan document (using `plan-template.md`) that's detailed enough to generate a TRD from.

The key insight: the exemplar plan that this template is based on was written by someone with deep codebase knowledge. You need to replicate that depth by exploring the repositories before asking detailed questions. Don't front-load the user with a wall of questions — research first, then ask targeted questions informed by what you found.

---

## Phase 1: Understand the Feature (ask the user)

Start here. You need the big picture before you can explore anything.

1. **What are you building?** Get a 2-3 sentence summary of the feature from the user's perspective. What does the user see and do?
2. **Which repositories are involved?** Ask for repo names/paths and what layer each serves (server, web, mobile, infra, etc.). If the user mentions a work item ID, check for linked context files or wiki pages.
3. **Is there an existing PRD, wiki page, or design doc?** The user may have partial documentation — an ADO wiki page, a Confluence doc, a CONTEXT.md, API documentation for an external service. Gather these paths/URLs now.

Don't go deeper yet. You need to explore the code before you can ask good questions about data models, API surfaces, and error handling.

---

## Phase 2: Research the Repositories (use sub-agents)

This is the most important phase. Spawn sub-agents to explore each repository in parallel. Each sub-agent should:

1. **Understand the tech stack and conventions** — framework, language, project structure, how similar features are organized.
2. **Find analogous features** — look for existing features that are structurally similar to what's being built. How are they wired up? What patterns do they follow? These become the blueprint.
3. **Identify the integration points** — where will the new feature connect to existing code? What models, services, or APIs will it touch?
4. **Look for existing scaffolding** — has any groundwork already been laid? Prior PRs, existing data models, configuration sections, shared infrastructure that can be reused.
5. **Read any documentation the user pointed to** — wiki pages, CONTEXT.md files, external API docs.

Have each sub-agent report back with a structured summary. This research directly informs the questions you'll ask in Phase 3 and the plan you'll draft in Phase 4.

---

## Phase 3: Targeted Questions (ask the user)

Now that you understand the codebase, ask specific questions. Group them logically — don't scatter them across multiple messages. Aim for one focused round of questions, maybe two.

### Data flow and architecture

- "Here's how I think the request flow works: [describe based on research]. Is that right, or am I missing something?"
- "I see [existing pattern X] used for [similar feature]. Should we follow the same pattern, or is there a reason to diverge?"
- "For the data model, I'm thinking [proposed structure based on analogous features]. Does that match your mental model?"

### Cross-repo coordination

- "How should real-time updates flow from server to clients?" (if applicable — maybe there's already SignalR, WebSockets, polling, etc.)
- "Should web and mobile have identical behavior, or are there differences?" (if multi-client)

### Edge cases (informed by what you found)

- "I see [existing feature] handles [error case] by [doing X]. Should this feature handle it the same way?"
- "What happens if [specific failure scenario based on the architecture]?"
- "Are there any external service dependencies with unknown behavior we should verify?"

### Rollout

- "Should this be behind a feature flag? What's the rollout strategy?"
- "Any prerequisites that need to land first — other PRs, config changes, external API availability?"

### What NOT to ask

- **Don't ask about effort estimates.** They aren't useful and aren't included in the plan.
- **Don't ask questions the code already answers.** If you found the answer during research, state it as a fact and let the user correct you if wrong.
- **Don't ask abstract architecture questions.** "What pattern should we use?" is vague. "Should we use the same Channel<T> + BackgroundService pattern that [existing feature] uses?" is specific and answerable.

---

## Phase 4: Draft the Plan

Using `plan-template.md` as the structure, draft the plan. Key principles:

1. **Be specific about files.** The exemplar lists every new and modified file with its purpose. This is what makes the plan actionable for TRD generation. Use your research from Phase 2 — you explored the repo structure, so you should know where files belong.

2. **Show the data flow.** The ASCII diagram in "How It Works" is one of the most valuable parts. It makes the architecture instantly scannable.

3. **Per-repo sections are self-contained.** Someone reading only the "Mobile" section should understand everything that changes in mobile without reading the "Server" section. Cross-references are fine, but each section should stand alone.

4. **Error handling matrix is critical.** This is where you demonstrate that edge cases have been thought through. Use the cross-repo table format — it forces completeness.

5. **Dependencies surface blockers.** Mark each as Available / Must Create / Unknown. Unknowns are especially important — they represent risks that should be resolved before implementation starts.

6. **Never include effort estimates.** No day counts, no story points, no t-shirt sizes. The plan describes *what* changes, not *how long* it takes.

Save the plan to `.agents/specs/{work-item-id}/plan.md` if a work item ID is known, otherwise to `.agents/spec/plan.md`.

---

## Phase 5: Review and Refine

Present the plan to the user. They'll likely have corrections — wrong file paths, missing edge cases, architectural decisions that need to change. Iterate until they confirm it's solid.

The plan is ready for TRD generation when:
- Every repository section has complete file inventories (new + modified)
- The data flow is clear end-to-end
- Error handling covers the important scenarios
- Dependencies and prerequisites are identified with known statuses
- The user says it's good

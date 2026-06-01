# Model Routing

Use this file as the source of truth for model assignments and cross-family checking policy.

## Model Roster

All models use the `github-copilot` provider. Format: `github-copilot/<model-id>`

| Model ID | Full ID | Use Cases |
|----------|---------|-----------|
| `claude-haiku-4.5` | `github-copilot/claude-haiku-4.5` | Lightweight coordination, file reading, structured summaries |
| `claude-sonnet-4.6` | `github-copilot/claude-sonnet-4.6` | Moderate reasoning, synthesis, writing |
| `claude-opus-4.5` | `github-copilot/claude-opus-4.5` | Deep planning, complex PRD/TRD, rigorous interviewing |
| `gpt-5.4` | `github-copilot/gpt-5.4` | Adversarial review, cross-family analysis, summarization |
| `gpt-5.4-mini` | `github-copilot/gpt-5.4-mini` | Structured cross-checks, mechanical analysis, lightweight fixes |
| `gpt-5.3-codex` | `github-copilot/gpt-5.3-codex` | Code implementation, test writing |

---

## Sub-Agent Model Assignments

| Sub-Agent | Model | Family | Rationale |
|-----------|-------|--------|-----------|
| `sdd-orchestrator` | `github-copilot/claude-haiku-4.5` | Anthropic | Lightweight coordination and routing only |
| `sdd-spec-writer` | `github-copilot/claude-opus-4.5` | Anthropic | Heaviest planning work; opus earns its cost here |
| `sdd-researcher` | `github-copilot/claude-haiku-4.5` | Anthropic | Read-heavy file exploration; no deep reasoning needed |
| `sdd-qa-reviewer` | `github-copilot/gpt-5.4-mini` | OpenAI | Structured checklist QA; cross-family from spec-writer |
| `sdd-test-coverage` | `github-copilot/gpt-5.4-mini` | OpenAI | Structured test gap analysis; cross-family from spec-writer |
| `sdd-panel-expert` | `github-copilot/gpt-5.4` | OpenAI | Adversarial blind review; needs full reasoning depth |
| `sdd-executor` | `github-copilot/gpt-5.3-codex` | OpenAI | Code implementation; codex variant optimized for coding |
| `sdd-cross-checker` | `github-copilot/gpt-5.4-mini` | OpenAI | Structured traceability checks; cross-family from executor |
| `sdd-fix-agent` | `github-copilot/gpt-5.4-mini` | OpenAI | Mechanical fixes; same cross-family benefit |
| `sdd-documentation` | `github-copilot/gpt-5.4` | OpenAI | PR description summarization; gpt-5.4 excels at this |

---

## Cross-Family Checking Rule

The most important model policy in this workflow:

**No model family should review its own work at critical quality gates.**

- Planning work is done by **Anthropic** models (spec-writer = Opus)
- Cross-checks and quality gates use **OpenAI** models (QA reviewer, test coverage, panel expert)
- Implementation is done by **OpenAI** models (executor = codex)
- Implementation cross-checks are also **OpenAI** (cross-checker = gpt-5.4-mini, different from executor but same family is acceptable here since cross-checker is checking against the Anthropic-written plan)

The key gates where this matters most:

| Gate | Produced By | Reviewed By | Rationale |
|------|------------|-------------|-----------|
| PRD (Stage 2b QA) | Anthropic spec-writer | OpenAI qa-reviewer | Different vendor catches testability gaps |
| TRD (Stage 3b test coverage) | Anthropic spec-writer | OpenAI test-coverage | Different vendor catches test strategy gaps |
| TRD (Stage 3.5 panel) | Anthropic spec-writer | OpenAI panel-expert | Different vendor catches architectural blindspots |

---

## Hard Gate Rule

If the active model does not match the recommended model for the current sub-agent:

1. The orchestrator notes the mismatch
2. Reports to the user: "Stage [N] typically uses [model]. Current sub-agent is configured for [model]. Proceed anyway?"
3. Waits for explicit user approval before continuing on a mismatched model

Do not quietly continue on the wrong model. Model diversity at quality gates is a core correctness property of this workflow.

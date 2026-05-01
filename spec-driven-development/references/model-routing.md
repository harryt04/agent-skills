# Model Routing

Use this file as the single source of truth for stage-level model guidance.

## Recommended Models by Stage

| Stage | Recommended Model | Why |
| ----- | ----------------- | --- |
| 1 — App Outline / Context Gathering | sonnet 4.6, or haiku latest if lightweight | Good balance for interviewing and synthesis |
| 2 — PRD | opus 4.6, or gpt 5.4 | Better product reasoning and trade-off handling |
| 3 — TRD | opus 4.6, or gpt 5.4 | Better decomposition, dependency design, and constraint handling |
| 4 — Orchestration Plan | gpt 5.4, or opus 4.6 | Strong prompt construction and instruction fidelity |
| 5 — Execute Orchestration | codex 5.3, or sonnet 4.6 | Efficient code-level execution per work unit |
| 6 — Implementation Cross-Check | gpt 5.4, or opus 4.6 | Strong traceability and gap detection |
| 7 — Test Coverage | sonnet 4.6, or codex 5.3 for heavy test writing | Balanced analysis plus practical test work |
| 8 — Post-Test Cross-Check | gpt 5.4, or opus 4.6 | Strong final audit quality |
| 9 — PR Description | gpt 5.4, then sonnet latest, then haiku latest | Strong summarization and final write-up quality |

## Mismatch Handling

Treat model guidance as advisory by default.

When the current model is weaker than recommended for the stage:

1. State the likely trade-off in one short sentence.
2. Suggest switching if the stage is high-risk or document quality matters.
3. Proceed on the current model unless the developer wants to switch.

Do not interrupt every stage with a mandatory confirmation loop. Reserve harder warnings for cases where a low-capability model is likely to cause expensive rework.

## Suggested Wording

Use short wording like:

`This looks like Stage 3. A stronger model would likely produce a better TRD with fewer missing dependencies, but I can proceed here if you want.`

If the developer wants to switch:

- Tell them to use `Ctrl+P` to change models in OpenCode, or start a new session with the desired model.
- Resume from the same stage after the switch.

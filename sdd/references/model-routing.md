# Model Routing

Use this file as the source of truth for hard model-family gates.

## Recommended Model Families by Stage

| Stage | Name | Recommended Model Family |
| --- | --- | --- |
| 1 | Context / Grilled Planning Input | `gpt`, `sonnet`, or `haiku` if simple |
| 2 | PRD | `gpt` or `opus` |
| 3 | TRD | `gpt` or `opus` |
| 4 | Orchestration | `gpt` |
| 5 | Execution + Cross-Check + Build/Lint/Test Verification | `codex` or `sonnet` |
| 6 | PR Description | `gpt`, `sonnet`, or `haiku` if simple |

## Hard Gate Rule

If the active model family is not approved for the current stage:

1. Stop immediately.
2. Tell the developer which stage they are trying to run.
3. Tell them which model families are recommended for that stage.
4. Ask them to switch models before continuing.
5. Proceed only if the developer explicitly says to continue anyway.

Do not quietly continue on the wrong model.

## Suggested Wording

Use short wording like:

`This looks like Stage 4. SDD requires a gpt-family model for orchestration work. Please switch models before continuing, or tell me explicitly to proceed anyway on the current model.`

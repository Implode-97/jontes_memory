---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:53:06
source: codex-conversation
confidence: high
---

# Learning: workspace assignment required input guard readability

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/scripts/required-input-guard.sh` for new-maintainer readability, KISS, YAGNI, and maintainability.

## Learning
The guard is a small, acceptable generated-input shape check: it verifies `workspace_teams` exists, rows have non-empty `team_key`, states are in the lifecycle enum, and duplicate placement keys are rejected. The main readability debt is not the guard behavior itself, but copied validation that also appears in `destroy-guard.sh`, plus comment-stripping despite the input being Terragrunt-generated JSON rather than raw YAML.

## Future Use
For future workspace-assignment guard reviews, preserve strict shape validation and duplicate-key rejection. Prefer narrow cleanup: remove generated-JSON comment handling, avoid inventing broad shell abstractions, and only share validation with the destroy guard if independent script execution remains obvious.

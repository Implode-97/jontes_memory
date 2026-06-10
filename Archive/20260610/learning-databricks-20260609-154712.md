---
type: short-term-learning
topic: databricks
created: 2026-06-09 15:47:12
source: codex-conversation
confidence: high
---

# Learning: Workspace Assignment Destroy Guard Review

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/scripts/destroy-guard.sh` for looks, readability, KISS/YAGNI, maintainability, and Databricks/Terragrunt lifecycle safety.

## Learning
The workspace assignment destroy guard is acceptable with cleanup because the state-machine complexity protects real lifecycle invariants: first apply must bootstrap only enabled rows, offboarding must pass through `destroy_pending`, `destroyed` tombstones must not resurrect, and row removal is allowed only after prior Terraform output reports `destroyed`. The main debt is duplicated input-shape validation with `required-input-guard.sh` and an overly broad output fallback where a missing or malformed `workspace_team_lifecycle_states.value` can look like bootstrap state.

## Future Use
For future reviews of workspace-assignment guards, preserve fail-closed Terraform output handling and row-removal protection. Prefer narrow cleanup that shares or centralizes generated input validation, distinguishes absent bootstrap output from malformed present output, and makes the jq transition matrix table-like without hiding the lifecycle policy behind broad shell abstractions.

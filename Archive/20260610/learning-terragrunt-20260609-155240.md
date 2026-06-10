---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:52:40
source: codex-conversation
confidence: high
---

# Learning: Keep Workspace Assignment Required Input Guard Ownership Clear

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/scripts/required-input-guard.sh` for looks, readability, KISS, YAGNI, maintainability, and guard pipeline safety.

## Learning
The required input guard is intentionally small and useful for fast generated-input validation before `init`, `plan`, and `apply`. Its `workspace_teams` checks and duplicate detection are readable and should be preserved. The main ownership risk is that the generated JSON also contains `teams`, but this guard does not validate that sibling array, while the registry guard later consumes it and also runs on `validate`, where the required input guard currently does not run.

## Future Use
For future workspace-assignment guard reviews, preserve the fast fail-closed preflight. Prefer narrow cleanup: either run this guard on `validate` too and validate `teams` shape here, or make the registry guard independently validate the generated input it consumes. Do not replace the simple jq checks with a broad abstraction.

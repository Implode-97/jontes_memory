---
type: short-term-learning
topic: terragrunt
created: 2026-06-11 18:16:18
source: codex-conversation
confidence: high
---

# Learning: CAV-131446 native dependency refactor

## Context
The user asked whether the CAV-131446 compute policy resolver script was needed, then asked to implement the simpler long-term enterprise Terragrunt shape.

## Learning
Commit `47210ef` on `feature/codex/CAV-131446-compute-policy-on-CAV-133921-platform-e2e` removes `units/dbx_compute_policy/scripts/resolve_workspace_assignment_contract.sh` and replaces the shell/JQ resolver with a native Terragrunt `dependency "workspace_assignments"` block. The unit now reads `dependency.workspace_assignments.outputs.workspace_team_assignments.teams` directly in `generate` and `inputs`. Mocks are allowed only for `init`, `validate`, and `plan`; live `apply` and `destroy` require the real sibling output. The MR `!29` description was updated to reflect this.

## Future Use
For future catalog units, prefer native Terragrunt dependency blocks over custom `run_cmd` output readers. Use mocks for non-live commands only, and make live commands fail closed when required upstream outputs are missing.

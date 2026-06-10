---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:46:55
source: codex-conversation
confidence: high
---

# Learning: workspace assignment destroy guard cleanup lens

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_workspace_assignments/scripts/destroy-guard.sh` for readability, KISS, YAGNI, maintainability, and new platform maintainer safety.

## Learning
The guard's core lifecycle state machine is justified because it protects workspace team placement from direct `enabled` to `destroyed`, resurrection after `destroyed`, and premature YAML row removal. The readability debt is concentrated in duplicated input-shape validation shared with `required-input-guard.sh`, the brittle textual Terraform-output bootstrap detection, and treating a missing `workspace_team_lifecycle_states` output as empty prior state outside an explicitly proven first-apply case.

## Future Use
For future workspace-assignment guard reviews, preserve fail-closed lifecycle behavior and all-violations reporting. Prefer narrow cleanup: share or remove duplicated prelude only if independent script execution remains clear, use Terragrunt/Terraform hook context deliberately, and make bootstrap/no-state handling distinguishable from missing or renamed lifecycle outputs.

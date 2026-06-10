---
type: short-term-learning
topic: databricks
created: 2026-06-09 15:25:26
source: codex-conversation
confidence: high
---

# Learning: dbx team identity registry guard readability

## Context
This came from reviewing `data-platform-terragrunt-catalog/units/dbx_team_identities/scripts/registry-guard.sh` for looks, readability, KISS, YAGNI, maintainability, and lifecycle safety.

## Learning
The registry guard's empty-registry branch is justified because it protects whole-registry deletion and relies on the module's `team_lifecycle_states` output as the prior-state source of truth. The main readability debt is not that it shells out to `terraform output -json`; official Terraform docs still recommend `-json` for automation, and Terragrunt before hooks support this pre-plan/pre-apply guard shape. The main cleanup target is duplicated input shape and duplicate-key validation shared with `destroy_guard.sh`, plus the hidden assumption that missing `team_lifecycle_states` can be treated as `{}` before being rejected only in the empty-current path.

## Future Use
For future reviews, preserve the final-row deletion protection and prior-state output lookup, but penalize duplicated guard validation and make missing-output semantics explicit where possible.

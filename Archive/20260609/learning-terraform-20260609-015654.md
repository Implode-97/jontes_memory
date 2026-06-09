---
type: short-term-learning
topic: terraform
created: 2026-06-09 01:56:54
source: codex-conversation
confidence: high
---

# Learning: Team identity wrapper main.tf cleanup targets

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/main.tf`, the user asked for a pragmatic platform maintainer view focused on role-set edits, nesting-chain edits, lifecycle behavior, and KISS/YAGNI.

## Learning
The wrapper's lifecycle split is worth preserving: `team_lifecycle_states` keeps destroyed tombstones while `managed_teams` omits only `state = "destroyed"`, and the explicit module-level `depends_on = [module.groups]` is justified because the membership child resolves created groups through Databricks data sources. The cleanup pressure is narrower: `group_specs` carries unused team metadata, and `nesting_specs` repeats a linear adjacent role chain that could be derived from `role_names` if kept readable.

## Future Use
For future reviews or edits, keep the lifecycle and provider/data-source ordering intact. Prefer cleanup that removes unused local fields and considers a simple adjacent-role nesting local before changing public outputs, tests, or Terragrunt guard behavior.

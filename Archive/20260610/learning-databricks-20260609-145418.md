---
type: short-term-learning
topic: databricks
created: 2026-06-09 14:54:18
source: codex-conversation
confidence: high
---

# Learning: Treat Account-Wide Databricks Defaults As Activation Decisions

## Context
While scoring the `dbx_budget_policy` Terragrunt wrapper, reviewers agreed that the file was readable but risky to activate because normal enabled team rows would automatically produce account-wide Databricks budget policies and grants.

## Learning
For Databricks account-scoped controls, `workspace_ids = []` or empty workspace bindings should be treated as an explicit all-workspaces decision, not a harmless default. Avoid broad automatic enablement from shared registries unless the blast radius is named in code or the feature has an opt-in field. Cost/owner attribution tags should be protected from casual override; merging `extra_tags` after platform attribution weakens that contract.

## Future Use
When reviewing or designing Terragrunt wrappers for Databricks account-level resources, check default scope, registry state defaults, dependency ordering for generated principals/groups, and tag merge precedence before activation. Prefer small explicit locals such as `active_teams`, no redundant lookup maps, and account-stack wiring that matches the resource scope.

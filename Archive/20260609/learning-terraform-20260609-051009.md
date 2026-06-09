---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:10:09
source: codex-conversation
confidence: high
---

# Learning: Keep Databricks workspace assignment tests contract-focused

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_team_role_nesting_dev_mock_apply.tftest.hcl`, the user asked for a Databricks entitlement/team-nesting semantics lens.

## Learning
For this wrapper, mock apply tests are justified when they protect the team entitlement mapping and stable downstream `workspace_team_assignments` contract. The useful semantic mapping is `consumer -> workspace_consume`, `reader -> sql_access`, and `contributor -> workspace_access`. Exact mocked `databricks_group_member` resource IDs are weak evidence because they mostly preserve output plumbing and arbitrary fixture IDs. The built-in `users` baseline fixture is also noisy in team-nesting tests, especially while Databricks system-group entitlement behavior is changing.

## Future Use
When reviewing or editing these tests, prefer assertions on stable downstream fields and direct group/member mapping facts. Keep users-baseline assertions in focused baseline tests, and avoid duplicating dev/prd fixture shape unless an environment-specific contract is actually being protected.

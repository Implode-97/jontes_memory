---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:02:47
source: codex-conversation
confidence: high
---

# Learning: Workspace Assignment Mock Apply Test Safety Boundary

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_apply_outputs.tftest.hcl`, the user asked for a Databricks entitlement safety lens.

## Learning
The test is useful because it checks safety-critical workspace entitlement behavior without live Databricks calls: canonical account-group names, observed entitlement flags for `workspace_access`, `databricks_sql_access`, and `workspace_consume`, legacy `users` baseline behavior, and empty `workspace_team_assignments` when `team_keys = []`. Its weaker side is that many assertions go through diagnostic outputs, especially `entitlement_groups` and `workspace_users_baseline`, which should not be preserved as broad public API before freeze. The `users` baseline should be treated as migration/legacy behavior because Databricks is locking system-group entitlements in 2026.

## Future Use
For future reviews, recommend narrow cleanup rather than rework: keep direct safety assertions, add missing `USER` assignment checks for all entitlement groups if needed, and avoid expanding output contracts just for tests.

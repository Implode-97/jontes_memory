---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:03:27
source: codex-conversation
confidence: high
---

# Learning: Workspace assignments mock apply tests should prove behavior, not fixture echoes

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_apply_outputs.tftest.hcl`, the user wanted a Terraform test-framework and mock-provider semantics lens focused on `command = apply`, overrides, and assertion value.

## Learning
For this wrapper, mock-provider `command = apply` is technically valid when testing apply-time computed outputs from Databricks account/workspace resources, but the score drops when broad `override_resource` blocks feed values that the assertions then read back through diagnostic outputs. Canonical naming and empty `workspace_team_assignments.teams` are plan-known and do not need a heavy apply fixture. `workspace_users_baseline` assertions are also less durable because Databricks is locking system-group entitlements in 2026.

## Future Use
Prefer narrow mock apply coverage for safety-critical account entitlement groups and `USER` workspace assignment wiring. Move plan-known output shape checks to plan tests, frame users-baseline coverage as legacy/migration behavior, and avoid preserving broad diagnostic outputs before API freeze unless real callers consume them.

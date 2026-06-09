---
type: short-term-learning
topic: databricks
created: 2026-06-09 04:50:10
source: codex-conversation
confidence: high
---

# Learning: Workspace assignment wrapper main should avoid durable users entitlement management

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/main.tf`, the user asked for a Databricks entitlement/provider semantics lens focused on account/workspace provider split, `users` baseline behavior, `USER` assignment, `workspace_consume` conflicts, and team group nesting.

## Learning
The wrapper's account-vs-workspace split, account entitlement groups, `USER` workspace assignment, `workspace_consume` null-omission handling, and account-level team group nesting are mostly sound. The main score reducer is the unconditional `databricks_entitlements` resource for the workspace `users` system group: Databricks' June 2026 locked system-group entitlement rollout makes Terraform-managed `users` hardening an expiring workflow that can block the whole apply.

## Future Use
For future reviews of this wrapper, prefer a narrow rework that targets account entitlement groups and new system-group behavior instead of preserving long-term Terraform management of `users`.

---
type: short-term-learning
topic: terraform
created: 2026-06-09 04:55:31
source: codex-conversation
confidence: high
---

# Learning: Workspace Assignment Outputs Should Stay Consumer-Shaped

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/outputs.tf`, the user asked for a strict KISS/YAGNI Terraform output API review before module API freeze.

## Learning
For this wrapper, preserve `workspace_team_assignments` because the Terragrunt catalog documents it as a real downstream input for workspace-scoped consumers such as cluster policy units. Treat `workspace_users_baseline`, `workspace_context`, `team_role_nesting`, `team_role_membership_resources`, and broad `entitlement_groups` diagnostic detail as cleanup candidates unless a real downstream caller needs them. Databricks' June 2026 system-group entitlement rollout makes public contracts around managed `users` entitlements especially fragile.

## Future Use
In future reviews or edits, prefer narrow consumer-shaped outputs, keep provider-owned IDs out of public outputs by default, and move test assertions to resources or locals instead of preserving debug outputs for tests.

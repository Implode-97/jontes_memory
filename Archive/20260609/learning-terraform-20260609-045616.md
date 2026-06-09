---
type: short-term-learning
topic: terraform
created: 2026-06-09 04:56:16
source: codex-conversation
confidence: high
---

# Learning: workspace assignment outputs should stay consumer-shaped

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/outputs.tf`, the user asked whether entitlement diagnostics, assignment IDs, and membership IDs were useful safety signals or too much public API before freeze.

## Learning
For this wrapper, `workspace_team_assignments` is the stable downstream contract because Terragrunt consumers such as cluster policy read its workspace context, active teams, role lists, and canonical group display names. The broader outputs are weaker public API candidates: `workspace_users_baseline` is tied to Databricks' system-group entitlement migration, `team_role_membership_resources` exposes provider/resource IDs mostly used by tests, `team_role_nesting` duplicates display-name facts, and full `entitlement_groups` mixes useful group identity fields with assignment IDs and observed entitlement diagnostics.

## Future Use
Preserve narrow consumer-shaped outputs, but before API freeze prune or explicitly label diagnostic/test-only outputs. Prefer tests asserting resources directly over freezing provider-owned IDs in public wrapper outputs.

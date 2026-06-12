---
type: short-term-learning
topic: terragrunt
created: 2026-06-12 00:16:08
source: codex-conversation
confidence: high
---

# Learning: Compute Policy Defaults From Workspace Team Placement

## Context
During the CAV-131446 compute policy catalog MR, Terragrunt `dependency.workspace_assignments.outputs` caused generated-stack evaluation failures, and upgrading Terragrunt did not fix the issue.

## Learning
For default Databricks compute policy grants, derive team eligibility directly from `workspace_teams.yml` and enabled rows in `_org/teams.yml`, then generate the canonical contributor principal as `grp_team_<team_key>_contributor`. Keep `dbx_workspace_assignments` as an ordering-only dependency so group assignment happens before policy grants, but do not consume its outputs for the default grant path.

## Future Use
When continuing compute-policy catalog work, avoid reintroducing workspace-assignment output reads for default policy grants. Prefer fail-loud indexing for active workspace team rows, preserve explicit-policy override semantics, and keep duplicate-key guards before seed maps collapse duplicates.

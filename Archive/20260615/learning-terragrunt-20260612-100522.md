---
type: short-term-learning
topic: terragrunt
created: 2026-06-12 10:05:22
source: codex-conversation
confidence: high
---

# Learning: Compute Policy Team Opt-Ins Live In Org Teams

## Context
During CAV-131446 catalog work, the compute policy unit was refactored after the user changed the `_org` structure for cluster policies. The previous approach used `_org/cluster_policy_bundles.yml` plus workspace-local `cluster_policies.yml` team sandbox requests.

## Learning
For Databricks compute policies in `data-platform-terragrunt-catalog`, reusable profiles should live in `_org/cluster_policy_profiles.yml` under `compute_profiles`, with `is_default: true` marking automatic low-risk policies. Explicit team approvals should live directly on the `_org/teams.yml` team row via `compute_profile_ids`. The workspace-local `cluster_policies.yml` should not be used for team sandbox policies; keep it optional for product execution policy requests only.

## Future Use
When changing this feature, derive team policies from active `workspace_teams.yml` rows, enabled `_org/teams.yml` rows, default profiles, and team `compute_profile_ids`. Do not reintroduce bundles or workspace-local team sandbox policy files unless the user deliberately reverses this model.

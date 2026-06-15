---
type: short-term-learning
topic: terragrunt
created: 2026-06-12 14:07:34
source: codex-conversation
confidence: high
---

# Learning: Compute Policy Profiles Are File-Backed And Cost Tagged

## Context
During CAV-131446 compute policy catalog cleanup, the user clarified the intended `_org` contract after scoring the refactored unit.

## Learning
Databricks compute policy profiles in `data-platform-terragrunt-catalog` should be file-backed only: `_org/cluster_policy_profiles.yml` entries use `definition_file`, not inline `policy_definition` or `policy_definition_json`. Environments are expected to keep a default compute profile. Team-specific opt-ins stay on `_org/teams.yml` through `compute_profile_ids`. Team `cost_center` and `unit_name` should be propagated into cluster policy custom tags via the compute policy module's `authoritative_custom_tags`, which renders hidden `custom_tags.*` rules.

## Future Use
For this feature, prefer simplifying Terragrunt profile loading to file reads, keep profile IDs honest to their cost/blast radius, set auto-termination lower bounds above zero for cost control, and preserve team cost attribution in generated policy tags.

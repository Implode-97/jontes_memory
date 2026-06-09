---
type: short-term-learning
topic: terraform
created: 2026-06-09 01:59:31
source: codex-conversation
confidence: high
---

# Learning: team identity wrapper output contract

## Context
During a read-only review of `data-platform-terraform-modules/modules/databricks-account-team-identities-wrapper/outputs.tf`, the user asked for a Databricks/Terragrunt consumer contract view focused on guards, workspace/sandbox downstream modules, and account group consumers.

## Learning
The output contract is mostly justified: preserve sorted `managed_team_keys`, tombstone-preserving `team_lifecycle_states`, and per-role group display names plus Databricks SCIM IDs and ACL principal IDs. The main maintainability issue is that `team_groups` also echoes registry metadata such as redundant `team_key`, `display_name`, and managed-only `state`; `cost_center` and `unit_name` remain justified because sandbox catalog consumers pass them downstream.

## Future Use
For future reviews or edits, keep lifecycle guard outputs separate from managed-resource outputs. Consider pruning or documenting extra `team_groups` metadata before the contract hardens, but do not remove provider-created role group facts without checking downstream consumers.

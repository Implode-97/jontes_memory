---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:32:51
source: codex-conversation
confidence: high
---

# Learning: Reject ambiguous canonical naming tokens

## Context
During read-only scoring of `databricks-workspace-assignments-wrapper/variables.tf`, reviewers focused on the variable contract for `region_code`, `workspace_target_name`, `workspace_id`, and `team_keys`.

## Learning
For Terraform modules that generate stable platform identity names from caller tokens, silent normalization can hide mistakes. Trimming and deduplicating `team_keys` is operationally convenient, but padded or duplicate keys should usually fail validation when the tokens produce canonical Databricks group display names. Positive `number` validation is also weaker than an integer check for ID inputs such as Databricks `workspace_id`.

## Future Use
When reviewing IaC variable contracts, keep naming-token inputs simple and required, but prefer rejecting raw whitespace, duplicate normalized keys, fractional IDs, and overlong generated names. Use descriptions to state when referenced canonical identities must already exist, especially for team or workspace assignment wrappers.

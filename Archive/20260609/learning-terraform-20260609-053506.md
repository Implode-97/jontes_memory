---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:35:06
source: codex-conversation
confidence: high
---

# Learning: Workspace assignments provider metadata is correctly scoped

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/version.tf`, the user asked for a Databricks provider alias/domain lens focused on account-vs-workspace provider usage.

## Learning
For this wrapper, the default `databricks` provider is intentionally account-scoped for account groups and `databricks_mws_permission_assignment`, while `databricks.workspace` is the only alternate provider configuration directly used by the module for workspace group lookup and entitlements. `configuration_aliases = [databricks.workspace]` is the right Terraform metadata shape; adding a `databricks.account` alias would be unnecessary unless the implementation starts explicitly using `provider = databricks.account`.

## Future Use
In future reviews, do not flag the missing account alias in `version.tf` as a problem. Treat the version cap as the only small cleanup candidate if repo policy changes toward minimum-only provider constraints, and rely on README/provider mapping to explain that the default provider must be account-scoped.

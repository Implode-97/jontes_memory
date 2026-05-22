---
type: short-term-learning
topic: databricks
created: 2026-05-21 08:09:12
source: codex-conversation
confidence: high
---

# Learning: Databricks current_user needs workspace context

## Context
While reviewing Terraform changes in data-platform-terraform-modules, a module using an account-scoped Databricks provider attempted to read `data.databricks_current_user` to identify the IaC deployer service principal. A real-provider Terraform test failed before apply.

## Learning
`databricks_current_user` is treated by the Databricks Terraform provider as a workspace-level data source. With an account-scoped provider configured for `https://accounts.cloud.databricks.com` and no `workspace_id`, it fails with an error that workspace-level resources require a workspace ID.

## Future Use
When reviewing or implementing account-level Databricks modules, do not rely on `databricks_current_user` to discover the deployer identity unless the provider also has workspace context. Prefer an account-compatible source of the service principal identity, or explicitly validate/design the provider contract before adding deployer membership logic.

---
type: short-term-learning
topic: terraform
created: 2026-05-20 18:10:26
source: codex-conversation
confidence: high
---

# Learning: Derive the Databricks deployer SP from provider context

## Context
This supersedes the earlier metastore deployer-grant direction from the Databricks Terragrunt E2E cleanup discussion. The user questioned a new module input that passed the deployer service principal application ID into the metastore wrapper.

## Learning
For this platform, the IaC deployer service principal is provider-owned context. Do not add live/Terragrunt inputs for values the Databricks provider can report from the authenticated identity. The metastore wrapper should derive the current provider identity with `databricks_current_user`, resolve it as a service principal, and add it to the existing metastore admin group. This avoids fake flexibility and keeps the lifecycle coupled to the metastore wrapper.

## Future Use
Before adding Terraform inputs, check whether the value is already implied by provider authentication. If there is no current use case for multiple arbitrary principals, keep the module contract narrow and refactor later when real requirements appear.

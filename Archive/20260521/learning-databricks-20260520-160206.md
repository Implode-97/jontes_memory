---
type: short-term-learning
topic: databricks
created: 2026-05-20 16:02:06
source: codex-conversation
confidence: high
---

# Learning: Terragrunt E2E needs explicit Unity Catalog deployer grants

## Context
This came from fixing the platform Terragrunt E2E after Databricks failed to create a sandbox external location with `User does not have CREATE EXTERNAL LOCATION on Metastore`.

## Learning
The CI Databricks service principal can create the metastore through account-level permissions, but it is not automatically the metastore owner when the IaC sets ownership to the regional metastore admin group. Account admin capability also does not imply Unity Catalog object-creation privileges inside the metastore. The correct platform pattern is a separate bootstrap grant step after metastore and workspace creation that grants the current deployer principal `CREATE_CATALOG`, `CREATE_EXTERNAL_LOCATION`, and `CREATE_STORAGE_CREDENTIAL` on the metastore, rather than making the deployer principal metastore owner/admin.

## Future Use
When debugging Unity Catalog E2E failures, distinguish account-level bootstrap permissions from metastore-level UC privileges. Prefer explicit least-privilege `databricks_grant` resources and a Terragrunt graph edge before downstream catalog/external-location units.

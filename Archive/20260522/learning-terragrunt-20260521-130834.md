---
type: short-term-learning
topic: terragrunt
created: 2026-05-21 13:08:34
source: codex-conversation
confidence: high
---

# Learning: Terragrunt catalog centralizes deployer SP ID in common vars

## Context
While wiring the catalog repo for the Databricks deployer admin-access module change, the user wanted the `DATABRICKS_CLIENT_ID` lookup to be easy to replace centrally instead of repeated across feature units.

## Learning
In `data-platform-terragrunt-catalog`, source the Databricks deployer service-principal application/client ID through `example/non-prod-env/_common_vars.hcl` as `databricks_deployer_service_principal_application_id = get_env("DATABRICKS_CLIENT_ID")`. Feature units such as `dbx_metastore` and `dbx_workspace` should pass `deployer_service_principal_application_id = local.common.databricks_deployer_service_principal_application_id` to wrapper modules.

## Future Use
When adding more Databricks account/workspace wrapper inputs that refer to the current CI deployer identity, prefer the common vars local over direct `get_env("DATABRICKS_CLIENT_ID")` calls in each unit. Validate generated stack units under `example/non-prod-env/.../.terragrunt-stack`, not the raw `units/*` templates, because the templates need parent root/common files to resolve.

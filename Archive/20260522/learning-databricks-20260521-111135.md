---
type: short-term-learning
topic: databricks
created: 2026-05-21 11:11:35
source: codex-conversation
confidence: high
---

# Learning: Pass Databricks deployer SP ID from account-scoped Terragrunt config

## Context
During the platform E2E blocker work, account-level Terraform modules needed to grant the IaC deployer service principal metastore and workspace administration permissions. Earlier memory said to derive this from the provider, but that is not viable with account-scoped Databricks providers because `databricks_current_user` requires workspace context.

## Learning
Use an explicit `deployer_service_principal_application_id` input for account-level wrappers and resolve the service principal through account-compatible Databricks data sources. In Terragrunt, source the value from account/environment scoped config, normally `get_env("DATABRICKS_CLIENT_ID")`, so the granted SP matches the CI OIDC identity.

## Future Use
Do not look up the deployer by display name or hard-code one global SP ID in a generic root. If multi-account deployment becomes real, use one CI environment per account or account-scoped live config with explicit per-account application IDs.

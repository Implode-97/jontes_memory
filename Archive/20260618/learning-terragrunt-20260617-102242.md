---
type: short-term-learning
topic: terragrunt
created: 2026-06-17 10:22:42
source: codex-conversation
confidence: high
---

# Learning: data product catalog feature placement

## Context
During a read-only investigation of `/Users/jnyjc2/code/vibe_code/sandbox_catalog`, the user asked for a future Databricks data product catalog feature inspired by `dbx_sandbox_catalogs`.

## Learning
Keep data product catalogs as a separate public feature from sandbox catalogs. The current sandbox unit lives regionally under `dbcore`, reads `dbcore/sandbox_catalogs.yml`, uses `_org/teams.yml` only for team metadata, and delegates resource creation to a focused Terraform wrapper. A data product catalog feature should follow that regional `dbcore` placement and guard/test shape, but use a separate YAML registry, service-principal ownership, GitLab OIDC federation policies, product-specific lifecycle rules, and its own storage pool rather than adding product modes to the sandbox wrapper.

## Future Use
For future implementation or review, prefer a new `units/dbx_data_product_catalogs` catalog unit and a dedicated Terraform wrapper/primitive set. Reuse low-level catalog, external-location, storage-credential, grant, guard, and E2E patterns, while keeping live environment placement and registry normalization in Terragrunt.

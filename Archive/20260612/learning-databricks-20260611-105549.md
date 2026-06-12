---
type: short-term-learning
topic: databricks
created: 2026-06-11 10:55:49
source: codex-conversation
confidence: high
---

# Learning: Databricks Catalog Rename Terraform Pattern

## Context
The user asked whether a Terraform/Terragrunt module can support renaming Databricks Unity Catalog catalogs by using a UUID registry key separate from the catalog name.

## Learning
As of June 11, 2026, Databricks documents catalog rename support in the platform/UI and the Go SDK exposes `new_name` on catalog updates, but the Terraform provider still documents `databricks_catalog.id` as the catalog name and has an open issue where catalog rename fails. A stable Terragrunt key, such as a UUID, preserves Terraform resource addresses across name changes, but it does not by itself solve provider state because the provider/resource ID and related child-resource IDs remain name-based.

## Future Use
For Databricks catalog modules, recommend stable registry keys for address stability, but treat catalog renames as explicit migrations: API/UI rename, then state remove/import for the catalog and any dependent name-keyed resources. Avoid hiding rename logic inside a generic Terraform module unless the provider gains tested native rename support.

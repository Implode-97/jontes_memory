---
type: short-term-learning
topic: terragrunt
created: 2026-06-29 11:09:48
source: codex-conversation
confidence: high
---

# Learning: hcl validate can be mock-backed

## Context
While validating the sandbox catalog `dbx_external_locations` generated Terragrunt unit, `terragrunt hcl validate` succeeded only after a dummy `DATABRICKS_CLIENT_ID` was provided.

## Learning
For `data-platform-terragrunt-catalog` external locations, a successful `terragrunt hcl validate --working-dir .terragrunt-stack/dbx_external_locations` does not prove upstream stack state exists. The rendered unit allows mock outputs for `validate`, and Terragrunt warns that dependency configs with no outputs are returning mocks for `dbx_data_product_catalogs`, `dbx_metastore`, and `dbx_workspace`. Removing `validate` from the mock allow-list in a temp copy makes validation fail on missing `dependency.*.outputs`.

## Future Use
Treat green HCL validation as a render/schema check only. Inspect warnings for mock substitution, and use a separate no-mock or real-state-safe check when the question is whether dependency outputs will exist outside validation.

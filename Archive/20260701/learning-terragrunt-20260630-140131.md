---
type: short-term-learning
topic: terragrunt
created: 2026-06-30 14:01:31
source: codex-conversation
confidence: high
---

# Learning: Name Workspace Dependencies By Role

## Context
While reviewing `dbx_data_product_catalogs`, the user objected to naming the workspace dependency `dev_workspace` because `_workspace_provider.hcl` supplies a generic `workspace_dependency_path`, even if the current example points at `ws_dev`.

## Learning
Terragrunt units should name workspace dependencies by their role in the unit contract, not by the current example workspace. For data product catalogs and similar Unity Catalog features, use names such as `provider_workspace` or `provider_workspace_dependency_path` when the workspace only anchors the configured Databricks workspace provider and supplies `provider_workspace_id`.

## Future Use
Avoid `dev_workspace` naming unless the unit truly requires a dev-only workspace. If the selected workspace can vary through `_workspace_provider.hcl`, keep generated provider blocks, dependency blocks, mocks, and inputs aligned to neutral provider-workspace terminology.

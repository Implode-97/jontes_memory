---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:58:20
source: codex-conversation
confidence: high
---

# Learning: Workspace Provider Parent Needs Active Consumers

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/provider_databricks_workspace.hcl` for readability, KISS, YAGNI, and maintainability.

## Learning
The shared Databricks workspace provider parent is only justified when a current unit actively includes it and needs a simple unaliased workspace provider generated from `_workspace_provider.hcl`. Current catalog workspace-scoped units such as `dbx_workspace_assignments` and `dbx_sandbox_catalogs` generate their own workspace provider blocks because they need aliases, workspace IDs, or unit-specific dependency resolution. A hard-coded `mock-ws-prd` URL in the shared parent hides selector-specific workspace routing, especially when `_workspace_provider.hcl` can point from `ws_dev` or `dbcore` to a different workspace.

## Future Use
For future Terragrunt provider-generation reviews, penalize inactive workspace provider parents and hidden mock/path contracts. Prefer unit-local provider generation until a real shared include exists, or require the mock URL and dependency path to live together in `_workspace_provider.hcl` with a small comment naming the intended consumer pattern.

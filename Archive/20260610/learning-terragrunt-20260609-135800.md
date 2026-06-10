---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 13:58:00
source: codex-conversation
confidence: high
---

# Learning: avoid dormant Databricks workspace provider parents

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/provider_databricks_workspace.hcl`, a shared Terragrunt parent that generates a workspace-scoped Databricks provider but is not currently included by catalog units.

## Learning
For catalog provider-parent reviews, penalize shared Databricks workspace provider configs when they are dormant and duplicate provider generation already embedded in active units such as `dbx_sandbox_catalogs` or `dbx_workspace_assignments`. A parent is easier to accept when it is actively included, preserves the clear provider-generation contract, and derives mock endpoint values from `_workspace_provider.hcl` selectors instead of hard-coding a workspace slug that can drift from the selected dependency path.

## Future Use
When reviewing similar Terragrunt parent files, first search for active includes by filename and compare generated provider aliases, `workspace_id` handling, dependency paths, and mock URL ownership before scoring.

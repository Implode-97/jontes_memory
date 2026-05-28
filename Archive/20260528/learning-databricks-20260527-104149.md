---
type: short-term-learning
topic: databricks
created: 2026-05-27 10:41:49
source: codex-conversation
confidence: high
---

# Learning: Workspace IDs Own Databricks Binding Semantics

## Context
During the sandbox catalog module MR, the user corrected a `provider_workspace_id` fallback that inferred workspace bindings from the configured Databricks provider workspace.

## Learning
For Databricks Unity Catalog modules, the configured provider workspace is execution context only and must not change the resource access contract. `workspace_ids` should be the single source of truth for workspace binding behavior. A non-empty set means create explicit read-write workspace binding resources for exactly those IDs. An empty set is a valid all-workspaces/open mode and should not fail validation or create binding resources.

## Future Use
Do not add `provider_workspace_id` inputs or subtract/add the provider workspace from binding sets. When supporting empty workspace bindings, also set the Databricks securable isolation mode to open; omitting binding resources alone is not enough when a resource is isolated.

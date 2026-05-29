---
type: short-term-learning
topic: databricks
created: 2026-05-28 10:21:09
source: codex-conversation
confidence: high
---

# Learning: Workspace Provider Owns Automatic Binding

## Context
During the Unity Catalog sandbox module refactor, the account-scoped catalog, external location, storage credential, and catalog grant modules were renamed to explicit `databricks-workspace-*` modules.

## Learning
For these Unity Catalog securable modules, callers should pass the intended access contract as `workspace_ids`. The lower module should own the Databricks-specific detail that a workspace-level provider automatically binds the creating workspace for isolated securables, so it subtracts `provider_workspace_id` internally before creating explicit `databricks_workspace_binding` resources. Do not push that `setsubtract` into wrapper or Terragrunt call sites, because that would make callers pass a mutated access set rather than the real intended set.

## Future Use
When working on these modules or their Terragrunt callers, keep `workspace_ids` as the declarative access contract. Use `provider_workspace_id` only to model the implicit Databricks provider-workspace binding and to validate selected-workspace mode.

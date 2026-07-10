---
type: short-term-learning
topic: databricks
created: 2026-07-06 14:35:30
source: codex-conversation
confidence: high
---

# Learning: Do not set workspace_id on workspace providers

## Context
During the CAV-133911 Terragrunt external-location MR, generated Databricks workspace provider blocks included both `host` and `workspace_id`. The user pointed out that `workspace_id` does not appear to be supported for the Databricks workspace provider.

## Learning
For workspace-level Databricks providers, configure the provider with the workspace `host` and do not add `workspace_id` to the provider block. Keep `workspace_id` values only where they are actual module inputs or resource arguments. For Unity Catalog modules, `provider_workspace_id` remains a separate module contract used to model the provider workspace as the Databricks implicit binding anchor.

## Future Use
When generating Terragrunt Databricks workspace providers, use `host` only unless current official provider docs prove another argument is needed. Do not confuse provider configuration with the module-level `workspace_ids` or `provider_workspace_id` access contract.

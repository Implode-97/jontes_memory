---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 14:01:36
source: codex-conversation
confidence: high
---

# Learning: Penalize Commented Catalog Root State

## Context
This came from scoring `data-platform-terragrunt-catalog/example/non-prod-env/root.hcl` for looks, readability, KISS, YAGNI, maintainability, and safety-aware simplification.

## Learning
In the catalog example root, a broad commented-out `remote_state` block plus locals used only by that commented block should be treated as dormant state scaffolding, not useful shared root behavior. The live analog has active S3 `remote_state` with `use_lockfile = true` and no DynamoDB table, so an inert catalog block containing `dynamodb_table = local.root_common.lock_table_name` reads as stale drift.

## Future Use
When reviewing catalog root files, preserve active backend generation if it is actually needed. Otherwise prefer deleting commented backend templates and unused root locals, and update docs/config that claim inactive state behavior.

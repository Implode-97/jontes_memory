---
type: short-term-learning
topic: databricks
created: 2026-06-17 11:54:04
source: codex-conversation
confidence: high
---

# Learning: refactor_aula greenfield pass-through shape

## Context
During the `refactor_aula` Databricks Apps refactor, the user clarified that this should be treated as greenfield and old implementation files should be removed, not merely excluded from bundle sync.

## Learning
The active app shape is a thin root `app.py` plus lowercase `aula_app/` modules and tests. Legacy top-level `Pages/`, `Utilities/`, `Resources/`, `Queries/`, `dst_data/`, `app_runner.py`, and `sync_files*.py` were deleted. The app remains user pass-through only: no Databricks App resources for SQL warehouses, Unity Catalog securables, or volumes. The default/selectable warehouse config mirrors `../everything_app` with ATS Warehouse ID `30c57a3c1e729b60` and warehouse options JSON. The App-level serverless `budget_policy_id` is wired to required bundle variable `firecrackers_budget_policy_id`; do not use a fake default.

## Future Use
Future agents should keep this greenfield shape, avoid restoring compatibility shims or legacy query-file loaders, and require the real Firecrackers budget policy ID at deploy/validate time.

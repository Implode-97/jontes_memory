---
type: short-term-learning
topic: databricks
created: 2026-06-17 11:43:16
source: codex-conversation
confidence: high
---

# Learning: refactor_aula warehouse and budget policy config

## Context
During the greenfield `refactor_aula` Databricks App refactor, the user clarified that the app should inherit the warehouse selector behavior from the prior `../everything_app` pass-through proof of concept.

## Learning
For this refactor, keep pass-through-only access and use the `everything_app` warehouse defaults/options: default warehouse `ATS Warehouse` with ID `30c57a3c1e729b60`, plus the full selectable warehouse JSON list in both `databricks.yml` and `app.yaml`. The app-level serverless budget policy should target Firecrackers, but the Databricks App bundle field is `budget_policy_id`, so use an explicit `firecrackers_budget_policy_id` DAB variable until the provider-created `policy_ids["team:firecrackers"]` value is supplied.

## Future Use
Do not remove the warehouse selector config as an accidental legacy carry-over. Before deployment, replace the budget-policy placeholder with the actual Firecrackers policy ID from the account budget policy stack or API.

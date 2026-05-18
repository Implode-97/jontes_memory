---
type: short-term-learning
topic: terragrunt
created: 2026-05-15 13:21:30
source: codex-conversation
confidence: high
---

# Learning: Serverless budget policy account scope

## Context
This corrected a prior MR review assumption for the Terragrunt catalog serverless budget policy feature.

## Learning
The serverless budget policy feature has been changed from workspace-scoped to account-wide/account-scoped. Future reviews should not treat account scope or empty workspace bindings as automatically wrong, but should still flag stale Jira/docs if they still describe a workspace-scoped contract.

## Future Use
When reviewing `dbx_budget_policy` or successor `serverless-budget-policy` catalog work, judge account-level implementation against Databricks account-level serverless usage policy semantics, naming collision risk, cost attribution tags, and explicit E2E/CI rollout decisions.

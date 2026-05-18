---
type: short-term-learning
topic: terragrunt
created: 2026-05-15 12:54:24
source: codex-conversation
confidence: high
---

# Learning: Terragrunt catalog MR review workflow

## Context
This came from reviewing Neenu's latest Terragrunt catalog MR for the serverless budget policy feature.

## Learning
The `ng-iac` workspace contains a repo-local `skills/iac-mr-review` skill for reviewing colleague MRs in the Data Platform IaC repos. For serverless budget policy work, `CAV-131549` currently describes a workspace-scoped `dbx_serverless_budget_policy` contract unless Jira or an ADR is updated to say account scope is the accepted direction.

## Future Use
For future catalog/module/live MR reviews, use `iac-mr-review` plus `iac-code-style`, check Jira before accepting scope changes, and treat account-wide serverless budget policy implementations as suspect when the story still says every policy must bind to the current workspace.

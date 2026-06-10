---
type: short-term-learning
topic: terragrunt
created: 2026-06-09 15:39:01
source: codex-conversation
confidence: high
---

# Learning: dbx_workspace Terragrunt Sleep Is Cleanup Debt

## Context
This came from scoring `data-platform-terragrunt-catalog/units/dbx_workspace/terragrunt.hcl` for looks, readability, KISS, YAGNI, and maintainability under a strict Terragrunt review lens.

## Learning
The `dbx_workspace` unit's real complexity is mostly justified: regional/account config reads, the regional metastore dependency with plan-time mocks, and the generated `databricks.mws` provider alias all reflect real wrapper/provider boundaries. The main readability and maintainability debt is the unconditional post-apply `sleep 300`, which preserves a real fresh workspace/metastore readiness concern but is blind, slow, and not supported by official docs as a preferred readiness mechanism. The repeated GitLab registry token `extra_arguments` and `remove_tests` init hook also look like shared catalog boilerplate that should move upward or into packaging.

## Future Use
When reviewing this unit or siblings, preserve account-provider and metastore dependency safety. Prefer replacing fixed waits with bounded readiness checks or downstream/E2E readiness gates, and centralize repeated registry/test-removal boilerplate before adding more unit-local copies.

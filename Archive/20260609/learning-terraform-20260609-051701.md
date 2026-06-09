---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:17:01
source: codex-conversation
confidence: high
---

# Learning: Workspace assignment prd mock test should not duplicate dev shape

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_team_role_nesting_prd_mock_apply.tftest.hcl`, the user asked for a Terraform mock semantics and Databricks nesting lens focused on prd-specific value.

## Learning
For this wrapper, a separate prd mock-apply team-nesting file is weak unless it proves distinct prd behavior. The module's default mapping is explicitly the same for dev and prd: consumer to workspace_consume, reader to sql_access, and contributor to workspace_access. Duplicating the dev fixture with `ws_prd` and a different team key mostly preserves test data and diagnostic outputs, not provider behavior. Exact mocked membership resource IDs are arbitrary evidence; direct group/member edge checks and `workspace_team_assignments` are stronger.

## Future Use
When reviewing or editing these tests, prefer one consolidated team-nesting mock apply test unless prd adds a real contract. Keep assertions on stable downstream fields and direct nesting facts; move users-baseline coverage to focused baseline tests.

---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:06:41
source: codex-conversation
confidence: high
---

# Learning: Databricks workspace assignment validation test is shippable but weak

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-workspace-assignments-wrapper/tests/mock_plan_validation.tftest.hcl`, the user asked for Terraform test semantics and maintainer readability feedback focused on mock/provider setup, plan vs apply, `expect_failures`, naming, and ease of adding future cases.

## Learning
The file's mock-provider plan shape is appropriate for fast variable-validation coverage, and `expect_failures` against variables is the right Terraform test primitive. The cleanup pressure is not provider setup but contract strength: the happy path asserts `workspace_context.region_code`, which is likely an input echo/test-shaped output, while `workspace_team_assignments` is the stronger downstream contract. The invalid cases are useful but representative rather than exhaustive, missing high-value edge branches such as empty/trimspace-only values, max-length failures, and isolated uppercase/space cases.

## Future Use
For future reviews of this wrapper's validation tests, accept the plan/`expect_failures` approach but prefer narrowing the happy assertion to `workspace_team_assignments` and adding explicit edge-case runs before scoring the file as high quality.

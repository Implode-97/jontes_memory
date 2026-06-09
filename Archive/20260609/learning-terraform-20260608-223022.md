---
type: short-term-learning
topic: terraform
created: 2026-06-08 22:30:22
source: codex-conversation
confidence: high
---

# Learning: Mock Plan Invalid Tests For Databricks Group Membership

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-group-membership/tests/invalid_inputs_mock_plan.tftest.hcl`, the user asked for a Terraform maintainability/test-contract perspective focused on mock safety, readability, KISS/YAGNI, and public input/precondition coverage.

## Learning
For this module, a compact mock-provider plan-only invalid-input suite is a good fit in the default `tests/` path. It should assert variable validation failures with `expect_failures = [var.*]` and cross-variable precondition failures on `data.databricks_group.target`, without adding live Databricks lookups or broad fixture machinery. The current pattern is readable and shippable, but coverage should be judged representative rather than exhaustive.

## Future Use
In future reviews, preserve the simple per-run negative cases and mock plan posture. Penalize unnecessary helper abstraction or live test coupling, and suggest only narrow additions for missing public contract branches such as max length, empty member strings, embedded user whitespace, and self-nesting edge cases when those branches matter.

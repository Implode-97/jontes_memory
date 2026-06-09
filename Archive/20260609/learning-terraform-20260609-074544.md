---
type: short-term-learning
topic: terraform
created: 2026-06-09 07:45:44
source: codex-conversation
confidence: high
---

# Learning: storage credential invalid-input test is clean but precondition naming matters

## Context
During a read-only review of `data-platform-terraform-modules/modules/databricks-workspace-storage-credential/tests/invalid_inputs_mock_plan.tftest.hcl`, the user asked for a new platform maintainer perspective focused on scanability, run names, KISS, YAGNI, and maintainability.

## Learning
For storage credential invalid-input tests, the strong pattern is a mocked Databricks provider, an explicit workspace-level current-config override, one valid root `variables` baseline, and one invalid override per `run` with precise `expect_failures`. Variable validation runs should name the variable-shaped failure directly, while selected-workspace provider membership failures should be labeled as resource preconditions because `expect_failures = [databricks_storage_credential.this]` is not a variable validation.

## Future Use
When reviewing adjacent Terraform `.tftest.hcl` files, preserve compact plan-only negative cases and avoid null/wrong-type matrices. Penalize misleading run names more than fixture repetition, and recommend small renames before asking for new abstractions or broader coverage.

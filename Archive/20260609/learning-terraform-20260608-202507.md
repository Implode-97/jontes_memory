---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:25:07
source: codex-conversation
confidence: high
---

# Learning: Keep Live Terraform Tests Opt-In

## Context
While scoring Databricks workspace wrapper tests, real-provider `.tftest.hcl` files under the default `tests/` directory were compared against the active CI runner that executes plain `terraform test`.

## Learning
Terraform `command = plan` and especially `command = apply` tests that need shared AWS/Databricks fixtures, credentials, account IDs, VPCs, metastores, or service principals should not be treated like ordinary mock tests. If they live under default `tests/`, a plain CI/local `terraform test` can run them unintentionally. Apply tests are more sensitive because they create real infrastructure and only attempt cleanup afterward.

## Future Use
When reviewing or writing Terraform tests, prefer mock tests in default `tests/`. Put real-provider smoke tests in an explicit opt-in path such as `real_tests/`, or require clear CI filters/manual protected jobs. If a real test is kept, it should document fixture ownership, add traceable tags/cleanup guidance, and assert facts mocks cannot prove.

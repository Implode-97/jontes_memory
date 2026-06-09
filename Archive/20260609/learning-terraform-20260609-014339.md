---
type: short-term-learning
topic: terraform
created: 2026-06-09 01:43:39
source: codex-conversation
confidence: high
---

# Learning: Mock Service Principal Validation Tests

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-service-principal/tests/invalid_inputs_real_provider_plan.tftest.hcl`, the user asked whether a plan-only validation file earns a real Databricks provider setup in default CI.

## Learning
For `databricks-account-service-principal`, invalid `service_principal_display_name` tests should be mock-provider plan tests when they only exercise `variables.tf` validation. A hard-coded Databricks account provider is unnecessary shared-environment coupling for plain `terraform test`, even though plan-only expected variable failures are less risky than live apply tests.

## Future Use
When reviewing or editing this module's invalid-input tests, prefer concise explicit negative cases with `expect_failures = [var.service_principal_display_name]`, but replace real-provider setup with mock-provider setup and keep live Databricks account fixtures in the apply smoke or protected/manual lanes.

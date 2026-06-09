---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:47:24
source: codex-conversation
confidence: high
---

# Learning: Catalog Grant Invalid Input Tests Are Clean But Not Complete

## Context
During a read-only KISS/YAGNI review of `data-platform-terraform-modules/modules/databricks-workspace-catalog-grant/tests/invalid_inputs_mock_plan.tftest.hcl`, the user asked for a strict Terraform validation-test score without running tests or using other agents.

## Learning
For this tiny Databricks workspace catalog grant module, the mock-plan invalid-input shape is mostly good: top-level valid baseline variables, a mock Databricks provider, an `is_account = false` current-config override, and one invalid value per `run` using `expect_failures = [var.*]`. This avoids repeated fixture noise and gives clear diagnostics. The cleanup pressure is coverage, not structure: add only high-value negative branches such as uppercase/malformed underscore catalog names and, somewhere in mock plan coverage, the workspace-provider precondition with `is_account = true`.

## Future Use
Preserve the explicit one-invalid-case-per-run style for diagnostics. Do not add table abstractions or provider privilege enum tests unless the module contract changes.

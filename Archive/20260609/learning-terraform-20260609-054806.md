---
type: short-term-learning
topic: terraform
created: 2026-06-09 05:48:06
source: codex-conversation
confidence: high
---

# Learning: Keep grant validation tests module-local

## Context
During read-only scoring of `databricks-workspace-catalog-grant/tests/invalid_inputs_mock_plan.tftest.hcl`, reviewers evaluated Terraform `expect_failures`, mock providers, and Databricks privilege boundaries.

## Learning
For a small Unity Catalog grant module that passes privilege strings through, invalid-input tests should focus on the module-owned contract: local naming regexes, non-empty principal, non-empty privilege set, and no blank privilege values. They should avoid Databricks privilege-enum matrices, principal existence checks, catalog lookup, or workspace-binding behavior unless the module itself owns those semantics.

## Future Use
When reviewing Terraform validation tests, prefer one invalid input per `run` with `expect_failures = [var.<name>]`, a compact top-level valid fixture, and mock provider setup that keeps failures focused. Add provider-scope precondition tests separately only when that guard is part of the module contract.

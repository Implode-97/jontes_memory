---
type: short-term-learning
topic: terraform
created: 2026-06-08 23:26:17
source: codex-conversation
confidence: high
---

# Learning: Keep metastore invalid-input tests focused

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/databricks-account-metastore/tests/invalid_inputs_mock_plan.tftest.hcl`, the user asked for a Terraform validation semantics perspective.

## Learning
For the Databricks account metastore module, the invalid-input mock plan file is intentionally narrow and shippable when it uses one valid top-level baseline and one overridden invalid variable per `run`. `expect_failures` should point to the specific variable under test, which helps detect accidental extra diagnostics and keeps validation failures isolated. Do not add exhaustive null, wrong-type, or Terraform type-system cases unless a real module contract risk appears.

## Future Use
When reviewing adjacent validation tests, preserve this focused structure: mock provider for plan safety, baseline valid variables, one negative case per validation rule, and no table-like abstraction unless the file becomes materially larger.

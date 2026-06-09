---
type: short-term-learning
topic: terraform
created: 2026-06-08 20:13:05
source: codex-conversation
confidence: high
---

# Learning: mock apply tests should prove apply-only wrapper behavior

## Context
During a read-only scoring review of `data-platform-terraform-modules/modules/aws-databricks-workspace-wrapper/tests/mock_apply_outputs.tftest.hcl`, the user asked for a strict KISS/YAGNI Terraform test review focused on duplicated mocks, output-mirroring assertions, and whether apply tests prove more than plan tests.

## Learning
For this workspace wrapper, mocked apply coverage is useful only when it protects apply-only semantics or bootstrap contracts, such as Databricks workspace URL/ID materialization, metastore readiness diagnostics, identity federation timing, admin group membership, and MWS permission assignment. Broad assertions that mirror the output API, repeat mocked plan assertions, or preserve whole internal output objects should be treated as maintenance debt.

## Future Use
When reviewing adjacent wrapper tests, keep mocked apply tests lean: preserve a small bootstrap/apply contract test, move naming and validation assertions to plan tests, and avoid using tests to freeze broad diagnostic or internal passthrough outputs.

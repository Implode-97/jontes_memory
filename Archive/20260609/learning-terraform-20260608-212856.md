---
type: short-term-learning
topic: terraform
created: 2026-06-08 21:28:56
source: codex-conversation
confidence: high
---

# Learning: Avoid Test-Only Terraform Outputs

## Context
While scoring the `aws-s3-bucket-secure` module, several tests asserted hardening defaults through public diagnostic outputs and one mocked apply test overrode the final policy document it was meant to verify.

## Learning
Terraform module outputs should represent real downstream contracts, not exist mainly to keep tests stable across refactors. If a value is only used by the module's own `.tftest.hcl` files, prefer direct resource or data-source assertions in tests instead of freezing internal resource shape as public API. Mock overrides should not replace the invariant under test; for example, do not override the final rendered policy document and then claim to test policy merge behavior.

## Future Use
When reviewing or writing Terraform tests, check whether outputs are test-only. Score those lower before API freeze and recommend removing or clearly marking diagnostic outputs. For mocked tests, preserve the real logic path for the behavior being asserted.
